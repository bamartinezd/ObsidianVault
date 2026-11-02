# Arquitectura de Software — Jerarquía de Conceptos

Este documento organiza los distintos niveles de abstracción en arquitectura de software, desde lo más general (cómo se despliega el sistema) hasta lo más granular (cómo se escribe una clase).

---

## 1. Estilo Arquitectónico (deployment / comunicación)

Define **cómo se empaqueta, despliega y comunica** el sistema completo.

| Estilo | Descripción | ¿Qué lo diferencia? |
|---|---|---|
| **Monolito** | Todo en un solo proceso / deploy | Un solo `main()`, un solo binario |
| **Microservicios** | Cada funcionalidad en su propio proceso, comunicación por red | Múltiples procesos, cada uno desplegable independientemente |
| **Serverless (FaaS)** | Funciones individuales que escalan solas, sin gestión de servidores | La plataforma orquesta la infraestructura |
| **Service-Based** | Similar a microservicios pero con servicios más grandes (menos granulares) | Nivel intermedio entre monolito y microservicios |
| **SOA (Service-Oriented Architecture)** | Servicios reutilizables con un bus de integración central (ESB) | Comunicación vía ESB, contratos pesados (SOAP/WSDL) |
| **Event-Driven Architecture (EDA)** | Servicios se comunican mediante eventos asíncronos | Desacoplamiento total, productores no conocen consumidores |

### ¿Qué es comparable en este nivel?

Todas las opciones de arriba. Responden a la pregunta: **¿Un solo proceso o muchos? ¿Cómo se hablan?**

> Este repo usa **Microservicios + EDA** (tres servicios que se envían eventos por Kafka/RabbitMQ/SNS).

---

## 2. Arquitectura Interna (organización del código dentro de cada servicio)

Define **cómo se estructuran las carpetas, dependencias y flujos** *dentro de un mismo proceso*.

| Arquitectura | Idea central | ¿Cuándo usarla? |
|---|---|---|
| **Layered (N-capas)** | Separación horizontal: Presentation → Application → Domain → Infrastructure | Servicios CRUD simples, sin lógica compleja ni providers intercambiables |
| **Hexagonal (Ports & Adapters)** | El dominio en el centro; la infraestructura son "adaptadores" que se enchufan a "puertos" (interfaces) | Cuando tenés infraestructura intercambiable (Kafka/RabbitMQ/SNS, SQL/NoSQL, etc.) |
| **Onion** | Capas concéntricas; el dominio en el núcleo, la infraestructura en el borde exterior | Similar a Hexagonal pero con más énfasis en las capas de Application |
| **Clean** | Variante de Onion + Hexagonal popularizada por Uncle Bob; separa Use Cases como capa explícita | Microservicios con reglas de negocio complejas, múltiples casos de uso |

### ¿Qué es comparable en este nivel?

Todas las opciones de arriba. Responden a la pregunta: **¿Cómo ordeno el código dentro de este microservicio?**

> Este repo actualmente no sigue ninguna de estas (todo está en carpetas planas: `Publishers/`, `Consumers/`, `Models/`). Podría beneficiarse de **Hexagonal** para aislar los providers de mensajería.

### Ejemplos de estructura C# para cada arquitectura

#### Layered (N-capas)

```
Solution/
├── MyApp.Api/                    ← Presentation (Controllers, DTOs)
│   ├── Controllers/
│   ├── DTOs/
│   └── Program.cs
├── MyApp.Application/            ← Application Services
│   ├── Services/
│   └── Interfaces/
├── MyApp.Domain/                 ← Domain (entidades, value objects)
│   ├── Entities/
│   └── ValueObjects/
└── MyApp.Infrastructure/         ← Infrastructure (DB, external APIs)
    ├── Data/
    ├── Repositories/
    └── Messaging/
```

**Regla:** Las dependencias van hacia abajo → `Api → Application → Domain ← Infrastructure`

#### Hexagonal (Ports & Adapters)

```
Solution/
├── MyApp.Domain/                 ← Núcleo puro (entidades, puertos)
│   ├── Entities/
│   ├── Ports/
│   │   ├── In/                   ← Puertos de entrada (use cases)
│   │   └── Out/                  ← Puertos de salida (repositories, messaging)
│   └── ValueObjects/
├── MyApp.Application/            ← Casos de uso implementando puertos de entrada
│   └── UseCases/
├── MyApp.Infrastructure/         ← Adaptadores implementando puertos de salida
│   ├── Adapters/
│   │   ├── KafkaPublisherAdapter.cs
│   │   ├── RabbitMQPublisherAdapter.cs
│   │   └── SqlOrderRepository.cs
│   └── Config/
└── MyApp.Api/                    ← Adaptadores de entrada (REST, gRPC, etc.)
    ├── Controllers/
    └── Adapters/
```

**Clave:** El Domain **nunca** depende de nada. Infrastructure implementa interfaces (puertos) definidas en Domain.

#### Onion (Capas concéntricas)

```
Solution/
├── MyApp.Domain/                 ← Núcleo más interno
│   ├── Entities/
│   ├── ValueObjects/
│   └── Events/
├── MyApp.DomainServices/         ← Servicios de dominio (sin estado)
│   ├── OrderService.cs
│   └── PricingService.cs
├── MyApp.ApplicationServices/    ← Casos de uso / orquestación
│   ├── OrderUseCases.cs
│   └── Interfaces/
├── MyApp.Infrastructure/         ← Capas externas
│   ├── Persistence/
│   ├── Messaging/
│   └── ExternalServices/
└── MyApp.Api/                    ← Capa más externa
    └── Controllers/
```

#### Clean (Uncle Bob)

```
Solution/
├── MyApp.Domain/                 ← Entities (más interno)
│   ├── Entities/
│   └── ValueObjects/
├── MyApp.Application/            ← Use Cases + Interfaces (Boundary)
│   ├── UseCases/
│   │   ├── CreateOrderUseCase.cs
│   │   └── GetOrderUseCase.cs
│   ├── Interfaces/               ← Ports (interfaces que define el dominio)
│   │   ├── IOrderRepository.cs
│   │   └── IMessagePublisher.cs
│   └── DTOs/
├── MyApp.Infrastructure/         ← Frameworks & Drivers
│   ├── Persistence/
│   │   └── OrderRepository.cs
│   └── Messaging/
│       └── KafkaPublisher.cs
└── MyApp.Api/                    ← Controllers, Web API, Entry Points
    └── Controllers/
```

**Regla de dependencia:** Las flechas apuntan HACIA el centro:
```
Api → Application ← Infrastructure
      Application → Domain
```

---

## 3. Patrones Arquitectónicos Internos

Resuelven un problema específico dentro de la estructura del sistema. Operan dentro de la arquitectura interna.

| Patrón | Problema que resuelve | ¿Con qué arquitectura se lleva? |
|---|---|---|
| **CQRS** | Separar modelos de lectura (queries) de escritura (commands) | Cualquiera (Hexagonal + CQRS es muy común) |
| **Event Sourcing** | Almacenar el estado como secuencia de eventos, no como snapshot | Cualquiera (especialmente útil con EDA + CQRS) |
| **Saga** | Coordinar transacciones distribuidas entre microservicios | Microservicios (orquestada o coreografiada) |
| **Repository** | Abstraer el almacenamiento (parece una colección en memoria) | Cualquiera (Layered, Hexagonal, etc.) |
| **Unit of Work** | Agrupar varias operaciones en una sola transacción | Cualquiera |
| **Circuit Breaker** | Prevenir fallos en cascada cuando un servicio externo falla; abre el circuito tras umbral de errores y rechaza llamadas hasta recuperación | Microservicios, EDA |

### ¿Qué es comparable en este nivel?

Solo entre sí. No compiten con la arquitectura interna — la complementan.

---

## 4. Patrones de Diseño (GoF — Gang of Four)

Patrones de nivel más bajo. Resuelven problemas de diseño de *clases y objetos*.

| Patrón | Propósito | Ejemplo en este repo |
|---|---|---|
| **Strategy** | Intercambiar algoritmos en tiempo de ejecución | `IMessagePublisher` → `KafkaPublisher`, `RabbitMQPublisher`, `SnsPublisher` |
| **Factory** | Crear objetos sin exponer la lógica de creación | Crear el publisher según el string "kafka" / "rabbitmq" / "sns" |
| **Observer** | Notificar a múltiples objetos cuando algo cambia | Un publisher notificando a múltiples consumidores (Kafka/RabbitMQ lo implementan) |
| **Singleton** | Una sola instancia global | `Channel` de RabbitMQ, `Producer` de Kafka |

### ¿Qué es comparable en este nivel?

Solo entre sí. Son independientes de los niveles superiores.

---

## 5. Metodologías / Prácticas / Enfoques

No definen estructura de código, sino **cómo se concibe, modela o construye** el software.

| Práctica | ¿Qué es? | ¿Con qué se compara? |
|---|---|---|
| **DDD (Domain-Driven Design)** | Metodología de modelado: ubica el dominio como centro, define bounded contexts, ubiquitous language, agregados, etc. | Se compara con **ningún otro estilo de despliegue**. DDD te ayuda a **descubrir los bounded contexts** que luego serán microservicios o módulos en un monolito. |
| **TDD (Test-Driven Development)** | Práctica de desarrollo: primero el test, luego el código, luego refactor | Se compara con **BDD**, **ATDD**, **test-last**. No tiene relación con arquitectura. |
| **BDD (Behavior-Driven Development)** | Variante de TDD con lenguaje ubicuo (Given/When/Then) | Comparable con **TDD**, no con arquitecturas |
| **SOLID** | Principios de diseño OOP (SRP, OCP, LSP, ISP, DIP) | Son principios, no una arquitectura. Se aplican dentro de cualquier arquitectura. |
| **Event Storming** | Taller colaborativo para modelar procesos de negocio con eventos | Comparable con **Event Mapping**, **Example Mapping**. Es una técnica de modelado. |

### DDD vs. ¿qué?

DDD **no tiene competidor directo** en el mismo nivel de la jerarquía porque no es un estilo de despliegue ni una organización de código. Es una **metodología de modelado**. Podrías decir que se compara con:

- **Anemic Domain Model** (anti-patrón: entidades sin comportamiento)
- **Transaction Script** (lógica en servicios, no en el dominio)
- **Análisis estructurado tradicional** (enfoque procedural vs. orientado a objetos)

Pero en la práctica, DDD convive con todos los niveles anteriores.

---

## Resumen visual

```
NIVEL                     ┌──────────────────────────────────────────┐
                          │           ESTILO ARQUITECTÓNICO          │
                          │  Monolito / Microservicios / Serverless  │
                          │  SOA / Service-Based / EDA               │
                          └────────────────┬─────────────────────────┘
                                           │ contiene
                          ┌─────────────────V─────────────────────────┐
                          │       ARQUITECTURA INTERNA               │
                          │  Layered / Hexagonal / Onion / Clean      │
                          └────────────────┬─────────────────────────┘
                                           │ puede usar
                          ┌─────────────────V─────────────────────────┐
                          │   PATRONES ARQUITECTÓNICOS               │
                          │  CQRS / Event Sourcing / Saga / Repository│
                          └────────────────┬─────────────────────────┘
                                           │ implementado con
                          ┌─────────────────V─────────────────────────┐
                          │       PATRONES DE DISEÑO (GoF)           │
                          │  Strategy / Factory / Observer / Singleton│
                          └────────────────┬─────────────────────────┘
                                           │ guiado por
                          ┌─────────────────V─────────────────────────┐
                          │   METODOLOGÍAS / PRÁCTICAS               │
                          │  DDD / TDD / BDD / SOLID                 │
                          └──────────────────────────────────────────┘
```

Cada nivel es **ortogonal** a los demás. Puedes tener:

- Monolito + Hexagonal + CQRS + Strategy + DDD
- Microservicios + Layered + Repository + TDD
- Serverless + Clean + CQRS + Event Sourcing + DDD

Todas son combinaciones válidas.
