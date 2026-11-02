# Redux vs State Management Moderno — React & Angular (Julio 2026)

## Tabla de contenidos

1. [Conceptos clave: Redux vs programación reactiva y state management moderno](#1-conceptos-clave-redux-vs-programación-reactiva-y-state-management-moderno)
2. [Zero to Mastery: Fundamentos de Angular](#2-zero-to-mastery-fundamentos-de-angular)
3. [Zero to Mastery: Fundamentos de React](#3-zero-to-mastery-fundamentos-de-react)
   - [Fundamentos esenciales (3.1–3.6)](#fundamentos-esenciales)
   - [Hooks de optimización (3.7–3.9)](#hooks-de-optimización)
   - [Referencia y contexto (3.10–3.12)](#referencia-y-contexto)
   - [Renderizado y reconciliación (3.13–3.16)](#renderizado-y-reconciliación)
   - [Formularios (3.17)](#formularios)
   - [Enrutamiento y HTTP (3.18–3.19)](#enrutamiento-y-http)
   - [Temas avanzados (3.20–3.22)](#temas-avanzados)
4. [Angular vs React: Conceptos clave lado a lado](#4-angular-vs-react-conceptos-clave-lado-a-lado)
5. [Resumen ejecutivo](#5-resumen-ejecutivo)
6. [Las 4 implementaciones en este repo](#6-las-4-implementaciones-en-este-repo)
7. [Comparación lado a lado: la misma operación en los 4 paradigmas](#7-comparación-lado-a-lado)
8. [Redux estricto: React (RTK) vs Angular (NgRx)](#8-redux-estRICTO-react-vs-angular)
9. [State management moderno: React (useState) vs Angular (Signals)](#9-state-management-moderno)
10. [Tabla comparativa final](#10-tabla-comparativa-final)
11. [Cuándo usar qué](#11-cuándo-usar-qué)

---

## 1. Conceptos clave: Redux vs programación reactiva y state management moderno

No todos estos términos pertenecen al mismo nivel ni al mismo paradigma. En este documento conviene presentarlos como dos capas distintas:

### A. Núcleo del patrón Redux

Estos conceptos son propios del modelo clásico de Redux:

- State: la fuente única de verdad.
- Action: un evento que describe un cambio.
- Reducer: una función pura que transforma el estado.
- Store: el contenedor central del estado.
- Dispatch: la forma de enviar una acción.
- Selector: una lectura derivada del estado.

### B. Conceptos de programación reactiva y state management moderno

Estos términos aparecen más claramente en enfoques modernos como Angular con Signals/RxJS o React con librerías reactivas:

- Observable / stream: una fuente de valores o eventos.
- Observer / subscription: quien escucha y se suscribe a ese flujo.
- Operator: una función que transforma, filtra o combina datos.
- switchMap: un operador que, por cada valor recibido, dispara un nuevo flujo y cancela el anterior si llega uno nuevo.
- Computed: un valor derivado, memoizado y recalculado automáticamente.
- Effect: una reacción que ejecuta efectos secundarios cuando cambian las dependencias.
- Dependency tracking: el sistema detecta qué partes del estado dependen de otras.

### C. Principales operadores reactivos (RxJS)

En la programación reactiva, los operadores son funciones que transforman, filtran, combinan o controlan flujos de datos. En RxJS, los más usados son estos:

#### 2. Operadores de transformación

- `map`: transforma cada valor emitido en otro valor.
- `pluck`: extrae una propiedad de un objeto.
- `scan`: acumula un valor sobre el tiempo, parecido a un reducer.

#### 3. Operadores de filtrado

- `filter`: permite pasar solo los valores que cumplen una condición.
- `distinctUntilChanged`: evita repetir valores consecutivos.
- `take`: toma solo los primeros N valores.
- `skip`: omite los primeros N valores.

#### 4. Operadores de manejo de tiempo

- `debounceTime`: espera un tiempo antes de emitir, útil para búsquedas.
- `throttleTime`: limita la frecuencia con la que se emiten valores.

#### 4. Operadores de transformación de flujo

- `switchMap`: cancela el flujo anterior y se queda con el más reciente.
- `mergeMap`: ejecuta múltiples flujos en paralelo.
- `concatMap`: ejecuta los flujos uno tras otro, en orden.
- `exhaustMap`: ignora nuevos valores si ya hay uno en curso.

#### 5. Operadores de combinación

- `combineLatest`: combina los últimos valores de varios flujos.
- `withLatestFrom`: combina un flujo principal con el último valor de otro.
- `merge`: une varios flujos en uno solo.
- `concat`: encadena flujos uno tras otro.

#### 6. Operadores de control y efectos

- `tap`: observa el flujo sin modificarlo, útil para logs o depuración.
- `startWith`: añade un valor inicial al flujo.
- `endWith`: añade un valor final al flujo.
- `takeUntil`: detiene el flujo cuando otro flujo emite.

#### 7. Operadores de manejo de errores

- `catchError`: captura errores y permite reaccionar a ellos.
- `retry`: reintenta la operación cuando falla.

#### 8. Ejemplo mental rápido

- `map`: "cambiar el valor"
- `filter`: "quedarse solo con algunos"
- `switchMap`: "cambiar al último flujo y cancelar los anteriores"
- `mergeMap`: "hacer varias cosas al mismo tiempo"
- `concatMap`: "hacer una cosa después de otra"
- `debounceTime`: "esperar un poco antes de reaccionar"

En la práctica, estos operadores permiten construir lógica reactiva muy potente para formularios, búsquedas, HTTP, eventos del usuario y sincronización de estado.

### D. Conceptos modernos de state management

- **Computed**: un valor derivado, memorizado y recalculado automáticamente cuando cambian sus dependencias.
- **Effect**: una reacción que ejecuta efectos secundarios cuando cambian las dependencias.
- **Dependency tracking**: el sistema detecta qué partes del estado dependen de otras.

### E. Comparación rápida

| Concepto | Redux | State management moderno |
|---|---|---|
| State | Central y explícito | Central o local, pero más reactivo |
| Action / reducer | Base del flujo | Menos central en algunos modelos |
| Selector | Lectura derivada | A menudo reemplazado por computed o derivaciones reactivas |
| Computed | No es el concepto central | Muy común y natural |
| Effect | No forma parte del núcleo | Muy importante para side effects |
| Observable / subscription | No es el modelo base | Muy comunes en RxJS y programación reactiva |

En resumen, Redux se enfoca más en “acciones que actualizan estado”, mientras que los modelos modernos se enfocan más en “estado que reacciona automáticamente y en efectos que responden a cambios”.

---

# PARTE I — ZERO TO MASTERY

---

## 2. Zero to Mastery: Fundamentos de Angular

### 2.1 ¿Qué es Angular?

Angular es un **framework** (no librería) opinionado para construir aplicaciones web. Developed by Google. Usa **TypeScript** como lenguaje base.

**Filosofía:** todo tiene una forma "correcta" de hacerse. Angular te da las herramientas, las reglas y la estructura.

### 2.2 Componentes

Un componente es la unidad básica de UI. Tiene una **clase** (lógica), un **template** (HTML), y **estilos** (CSS).

```ts
@Component({
  selector: 'app-saludo',         // ← como un tag HTML: <app-saludo>
  standalone: true,               // ← no necesita declaring en un módulo
  template: `
    <h1>Hola, {{ nombre }}</h1>
    <button (click)="decir()">Decir hola</button>
  `,
})
export class SaludoComponent {
  nombre = 'Mundo';

  decir() {
    alert(`Hola, ${this.nombre}`);
  }
}
```

**Uso en otro template:**
```html
<app-saludo />
```

### 2.3 Templates y Binding

Angular usa templates HTML extendidos con sintaxis especial:

| Sintaxis | Nombre | Qué hace | Ejemplo |
|---|---|---|---|
| `{{ valor }}` | Interpolación | Inserta texto | `<h1>{{ titulo }}</h1>` |
| `[propiedad]="expresión"` | Property binding | Enlaza una propiedad del DOM | `<img [src]="fotoUrl">` |
| `(evento)="handler()"` | Event binding | Escucha eventos del DOM | `<button (click)="save()">` |
| `[(ngModel)]="variable"` | Two-way binding | Sincroniza input ↔ variable | `<input [(ngModel)]="nombre">` |
| `*ngIf="condición"` | Directiva estructural | Muestra/oculta elementos | `<div *ngIf="activo">` |
| `*ngFor="let item of lista"` | Directiva estructural | Itera sobre una lista | `<li *ngFor="let c of clientes">` |
| `\| async` | Pipe | Suscribe a Observable | `{{ clients$ \| async }}` |

**Ejemplo completo:**
```html
<!-- Property binding -->
<img [src]="client.photoUrl" [alt]="client.name">

<!-- Event binding -->
<button (click)="deleteClient(client.id)">Eliminar</button>

<!-- Two-way binding -->
<input [(ngModel)]="searchTerm" placeholder="Buscar...">

<!-- Directivas estructurales -->
<div *ngIf="clients.length > 0; else vacio">
  <p *ngFor="let c of clients">{{ c.name }}</p>
</div>
<ng-template #vacio>
  <p>No hay clientes</p>
</ng-template>
```

### 2.3.1 Nueva sintaxis de control de flujo (`@if`, `@for`, `@switch`)

Angular también ofrece una sintaxis más moderna para controlar la renderización en los templates. En lugar de usar `*ngIf` y `*ngFor`, se puede escribir de forma más explícita con bloques:

```html
@if (isLoggedIn) {
  <h1>Bienvenido</h1>
} @else {
  <p>Inicia sesión</p>
}
```

```html
@for (item of items; track item.id) {
  <li>{{ item.name }}</li>
} @empty {
  <p>No hay elementos</p>
}
```

```html
@switch (status) {
  @case ('loading') { <p>Cargando...</p> }
  @case ('error') { <p>Error</p> }
  @default { <p>Listo</p> }
}
```

Esta sintaxis es más legible y evita la microsintaxis de las directivas estructurales tradicionales como `*ngIf` y `*ngFor`. En la práctica, se considera una mejora para templates más claros y mantenibles.

### 2.4 Directivas

Las directivas son clases que modifican el comportamiento o apariencia de elementos del DOM.

#### Directivas de componente
Son componentes sin template propio. Se usan como atributos:
```ts
@Directive({ selector: '[appHighlight]' })
export class HighlightDirective {
  @Input() appHighlight = '';
  @HostListener('mouseenter') onEnter() {
    this.el.nativeElement.style.backgroundColor = this.appHighlight;
  }
  constructor(private el: ElementRef) {}
}
```
```html
<p appHighlight="yellow">Texto resaltado</p>
```

#### Directivas de atributo
Modifican existencia/apariencia de un elemento:
```html
<!-- NgClass: agrega/quita clases CSS -->
<div [ngClass]="{ 'activo': isActive, 'deshabilitado': !isActive }">

<!-- NgStyle: aplica estilos inline -->
<div [ngStyle]="{ 'color': color, 'font-size': size + 'px' }">
```

#### Directivas estructurales
Agregan o quitan elementos del DOM:
```html
<!-- *ngIf: condicional -->
<div *ngIf="isLoggedIn; else loginTemplate">
  Bienvenido
</div>
<ng-template #loginTemplate>
  <a href="/login">Iniciar sesión</a>
</ng-template>

<!-- *ngFor: iteración -->
<li *ngFor="let client of clients; let i = index; trackBy: trackById">
  {{ i + 1 }}. {{ client.name }}
</li>
```

#### Directivas personalizadas (ejemplo real)
```ts
@Directive({ selector: '[appClickOutside]', standalone: true })
export class ClickOutsideDirective {
  @Output() appClickOutside = new EventEmitter<void>();

  @HostListener('document:click', ['$event'])
  onDocumentClick(event: MouseEvent) {
    if (!this.el.nativeElement.contains(event.target)) {
      this.appClickOutside.emit();
    }
  }

  constructor(private el: ElementRef) {}
}
```
```html
<div (appClickOutside)="cerrarMenu()">Menú</div>
```

### 2.5 Lifecycle Hooks

Cada componente tiene un ciclo de vida. Angular expone hooks para ejecutar lógica en cada fase:

```
Creación                    Actualización                 Destrucción
─────────────────           ─────────────────             ─────────────
constructor()               ngOnChanges()                 ngOnDestroy()
ngOnInit()                  ngOnInit() (después)
ngAfterContentInit()        ngAfterContentChecked()
ngAfterViewInit()           ngAfterViewChecked()
```

| Hook | Cuándo se ejecuta | Uso típico |
|---|---|---|
| `constructor` | Instanciación de la clase | Inyectar servicios |
| `ngOnInit` | Una vez, después del primer `ngOnChanges` | Cargar datos iniciales |
| `ngOnChanges` | Cada vez que un `@Input` cambia | Reaccionar a cambios de props |
| `ngDoCheck` | Cada ciclo de detección de cambios | Custom change detection |
| `ngAfterContentInit` | Después de proyectar contenido (`<ng-content>`) | Manipular contenido hijo |
| `ngAfterViewInit` | Después de inicializar la vista del componente | Manipular el DOM, child views |
| `ngOnDestroy` | Antes de destruir el componente | Unsubscribe, limpiar timers |

**Ejemplo práctico:**
```ts
@Component({ ... })
export class ClientDetailComponent implements OnInit, OnDestroy {
  private sub?: Subscription;

  ngOnInit() {
    // Suscribirme a cambios de ruta
    this.sub = this.route.paramMap.subscribe(params => {
      const id = params.get('id');
      this.loadClient(id);
    });
  }

  ngOnDestroy() {
    this.sub?.unsubscribe();  // ← LIMPIAR suscripción
  }
}
```

### 2.6 Inyección de Dependencias (DI)

Angular tiene un sistema de DI propio. Los servicios se registran y se inyectan automáticamente:

```ts
// Definir un servicio
@Injectable({ providedIn: 'root' })  // ← singleton global
export class ClientService {
  getClients() { return [...]; }
}

// Usar en un componente
export class ClientListComponent {
  private clientService = inject(ClientService);  // ← forma moderna

  // O constructor injection (forma clásica)
  constructor(private clientService: ClientService) {}
}
```

**Jerarquía de inyectores:**
```
Root (global)
└── Environment
    └── Component
        └── Component hijo
```

Un servicio en `providedIn: 'root'` es un singleton: Angular lo crea una sola vez a nivel global y lo comparte en toda la aplicación. Si lo proves en un componente, en cambio, esa instancia queda limitada al alcance de ese componente y sus hijos, por lo que cada contexto puede terminar con una copia distinta.

#### Decoradores útiles en DI

Además de la inyección básica, Angular ofrece decoradores para controlar cómo se resuelve una dependencia:

- `@SkipSelf()`: salta la instancia más cercana del inyector y busca una más arriba en la jerarquía. Se usa cuando quieres evitar obtener un servicio que fue proporcionado en el componente o en un injector hijo cercano y permite que el compilador haga Tree Shaking si el servicio no se utiliza en el codigo.
- `@Optional()`: permite que la dependencia sea opcional. Si no existe, en lugar de fallar se inyecta `null` o `undefined`.
- `@Host()`: limita la búsqueda al host del componente actual, es decir, al injector del componente que está usando la directiva o el componente que contiene la instancia.

```ts
constructor(
  @Optional() private service?: MiServicio,
  @SkipSelf() private parentService: MiServicio,
  @Host() private hostService: MiServicio
) {}
```

Estos decoradores son útiles cuando se trabaja con jerarquías de componentes, servicios compartidos o directivas que necesitan un control más fino sobre la resolución de dependencias.

### 2.7 Change Detection

Angular detecta cambios automáticamente y re-renderiza el DOM. Hay dos estrategias:

| Estrategia | Cómo funciona | Performance |
|---|---|---|
| **Default** | Revisa todo el árbol de componentes en cada evento | Lento para apps grandes |
| **OnPush** | Solo revisa cuando cambian `@Input` por referencia | Rápido, recomendado |

```ts
@Component({
  changeDetection: ChangeDetectionStrategy.OnPush,
  ...
})
export class OptimizedComponent {
  @Input() data!: any;  // ← solo re-renderiza si cambia la referencia
}
```

### 2.8 Pipes

Los pipes transforman datos en el template sin mutar el original. En Angular, los ejemplos más comunes como `currency`, `date`, `uppercase`, `slice`, `json` y `keyvalue` suelen comportarse como **pipes puros**, porque normalmente se reevaluarán cuando cambia la referencia del valor de entrada.

```ts
// Pipes built-in
{{ price | currency:'USD' }}          // → "$1,234.56"   // puro
{{ date | date:'shortDate' }}         // → "7/11/26"     // puro
{{ name | uppercase }}                // → "ANA GARCÍA"   // puro
{{ text | slice:0:50 }}              // → primeros 50 caracteres // puro
{{ items | json }}                    // → JSON formateado // puro
{{ objects | keyvalue }}              // → [{key, value}, ...] // puro

// Pipe personalizado
@Pipe({ name: 'filtro', standalone: true })
export class FiltroPipe implements PipeTransform {
  transform(items: any[], campo: string, valor: any): any[] {
    return items.filter(i => i[campo] === valor);
  }
}
```
```html
<div *ngFor="let c of clients | filtro:'country':'ES'">
  {{ c.name }}
</div>
```

En este caso, el pipe `filtro` también suele considerarse **puro** si transforma una lista a partir de un input que cambia por referencia. La clave es que Angular puede reutilizar la salida si el input no cambió.

#### Pipes puros vs impuros

Angular distingue entre dos tipos de pipes:

- **Pipes puros**: se ejecutan solo cuando cambia la referencia del input. Son más eficientes y se usan por defecto para transformaciones simples como `uppercase`, `date`, `currency` o un pipe propio que transforma una lista.
- **Pipes impuros**: se reevaluan con más frecuencia, por ejemplo ante cada ciclo de detección de cambios. Son útiles cuando el valor puede cambiar sin que cambie la referencia (por ejemplo, una lista que se modifica en el mismo objeto o un stream de datos que cambia constantemente).

```ts
@Pipe({
  name: 'miPipe',
  pure: true   // por defecto
})
export class MiPipe implements PipeTransform {
  transform(value: string): string {
    return value.toUpperCase();
  }
}
```

En la práctica, los pipes puros son preferibles por rendimiento. Los impuros son más flexibles, pero más costosos porque Angular debe reevaluarlos con mayor frecuencia.

### 2.9 Signals (Angular 16+)

Signals son la evolución del state management en Angular:

```ts
// Crear un signal
count = signal(0);

// Leer (como una función)
this.count()  // → 0

// Mutar
this.count.set(5);
this.count.update(prev => prev + 1);

// Computed (derivado, cacheado)
doubleCount = computed(() => this.count() * 2);

// Effect (side effect reactivo)
constructor() {
  effect(() => {
    console.log('Count cambió:', this.count());
  });
}
```

**Diferencia con BehaviorSubject:**
- Signals son **síncronos** (no necesitan `| async` pipe)
- `computed()` se cachea y solo recalcula cuando cambian sus dependencias
- Angular rastrea automáticamente qué componentes leen qué signals

### 2.10 Routing

```ts
// Definir rutas
const routes: Routes = [
  { path: '', redirectTo: 'clients', pathMatch: 'full' },
  { path: 'clients', component: ClientListComponent },
  { path: 'clients/:id', component: ClientDetailComponent },
  { path: 'clients/:id/edit', component: ClientFormComponent },
  { path: '**', component: NotFoundComponent },  // ← wildcard
];

// Navegar
this.router.navigate(['/clients', 123]);
this.router.navigate(['/clients'], { queryParams: { page: 2 } });

// Leer parámetros
this.route.paramMap.subscribe(params => {
  const id = params.get('id');
});
```

### 2.11 HTTP Client

```ts
// Configurar
provideHttpClient(),

// Usar
export class ClientService {
  private http = inject(HttpClient);

  getClients(): Observable<Client[]> {
    return this.http.get<Client[]>('/api/clients');
  }

  createClient(data: Omit<Client, 'id'>): Observable<Client> {
    return this.http.post<Client>('/api/clients', data);
  }
}
```

### 2.11.1 Plataforma y SSR: `PLATFORM_ID` e `isPlatformBrowser`

Cuando una app Angular se ejecuta con Angular Universal, el código puede renderizarse tanto en el servidor como en el navegador. Por eso, a veces conviene saber en qué entorno estamos para evitar usar APIs del browser como `window`, `document` o `localStorage` en el servidor.

- `PLATFORM_ID` es un token de inyección que identifica la plataforma actual: `browser`, `server` o `webworker`.
- `isPlatformBrowser()` es un helper que devuelve `true` cuando la app está corriendo en el navegador.

```ts
import { Inject, Injectable, PLATFORM_ID } from '@angular/core';
import { isPlatformBrowser } from '@angular/common';

@Injectable({ providedIn: 'root' })
export class BrowserOnlyService {
  constructor(@Inject(PLATFORM_ID) private platformId: Object) {}

  isBrowser(): boolean {
    return isPlatformBrowser(this.platformId);
  }
}
```

Esto es clave en SSR porque el servidor no tiene el DOM ni las APIs del navegador, y si intentás usarlas sin protección, podés romper el render inicial.

### 2.12 HttpInterceptor

`HttpInterceptor` permite interceptar las peticiones HTTP antes de que salgan o las respuestas antes de que lleguen al componente. Es muy útil para tareas transversales como:

- agregar el token de autenticación,
- registrar logs,
- manejar errores globalmente,
- reintentar peticiones,
- aplicar caching o transformación de headers.

```ts
export const authInterceptor: HttpInterceptorFn = (req, next) => {
  const token = localStorage.getItem('token');

  if (!token) {
    return next(req);
  }

  const authReq = req.clone({
    setHeaders: { Authorization: `Bearer ${token}` }
  });

  return next(authReq);
};

// Configuración
provideHttpClient(withInterceptors([authInterceptor]))
```

En Angular, un `HttpInterceptor` funciona como una capa transversal: el componente sigue llamando al servicio, pero la infraestructura puede modificar la petición o la respuesta sin que el componente tenga que preocuparse por eso.

### 2.13 ControlValueAccessor

`ControlValueAccessor` es una interfaz de Angular que permite que un componente personalizado se comporte como un control de formulario nativo, como un `input`.

Se usa cuando querés que tu componente pueda integrarse con:

- `ngModel`,
- `Reactive Forms`,
- `FormControl`.

Esto es útil para crear componentes reutilizables como inputs personalizados, selects, datepickers o autocompletados.

```ts
@Component({
  selector: 'app-custom-input',
  template: `<input [value]="value" (input)="onInput($event)" />`
})
export class CustomInputComponent implements ControlValueAccessor {
  value = '';

  writeValue(value: string): void {
    this.value = value ?? '';
  }

  registerOnChange(fn: any): void {
    this.onChange = fn;
  }

  registerOnTouched(fn: any): void {
    this.onTouched = fn;
  }

  setDisabledState(isDisabled: boolean): void {
    // lógica de disabled
  }

  onChange = (_: any) => {};
  onTouched = () => {};

  onInput(event: Event) {
    const value = (event.target as HTMLInputElement).value;
    this.value = value;
    this.onChange(value);
  }
}
```

En resumen, `ControlValueAccessor` permite que un componente propio “hable” con Angular Forms como si fuera un input estándar.

---

## 3. Zero to Mastery: Fundamentos de React

### Fundamentos esenciales

#### 3.1 ¿Qué es React?

React es una **librería** (no framework) para construir UI. Developed by Meta. Usa **JSX** (HTML dentro de JavaScript) y **JavaScript/TypeScript**.

**Filosofía:** todo es un componente. El estado fluye hacia abajo. Las actualizaciones son declarativas (describís QUÉ debería verse, no CÓMO cambiarlo).

#### 3.2 Componentes

```tsx
function Saludo({ nombre }: { nombre: string }) {
  const decir = () => alert(`Hola, ${nombre}`);

  return (
    <div>
      <h1>Hola, {nombre}</h1>
      <button onClick={decir}>Decir hola</button>
    </div>
  );
}

// Uso
<Saludo nombre="Mundo" />
```

#### 3.3 JSX

JSX es sintaxis de azúcar para `React.createElement()`:

```tsx
// JSX
<div className="card">
  <h1>{titulo}</h1>
  <img src={fotoUrl} />
  <button onClick={handler}>Click</button>
</div>

// Equivalente sin JSX (no se hace en la práctica)
React.createElement('div', { className: 'card' },
  React.createElement('h1', null, titulo),
  React.createElement('img', { src: fotoUrl }),
  React.createElement('button', { onClick: handler }, 'Click')
);
```

**Reglas de JSX:**
- Todo debe tener un solo elemento raíz (usa `<>...</>` para fragments)
- `className` en vez de `class`
- `htmlFor` en vez de `for`
- Las expresiones van en `{}` (no `{{}}` como Angular)
- Los eventos son `onClick`, `onChange`, etc. (camelCase)

#### 3.4 Props

Las props son datos que el componente padre pasa al hijo. Son **read-only** (inmutables).

```tsx
interface ClientCardProps {
  client: Client;
  onDelete: (id: number) => void;
}

function ClientCard({ client, onDelete }: ClientCardProps) {
  return (
    <div className="card">
      <h3>{client.name}</h3>
      <button onClick={() => onDelete(client.id)}>Eliminar</button>
    </div>
  );
}

// Padre
<ClientCard client={client} onDelete={handleDelete} />
```

**Props default:**
```tsx
function Badge({ text, variant = 'primary' }: { text: string; variant?: string }) {
  return <span className={`badge-${variant}`}>{text}</span>;
}
```

#### 3.5 State con useState

`useState` es el hook básico para estado local:

```tsx
import { useState } from 'react';

function Contador() {
  const [count, setCount] = useState(0);
  const [nombre, setNombre] = useState('Ana');

  return (
    <div>
      <p>{nombre}: {count}</p>
      <button onClick={() => setCount(count + 1)}>+1</button>
      <button onClick={() => setCount(prev => prev + 1)}>+1 (con callback)</button>
      <button onClick={() => setCount(0)}>Reset</button>
      <input value={nombre} onChange={(e) => setNombre(e.target.value)} />
    </div>
  );
}
```

**Regla de oro:** nunca mutar el state directamente. Siempre usar la función de setter:
```ts
// ❌ MAL
state.count = state.count + 1;

// ✅ BIEN
setCount(count + 1);
setCount(prev => prev + 1);  // ← preferido cuando el nuevo valor depende del anterior
```

**Objetos y arrays:**
```ts
const [user, setUser] = useState({ name: 'Ana', age: 25 });
setUser({ ...user, name: 'Juan' });  // spread + override

const [items, setItems] = useState([1, 2, 3]);
setItems([...items, 4]);             // agregar
setItems(items.filter(i => i !== 2)); // filtrar
setItems(items.map(i => i === 2 ? 99 : i)); // transformar
```

#### 3.6 useEffect

`useEffect` ejecuta side effects después del render. El ciclo de React en tres pasos:

1. React calcula qué debe mostrar en pantalla a partir del estado y las props.
2. React actualiza el DOM con ese resultado (commit).
3. Luego ejecuta los efectos definidos con `useEffect`.

```tsx
function ClientDetail({ id }: { id: number }) {
  const [client, setClient] = useState<Client | null>(null);

  useEffect(() => {
    fetch(`/api/clients/${id}`)
      .then(res => res.json())
      .then(data => setClient(data));

    return () => { console.log('Cleanup'); };
  }, [id]);

  return client ? <h1>{client.name}</h1> : <p>Cargando...</p>;
}
```

**Patrones comunes:**
```ts
useEffect(() => { loadData(); }, []);           // solo al montar
useEffect(() => { console.log('Siempre'); });    // en cada render (cuidado)
useEffect(() => { fetchClients(country); }, [country]); // cuando country cambia

// Cleanup (unsubscribe, abort, etc.)
useEffect(() => {
  const controller = new AbortController();
  fetch(url, { signal: controller.signal });
  return () => controller.abort();
}, [url]);
```

**Cuidado con los bucles infinitos:** si el efecto actualiza el estado y no tiene dependencias, React vuelve a renderizar y el efecto se ejecuta de nuevo indefinidamente.

---

### Hooks de optimización

#### 3.7 useMemo

Memoiza un valor calculado para evitar recomputarlo innecesariamente:

```tsx
function ListaClientes({ clientes }: { clientes: string[] }) {
  const [filtro, setFiltro] = useState('');

  const clientesFiltrados = useMemo(() => {
    return clientes.filter(c => c.toLowerCase().includes(filtro.toLowerCase()));
  }, [clientes, filtro]);

  return (
    <div>
      <input value={filtro} onChange={(e) => setFiltro(e.target.value)} />
      <ul>
        {clientesFiltrados.map(c => <li key={c}>{c}</li>)}
      </ul>
    </div>
  );
}
```

**Cuándo usarlo:** cálculos costosos, listas grandes, estabilidad de referencias. No usar para todo; si el cálculo es simple, puede ser más claro no memoizarlo.

#### 3.8 useCallback

Memoriza una función para evitar crear una nueva referencia en cada render:

```tsx
function Parent() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    setCount(prev => prev + 1);
  }, []);

  return (
    <div>
      <p>{count}</p>
      <Child onClick={handleClick} />
    </div>
  );
}

function Child({ onClick }: { onClick: () => void }) {
  return <button onClick={onClick}>Incrementar</button>;
}
```

**Cuándo usarlo:** cuando la función se pasa como prop a componentes memoizados (`React.memo`) o forma parte de dependencias sensibles a cambios.

#### 3.9 useReducer

`useReducer` es como `useState` pero para lógica compleja (patrón Redux):

```ts
type Action =
  | { type: 'ADD'; payload: Client }
  | { type: 'DELETE'; payload: number }
  | { type: 'UPDATE'; payload: { id: number; changes: Partial<Client> } };

function clientReducer(state: Client[], action: Action): Client[] {
  switch (action.type) {
    case 'ADD':
      return [...state, action.payload];
    case 'DELETE':
      return state.filter(c => c.id !== action.payload);
    case 'UPDATE':
      return state.map(c =>
        c.id === action.payload.id ? { ...c, ...action.payload.changes } : c
      );
  }
}

function ClientManager() {
  const [clients, dispatch] = useReducer(clientReducer, DUMMY_CLIENTS);

  return (
    <>
      <button onClick={() => dispatch({ type: 'ADD', payload: newClient })}>
        Agregar
      </button>
      {clients.map(c => <p key={c.id}>{c.name}</p>)}
    </>
  );
}
```

---

### Referencia y contexto

#### 3.10 useRef

`useRef` referencia elementos del DOM o valores mutables que no causan re-render:

```tsx
function TextInput() {
  const inputRef = useRef<HTMLInputElement>(null);

  const focus = () => {
    inputRef.current?.focus();
  };

  return (
    <>
      <input ref={inputRef} type="text" />
      <button onClick={focus}>Focus</button>
    </>
  );
}
```

**Uso como variable mutable:**
```tsx
function Timer() {
  const intervalRef = useRef<number>();

  useEffect(() => {
    intervalRef.current = setInterval(() => console.log('tick'), 1000);
    return () => clearInterval(intervalRef.current);
  }, []);

  return <button onClick={() => clearInterval(intervalRef.current)}>Stop</button>;
}
```

#### 3.11 useContext

`useContext` permite compartir estado entre componentes sin prop drilling:

```tsx
const ClientContext = createContext<ClientContextType | null>(null);

function App() {
  const [clients, setClients] = useState(DUMMY_CLIENTS);

  return (
    <ClientContext.Provider value={{ clients, setClients }}>
      <ClientList />
    </ClientContext.Provider>
  );
}

function ClientList() {
  const { clients } = useContext(ClientContext);
  return clients.map(c => <p key={c.id}>{c.name}</p>);
}
```

#### 3.12 Hooks personalizados

Extraen lógica reutilizable en funciones que empiezan con `use`:

```ts
function useLocalStorage<T>(key: string, initial: T): [T, (v: T) => void] {
  const [value, setValue] = useState<T>(() => {
    const stored = localStorage.getItem(key);
    return stored ? JSON.parse(stored) : initial;
  });

  const setAndStore = (v: T) => {
    setValue(v);
    localStorage.setItem(key, JSON.stringify(v));
  };

  return [value, setAndStore];
}

// Uso
const [theme, setTheme] = useLocalStorage('theme', 'light');
```

---

### Renderizado y reconciliación

#### 3.13 Reconciliación y keys en listas

React compara el árbol previo con el nuevo para decidir qué actualizar. Las `key` identifican de forma estable cada elemento en una lista:

```tsx
// ✅ BIEN: key estable y único
{clients.map(client => (
  <ClientCard key={client.id} client={client} />
))}

// ❌ MAL: index como key (causa bugs con reordenamiento)
{clients.map((client, index) => (
  <ClientCard key={index} client={client} />
))}
```

Usar el `index` como `key` no es recomendable cuando la lista puede cambiar de orden, insertarse o eliminarse elementos, porque React puede asociar el elemento incorrecto con el mismo índice.

#### 3.14 Condicional Rendering

```tsx
// Operador ternario
{isLoggedIn ? <Dashboard /> : <LoginForm />}

// && cortocircuito
{error && <p className="error">{error}</p>}

// Variable
let content;
if (loading) content = <Spinner />;
else if (error) content = <Error message={error} />;
else content = <ClientList />;

return <div>{content}</div>;
```

#### 3.15 Batching automático en React 18

Desde React 18, las actualizaciones de estado se agrupan automáticamente. Varias llamadas a `setState` dentro del mismo evento se procesan juntas:

```tsx
function Counter() {
  const [count, setCount] = useState(0);
  const [text, setText] = useState('');

  const handleClick = () => {
    setCount(1);
    setText('hola');
    setCount(2);
  };

  return (
    <div>
      <p>{count}</p>
      <p>{text}</p>
      <button onClick={handleClick}>Actualizar</button>
    </div>
  );
}
```

React hace un solo render final con el estado resultante. Si varias actualizaciones dependen del valor anterior, usar la forma funcional:

```tsx
setCount(prev => prev + 1);
setCount(prev => prev + 1);
```

#### 3.16 React Fiber

React Fiber es la arquitectura interna que divide el trabajo de renderizado en unidades pequeñas y manejables. Permite:
- dividir el trabajo por componentes,
- priorizar lo más importante,
- interrumpir una tarea si entra algo urgente,
- reanudarla más tarde sin perder el progreso.

Esto permite que la UI responda mejor en aplicaciones con muchas actualizaciones, animaciones o interacción intensa.

---

### Formularios

#### 3.17 Formularios controlados vs no controlados

**Controlado** — el valor vive en React (`useState`) y se sincroniza con cada cambio:

```tsx
function LoginForm() {
  const [email, setEmail] = useState('');
  return (
    <input value={email} onChange={(e) => setEmail(e.target.value)} />
  );
}
```

**No controlado** — el valor se accede directamente al DOM:

```tsx
function LoginForm() {
  const inputRef = useRef<HTMLInputElement>(null);
  const submit = () => console.log(inputRef.current?.value);

  return (
    <>
      <input ref={inputRef} />
      <button onClick={submit}>Enviar</button>
    </>
  );
}
```

El controlado es el enfoque más común en React porque da más control y facilita validaciones y estado de la UI.

---

### Enrutamiento y HTTP

#### 3.18 React Router

```tsx
import { BrowserRouter, Routes, Route, Link, useParams, useNavigate } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/clients">Clientes</Link>
      </nav>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/clients" element={<ClientList />} />
        <Route path="/clients/:id" element={<ClientDetail />} />
        <Route path="*" element={<NotFound />} />
      </Routes>
    </BrowserRouter>
  );
}

function ClientDetail() {
  const { id } = useParams<{ id: string }>();
  const navigate = useNavigate();
  const goHome = () => navigate('/clients');

  return <button onClick={goHome}>Volver</button>;
}
```

#### 3.19 HTTP con fetch / axios

```tsx
// Fetch API (built-in)
useEffect(() => {
  fetch('/api/clients')
    .then(res => res.json())
    .then(data => setClients(data));
}, []);

// Con async/await
useEffect(() => {
  async function load() {
    const res = await fetch('/api/clients');
    const data = await res.json();
    setClients(data);
  }
  load();
}, []);

// Axios (librería externa, más features)
import axios from 'axios';
const res = await axios.get<Client[]>('/api/clients');
const newClient = await axios.post<Client>('/api/clients', data);
```

---

### Temas avanzados

#### 3.20 Server Components (RSC) vs SSR tradicional

**SSR** genera el HTML en el servidor antes de enviarlo al navegador. **RSC** renderiza componentes enteramente en el servidor y los envía como resultado ya resuelto.

**Diferencia clave:**
- SSR: el componente se renderiza en el servidor pero sigue siendo parte del árbol que React hidrata en el navegador.
- RSC: algunos componentes simplemente no se ejecutan en el cliente, reduciendo el bundle y mejorando el rendimiento.

Los RSC pueden acceder directamente a bases de datos sin exponer esa lógica al cliente.

#### 3.21 Error Boundaries

Componentes que capturan errores en un subárbol de React durante el render, evitando que toda la app se rompa:

```tsx
class ErrorBoundary extends React.Component<{ children: React.ReactNode }, { hasError: boolean }> {
  constructor(props: { children: React.ReactNode }) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError() {
    return { hasError: true };
  }

  render() {
    if (this.state.hasError) {
      return <h2>Algo salió mal.</h2>;
    }
    return this.props.children;
  }
}

// Uso
<ErrorBoundary>
  <UserProfile />
</ErrorBoundary>
```

**Limitación:** no capturan errores en callbacks de eventos, código async o código fuera del árbol de render (`setTimeout`, `Promise`, handlers de `onClick`).

#### 3.22 useImperativeHandle

Permite que un componente hijo exponga una API específica a su padre mediante `ref`:

```tsx
import { forwardRef, useImperativeHandle, useRef } from 'react';

const InputWithFocus = forwardRef(function InputWithFocus(props, ref) {
  const inputRef = useRef<HTMLInputElement>(null);

  useImperativeHandle(ref, () => ({
    focus() { inputRef.current?.focus(); },
    clear() { if (inputRef.current) inputRef.current.value = ''; }
  }));

  return <input ref={inputRef} />;
});

// Uso desde el padre
function Parent() {
  const ref = useRef<{ focus: () => void; clear: () => void }>(null);

  return (
    <>
      <button onClick={() => ref.current?.focus()}>Focus</button>
      <button onClick={() => ref.current?.clear()}>Limpiar</button>
      <InputWithFocus ref={ref} />
    </>
  );
}
```

Se usa con `forwardRef`, y normalmente no es la primera opción cuando se puede resolver la comunicación con props y callbacks.

---

## 4. Angular vs React: Conceptos clave lado a lado

### 4.1 La diferencia filosófica fundamental

| Aspecto | Angular | React |
|---|---|---|
| **Tipo** | Framework (opinionado) | Librería (flexible) |
| **Lenguaje** | TypeScript obligatorio | JavaScript/TypeScript |
| **Template** | HTML separado (.html) | JSX (HTML en JS) |
| **Estado local** | Propiedades de clase | Hooks (useState) |
| **Estado global** | Services + DI | Context + Hooks (o Redux) |
| **Mutación de datos** | Mutable (angular muta directo) | Inmutable (siempre copias) |
| **Render** | Change detection automática | Re-render declarativo |
| **CSS** | Scoped por componente | Scoped por archivo (import) |

### 4.2 Componentes

| Angular | React | Nota |
|---|---|---|
| `@Component({})` | `function Comp() {}` | Angular es clase, React es función |
| `template: '<div>'` | `return <div>` | Angular template string, React JSX |
| `@Input() data` | `function Comp({ data })` | Angular decorator, React destructuring |
| `@Output() event = new EventEmitter()` | `function Comp({ onEvent })` | Angular EventEmitter, React callback prop |
| `selector: 'app-x'` | N/A (se usa como `<X />`) | Angular tiene selector, React no |

### 4.3 Template Binding

| Operación | Angular | React |
|---|---|---|
| Texto | `{{ texto }}` | `{ texto }` |
| Atributo HTML | `[href]="url"` | `href={url}` |
| Evento | `(click)="handler()"` | `onClick={handler}` |
| Two-way | `[(ngModel)]="x"` | Controlled input: `value={x} onChange={e => setX(e.target.value)}` |
| Condicional | `*ngIf="cond"` | `{cond && <X />}` |
| Loop | `*ngFor="let x of arr"` | `arr.map(x => <X />)` |
| Estilo | `[ngStyle]="{...}"` | `style={{...}}` |
| Clase | `[ngClass]="{...}"` | `className={...}` o lib como clsx |

### 4.4 Lifecycle

| Angular | React | Cuándo |
|---|---|---|
| `constructor` | `function Comp()` (cuerpo) | Instanciación |
| `ngOnInit` | `useEffect(() => {...}, [])` | Montaje |
| `ngOnChanges` | `useEffect(() => {...}, [deps])` | Cambio de props |
| `ngAfterViewInit` | `useEffect(() => {...}, [])` + ref | Después del primer render |
| `ngOnDestroy` | `useEffect(() => { return cleanup }, [])` | Desmontaje |

### 4.5 State Management

| Angular | React | Nivel |
|---|---|---|
| Propiedades de clase | `useState` | Estado local simple |
| `signal()` + `computed()` | `useState` + `useMemo` | Estado reactivo |
| `BehaviorSubject` | `useState` + `Context` | Estado compartido |
| `effect()` | `useEffect` | Side effects |
| `NgRx` | `Redux Toolkit` | Redux estricto |
| `NgRx SignalStore` | `Zustand` / `Jotai` | Redux ligero |

### 4.6 Routing

| Angular | React |
|---|---|
| `RouterModule.forRoot(routes)` | `<BrowserRouter>` |
| `<router-outlet />` | `<Routes><Route /></Routes>` |
| `routerLink="/clients"` | `<Link to="/clients">` |
| `ActivatedRoute.paramMap` | `useParams()` |
| `this.router.navigate([...])` | `useNavigate()('/path')` |

### 4.7 HTTP

| Angular | React |
|---|---|
| `HttpClient` (inyectable) | `fetch` / `axios` (built-in o lib) |
| `http.get<T>('/api')` | `fetch('/api').then(r => r.json())` |
| Retorna `Observable<T>` | Retorna `Promise<T>` |
| `provideHttpClient()` en config | No necesita setup |
| `httpClient.post<T>(url, body)` | `axios.post<T>(url, body)` |

### 4.8 El mismo componente en ambos frameworks

**Angular:**
```ts
@Component({
  selector: 'app-counter',
  standalone: true,
  template: `
    <p>Count: {{ count }}</p>
    <button (click)="increment()">+1</button>
  `,
})
export class CounterComponent {
  count = 0;
  increment() { this.count++; }
}
```

**React:**
```tsx
function Counter() {
  const [count, setCount] = useState(0);
  return (
    <>
      <p>Count: {count}</p>
      <button onClick={() => setCount(c => c + 1)}>+1</button>
    </>
  );
}
```

**Diferencia clave:** en Angular, `this.count++` funciona porque Angular detecta el cambio y re-renderiza. En React, `setCount` es la **única** forma de cambiar el state y que se re-renderice. Si hacés `count = count + 1` sin usar `setCount`, el componente NO se actualiza.

### 4.9 Mutabilidad vs Inmutabilidad

```ts
// Angular: mutable (funciona)
this.clients.push(newClient);  // Angular detecta el cambio

// React: inmutable (obligatorio)
setClients([...clients, newClient]);  // Nueva referencia = re-render
// clients.push(newClient);           // ❌ NO funciona, no hay re-render
```

Esto es la diferencia más importante entre ambos frameworks. Angular muta objetos y detecta cambios via change detection. React necesita nuevas referencias para saber que algo cambió.

---

# PARTE II — STATE MANAGEMENT

---

## 5. Resumen ejecutivo

| Paradigma | React | Angular | Boilerplate | Curva aprendizaje |
|---|---|---|---|---|
| **Redux estricto** | Redux Toolkit (slices, thunks) | NgRx (actions, reducers, effects) | Alto | Alta |
| **Modern/simple** | useState + useContext | Signals + computed | Bajo | Baja |

**Redux** impone un flujo estricto: `UI → dispatch(Action) → Reducer → State → UI`. Toda mutación pasa por un reducer puro y trazable.

**State management moderno** omite actions/reducers: el estado se muta directamente con `setState` o `signal.update()`. Más simple, menos trazable.

---

## 6. Las 4 implementaciones en este repo

```
test-MessageQueue-services/
├── client-app-rxjs/              # Angular + BehaviorSubject (rxjs)
├── client-app-signals/           # Angular + Signals (moderno)
├── client-app-react/             # React + useState + Context (moderno)
├── client-app-react-redux/       # React + Redux Toolkit (redux estricto)  ← NUEVO
└── client-app-angular-ngrx/      # Angular + NgRx (redux estricto)         ← NUEVO
```

---

## 7. Comparación lado a lado: la misma operación en los 4 paradigmas

### Operación: Agregar un cliente

#### React + Redux Toolkit (`client-app-react-redux`)

**Paso 1: Definir la action + reducer (slice)**
```ts
// src/store/clientSlice.ts
const clientSlice = createSlice({
  name: "clients",
  initialState,
  reducers: {
    addClient: {
      reducer(state, action: PayloadAction<Client>) {
        state.clients.push(action.payload);  // immer: mutation segura
      },
      prepare(data: Omit<Client, "id" | "createdAt">) {
        return {
          payload: { ...data, id: Date.now(), createdAt: new Date() } as Client,
        };
      },
    },
  },
});

export const { addClient } = clientSlice.actions;
```

**Paso 2: Dispatch desde el componente**
```ts
// src/components/ClientForm.tsx
const dispatch = useAppDispatch();

const handleSubmit = (e: FormEvent) => {
  e.preventDefault();
  dispatch(addClient(form));  // ← dispatch la action
  navigate("/clients");
};
```

**Paso 3: Leer del store**
```ts
// src/components/ClientList.tsx
const clients = useAppSelector((state) => state.clients.clients);
```

#### Angular + NgRx (`client-app-angular-ngrx`)

**Paso 1: Definir la action**
```ts
// src/app/store/client.actions.ts
export const addClient = createAction(
  '[Client] Add',
  props<{ data: Omit<Client, 'id' | 'createdAt'> }>()
);

export const addClientSuccess = createAction(
  '[Client] Add Success',
  props<{ client: Client }>()
);
```

**Paso 2: Definir el reducer**
```ts
// src/app/store/client.reducer.ts
export const clientReducer = createReducer(
  initialState,
  on(ClientActions.addClientSuccess, (state, { client }) => ({
    ...state,
    clients: [...state.clients, client],
  })),
);
```

**Paso 3: Definir el effect (side effect: crear el ID)**
```ts
// src/app/store/client.effects.ts
addClient$ = createEffect(() =>
  this.actions$.pipe(
    ofType(ClientActions.addClient),
    map(({ data }) => {
      const client: Client = { ...data, id: Date.now(), createdAt: new Date() };
      return ClientActions.addClientSuccess({ client });
    })
  )
);
```

**Paso 4: Dispatch desde el componente**
```ts
// src/app/components/client-form/client-form.component.ts
onSubmit() {
  this.store.dispatch(addClient({ data }));  // ← dispatch la action
  this.router.navigate(['/clients']);
}
```

**Paso 5: Leer con selectors**
```ts
// src/app/components/client-list/client-list.component.ts
clients$: Observable<Client[]> = this.store.select(selectAllClients);
```

#### React + useState (`client-app-react`)

```ts
// src/services/ClientContext.tsx
const [clients, setClients] = useState<Client[]>(DUMMY_CLIENTS);

const addClient = useCallback((data: Omit<Client, "id" | "createdAt">) => {
  const newClient = { ...data, id: nextId, createdAt: new Date() };
  setClients(prev => [...prev, newClient]);  // ← mutación directa
}, [nextId]);
```

```ts
// src/components/ClientForm.tsx
const { addClient } = useClients();  // ← consume el context
addClient(form);                      // ← llama directamente
```

#### Angular + Signals (`client-app-signals`)

```ts
// src/app/services/client.service.ts
private clients = signal<Client[]>(DUMMY_CLIENTS);

addClient(data: Omit<Client, 'id' | 'createdAt'>) {
  const newClient = { ...data, id: this.nextId, createdAt: new Date() };
  this.clients.update(current => [...current, newClient]);  // ← mutación directa
}
```

```ts
// template
this.clientService.addClient(data);  // ← llama directamente
```

---

## 8. Redux estricto: React vs Angular

### Similitudes

| Aspecto | React (RTK) | Angular (NgRx) |
|---|---|---|
| **Concepto central** | Store único | Store único |
| **Mutación** | Solo via dispatch(action) | Solo via dispatch(action) |
| **Reducer** | Función pura `(state, action) => newState` | Función pura `createReducer(initialState, on(...))` |
| **Devtools** | Redux DevTools (browser extension) | NgRx Store DevTools |
| **Side effects** | Thunks / middleware | Effects (Observable-based) |
| **Selectores** | `useSelector(state => state.x)` | `store.select(selectX)` |
| **Boilerplate** | Medio (RTK reduce mucho) | Alto (actions, reducer, effects, selectors separados) |

### Diferencias clave

| Aspecto | React (RTK) | Angular (NgRx) |
|---|---|---|
| **Immutabilidad** | RTK usa **Immer** internamente: podés hacer `state.clients.push(x)` y RTK lo convierte a immutable | Debes retornar nuevos objetos manualmente `{ ...state, clients: [...] }` |
| **Side effects** | **Thunks**: `async (dispatch, getState) => { ... }` | **Effects**: clases con `@Injectable()`, basados en RxJS Observables |
| **Actions** | Se crean automáticamente con `createSlice` | Se definen manualmente con `createAction` + `props<>` |
| **Archivo por action** | Todo en un slice (actions + reducer juntos) | Archivos separados: `client.actions.ts`, `client.reducer.ts`, `client.effects.ts` |
| **Template binding** | JSX con hooks: `const x = useSelector(...)` | HTML con async pipe: `*ngIf="x$ | async as x"` |
| **Suscripciones** | Automáticas (re-render) | `async` pipe gestiona suscripciones |
| **Tipado** | Excelente (RTK infiere tipos automáticamente) | Bueno (pero más verboso: `props<{ id: number }>()`) |

### Estructura de archivos

```
React (RTK)                         Angular (NgRx)
─────────────────────────────────   ─────────────────────────────────
src/store/                          src/app/store/
  clientSlice.ts    ← todo junto      client.actions.ts   ← actions
  orderSlice.ts                       client.reducer.ts   ← reducer
  store.ts          ← configureStore   client.selectors.ts ← selectors
  hooks.ts          ← useAppDispatch   client.effects.ts   ← effects
                                      order.actions.ts
                                      order.reducer.ts
                                      order.selectors.ts
                                      order.effects.ts
```

**RTK simplifica mucho el boilerplate:** un `createSlice` genera actions + reducer en un solo archivo. NgRx mantiene todo separado (más explícito, más verboso).

---

## 9. State management moderno

### React: useState + useContext

```ts
// Estado en un Context Provider
const [clients, setClients] = useState<Client[]>(DUMMY_CLIENTS);

// Mutación directa
setClients(prev => [...prev, newClient]);

// Consumo
const { clients, addClient } = useClients();
```

### Angular: Signals

```ts
// Estado como signal
private clients = signal<Client[]>(DUMMY_CLIENTS);

// Mutación directa
this.clients.update(current => [...current, newClient]);

// Consumo
this.clientService.clientsSignal()  // lee el valor
this.clientService.clientById(1)    // computed
```

### Comparación moderna

| Aspecto | React (useState) | Angular (Signals) |
|---|---|---|
| **Estado** | `useState()` | `signal()` |
| **Lectura** | `clients` (variable) | `clients()` (función) |
| **Mutación** | `setState(prev => [...])` | `signal.update(current => [...])` |
| **Computed** | `useMemo(() => ..., [deps])` | `computed(() => ...)` |
| **Side effects** | `useEffect(() => { ... }, [deps])` | `effect(() => { ... })` |
| **State compartido** | `createContext` + Provider | `Injectable` service con signals |
| **Re-render** | Componente + todos los children del Context | Solo el componente que lee el signal |

### La diferencia clave: granularidad

**React Context:** cuando el contexto cambia, **TODOS** los consumers se re-renderizan:
```
Context cambia → ClientList re-render + ClientCard 1 re-render + ClientCard 2 re-render + ...
```

**Angular Signals:** solo se re-renderiza **el componente que lee el signal**:
```
Signal cambia → Solo ClientList re-render (los children no si no leyeron el signal)
```

Esto es por eso que en Angular signals, `computed()` cachea y solo notifica cuando su dependencia cambia. React no tiene esta granularidad built-in (por eso existen librerías como Zustand, Jotai, etc.).

---

## 10. Tabla comparativa final

| Criterio | React Redux Toolkit | Angular NgRx | React useState | Angular Signals |
|---|---|---|---|---|
| **Boilerplate** | Medio | Alto | Bajo | Bajo |
| **Trazabilidad** | Excelente | Excelente | Baja | Baja |
| **Devtools** | Sí (Redux DevTools) | Sí (NgRx DevTools) | No | No (aún en evolución) |
| **Debugging** | Time-travel, replay | Time-travel, replay | console.log | console.log |
| **Testing** | Fácil (reducer puro) | Fácil (reducer puro) | Medio (mock Context) | Medio (mock service) |
| **Side effects** | Thunks | Effects (RxJS) | useEffect | effect() |
| **Escalabilidad** | Excelente | Excelente | Limitada | Buena |
| **Rendimiento** | Bueno | Bueno | Malo con muchos consumers | Bueno (granular) |
| **Curva aprendizaje** | Media | Alta | Baja | Baja |
| **Team conventions** | Muy establecido | Muy establecido | Ad-hoc | Ad-hoc |
| **Community (2026)** | Muy grande | Grande | N/A (built-in) | Crece rápido |

---

## 11. Cuándo usar qué

| Situación | Recomendación |
|---|---|
| App pequeña, CRUD simple, 1-2 devs | **React useState** o **Angular Signals** |
| App mediana, estado compartido, 3-5 devs | **React useState + useReducer** o **Angular Signals + services** |
| App grande, muchos devs, auditoría requerida | **React Redux Toolkit** o **Angular NgRx** |
| Necesitás time-travel debugging | **Redux** (cualquier framework) |
| Necesitás granularidad de re-renders | **Angular Signals** |
| Equipo conoce Redux bien | **RTK** (React) o **NgRx** (Angular) |
| Equipo nuevo, Rapidez > formalidad | **useState** (React) o **Signals** (Angular) |

### La tendencia en 2026

La industria se está moviendo hacia **state management más ligero**:
- **React:** Zustand, Jotai, Valtio (más simples que Redux)
- **Angular:** Signals (reemplaza BehaviorSubject) + NgRx SignalStore (Redux sin boilerplate)
- **Redux/ NgRx** siguen siendo útiles para apps enterprise que necesitan auditoría y devtools

El patrón Redux no desaparece, pero se está simplificando. NgRx SignalStore es literalmente "Redux con signals": tenés un store centralizado pero sin la verbosidad de actions/reducers/effects separados.
