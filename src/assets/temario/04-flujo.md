[TOC]

# Introducción

![El profesor está decidiendo si entrar por la puerta que pone TRUE o por la que pone FALSE](img/04-flujo/image-20260420133059917.png){.rounded}

En las plantillas de Angular, el control del flujo permite decidir qué se muestra en pantalla y cuántas veces se repite un contenido en función del estado de la aplicación. Hasta hace poco, esto se resolvía principalmente con directivas estructurales como `*ngIf` y `*ngFor`, pero en las versiones más recientes Angular introduce una sintaxis más clara y directa basada en bloques de control como `@if` y `@for`.

Este cambio no solo mejora la legibilidad del código, sino que también lo acerca más a cómo se expresan las condiciones y los bucles en el propio lenguaje de programación, facilitando su comprensión, especialmente para quienes están empezando.



# Condicionales 

## Condicionales simples `@if`

El bloque `@if` permite mostrar o no una parte del HTML en función de una condición. Es el equivalente moderno de `*ngIf`, pero con una sintaxis más clara y estructurada.

Su funcionamiento es sencillo: si la condición se cumple, el contenido dentro del bloque se renderiza; si no se cumple, se omite completamente del DOM.

**Ejemplo:**

```html
@if (usuarioLogueado) {
  <p>Bienvenido de nuevo</p>
}
```

En este caso, el mensaje solo aparecerá si la variable `usuarioLogueado` es verdadera.

> [!caution]
>
> Cuando la condición del `@if` no se cumple, el elemento **no se oculta**, sino que **no se crea en el DOM**. Esto significa que Angular no lo renderiza en absoluto, por lo que no existe en el árbol del DOM ni ocupa memoria en la interfaz.

> [!tip]
>
> Por supuesto, se puede usar cualquier expresión booleana en la condición del `@if`.
>
> ```html
> @if (usuarioLogueado && rol === 'admin') {
>   <p>Acceso completo</p>
> }
> ```



## Condicionales compuestas `@else`

También podemos añadir una alternativa con `@else`, lo que nos permite definir qué mostrar cuando la condición no se cumple:

```html
@if (usuarioLogueado) {
  <p>Bienvenido de nuevo</p>
} @else {
  <p>Por favor, inicia sesión</p>
}
```

## Condicionales múltiples `@else if`

Otra variante útil es encadenar condiciones usando `@else if`, lo que permite construir lógica más completa directamente en la plantilla:

```html
@if (rol === 'admin') {
  <p>Panel de administración</p>
} @else if (rol === 'editor') {
  <p>Panel de edición</p>
} @else {
  <p>Acceso limitado</p>
}
```

Este enfoque hace que la lógica de presentación sea más legible y cercana a la forma en la que se escribe en TypeScript, evitando el uso de múltiples directivas anidadas.

# Bucles `@for`

El bloque `@for` permite iterar sobre colecciones de datos y generar contenido repetido en la plantilla de forma dinámica. Es el equivalente moderno de `*ngFor`, pero con una sintaxis más explícita y estructurada.

Su objetivo es sencillo: recorrer un array y renderizar un bloque de HTML por cada elemento.

**Ejemplo:**

```html
@for (item of lista; track $index) {
  <p>{{ item }}</p>
}
```

En este caso, Angular genera un `<p>` por cada elemento del array `lista`.

> [!warning]
>
> 🧙‍♂️Para entenderlo mejor, obvia la parte de `track`. Después explicaremos para que sirve.

**Ejemplo completo:**

Tenemos el siguiente array definido en nuestro modelo de datos y queremos mostrarlo de forma dinámica en nuestra plantilla.

```typescript
export class App {
  //Atributos
  diasSemana: string[] = ['Lunes', 'Martes', 'Miércoles', 'Jueves', 'Viernes'];
  //...  
}
```

Después, en la plantilla HTML, usamos `@for` para recorrer el array y mostrar cada elemento dentro de una lista no numerada (`ul`)

```html
<ul>
	@for (dia of diasSemana; track $index) {
		<li>{{ dia }}</li>
  	}
</ul>
```

Angular procesará el `@for` y generará un HTML equivalente al siguiente:

```html
<ul>
  <li>Lunes</li>
  <li>Martes</li>
  <li>Miércoles</li>
  <li>Jueves</li>
  <li>Viernes</li>
</ul>
```

> [!important]
>
> **Cuando el array cambia** (por ejemplo, al añadir, eliminar o modificar elementos), **la vista se actualiza automáticamente**. Angular detecta estos cambios y vuelve a renderizar el `@for` sin necesidad de recargar la página ni realizar ninguna acción adicional.



## Track

El `track` es una parte del `@for` que le permite a Angular identificar de forma única cada elemento de una lista mientras se renderiza en pantalla.

Su función principal es ayudar a Angular a saber **qué elementos son cuáles** cuando la lista cambia (por ejemplo, al añadir, eliminar o reordenar elementos). Esto evita que tenga que reconstruir toda la lista desde cero y mejora el rendimiento.

Cuando trabajamos con listas simples (por ejemplo, arrays de strings o valores primitivos), normalmente no tenemos un identificador único para cada elemento.

En esos casos, se suele utilizar el índice del bucle:

```html
@for (dia of diasSemana; track $index) {
	<li>{{ dia }}</li> 
}
```

Aquí `$index` representa la posición del elemento dentro del array (0, 1, 2, 3...).

> [!tip]
>
> Podemos usar `$index` para obtener el índice del bucle, por si nos hiciese falta por alguna razón:
>
> ```html
> <ul>
>   @for (dia of diasSemana; track $index) {
>     <li>{{ $index }} - {{ dia }}</li>
>   }
> </ul>
> ```
>
> Esto mostraría:
>
> > - 0 - Lunes
> > - 1 - Martes
> > - 2 - Miércoles
> > - 3 - Jueves
> > - 4 - Viernes



**¿Y si trabajamos con objetos?**

Cuando la lista está formada por objetos, lo ideal es que cada uno tenga un identificador único (como un `id`). En ese caso, se debe usar ese identificador en el `track` en lugar de `$index`:

```html
@for (usuario of usuarios; track usuario.id) {
  <li>{{ usuario.nombre }}</li>
}
```

De esta forma, Angular puede identificar cada elemento de forma precisa aunque el contenido cambie.

> [!tip]
>
> - Listas simples (strings, números) → `track $index`
> - Objetos con identificador → `track item.id`
> - Evita usar el valor del elemento como track, si es que puede repetirse



## Empty

Además, `@for` permite detectar casos especiales dentro del bucle, como cuando la lista está vacía. Esto nos evita tener que comprobar manualmente si la lista está vacía o no con un `@if`.

```html
@for (usuario of usuarios; usuario.id) {
  <p>{{ usuario.nombre }}</p>
} @empty {
  <p>No hay elementos disponibles</p>
}
```

Si la lista contiene elementos los muestra, si estuviera vacía, mostraría el párrafo del `@empty`.

# Condicionales múltiples `@switch`

El bloque `@switch` permite mostrar diferentes contenidos en función del valor de una variable. Es el equivalente moderno de la estructura `switch` de TypeScript, pero aplicado directamente en la plantilla de Angular.

Se utiliza cuando una misma variable puede tomar varios valores posibles y queremos mostrar una vista distinta para cada uno de ellos, evitando encadenar múltiples `@else if`.

```typescript
rol: string = 'admin'; //Una variable que representa el rol del usuario
```

En lugar de hacer múltiples `@else if` encadenados, podemos usar `@switch` para evaluar todos los posibles valores de `rol`.

```html
@switch (rol) {
  @case ('admin') {
    <div>Panel de administración</div>
  }
  @case ('editor') {
    <div>Panel de edición</div>
  }
  @default {
    <div>Panel de usuario estándar</div>
  }
}
```

> [!important]
>
> `@switch` es especialmente útil cuando una variable puede tener varios estados definidos y queremos mostrar una vista distinta (u otro componente) para cada uno de ellos de forma ordenada y mantenible.



# Antes en Angular

Antes de la llegada de la sintaxis de control de flujo moderna (`@if`, `@for`, `@switch`), Angular utilizaba directivas estructurales como `*ngIf` y `*ngFor` para manejar condiciones y bucles en las plantillas.

No es necesario profundizar en su uso, ya que hoy en día han sido sustituidas, pero es importante reconocerlas porque aún podemos encontrarlas en proyectos antiguos.

Era prácticamente igual, pero en lugar de usar la sintaxis más tradicional parecida a un lenguaje de programación con las `{ }`, era como un atributo HTML del elemento que queríamos evaluar.

## `*ngIf`

```html
<p *ngIf="usuarioLogueado">
  Bienvenido de nuevo
</p>
```

## `*ngFor`

```html
<ul>
  <li *ngFor="let dia of diasSemana; let i = index">
    {{ i }} - {{ dia }}
  </li>
</ul>
```

> [!note]
>
> 👴Estas directivas eran la forma anterior de controlar el flujo en plantillas, pero en Angular moderno (17+) se han reemplazado por una sintaxis más clara y estructurada basada en bloques como `@if` y `@for`.

> [!warning]
>
> Angular no “eliminó” `*ngIf` y `*ngFor` en Angular 17, sino que `@if` y `@for` son el enfoque recomendado en proyectos nuevos. `*ngIf` y `*ngFor` siguen funcionando para mantener compatibilidad.



{{ “Pensar” en un ejercicio para practicar con todo esto. Se puede hacer un proyecto un poco más grande, con componentes para agregarle todo lo que vayamos necesitando}}
