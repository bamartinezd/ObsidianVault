# Message Queue Services — Teoría, Providers y Patrones

## Tabla de contenidos

1. [Teoría base](#1-teoría-base)
2. [Los 3 Providers implementados](#2-los-3-providers-implementados)
3. [Flujo completo de cada provider](#3-flujo-completo-de-cada-provider)
4. [Comparativa Pros/Cons](#4-comparativa-proscons)
5. [Patrones de diseño del repo](#5-patrones-de-diseño-del-repo)
6. [Patrones que se suelen usar con MQ](#6-patrones-que-se-suelen-usar-con-mq)
7. [Qué hace bien el repo y qué se podría mejorar](#7-qué-hace-bien-el-repo-y-qué-se-podría-mejorar)

---

## 1. Teoría base

### ¿Qué es un Message Queue?

Un **message queue** es un sistema intermediario que permite la comunicación **asíncrona** entre procesos. Un **productor** envía un mensaje a la cola y un **consumidor** lo recibe en otro momento. El productor no espera al consumador (a diferencia de una llamada síncrona HTTP).

**Componentes clave:**

| Componente | Rol |
|---|---|
| **Producer** | Emite el mensaje al broker |
| **Broker** | Almacena el mensaje (cola o topic) |
| **Consumer** | Recibe y procesa el mensaje |
| **Topic / Exchange** | Nombre lógico del canal de comunicación |

### ¿Por qué usar colas en vez de HTTP directo?

| Situación | HTTP (síncrono) | Message Queue (asíncrono) |
|---|---|---|
| El consumer está caído | Error 500/503, retry en el caller | El mensaje queda encolado, se procesa cuando vuelva |
| Gran volumen de mensajes | El producer se satura esperando respuestas | El broker absorbe el pico, los consumers procesan a su ritmo |
| Múltiples consumers | Point-to-point (1 a 1) | Pub/Sub (1 a N consumidores) |
| Tolerancia a fallos | Un fallo rompe la cadena | Aislamiento total entre producer y consumer |

### Modelos de mensajería

| Modelo | Descripción | Ejemplo |
|---|---|---|
| **Point-to-Point (Cola)** | Un mensaje va de un producer a un solo consumer | SQS clásico |
| **Pub/Sub (Publish/Subscribe)** | Un mensaje lo reciben todos los suscriptores | Kafka topics, SNS, RabbitMQ Fanout |
| **Request/Reply** | El producer envía y espera respuesta en otra cola | Patrón menos común, AMQP reply-to |

---

## 2. Los 3 Providers implementados

Cada microservicio en este repo implementa **3 providers** de mensajería intercambiables. La elección se hace por argumento de línea de comandos (`kafka`, `rabbitmq`, o `sns`).

| Provider | Tecnología | NuGet | Modelo |
|---|---|---|---|
| `kafka` | Apache Kafka | `Confluent.Kafka` | Log distribuido persistente |
| `rabbitmq` | RabbitMQ | `RabbitMQ.Client` | Cola tradicional con exchanges |
| `sns` | AWS SNS → SQS | `AWSSDK.SNS` + `AWSSDK.SQS` | Pub/Sub managed de AWS |

### Arquitectura general del repo

```
ServiceOrders ──publica──>  orden de servicio  (OrderCreatedEvent → topic "service-orders")
SkillService  ──publica──>  habilidad          (SkillCreatedEvent  → topic "skill-events")
ClientService ──publica──>  cliente PII        (ClientCreatedEvent → topic "client-events")
ClientService ──consume──>  órdenes filtradas por país desde "service-orders"
```

Cada servicio tiene una **interfaz** (ej: `IOrderPublisher`) y 3 implementaciones concretas. El provider se elige en `Program.cs` con un `switch`:

```csharp
IOrderPublisher publisher = provider.ToLowerInvariant() switch
{
    "kafka"    => new KafkaOrderPublisher("localhost:9092", "service-orders"),
    "rabbitmq" => new RabbitMqOrderPublisher("localhost", "service-orders"),
    "sns"      => new SnsSqsOrderPublisher("us-east-1", "arn:aws:sns:...:service-orders"),
    _          => throw new ArgumentException($"Unknown provider: {provider}")
};
```

---

## 3. Flujo completo de cada provider

### 3.1 Apache Kafka

#### ¿Cómo funciona Kafka?

Kafka almacena mensajes en un **log persistente** dividido en **partitions** dentro de un **topic**. Cada partition es un archivo append-only donde las entradas tienen un **offset** (posición secuencial). Los consumidores llevan registro de su offset: si se caen y vuelven, retoman desde donde quedaron.

```
Topic "service-orders"
┌─────────────────────────────────────────────────────────┐
│ Partition 0: [msg0] [msg1] [msg2] [msg3] [msg4] ...   │
│ Partition 1: [msg0] [msg1] [msg2] ...                  │
│ Partition 2: [msg0] [msg1] ...                          │
└─────────────────────────────────────────────────────────┘
      ↑
      Offset (posición del último mensaje leído por el consumer)
```

**Clave (`Key`):** si dos mensajes tienen la misma key, van a la misma partition → garantiza orden por esa key.

#### Publisher: KafkaOrderPublisher

**Archivo:** `ServiceOrders/Publishers/KafkaOrderPublisher.cs`

```csharp
// Configuración
var config = new ProducerConfig { BootstrapServers = "localhost:9092" };
_producer = new ProducerBuilder<string, string>(config).Build();

// Publicación
var json = JsonSerializer.Serialize(orderEvent);
await _producer.ProduceAsync("service-orders", new Message<string, string>
{
    Key = orderEvent.OrderId,   // ← misma orderId = misma partition
    Value = json
});
```

**Qué hace:**
1. Serializa el `OrderCreatedEvent` a JSON.
2. Lo envía al topic `"service-orders"` con **key = OrderId**.
3. Kafka asigna la partition basándose en el hash de la key (mismo OrderId siempre cae en la misma partition).
4. `ProduceAsync` es awaitable: confirma que el broker recibió el mensaje (o lanza exception si no).

**Dato:** también se publica desde `ClientService` y `SkillService` a sus respectivos topics (`"client-events"`, `"skill-events"`) usando la misma lógica.

#### Consumer: KafkaOrderConsumer

**Archivo:** `ClientService/Consumers/KafkaOrderConsumer.cs`

```csharp
var config = new ConsumerConfig
{
    BootstrapServers = "localhost:9092",
    GroupId = "client-service-group",    // ← grupo de consumidores
    AutoOffsetReset = AutoOffsetReset.Earliest,  // ← si no hay offset, empezar desde el inicio
    EnableAutoCommit = true              // ← auto-confirmar lectura
};

using var consumer = new ConsumerBuilder<string, string>(config).Build();
consumer.Subscribe("service-orders");  // ← suscribirse al topic

while (!cancellationToken.IsCancellationRequested)
{
    var result = consumer.Consume(cancellationToken);  // ← bloquea hasta recibir msg
    var orderEvent = JsonSerializer.Deserialize<OrderCreatedEvent>(result.Message.Value);

    if (orderEvent.OriginCountry.Equals(filterCountry, StringComparison.OrdinalIgnoreCase))
    {
        // Procesar: imprimir datos de la orden
    }
    else
    {
        // Ignorar: orden de otro país
    }
}
```

**Qué hace:**
1. Se conecta con el **consumer group** `"client-service-group"`.
2. Kafka reparte las partitions entre los consumers del mismo grupo (load balancing).
3. **Bloquea** en `consumer.Consume()` esperando mensajes.
4. Deserializa el JSON a `OrderCreatedEvent`.
5. **Filtra por `OriginCountry`** en el consumer (no en el broker).
6. Si coincide con `filterCountry`, imprime los datos. Si no, ignora el mensaje.
7. `EnableAutoCommit = true` → el offset se confirma automáticamente después de consumir.

**Diferencia clave con los otros providers:** Kafka mantiene el log después de consumir. Si mañana agregás otro consumer al mismo grupo, puede leer mensajes viejos (hasta la retención del topic).

#### Flujo visual Kafka

```
ServiceOrders                           Kafka                              ClientService
    │                                     │                                     │
    │  ProduceAsync("service-orders")     │                                     │
    │  Key: OrderId, Value: JSON ────────>│  Guarda en partition X              │
    │                                     │                                     │
    │                                     │  consumer.Consume() ───────────────>│
    │                                     │  (bloquea hasta tener msg)          │
    │                                     │                                     │
    │                                     │  <── Deserialize JSON               │
    │                                     │  <── Filter by OriginCountry        │
    │                                     │  <── Procesar o ignorar             │
    │                                     │  <── AutoCommit offset              │
```

---

### 3.2 RabbitMQ

#### ¿Cómo funciona RabbitMQ?

RabbitMQ es un **broker de colas tradicional**. El productor publica a un **exchange**, y RabbitMQ enruta el mensaje a las **colas** vinculadas a ese exchange según el tipo de exchange.

| Exchange Type | Comportamiento |
|---|---|
| **Direct** | Enruta por routing key exacta |
| **Fanout** | Envía a TODAS las colas vinculadas (broadcast) |
| **Topic** | Enruta por patrón wildcard (`*.log`, `order.#`) |
| **Headers** | Enruta por headers del mensaje |

**Este repo usa `Fanout`** → el mensaje llega a todos los suscriptores.

```
Exchange "service-orders" (Fanout)
    │
    ├──> Cola A (ClientService, exclusive)
    └──> Cola B (otro consumer, exclusive)
```

**Diferencia con Kafka:** no hay log persistente. Si la cola es `exclusive` y `autoDelete`, desaparece cuando el consumer se desconecta. No hay "replay" de mensajes viejos.

#### Publisher: RabbitMqOrderPublisher

**Archivo:** `ServiceOrders/Publishers/RabbitMqOrderPublisher.cs`

```csharp
// Configuración
var factory = new ConnectionFactory { HostName = "localhost" };
_connection = factory.CreateConnectionAsync().GetAwaiter().GetResult();
_channel = _connection.CreateChannelAsync().GetAwaiter().GetResult();
_channel.ExchangeDeclareAsync("service-orders", ExchangeType.Fanout, durable: true)
        .GetAwaiter().GetResult();

// Publicación
var json = JsonSerializer.Serialize(orderEvent);
var body = Encoding.UTF8.GetBytes(json);
await _channel.BasicPublishAsync("service-orders", routingKey: "", body: body);
```

**Qué hace:**
1. Se conecta a RabbitMQ en `localhost`.
2. Declara un **exchange** `"service-orders"` tipo `Fanout`, **durable** (sobrevive restarts del broker).
3. Serializa el `OrderCreatedEvent` a JSON, lo convierte a bytes.
4. Publica con `routingKey: ""` (vacío porque es Fanout: no filtra por routing key).

**Nota:** los constructores de RabbitMQ usan `.GetAwaiter().GetResult()` porque los constructores no pueden ser `async`. En producción esto debería usar un factory o `IAsyncDisposable`.

#### Consumer: RabbitMqOrderConsumer

**Archivo:** `ClientService/Consumers/RabbitMqOrderConsumer.cs`

```csharp
var factory = new ConnectionFactory { HostName = "localhost" };
using var connection = await factory.CreateConnectionAsync();
using var channel = await connection.CreateChannelAsync();

// Declarar el mismo exchange (idempotente)
await channel.ExchangeDeclareAsync("service-orders", ExchangeType.Fanout, durable: true);

// Declarar cola exclusiva (se borra cuando el consumer se desconecta)
var queueDeclare = await channel.QueueDeclareAsync(
    queue: "",
    durable: false,
    exclusive: true,
    autoDelete: true
);

// Vincular cola al exchange
await channel.QueueBindAsync(queueDeclare.QueueName, "service-orders", routingKey: "");

// Consumir
var consumer = new AsyncEventingBasicConsumer(channel);
consumer.ReceivedAsync += async (model, ea) =>
{
    var body = ea.Body.ToArray();
    var json = Encoding.UTF8.GetString(body);
    var orderEvent = JsonSerializer.Deserialize<OrderCreatedEvent>(json);

    if (orderEvent.OriginCountry.Equals(filterCountry, StringComparison.OrdinalIgnoreCase))
    {
        // Procesar
    }
    else
    {
        // Ignorar
    }
};

await channel.BasicConsumeAsync(queueDeclare.QueueName, autoAck: true, consumer: consumer);
await Task.Delay(Timeout.Infinite, cancellationToken);  // ← mantener vivo
```

**Qué hace:**
1. Se conecta a RabbitMQ y declara el **mismo exchange** `"service-orders"` Fanout (declarar es idempotente).
2. Declara una **cola exclusiva** (`queue: ""` → nombre automático, `exclusive: true` → solo este consumer, `autoDelete: true` → se borra al cerrar conexión).
3. **Vincula** la cola al exchange con `QueueBind`.
4. Crea un `AsyncEventingBasicConsumer` que escucha eventos.
5. En `ReceivedAsync`: deserializa, filtra por `OriginCountry`, procesa o ignora.
6. `autoAck: true` → confirma receiving automáticamente (sin ack manual).
7. `Task.Delay(Timeout.Infinite)` mantiene el proceso vivo indefinidamente.

**Diferencia clave:** la cola es **exclusiva y auto-delete**. Cada instancia de ClientService crea su propia cola temporal. Si ConsumerService se cae y vuelve, crea una cola nueva y pierde los mensajes que estaban encolados. Para persistencia, se usarían colas durables con nombre fijo.

#### Flujo visual RabbitMQ

```
ServiceOrders                              RabbitMQ                           ClientService
    │                                        │                                     │
    │  BasicPublish("service-orders")        │                                     │
    │  Fanout, routingKey="" ───────────────>│  Enruta a TODAS las colas           │
    │                                        │  vinculadas al exchange             │
    │                                        │                                     │
    │                                        │  QueueBind() <─────────────────────│
    │                                        │  (crea cola exclusiva temporal)     │
    │                                        │                                     │
    │                                        │  ReceivedAsync callback ───────────>│
    │                                        │  (event-driven, no polling)         │
    │                                        │                                     │
    │                                        │  <── Deserialize JSON               │
    │                                        │  <── Filter by OriginCountry        │
    │                                        │  <── Procesar o ignorar             │
    │                                        │  <── autoAck: true                  │
```

---

### 3.3 AWS SNS + SQS

#### ¿Cómo funciona SNS + SQS?

**SNS (Simple Notification Service)** es un sistema de pub/sub managed por AWS. Publicás un mensaje a un **Topic** y AWS lo distribuye a todas las suscripciones (SQS, HTTP, email, Lambda, etc.).

**SQS (Simple Queue Service)** es una cola managed por AWS. Recibe mensajes de SNS y los mantiene hasta que un consumer los procese y los elimine.

```
Publisher → SNS Topic → SQS Queue (suscrita al topic) → Consumer
```

**Diferencia con Kafka/RabbitMQ:** no tenés que administrar brokers. AWS maneja la disponibilidad, escalado, y retención. A cambio, pagás por request.

#### Publisher: SnsSqsOrderPublisher

**Archivo:** `ServiceOrders/Publishers/SnsSqsOrderPublisher.cs`

```csharp
// Configuración
_snsClient = new AmazonSNSClient(RegionEndpoint.GetBySystemName("us-east-1"));
_topicArn = "arn:aws:sns:us-east-1:000000000000:service-orders";

// Publicación
var json = JsonSerializer.Serialize(orderEvent);
await _snsClient.PublishAsync(_topicArn, json);
```

**Qué hace:**
1. Crea un cliente SNS para la región `us-east-1`.
2. Serializa el `OrderCreatedEvent` a JSON.
3. Publica al **SNS Topic** identificado por su ARN (Amazon Resource Name).

**Nota:** no hay control de particiones ni keys. SNS distribuye a todas las suscripciones (SQS queues) vinculadas al topic. El orden no está garantizado.

#### Consumer: SnsSqsOrderConsumer

**Archivo:** `ClientService/Consumers/SnsSqsOrderConsumer.cs`

```csharp
_sqsClient = new AmazonSQSClient(RegionEndpoint.GetBySystemName("us-east-1"));
_queueUrl = "https://sqs.us-east-1.amazonaws.com/000000000000/service-orders-queue";

while (!cancellationToken.IsCancellationRequested)
{
    var request = new ReceiveMessageRequest
    {
        QueueUrl = _queueUrl,
        MaxNumberOfMessages = 10,
        WaitTimeSeconds = 20         // ← Long Polling
    };

    var response = await _sqsClient.ReceiveMessageAsync(request, cancellationToken);

    foreach (var message in response.Messages)
    {
        // SNS envuelve los mensajes: { "Message": "<json-real>", ... }
        string actualMessage;
        using (var doc = JsonDocument.Parse(message.Body))
        {
            actualMessage = doc.RootElement.TryGetProperty("Message", out var msgProp)
                ? msgProp.GetString() ?? message.Body
                : message.Body;
        }

        var orderEvent = JsonSerializer.Deserialize<OrderCreatedEvent>(actualMessage);

        if (orderEvent.OriginCountry.Equals(filterCountry))
        {
            // Procesar
        }

        // Eliminar msg de la cola para que no se reenvíe
        await _sqsClient.DeleteMessageAsync(_queueUrl, message.ReceiptHandle);
    }
}
```

**Qué hace:**
1. Hace **long polling** a SQS: espera hasta 20 segundos por mensajes (`WaitTimeSeconds: 20`).
2. Recibe hasta 10 mensajes por request (`MaxNumberOfMessages: 10`).
3. **Desenvuelve el envelope de SNS:** SNS envuelve el JSON real en `{ "Type": "Notification", "Message": "<json-del-evento>", ... }`. El consumer parsea con `JsonDocument` para extraer solo el `Message`.
4. Deserializa el `OrderCreatedEvent` y filtra por `OriginCountry`.
5. **Elimina el mensaje** de SQS con `DeleteMessageAsync` usando el `ReceiptHandle`. Si no lo elimina, SQS lo reenvía después del `VisibilityTimeout`.

**Diferencia clave:** el manejo del envelope de SNS. Este es un error clásico: recibir el body completo de SNS como si fuera el JSON del evento y fallar al deserializar.

**Long Polling vs Short Polling:**
- Short Polling: consulta inmediata, retorna vacío si no hay mensajes (mucha carga en AWS).
- Long Polling (este repo): espera hasta 20s si no hay mensajes (reduce costos y latencia promedio).

#### Flujo visual SNS + SQS

```
ServiceOrders                              AWS                              ClientService
    │                                        │                                     │
    │  PublishAsync(ARN) ───────────────────>│  SNS Topic "service-orders"         │
    │                                        │                                     │
    │                                        │  Distribuye a suscripciones ───────>│
    │                                        │  SQS Queue "service-orders-queue"   │
    │                                        │                                     │
    │                                        │  ReceiveMessage (long poll) <───────│
    │                                        │  WaitTimeSeconds: 20                │
    │                                        │                                     │
    │                                        │  <── Body: { "Message": "{JSON}" } │
    │                                        │  <── Parse envelope de SNS          │
    │                                        │  <── Deserialize OrderCreatedEvent  │
    │                                        │  <── Filter by OriginCountry        │
    │                                        │  <── DeleteMessage (ack)            │
```

---

## 4. Comparativa Pros/Cons

| | **Kafka** | **RabbitMQ** | **SNS+SQS** |
|---|---|---|---|
| **Durabilidad** | Excelente (log persistente, replicas) | Depende de la cola (durable vs no) | Excelente (managed by AWS) |
| **Replay de mensajes** | Sí (re-leer desde un offset) | No (once consumed, gone) | Parcial (SQS retention configurable, hasta 14 días) |
| **Filtrado en broker** | Por key (partition) | Por routing key / headers | SNS filter policies (JSON) |
| **Filtrado en consumer** | Sí (este repo lo hace) | Sí (este repo lo hace) | Sí (este repo lo hace) |
| **Escalabilidad** | Horizontal (partitions) | Clustering (pero más complejo) | Managed, escala solo |
| **Complejidad operacional** | Alta (ZooKeeper/KRaft, brokers) | Media (un server) | Baja (AWS lo maneja) |
| **Costo** | Self-hosted o Confluent Cloud (pago) | Self-hosted o CloudAMQP (pago) | Pay-per-request |
| **Latencia** | Muy baja (<5ms) | Baja (~10ms) | Variable (10-100ms+) |
| **Orden de mensajes** | Dentro de una partition: garantizado | FIFO dentro de una cola | Solo con SQS FIFO queues |
| **Patrón base** | Log distribuido | Cola tradicional | Pub/Sub managed |

### Cuándo usar cada uno

| Escenario | Provider recomendado |
|---|---|
| Necesitás re-leer eventos viejos (audit, replay) | **Kafka** |
| Tensión simple pub/sub con reglas de routing | **RabbitMQ** |
| Ya estás en AWS y no querés operar infraestructura | **SNS+SQS** |
| Alto throughput (>100k msgs/sec) | **Kafka** |
| Consumidores event-driven con poca latencia | **RabbitMQ** |
| Prototipo rápido o microservicio pequeño | **SNS+SQS** |

---

## 5. Patrones de diseño del repo

### Strategy Pattern

Cada servicio tiene una **interfaz** y 3 implementaciones concretas. El provider se elige en runtime:

```
IOrderPublisher (interfaz)
├── KafkaOrderPublisher
├── RabbitMqOrderPublisher
└── SnsSqsOrderPublisher
```

Esto es **Strategy** implementado con un factory inline en `Program.cs`. Si mañana agregás un provider (Redis Streams, Azure Service Bus), solo sumás una clase que implemente la interfaz sin tocar el consumer ni el resto del código.

### Publisher/Subscriber

Todos los servicios son **productores**. Solo `ClientService` es también **consumidor**. El patrón es Pub/Sub, pero el filtrado se hace en el consumer, no en el broker.

### Dependency Inversion (DIP)

Los consumers y publishers dependen de **interfaces**, no de implementaciones concretas. `ClientService` no sabe si habla con Kafka, RabbitMQ o SQS: solo sabe que tiene un `IOrderConsumer`.

---

## 6. Patrones que se suelen usar con MQ

### 6.1 Idempotencia del consumidor

**Problema:** un mensaje puede llegar dos veces (redeliver en RabbitMQ, re-offset en Kafka, visibility timeout en SQS).

**Solución:** el consumer guarda un registro de `MessageId` o `OrderId` procesados. Si ya lo vio, lo ignora.

```csharp
// Ejemplo conceptual
var processedIds = new HashSet<string>();

if (processedIds.Contains(orderEvent.OrderId))
{
    Console.WriteLine("Duplicate, skipping");
    return;
}

// Procesar...
processedIds.Add(orderEvent.OrderId);
```

**En este repo:** NO está implementado. Si SQS hace redeliver (por timeout de visibilidad), se procesaría dos veces.

### 6.2 Circuit Breaker

**Problema:** si el broker está caído, el publisher lanza excepciones en loop, generando carga innecesaria.

**Solución:** después de N fallos consecutivos, el circuito "se abre" y dejás de intentar por un tiempo. Se cierra automáticamente cuando el broker se recupera.

**Estados:** Closed (normal) → Open (fallo detectado, no intenta) → Half-Open (prueba una vez).

**En el repo:** NO está implementado. Los publishers no tienen retry ni circuit breaker. Si Kafka está caído, `ProduceAsync` lanza excepción y el servicio crashea.

### 6.3 Outbox Pattern

**Problema:** querés guardar en BD **y** publicar un evento, pero no podés hacer ambas cosas atómicamente (BD y broker son distintos).

**Solución:** guardás el evento en una tabla `Outbox` dentro de la misma transacción de la BD. Un polling publisher lo saca de ahí y lo envía al broker.

```
BEGIN TRANSACTION
  INSERT INTO orders (...) 
  INSERT INTO outbox (event_type, payload, created_at)
COMMIT

-- Aparte, un worker:
SELECT * FROM outbox WHERE processed = false
→ Publicar al broker
→ Marcar como processed
```

**En el repo:** no hay base de datos, pero en producción esto sería esencial para consistencia.

### 6.4 Dead Letter Queue (DLQ)

**Problema:** un mensaje falla N veces y queda "envenenado", bloqueando la cola.

**Solución:** después de N reintentos, el mensaje se mueve a una cola de muertos (DLQ) para revisión manual o alertas.

**En el repo:** no está implementado. Si un consumer lanza excepción repetidamente, el mensaje puede quedar en loop infinito o perderse.

### 6.5 Retry con Exponential Backoff

**Problema:** el broker está temporalmente saturado o hay un glitch de red.

**Solución:** reintentar con delays crecientes (1s, 2s, 4s, 8s...) en vez de reintentar inmediatamente.

```csharp
// Ejemplo conceptual
for (int attempt = 0; attempt < maxRetries; attempt++)
{
    try
    {
        await publisher.PublishAsync(evt);
        return;
    }
    catch (Exception)
    {
        await Task.Delay(TimeSpan.FromSeconds(Math.Pow(2, attempt)));
    }
}
throw new Exception("Max retries exceeded");
```

**En el repo:** no hay retries. El `SqsSqsOrderConsumer` al menos tiene try-catch que no crashea el loop de polling.

### 6.6 Saga Pattern

**Problema:** una transacción distribuida (crear orden + validar skill + notificar cliente) toca múltiples servicios.

**Solución:**
- **Orquestada:** un orquestador central coordina los pasos y maneja compensaciones.
- **Coreografiada:** cada servicio reacciona al evento del anterior (como en este repo, donde ClientService consume lo que ServiceOrders publica).

**En el repo:** la estructura la permite pero no está implementado como tal. No hay manejo de compensaciones si un paso falla.

### 6.7 Message Versioning

**Problema:** si cambiás la estructura del `OrderCreatedEvent` (agregás un campo, renombrás uno), los consumers con la versión vieja crashearan al deserializar.

**Solución:** versionar los eventos. Un campo `Version` en el DTO, y consumers que manejen múltiples versiones.

```csharp
public class OrderCreatedEvent
{
    public int Version { get; set; } = 2;  // ← nuevo campo
    // ... resto de campos
}
```

**En el repo:** no hay versioning. Todos los eventos están en Shared y se compilan juntos.

### 6.8 Health Checks

**Problema:** no sabés si la conexión al broker está viva hasta que intentás publicar/consumir.

**Solución:** health checks periódicos que verifican conectividad y reportan el estado.

**En el repo:** no hay health checks.

---

## 7. Qué hace bien el repo y qué se podría mejorar

### Lo que hace bien

| Aspecto | Detalle |
|---|---|
| **Shared project** | Los DTOs de eventos (`Shared/Events/`) están centralizados, evitando duplicación entre servicios |
| **Interfaces por servicio** | `IOrderPublisher` vs `IClientPublisher` vs `ISkillPublisher`: cada interfaz expone solo lo que ese servicio necesita |
| **Filtrado en consumer** | El filtrado por país se hace en el consumer. Es más simple aunque consume más ancho de banda |
| **SQS envelope handling** | El consumer de SQS maneja correctamente el wrapping de SNS (`{ "Message": "..." }`), un error clásico |
| **Strategy pattern** | Providers intercambiables sin tocar la lógica de negocio |

### Lo que se podría mejorar

| Problema | Solución sugerida |
|---|---|
| **No hay DI (Dependency Injection)** | Todo se instancia manualmente en `Program.cs`. Usar `IServiceCollection` + `AddTransient<IOrderPublisher, KafkaOrderPublisher>()` |
| **Constructores sync** | Los publishers de RabbitMQ usan `.GetAwaiter().GetResult()` (blocking). Usar factory async o `IAsyncDisposable` |
| **No hay health checks** | Agregar health checks para Kafka, RabbitMQ y SQS |
| **No hay serialización polimórfica** | Agregar versionado de eventos para evitar breaking changes |
| **Hardcoded config** | Los bootstrap servers, ARNs, etc. están hardcoded. Debería estar en `appsettings.json` o variables de entorno |
| **No hay reintentos** | Agregar retry con exponential backoff en publishers |
| **No hay idempotencia** | Agregar tracking de mensajes procesados en consumers |
| **No hay DLQ** | Configurar dead letter queues para mensajes fallidos |
| **No hay Circuit Breaker** | Agregar Polly o similar para resiliencia |
| **RabbitMQ cola exclusive** | En producción, usar colas durables con nombre fijo para no perder mensajes |
| **No hay métricas** | Agregar contadores de mensajes procesados, errores, latencia (Prometheus, OpenTelemetry) |
