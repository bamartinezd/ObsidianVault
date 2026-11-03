# C# Fundamentals — Zero to Mastery

Guía completa desde cero hasta dominar los fundamentos de C#, actualizada con la evolución histórica y características de cada versión desde **.NET Core 1.0 hasta .NET 9 / C# 13**. Cada sección incluye teoría, código práctico y buenas prácticas.

---

## Tabla de Contenidos

1. [Variables y Tipos de Datos](#1-variables-y-tipos-de-datos)
2. [Operadores](#2-operadores)
3. [Estructuras de Control](#3-estructuras-de-control)
4. [Arrays y Colecciones](#4-arrays-y-colecciones)
5. [Métodos (Funciones)](#5-métodos-funciones)
6. [Programación Orientada a Objetos (OOP)](#6-programación-orientada-a-objetos-oop)
7. [Propiedades y Encapsulamiento](#7-propiedades-y-encapsulamiento)
8. [Herencia y Polimorfismo](#8-herencia-y-polimorfismo)
9. [Interfaces](#9-interfaces)
10. [Enums y Structs](#10-enums-y-structs)
11. [Generics](#11-generics)
12. [LINQ (Language Integrated Query)](#12-linq-language-integrated-query)
13. [Async/Await (Asincronía)](#13-asyncawait-asincronía)
14. [Manejo de Excepciones](#14-manejo-de-excepciones)
15. [Delegates y Events](#15-delegates-y-events)
16. [Expresiones Lambda](#16-expresiones-lambda)
17. [File I/O (Archivos)](#17-file-io-archivos)
18. [String Interpolation y Manejo de Strings](#18-string-interpolation-y-manejo-de-strings)
19. [Nullable Types](#19-nullable-types)
20. [Patrones Comunes y Buenas Prácticas](#20-patrones-comunes-y-buenas-prácticas)
21. [Conceptos Clave para Cuestionarios y Entrevistas](#21-conceptos-clave-para-cuestionarios-y-entrevistas)
22. [Evolución de C# y .NET (Desde .NET Core hasta .NET 9 / C# 13)](#22-evolución-de-c-y-net-desde-net-core-hasta-net-9--c-13)

---

## 1. Variables y Tipos de Datos

### Declaración de Variables

```csharp
// Implícito (el compilador infiere el tipo)
var nombre = "Carlos";          // string
var edad = 25;                  // int
var salario = 50000.50;         // double
var activo = true;              // bool

// Explícito (tú defines el tipo)
string ciudad = "Buenos Aires";
int cantidad = 100;
double precio = 19.99;
bool esValido = false;
```

### Tipos de Datos Primitivos

| Tipo | Tamaño | Rango | Ejemplo |
|------|--------|-------|---------|
| `byte` | 1 byte | 0 a 255 | `byte b = 255;` |
| `sbyte` | 1 byte | -128 a 127 | `sbyte sb = -100;` |
| `short` | 2 bytes | -32,768 a 32,767 | `short s = 1000;` |
| `ushort` | 2 bytes | 0 a 65,535 | `ushort us = 60000;` |
| `int` | 4 bytes | -2.1B a 2.1B | `int i = 100000;` |
| `uint` | 4 bytes | 0 a 4.2B | `uint ui = 3000000000;` |
| `long` | 8 bytes | ±9.2 quintillón | `long l = 9999999999;` |
| `ulong` | 8 bytes | 0 a 18.4 quintillón | `ulong ul = 100;` |
| `float` | 4 bytes | ±3.4 × 10³⁸ | `float f = 3.14f;` |
| `double` | 8 bytes | ±1.7 × 10³⁰⁸ | `double d = 3.14159;` |
| `decimal` | 16 bytes | ±7.9 × 10²⁸ | `decimal m = 199.99m;` |
| `char` | 2 bytes | Unicode | `char c = 'A';` |
| `string` | Variable | Texto | `string s = "Hola";` |
| `bool` | 1 bit lógico | true/false | `bool v = true;` |

### Constantes y Valores Literales

```csharp
const double PI = 3.14159265;           // Constante compile-time
const string APP_NAME = "MiApp";

// Literales numéricos
int hex = 0xFF;                          // 255 en hexadecimal
int bin = 0b1010;                        // 10 en binario
long big = 100_000_000;                  // Underscores para legibilidad
double ciento = 1_000.50;
```

### Conversión de Tipos

```csharp
// Implícita (sin pérdida de datos)
int miEntero = 42;
double miDouble = miEntero;            // int → double (OK)

// Explícita (con casting - puede haber pérdida)
double pi = 3.14159;
int redondeado = (int)pi;              // 3 (trunca, no redondea)

// Métodos de conversión seguros
string numStr = "123";
int num = int.Parse(numStr);           // Funciona
int num2 = int.TryParse(numStr, out int result) ? result : 0; // Seguro

// Convert clase estática
string a = "100";
int b = Convert.ToInt32(a);
double c = Convert.ToDouble(b);
```

---

## 2. Operadores

### Aritméticos

```csharp
int a = 10, b = 3;

Console.WriteLine(a + b);   // 13  (suma)
Console.WriteLine(a - b);   // 7   (resta)
Console.WriteLine(a * b);   // 30  (multiplicación)
Console.WriteLine(a / b);   // 3   (división entera)
Console.WriteLine(a % b);   // 1   (módulo/residuo)

// Preincremento/Postincremento
int x = 5;
int y = ++x;  // x=6, y=6 (primero incrementa)
int z = x++;  // z=6, x=7 (primero asigna, luego incrementa)
```

### Comparación

```csharp
int a = 10, b = 20;

Console.WriteLine(a == b);   // false  (igual)
Console.WriteLine(a != b);   // true   (diferente)
Console.WriteLine(a > b);    // false  (mayor)
Console.WriteLine(a < b);    // true   (menor)
Console.WriteLine(a >= b);   // false  (mayor o igual)
Console.WriteLine(a <= b);   // true   (menor o igual)
```

### Lógicos

```csharp
bool x = true, y = false;

Console.WriteLine(x && y);   // false  (AND)
Console.WriteLine(x || y);   // true   (OR)
Console.WriteLine(!x);       // false  (NOT)

// Cortocircuito
if (x && AlgoLento()) { }   // Si x es false, AlgoLento() NO se ejecuta
if (x || AlgoLento()) { }   // Si x es true, AlgoLento() NO se ejecuta
```

### Asignación

```csharp
int a = 10;

a += 5;    // a = 15  (a = a + 5)
a -= 3;    // a = 12  (a = a - 3)
a *= 2;    // a = 24  (a = a * 2)
a /= 4;    // a = 6   (a = a / 4)
a %= 4;    // a = 2   (a = a % 4)
```

### Null Coalescing y Null Conditional

```csharp
string nombre = null;

// ?? (null coalescing) - usa valor por defecto si es null
string nombreSeguro = nombre ?? "Desconocido";

// ??= (asignación null coalescing)
nombre ??= "Valor por defecto";  // Solo asigna si nombre es null

// ?. (null condicional) - evita NullReferenceException
int? longitud = nombre?.Length;  // null si nombre es null
```

### Pattern Matching

```csharp
object obj = 42;

// is pattern matching
if (obj is int numero)
{
    Console.WriteLine($"Es un int: {numero}");
}

// switch expression con patterns
string clasificar = obj switch
{
    int i when i > 0    => "Entero positivo",
    int i when i < 0    => "Entero negativo",
    int i               => "Cero",
    string s            => $"String: {s}",
    null                => "Es null",
    _                   => "Otro tipo"
};
```

---

## 3. Estructuras de Control

### If / Else

```csharp
int edad = 18;

if (edad >= 18)
{
    Console.WriteLine("Mayor de edad");
}
else if (edad >= 13)
{
    Console.WriteLine("Adolescente");
}
else
{
    Console.WriteLine("Niño");
}

// Ternario
string resultado = edad >= 18 ? "Adulto" : "Menor";

// Null ternary
string nombre = null;
string mensaje = nombre ?? "Sin nombre";
```

### Switch

```csharp
// Switch clásico
string dia = "Lunes";

switch (dia)
{
    case "Lunes":
    case "Martes":
    case "Miércoles":
    case "Jueves":
    case "Viernes":
        Console.WriteLine("Día laboral");
        break;
    case "Sábado":
    case "Domingo":
        Console.WriteLine("Fin de semana");
        break;
    default:
        Console.WriteLine("Día no válido");
        break;
}

// Switch expression (C# 8+)
string tipo = dia switch
{
    "Lunes" or "Martes" or "Miércoles" or "Jueves" or "Viernes"
        => "Laboral",
    "Sábado" or "Domingo"
        => "Fin de semana",
    _
        => "No válido"
};

// Switch con property patterns
object obj = new { Name = "Test", Age = 25 };
string desc = obj switch
{
    { Age: > 65 }           => "Jubilado",
    { Age: >= 18 }          => "Adulto",
    { Age: >= 0 }           => "Menor",
    null                    => "Sin datos",
    _                       => "Desconocido"
};
```

### For, ForEach, While, Do-While

```csharp
// For - cuando sabés cuántas veces iterar
for (int i = 0; i < 5; i++)
{
    Console.WriteLine($"Iteración {i}");
}

// ForEach - para recorrer colecciones
string[] frutas = { "Manzana", "Plátano", "Naranja" };
foreach (string fruta in frutas)
{
    Console.WriteLine(fruta);
}

// While - ejecuta mientras la condición sea true
int contador = 0;
while (contador < 5)
{
    Console.WriteLine(contador);
    contador++;
}

// Do-While - ejecuta al menos una vez
int num = 10;
do
{
    Console.WriteLine(num);
    num--;
} while (num > 0);
```

### Break, Continue, Return

```csharp
// Break - sale del loop
for (int i = 0; i < 100; i++)
{
    if (i == 5) break;         // Sale cuando i == 5
    Console.WriteLine(i);
}

// Continue - salta a la siguiente iteración
for (int i = 0; i < 10; i++)
{
    if (i % 2 == 0) continue;  // Salta pares
    Console.WriteLine(i);      // Imprime: 1, 3, 5, 7, 9
}

// Return - sale del método
void Buscar(int valor)
{
    if (valor < 0) return;     // Sale si es negativo
    Console.WriteLine($"Procesando {valor}");
}
```

---

## 4. Arrays y Colecciones

### Arrays

```csharp
// Declaración e inicialización
int[] numeros = new int[5];                    // Array de 5 elementos (0 por defecto)
int[] nums2 = { 1, 2, 3, 4, 5 };             // Inicialización directa
string[] nombres = new string[] { "Ana", "Bob", "Carlos" };

// Acceso y modificación
Console.WriteLine(numeros[0]);     // Primer elemento
numeros[0] = 10;                   // Asignar valor
Console.WriteLine(numeros.Length);  // Longitud

// Recorrer
foreach (int n in nums2)
{
    Console.WriteLine(n);
}

// Métodos útiles
Array.Sort(numeros);
Array.Reverse(numeros);
int indice = Array.IndexOf(nombres, "Bob");
Array.Resize(ref numeros, 10);
```

### List<T> (Lista genérica)

```csharp
using System.Collections.Generic;

// Crear lista
List<int> edades = new List<int>();
List<string> nombres = new List<string> { "Ana", "Bob" };

// Agregar
edades.Add(25);
edades.AddRange(new[] { 30, 35 });
edades.Insert(0, 18);             // Insertar en posición específica

// Eliminar
edades.Remove(25);                 // Primera ocurrencia
edades.RemoveAt(0);                // Por índice
edades.RemoveAll(x => x > 30);    // Por condición
edades.Clear();                    // Vaciar todo

// Consultar
Console.WriteLine(edades.Count);           // Cantidad
Console.WriteLine(edades.Contains(25));    // Existe?
int idx = edades.IndexOf(25);              // Índice
int found = edades.Find(x => x > 20);     // Primer match
List<int> all = edades.FindAll(x => x > 20); // Todos los matches
edades.Sort();
edades.Reverse();

// Copiar
List<int> copia = new List<int>(edades);
```

### Dictionary<TKey, TValue>

```csharp
// Crear diccionario
Dictionary<string, int> edades = new Dictionary<string, int>();

// Agregar
edades.Add("Ana", 25);
edades["Bob"] = 30;               // Otra forma de agregar

// Acceder
Console.WriteLine(edades["Ana"]);              // 25
if (edades.TryGetValue("Carlos", out int edad))
{
    Console.WriteLine($"Carlos tiene {edad} años");
}

// Verificar
Console.WriteLine(edades.ContainsKey("Ana"));   // true
Console.WriteLine(edades.ContainsValue(25));    // true

// Eliminar
edades.Remove("Ana");

// Recorrer
foreach (KeyValuePair<string, int> par in edades)
{
    Console.WriteLine($"{par.Key}: {par.Value}");
}

// Solo keys o values
foreach (string key in edades.Keys) { }
foreach (int val in edades.Values) { }
```

### Otras Colecciones

```csharp
// Queue<T> - FIFO (First In, First Out)
Queue<string> cola = new Queue<string>();
cola.Enqueue("Primero");
cola.Enqueue("Segundo");
string primero = cola.Dequeue();    // "Primero"
string peek = cola.Peek();          // "Segundo" (sin eliminar)

// Stack<T> - LIFO (Last In, First Out)
Stack<string> pila = new Stack<string>();
pila.Push("Primero");
pila.Push("Segundo");
string top = pila.Pop();           // "Segundo"
string vista = pila.Peek();        // "Primero"

// HashSet<T> - Sin duplicados, búsqueda rápida
HashSet<int> conjunto = new HashSet<int> { 1, 2, 3, 3, 4 };
Console.WriteLine(conjunto.Count);  // 4 (eliminó duplicado)
conjunto.Add(5);
bool tiene = conjunto.Contains(3);  // true

// LinkedList<T> - Lista doblemente enlazada
LinkedList<string> lista = new LinkedList<string>();
lista.AddFirst("Primero");
lista.AddLast("Último");
lista.AddAfter(lista.First, "Segundo");

// SortedList<TKey, TValue> - Ordenado por key
SortedList<int, string> ordenada = new SortedList<int, string>();
ordenada.Add(3, "Tercero");
ordenada.Add(1, "Primero");
// Se itera en orden: 1, 2, 3
```

---

## 5. Métodos (Funciones)

### Declaración y Llamada

```csharp
// Método básico
void Saludar()
{
    Console.WriteLine("¡Hola!");
}
Saludar();  // Llamada

// Con parámetros
void SaludarNombre(string nombre)
{
    Console.WriteLine($"¡Hola, {nombre}!");
}
SaludarNombre("Carlos");

// Con retorno
int Sumar(int a, int b)
{
    return a + b;
}
int resultado = Sumar(5, 3);  // 8

// Expression-bodied (una línea)
int Multiplicar(int a, int b) => a * b;
void Imprimir(string msg) => Console.WriteLine(msg);
```

### Parámetros

```csharp
// Parámetros por valor (copia)
void Modificar(int x)
{
    x = 10;  // NO modifica el original
}

// ref - pasa por referencia
void ModificarRef(ref int x)
{
    x = 10;  // SÍ modifica el original
}
int num = 5;
ModificarRef(ref num);  // num ahora es 10

// out - debe ser asignado dentro del método
bool TryDividir(int a, int b, out int resultado)
{
    if (b == 0)
    {
        resultado = 0;
        return false;
    }
    resultado = a / b;
    return true;
}
if (TryDividir(10, 3, out int res))
{
    Console.WriteLine($"Resultado: {res}");  // 3
}

// Parámetros opcionales (deben ir al final)
void Configurar(string host, int puerto = 8080, bool ssl = false)
{
    Console.WriteLine($"{host}:{puerto} SSL={ssl}");
}
Configurar("localhost");              // localhost:8080 SSL=false
Configurar("localhost", 3000);        // localhost:3000 SSL=false
Configurar("localhost", 443, true);   // localhost:443 SSL=true

// Named arguments
Configurar(ssl: true, host: "api.com", puerto: 443);

// Params - cantidad variable de argumentos
int Sumar(params int[] numeros)
{
    int total = 0;
    foreach (int n in numeros) total += n;
    return total;
}
Console.WriteLine(Sumar(1, 2, 3));       // 6
Console.WriteLine(Sumar(1, 2, 3, 4, 5)); // 15
```

### Sobrecarga (Overloading)

```csharp
// Mismo nombre, diferentes parámetros
int Sumar(int a, int b) => a + b;
double Sumar(double a, double b) => a + b;
int Sumar(int a, int b, int c) => a + b + c;

Console.WriteLine(Sumar(1, 2));          // 3 (int)
Console.WriteLine(Sumar(1.5, 2.5));      // 4.0 (double)
Console.WriteLine(Sumar(1, 2, 3));       // 6 (3 parámetros)
```

### Métodos Estáticos

```csharp
class Calculadora
{
    // Método de instancia - necesita un objeto
    public int Sumar(int a, int b) => a + b;

    // Método estático - se llama en la clase
    public static int PI() => 3;
}

// Uso
Calculadora calc = new Calculadora();
int r1 = calc.Sumar(1, 2);       // Método de instancia
int r2 = Calculadora.PI();        // Método estático
```

---

## 6. Programación Orientada a Objetos (OOP)

### Clases y Objetos

```csharp
class Persona
{
    // Campos (generalmente privados)
    private string _nombre;
    private int _edad;

    // Constructor
    public Persona(string nombre, int edad)
    {
        _nombre = nombre;
        _edad = edad;
    }

    // Método
    public void Presentarse()
    {
        Console.WriteLine($"Soy {_nombre}, tengo {_edad} años");
    }
}

// Crear objeto (instancia)
Persona persona = new Persona("Carlos", 25);
persona.Presentarse();
```

### Constructores

```csharp
class Producto
{
    public string Nombre { get; set; }
    public decimal Precio { get; set; }
    public string Categoria { get; set; }

    // Constructor por defecto
    public Producto()
    {
        Nombre = "Sin nombre";
        Precio = 0;
    }

    // Constructor con parámetros
    public Producto(string nombre, decimal precio)
    {
        Nombre = nombre;
        Precio = precio;
        Categoria = "General";
    }

    // Constructor con todos los parámetros
    public Producto(string nombre, decimal precio, string categoria)
    {
        Nombre = nombre;
        Precio = precio;
        Categoria = categoria;
    }

    // Constructor que llama a otro (chaining)
    public Producto(string nombre) : this(nombre, 0) { }
}

// Uso
Producto p1 = new Producto();
Producto p2 = new Producto("Laptop", 999.99m);
Producto p3 = new Producto("Mouse", 29.99m, "Electrónica");
Producto p4 = new Producto("Teclado");
```

---

## 7. Propiedades y Encapsulamiento

### Propiedades

```csharp
class CuentaBancaria
{
    // Campo privado
    private decimal _saldo;

    // Propiedad completa
    public decimal Saldo
    {
        get { return _saldo; }
        private set { _saldo = value; }
    }

    // Propiedad auto-implementada
    public string Titular { get; set; }

    // Propiedad de solo lectura
    public DateTime FechaCreacion { get; }

    // Propiedad calculada
    public bool TieneSaldo => _saldo > 0;

    // Propiedad con validación
    private string _email;
    public string Email
    {
        get => _email;
        set
        {
            if (string.IsNullOrEmpty(value))
                throw new ArgumentException("Email no puede ser vacío");
            _email = value;
        }
    }

    // Constructor
    public CuentaBancaria(string titular, decimal saldoInicial)
    {
        Titular = titular;
        _saldo = saldoInicial;
        FechaCreacion = DateTime.Now;
    }

    // Método que modifica saldo privado
    public void Depositar(decimal cantidad)
    {
        if (cantidad > 0) _saldo += cantidad;
    }
}
```

### Modificadores de Acceso

```csharp
class Ejemplo
{
    public int Publico;           // Accesible desde cualquier lugar
    protected int Protegido;      // Solo clases hijas
    private int Privado;          // Solo dentro de esta clase
    internal int Interno;         // Solo dentro del mismo assembly
    protected internal int ProtInterno;  // Clases hijas O mismo assembly
    private protected int PrivProt;      // Solo clases hijas DENTRO del mismo assembly
}
```

| Modificador | Misma Clase | Misma Assembly | Clase Hija | Externo |
|-------------|-------------|----------------|------------|---------|
| `public` | ✅ | ✅ | ✅ | ✅ |
| `private` | ✅ | ❌ | ❌ | ❌ |
| `protected` | ✅ | ❌ | ✅ | ❌ |
| `internal` | ✅ | ✅ | ❌ | ❌ |
| `protected internal` | ✅ | ✅ | ✅ | ❌ |
| `private protected` | ✅ | ✅* | ✅* | ❌ |

---

## 8. Herencia y Polimorfismo

### Herencia

```csharp
// Clase base (padre)
class Animal
{
    public string Nombre { get; set; }
    public int Edad { get; set; }

    // Constructor de la clase base
    public Animal(string nombre, int edad)
    {
        Nombre = nombre;
        Edad = edad;
    }

    // Método virtual - puede ser sobrescrito
    public virtual void HacerSonido()
    {
        Console.WriteLine("Sonido genérico");
    }

    // Método normal
    public void Dormir()
    {
        Console.WriteLine($"{Nombre} está durmiendo");
    }
}

// Clase derivada (hijo)
class Perro : Animal
{
    public string Raza { get; set; }

    // Constructor que llama al base
    public Perro(string nombre, int edad, string raza) : base(nombre, edad)
    {
        Raza = raza;
    }

    // Override - sobrescribe el método virtual
    public override void HacerSonido()
    {
        Console.WriteLine("¡Guau!");
    }

    // Método nuevo específico
    public void Buscar()
    {
        Console.WriteLine($"{Nombre} está buscando la pelota");
    }
}

class Gato : Animal
{
    public Gato(string nombre, int edad) : base(nombre, edad) { }

    public override void HacerSonido()
    {
        Console.WriteLine("¡Miau!");
    }
}

// Uso
Animal animal1 = new Perro("Rex", 5, "Pastor Alemán");
Animal animal2 = new Gato("Mishi", 3);

animal1.HacerSonido();  // "¡Guau!" (polimorfismo)
animal2.HacerSonido();  // "¡Miau!"

// No se puede acceder a métodos de Perro desde tipo Animal
// animal1.Buscar();  // ERROR de compilación
```

### Palabras Clave `new`, `override`, `sealed`

```csharp
class Base
{
    public virtual void Metodo() => Console.WriteLine("Base");
    public virtual void OtroMetodo() => Console.WriteLine("Otro Base");
}

class Derivada : Base
{
    // Oculta el método de la base (sin override)
    public new void Metodo() => Console.WriteLine("Derivada");

    // Sobrescribe verdaderamente
    public override void OtroMetodo() => Console.WriteLine("Override");
}

class Sello : Derivada
{
    // Previene que clases hijas sobrescriban
    public sealed override void OtroMetodo() => Console.WriteLine("Sellado");
}

// Clase sellada - no puede ser heredada
sealed class Constante
{
    public int Valor = 42;
}
// class Hijo : Constante { }  // ERROR: no se puede heredar de sealed
```

### Abstract

```csharp
// Clase abstracta - no se puede instanciar directamente
abstract class Forma
{
    public string Color { get; set; }

    // Método abstracto - NO tiene implementación, debe ser sobrescrito
    public abstract double CalcularArea();

    // Método concreto - tiene implementación
    public void Describir()
    {
        Console.WriteLine($"Soy una forma de color {Color}");
    }
}

class Circulo : Forma
{
    public double Radio { get; set; }

    public Circulo(double radio)
    {
        Radio = radio;
    }

    // DEBE implementar método abstracto
    public override double CalcularArea()
    {
        return Math.PI * Radio * Radio;
    }
}

class Rectangulo : Forma
{
    public double Ancho { get; set; }
    public double Alto { get; set; }

    public override double CalcularArea()
    {
        return Ancho * Alto;
    }
}

// Forma forma = new Forma();  // ERROR: no se puede instanciar abstract
Circulo c = new Circulo(5);
c.Color = "Rojo";
Console.WriteLine(c.CalcularArea());  // 78.54
c.Describir();                        // "Soy una forma de color Rojo"
```

---

## 9. Interfaces

### Declaración e Implementación

```csharp
// Definición de interface
interface IAnimal
{
    string Nombre { get; set; }          // Propiedad
    void HacerSonido();                   // Método
    bool EstaVivo { get; }               // Propiedad de solo lectura
}

interface IAlimentable
{
    void Comer(string alimento);
    bool TieneHambre { get; }
}

// Implementación
class Perro : IAnimal, IAlimentable
{
    public string Nombre { get; set; }
    public bool EstaVivo => true;
    public bool TieneHambre { get; private set; }

    public void HacerSonido()
    {
        Console.WriteLine("¡Guau!");
    }

    public void Comer(string alimento)
    {
        Console.WriteLine($"Comiendo {alimento}");
        TieneHambre = false;
    }
}
```

### Interface con Métodos Default (C# 8+)

```csharp
interface ILogger
{
    void Log(string mensaje);

    // Método default (implementación en la interface)
    void LogError(string error)
    {
        Console.WriteLine($"ERROR: {error}");
    }
}

class ConsolaLogger : ILogger
{
    public void Log(string mensaje)
    {
        Console.WriteLine(mensaje);
    }
    // No necesita implementar LogError, usa la default
}
```

### Herencia de Interfaces

```csharp
interface IAnimal
{
    string Nombre { get; }
}

interface IPerro : IAnimal
{
    void Ladrar();
}

// Implementación
class Labrador : IPerro
{
    public string Nombre { get; set; }

    public void Ladrar()
    {
        Console.WriteLine("¡Guau guau!");
    }
}

// Uso
IPerro perro = new Labrador { Nombre = "Rex" };
perro.Ladrar();
Console.WriteLine(perro.Nombre);

// También funciona como IAnimal
IAnimal animal = perro;
Console.WriteLine(animal.Nombre);
```

### Interface vs Clase Abstracta

| Característica | Interface | Clase Abstracta |
|----------------|-----------|-----------------|
| Múltiple herencia | ✅ Sí | ❌ No |
| Campos | ❌ No | ✅ Sí |
| Constructores | ❌ No | ✅ Sí |
| Métodos con implementación | ✅ (C# 8+) | ✅ Siempre |
| Se puede instanciar | ❌ No | ❌ No |
| Modificadores de acceso | `public` implícito | Cualquiera |

---

## 10. Enums y Structs

### Enums

```csharp
// Declaración básica
enum DiaSemana
{
    Lunes,      // 0
    Martes,     // 1
    Miércoles,  // 2
    Jueves,     // 3
    Viernes,    // 4
    Sábado,     // 5
    Domingo     // 6
}

// Con valores específicos
enum EstadoPedido
{
    Pendiente = 1,
    EnProceso = 2,
    Enviado = 3,
    Entregado = 4,
    Cancelado = 5
}

// Uso
DiaSemana hoy = DiaSemana.Martes;
Console.WriteLine(hoy);                    // Martes
Console.WriteLine((int)hoy);               // 1
Console.WriteLine((DiaSemana)3);           // Jueves

// Comparar
if (hoy == DiaSemana.Sábado || hoy == DiaSemana.Domingo)
{
    Console.WriteLine("¡Es fin de semana!");
}

// Obtener todos los valores
foreach (DiaSemana dia in Enum.GetValues(typeof(DiaSemana)))
{
    Console.WriteLine($"{dia} = {(int)dia}");
}

// Parse
EstadoPedido estado = Enum.Parse<EstadoPedido>("Enviado");
Enum.TryParse("Enviado", out EstadoPedido estado2);
```

### Structs

```csharp
// Struct - tipo de valor (se copia por valor)
struct Punto
{
    public double X { get; set; }
    public double Y { get; set; }

    // Constructor
    public Punto(double x, double y)
    {
        X = x;
        Y = y;
    }

    // Método
    public double DistanciaA(Punto otro)
    {
        double dx = X - otro.X;
        double dy = Y - otro.Y;
        return Math.Sqrt(dx * dx + dy * dy);
    }

    // Override
    public override string ToString() => $"({X}, {Y})";
}

// Uso
Punto p1 = new Punto(0, 0);
Punto p2 = new Punto(3, 4);

Console.WriteLine(p1);                          // (0, 0)
Console.WriteLine(p2.DistanciaA(p1));           // 5

// Diferencia importante: Struct es tipo de valor
Punto p3 = p1;       // Crea una COPIA
p3.X = 10;
Console.WriteLine(p1.X);   // 0 (p1 no cambió)
Console.WriteLine(p3.X);   // 10

// Record struct (C# 10+)
readonly record struct Coordenada(double X, double Y);
var c1 = new Coordenada(1, 2);
var c2 = new Coordenada(1, 2);
Console.WriteLine(c1 == c2);  // true (equality por valor)
```

### Struct vs Class

| Característica | Struct | Class |
|----------------|--------|-------|
| Tipo | Valor | Referencia |
| Copia | Por valor (copia completa) | Por referencia (mismo objeto) |
| Herencia | No puede heredar (excepto interfaces) | Sí |
| Constructor por defecto | Siempre tiene uno | Opcional |
| Memoria | Stack (generalmente) | Heap |
| Cuándo usar | Datos pequeños, inmutables | Objetos con comportamiento complejo |

---

## 11. Generics

### Métodos Genéricos

```csharp
// Método genérico - funciona con cualquier tipo
void Imprimir<T>(T valor)
{
    Console.WriteLine($"Tipo: {typeof(T).Name}, Valor: {valor}");
}

Imprimir(42);           // Tipo: Int32, Valor: 42
Imprimir("Hola");       // Tipo: String, Valor: Hola
Imprimir(3.14);         // Tipo: Double, Valor: 3.14

// Método genérico con restricciones
T Maximo<T>(T a, T b) where T : IComparable<T>
{
    return a.CompareTo(b) > 0 ? a : b;
}

Console.WriteLine(Maximo(10, 20));         // 20
Console.WriteLine(Maximo("banana", "apple")); // banana
```

### Clases Genéricas

```csharp
// Contenedor genérico
class Contenedor<T>
{
    private T _item;

    public Contenedor(T item)
    {
        _item = item;
    }

    public T Obtener() => _item;
    public void Establecer(T item) => _item = item;
}

// Uso
Contenedor<int> contInt = new Contenedor<int>(42);
Contenedor<string> contStr = new Contenedor<string>("Hola");

Console.WriteLine(contInt.Obtener());  // 42
Console.WriteLine(contStr.Obtener());  // "Hola"

// Clase genérica con restricciones
class Repositorio<T> where T : class, IEntidad, new()
{
    private List<T> _items = new List<T>();

    public void Agregar(T item)
    {
        _items.Add(item);
    }

    public T BuscarPorId(int id)
    {
        return _items.FirstOrDefault(x => x.Id == id);
    }
}

// Interfaz para restricción
interface IEntidad
{
    int Id { get; }
}
```

### Restricciones `where`

```csharp
class Ejemplo<T> where T : 
    class              // T debe ser tipo de referencia
    , struct            // T debe ser tipo de valor
    , new()             // T debe tener constructor sin parámetros
    , IComparable<T>    // T debe implementar IComparable<T>
{
}
```

### Diccionarios y Colecciones Genéricas

```csharp
// Tuplas
var persona = (Nombre: "Ana", Edad: 25);
Console.WriteLine(persona.Nombre);  // Ana
Console.WriteLine(persona.Edad);    // 25

// ValueTuple
(int, string, bool) dato = (42, "texto", true);
Console.WriteLine(dato.Item1);  // 42

// List con tipos múltiples
List<(string Nombre, int Edad, string Email)> personas = new()
{
    ("Ana", 25, "ana@email.com"),
    ("Bob", 30, "bob@email.com")
};
```

---

## 12. LINQ (Language Integrated Query)

### Sintaxis de Consulta

```csharp
using System.Linq;

List<int> numeros = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

// Consulta LINQ
IEnumerable<int> pares =
    from n in numeros
    where n % 2 == 0
    select n;

// Métodos de extensión (más común)
IEnumerable<int> impares = numeros.Where(n => n % 2 != 0);

foreach (int n in pares) Console.Write($"{n} ");   // 2 4 6 8 10
foreach (int n in impares) Console.Write($"{n} "); // 1 3 5 7 9
```

### Operadores Comunes

```csharp
List<string> nombres = new List<string>
{
    "Ana", "Bob", "Carlos", "Diana", "Eduardo", "Fabiola"
};

// Where - Filtrar
var largoMayor4 = nombres.Where(n => n.Length > 4);

// Select - Proyectar/Transformar
var mayusculas = nombres.Select(n => n.ToUpper());

// OrderBy / OrderByDescending - Ordenar
var ordenados = nombres.OrderBy(n => n);
var inversos = nombres.OrderByDescending(n => n);

// First / FirstOrDefault - Primer elemento
string primero = nombres.First();                    // "Ana" (excepción si vacío)
string primeroOrNull = nombres.FirstOrDefault(n => n.StartsWith("Z"));  // null

// Single / SingleOrDefault - Exactamente uno
string unico = nombres.Single(n => n == "Carlos");

// Any / All - Verificar condición
bool hayLargos = nombres.Any(n => n.Length > 4);     // true
bool todosLargos = nombres.All(n => n.Length > 3);   // false

// Count / LongCount
int cantidad = nombres.Count();
int cantidadLargos = nombres.Count(n => n.Length > 4);

// Sum / Min / Max / Average
List<int> edades = new List<int> { 25, 30, 35, 40 };
int suma = edades.Sum();
int min = edades.Min();
int max = edades.Max();
double promedio = edades.Average();

// Distinct - Sin duplicados
List<int> conDuplicados = new List<int> { 1, 1, 2, 2, 3 };
var unicos = conDuplicados.Distinct();  // {1, 2, 3}

// Take / Skip
var primeros3 = numeros.Take(3);       // 1, 2, 3
var saltar2 = numeros.Skip(2);         // 3, 4, 5, ...

// Aggregate
string frase = nombres.Aggregate((a, b) => $"{a}, {b}");

// GroupBy
var agrupados = numeros.GroupBy(n => n % 2 == 0 ? "Par" : "Impar");

// SelectMany - Aplanar colecciones anidadas
var listas = new List<List<int>>
{
    new List<int> { 1, 2 },
    new List<int> { 3, 4 }
};
var aplanado = listas.SelectMany(l => l);  // {1, 2, 3, 4}
```

---

## 13. Async/Await (Asincronía)

### Conceptos Básicos

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;

class Programa
{
    static readonly HttpClient client = new HttpClient();

    static async Task Main(string[] args)
    {
        Console.WriteLine("Iniciando...");
        string contenido = await ObtenerContenidoAsync("https://api.example.com/data");
        Console.WriteLine($"Longitud: {contenido.Length}");
    }

    static async Task<string> ObtenerContenidoAsync(string url)
    {
        HttpResponseMessage respuesta = await client.GetAsync(url);
        respuesta.EnsureSuccessStatusCode();
        return await respuesta.Content.ReadAsStringAsync();
    }
}
```

### Manejo de Errores y Paralelismo

```csharp
static async Task EjecutarEnParalelo()
{
    Task<int> tarea1 = CalcularAsync(1);
    Task<int> tarea2 = CalcularAsync(2);

    await Task.WhenAll(tarea1, tarea2);
    Console.WriteLine($"Resultados: {tarea1.Result}, {tarea2.Result}");
}

static async Task<int> CalcularAsync(int n)
{
    await Task.Delay(1000);
    return n * n;
}
```

---

## 14. Manejo de Excepciones

### Try-Catch-Finally

```csharp
try
{
    int[] numeros = { 1, 2, 3 };
    Console.WriteLine(numeros[10]);
}
catch (IndexOutOfRangeException ex)
{
    Console.WriteLine($"Error de índice: {ex.Message}");
}
catch (Exception ex)
{
    Console.WriteLine($"Error general: {ex.Message}");
}
finally
{
    Console.WriteLine("Liberando recursos...");
}
```

---

## 15. Delegates y Events

```csharp
class EmisorDeEventos
{
    public event EventHandler<string> MensajeRecibido;

    public void EnviarMensaje(string mensaje)
    {
        MensajeRecibido?.Invoke(this, mensaje);
    }
}

// Uso
var emisor = new EmisorDeEventos();
emisor.MensajeRecibido += (sender, msg) => Console.WriteLine($"Recibido: {msg}");
emisor.EnviarMensaje("Hola Eventos");
```

---

## 16. Expresiones Lambda

```csharp
Func<int, int, int> suma = (a, b) => a + b;
List<int> numeros = new() { 1, 2, 3, 4, 5 };
var pares = numeros.Where(n => n % 2 == 0).ToList();
```

---

## 17. File I/O (Archivos)

```csharp
using System.IO;

await File.WriteAllTextAsync("datos.txt", "Contenido seguro");
string contenido = await File.ReadAllTextAsync("datos.txt");
```

---

## 18. String Interpolation y Manejo de Strings

```csharp
string producto = "Laptop";
decimal precio = 1200.50m;
string info = $"Producto: {producto}, Precio: {precio:C2}";
```

---

## 19. Nullable Types

```csharp
int? edad = null;
int edadSegura = edad ?? 18;  // Fallback si es null
string? nombre = null;
Console.WriteLine(nombre?.ToUpper());
```

---

## 20. Patrones Comunes y Buenas Prácticas

### Repository Pattern, Builder y Records

```csharp
public record Persona(string Nombre, int Edad);

// Builder simplificado
public class QueryBuilder
{
    private string _table = "";
    public QueryBuilder From(string table) { _table = table; return this; }
    public string Build() => $"SELECT * FROM {_table}";
}
```

---

## 21. Conceptos Clave para Cuestionarios y Entrevistas

### Resumen de los 10 Conceptos

| # | Concepto | Pregunta clave | Respuesta clave |
|---|----------|---------------|-----------------|
| 1 | struct vs class | ¿Dónde se almacenan? | struct=Stack, class=Heap |
| 2 | GC Gen 0 | ¿Qué se elimina? | Objetos de vida corta |
| 3 | LINQ IEnumerable | ¿Cuándo ejecuta? | Deferred (al iterar) |
| 4 | async void | ¿Por qué es peligroso? | No captura excepciones |
| 5 | Boxing | ¿Qué costo tiene? | Copia del Stack al Heap |
| 6 | new() constraint | ¿Qué garantiza? | Constructor sin parámetros |
| 7 | yield | ¿Qué crea? | Máquina de estados |
| 8 | Events vs Delegates | ¿Diferencia clave? | Events encapsulan invocación |
| 9 | IDisposable | ¿Qué interfaz? | Interface `IDisposable` |
| 10 | Closures | ¿Problema con hilos? | Race conditions |

---

## 22. Evolución de C# y .NET (Desde .NET Core hasta .NET 9 / C# 13)

Esta sección detalla de forma cronológica todas las grandes características agregadas al lenguaje y al runtime desde la bifurcación moderna de .NET Core hasta las versiones más recientes.

---

### .NET Core 1.0 & 1.1 / C# 6.0 (2016)
**Hito Principal:** Nacimiento de la plataforma multiplataforma (Windows, Linux, macOS) y de código abierto.

*   **String Interpolation (`$"..."`):** Eliminó la necesidad de usar `string.Format("{0}", var)`.
*   **Null-Conditional Operator (`?.`):** Acceso seguro a miembros previniendo `NullReferenceException`.
*   **Expression-bodied Members (`=>`):** Sintaxis concisa para métodos y propiedades de una sola línea (`public int Edad => _edad;`).
*   **Expression-bodied Properties & Auto-property initializers:** Inicialización directa de propiedades auto-implementadas (`public string Nombre { get; set; } = "Ana";`).
*   **Nameof Operator (`nameof(...)`):** Devuelve el nombre de la variable o tipo como cadena para evitar cadenas mágicas en excepciones o bindings.
*   **Using Static:** Permite importar miembros estáticos de una clase directamente (`using static System.Math;`).

---

### .NET Core 2.0 & 2.1 / C# 7.0, 7.1, 7.2 & 7.3 (2017 - 2018)
**Hito Principal:** Consolidación de alto rendimiento, introducción de `Span<T>` y expansión masiva de APIS.

*   **Tuples & Deconstruction (C# 7.0):** Soporte nativo para tuplas con nombres de elementos `(int Id, string Name) datos = (1, "A");`.
*   **Pattern Matching con `is` y `switch` (C# 7.0):** Verificación de tipo y asignación limpia en un solo paso.
*   **Out Variables Inline (C# 7.0):** Declaración de variables `out` directamente en la llamada al método (`int.TryParse(s, out int res)`).
*   **Local Functions (C# 7.0):** Funciones declaradas dentro del cuerpo de otros métodos.
*   **Ref Returns & Ref Locals (C# 7.0):** Capacidad de retornar referencias a memoria directamente.
*   **Async Main (C# 7.1):** El punto de entrada `static async Task Main()` pasa a ser nativo.
*   **Literal Suffix Underscores (C# 7.2):** Separadores de dígitos `1_000_000` y literales binarios `0b1010`.
*   **`Span<T>` y `ReadOnlySpan<T>` (.NET Core 2.1):** Tipos contiguos de memoria de cero copias para manipulación ultra-rápida de buffers y cadenas.

---

### .NET Core 3.0 & 3.1 (LTS) / C# 8.0 (2019)
**Hito Principal:** Introducción del Nullable Reference Types, soporte para WPF/WinForms en .NET Core y APIs reactivas asíncronas.

*   **Nullable Reference Types (NRT):** El compilador advierte sobre posibles referencias nulas usando `string?` vs `string`.
*   **Switch Expressions:** Expresiones de patrón concisas reemplazando bloques `switch` verbosos.
*   **Async Streams (`IAsyncEnumerable<T>`):** Soporte para retornos asíncronos en bucles `await foreach`.
*   **Default Interface Methods:** Las interfaces ahora pueden incluir implementaciones por defecto para sus miembros.
*   **Property & Positional Patterns:** Pattern matching avanzado desestructurando objetos en el `switch`.
*   **Using Declarations:** Sintaxis simplificada sin bloques de llaves `using var stream = File.OpenRead(...);`.
*   **Ranges & Indices:** Nuevos operadores de índice `^1` (último elemento) y rango `1..4` para arreglos y colecciones.

---

### .NET 5 / C# 9.0 (2020)
**Hito Principal:** Unificación de la plataforma (.NET Core y Mono/Xamarin se unen bajo una sola denominación: .NET).

*   **Records (`record`):** Tipos de referencia orientados a datos e inmutables con igualdad basada en valor por defecto.
*   **Init-only Properties (`init`):** Propiedades que solo se pueden asignar durante la construcción del objeto.
*   **Top-Level Statements:** Eliminación del boilerplate de `class Program` y `static void Main()` para archivos únicos de entrada.
*   **Pattern Matching Enhancements:** Operadores lógicos `and`, `or`, `not` y relacionales (`> 10`) en expresiones pattern.
*   **Target-Typed New Expressions:** Inferencia del tipo al instanciar: `Persona p = new("Ana", 20);`.
*   **Source Generators:** El compilador permite analizar y generar código C# dinámicamente durante la compilación.

---

### .NET 6 (LTS) / C# 10.0 (2021)
**Hito Principal:** Rendimiento masivo, desarrollo web ultra-simplificado mediante Minimal APIs y soporte oficial para .NET MAUI.

*   **Global Usings:** `global using System;` aplica los imports a todo el proyecto.
*   **File-Scoped Namespaces:** Declaración limpia de namespaces `namespace MiProyecto;` eliminando un nivel de sangría.
*   **Record Structs:** Extensión de la sintaxis `record` para tipos de valor (`record struct`).
*   **Extended Property Patterns:** Acceso anidado de propiedades dentro de pattern matching `{ Prop.SubProp: 10 }`.
*   **Constant Interpolated Strings:** Cadenas interpoladas declaradas como `const` si los insumos también lo son.
*   **DateOnly & TimeOnly (.NET 6):** Nuevos tipos de datos del sistema para representar fechas sin hora y horas sin fecha.
*   **LINQ Enhancements:** Métodos como `By` (`MinBy`, `MaxBy`), `Chunk`, `FirstOrDefault` con valor predeterminado explícito.

---

### .NET 7 / C# 11.0 (2022)
**Hito Principal:** Avances en abstracción matemática genérica, rendimiento del runtime y desarrollo nativo Ahead-Of-Time (Native AOT).

*   **Raw String Literals:** Cadenas multilínea delimitadas por tres o más comillas (`"""..."""`) sin necesidad de escapar carácteres especiales o comillas interiores.
*   **Generic Math Support (`INumber<T>`):** Interfaces estáticas virtuales en interfaces que permiten a métodos genéricos operar matemáticamente.
*   **List Patterns:** Pattern matching para listas y arreglos (`array is [1, 2, ..]`).
*   **Required Members (`required`):** Campos y propiedades que deben ser asignados obligatoriamente en el inicializador del objeto.
*   **UTF-8 String Literals:** Creación eficiente de arreglos de bytes de texto UTF-8 añadiendo el sufijo `u8` (`"hola"u8`).
*   **Auto-default Structs:** Los constructores de `struct` asignan automáticamente valores por defecto a los campos no inicializados.

---

### .NET 8 (LTS) / C# 12.0 (2023)
**Hito Principal:** Soporte completo de IA (Tensor primitives), optimizaciones Native AOT avanzadas y unificación total con Blazor Web App.

*   **Primary Constructors (para Clases y Structs):** Declaración de parámetros de constructor directamente en el nombre de la clase/struct.
*   **Collection Expressions (`[...]`):** Sintaxis unificada para inicializar arreglos, listas y spans (`int[] a = [1, 2, 3];`).
*   **Spread Operator (`..`):** Permite aplanar e integrar colecciones dentro de una expresión de colección `[..coleccion1, ..coleccion2]`.
*   **Alias Any Type:** Soporte de directivas `using` para crear alias de cualquier tipo, incluidas tuplas y arreglos (`using Punto = (int X, int Y);`).
*   **Default Lambda Parameters:** Parámetros opcionales con valores predeterminados en expresiones lambda.
*   **Inline Arrays:** Mecanismo interno de alto rendimiento para crear arreglos de tamaño fijo dentro de un struct.
*   **TimeProvider (.NET 8):** Abstracción oficial del sistema para inyectar tiempo en pruebas unitarias de forma limpia.

---

### .NET 9 / C# 13.0 (2024 - Presente)
**Hito Principal:** Optimizaciones extremas impulsadas por IA, integración nativa de Cloud-Native con .NET Aspire y mejoras en concurrencia y memoria.

*   **Nuevo tipo `System.Threading.Lock`:** Mecanismo de bloqueo nativo moderno más limpio y eficiente que el objeto tradicional `object`.
*   **Evolución del parámetro `params`:** Ahora `params` admite cualquier tipo de colección (listas, `ReadOnlySpan<T>`, etc.), no solo arreglos `params ReadOnlySpan<int> valores`.
*   **Mejoras en `params` y params de extensión:** Flexibilidad total al pasar secuencias a métodos optimizados de bajo nivel.
*   **Enhanced Params & Indexing:** Nuevas mejoras en la desestructuración de índices y `field` contextual en propiedades (preview/rollout).
*   **HybridCache (.NET 9):** Nueva abstracción oficial de caché de alto rendimiento que combina memoria local en dos niveles (in-memory + distribuida).
*   **LINQ `CountBy` y `AggregateBy` (.NET 9):** Operaciones de agregación agrupada eficientes sin necesidad de hacer un `GroupBy` completo previamente.
*   **Optimización para Inteligencia Artificial:** APIS de `Tensor<T>`, soporte avanzado para bibliotecas de vectores numéricos y conectores nativos con LLMs.

---

### Resumen de Versiones C# / .NET

| Versión .NET | Versión C# | Año | Característica Clave |
|--------------|------------|-----|----------------------|
| **.NET Core 1.0** | C# 6.0 | 2016 | Interpolación de texto, `?.`, Expression Bodies |
| **.NET Core 2.0 / 2.1** | C# 7.0 - 7.3 | 2017-2018 | Tuplas nativas, Pattern Matching inicial, `Span<T>` |
| **.NET Core 3.1 (LTS)** | C# 8.0 | 2019 | Nullable Reference Types, Switch Expressions, Async Streams |
| **.NET 5** | C# 9.0 | 2020 | Unificación .NET, `record`, Top-Level Statements |
| **.NET 6 (LTS)** | C# 10.0 | 2021 | Global Usings, File-scoped Namespaces, Minimal APIs |
| **.NET 7** | C# 11.0 | 2022 | Raw String Literals, Generic Math, List Patterns |
| **.NET 8 (LTS)** | C# 12.0 | 2023 | Primary Constructors en clases, Collection Expressions `[...]` |
| **.NET 9** | C# 13.0 | 2024 | `System.Threading.Lock`, `params` con `Span<T>`, `CountBy`/`AggregateBy` |
| **.NET 10** | C# 14 | 2025 | `field` keyword, extension members, null conditional assign, improves in lambda and constructors |
| **.NET 11** | C# 15 | 2026 | Collection expression arguments, Union types, Closed hierarchies, Extension indexers, Labeled break and continue, Non-virtual static interface members on older runtimes, Memory safety (preview feature) |

---

## Recursos Recomendados

- **Microsoft Learn**: https://learn.microsoft.com/dotnet/csharp/
- **C# Documentation**: https://docs.microsoft.com/dotnet/csharp/
- **.NET Blog**: https://devblogs.microsoft.com/dotnet/
- **Awesome .NET**: https://github.com/quozd/awesome-dotnet

---

*Documento generado para dominar C# desde cero hasta nivel avanzado. Cada sección construye sobre la anterior. Practica con proyectos reales para consolidar el aprendizaje.*