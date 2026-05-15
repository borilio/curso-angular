[TOC]



# Introducción

![Representación de una máquina que recibe letras en mayúsculas y las devuelve en mayúsculas, coloreadas e infladas como globos](img/14-pipes/image-20260514140247686.png){.rounded-4}

Cuando desarrollamos una aplicación, muchas veces necesitamos mostrar los datos de una forma más clara o más atractiva para el usuario. Por ejemplo, una fecha con formato legible, un texto en mayúsculas o un precio con su símbolo de moneda. Para eso Angular incorpora una herramienta muy útil llamada **pipe**.

Un pipe permite transformar un dato directamente en la plantilla HTML sin modificar su valor original. Es decir, el dato sigue siendo el mismo dentro del componente, pero cambia la forma en la que se muestra en pantalla.

Por ejemplo, si tenemos una variable:

```typescript
nombre = 'salva';
```

Podemos mostrarla normalmente:

```html
<p>{{ nombre }}</p>
```

Y veríamos:

```text
salva
```

Pero si usamos un pipe:

```html
<p>{{ nombre | uppercase }}</p>
```

El resultado sería:

```text
SALVA
```

Como puede verse, el valor original no cambia, solo su presentación.

La sintaxis general de un pipe es muy sencilla:

```html
{{ valor | nombreDelPipe }}
```

Angular incluye varios pipes ya preparados, como `uppercase`, `lowercase`, `date`, `currency`, `percent` o `slice`. Además, si necesitamos algo más específico, también podemos crear pipes personalizados.

**Los pipes ayudan a mantener el código más limpio**, **evitan añadir lógica innecesaria** en el componente y **mejoran la presentación** de los datos. En definitiva, son una forma cómoda y elegante de adaptar la información para mostrarla al usuario.

> [!warning]
>
> 🤔 **¿Por qué usar un pipe y no una función?**
>
> Es normal pensar que un pipe se parece mucho a una función, ya que ambos reciben un valor y devuelven otro transformado. Sin embargo, en Angular los pipes están diseñados específicamente para trabajar en la plantilla HTML.
>
> Si llamamos una función desde la vista, Angular puede ejecutarla muchas veces durante la detección de cambios, aunque el dato no haya cambiado. En cambio, los **pipes puros** solo se vuelven a ejecutar cuando cambia el valor de entrada.
>
> Además, un pipe deja más claro que esa lógica pertenece a la presentación de datos y no al funcionamiento interno del componente.
>
> **Una función puede transformar datos, pero un pipe está pensado para hacerlo mejor dentro de la vista.**

# Pipes integradas

Angular incluye varias pipes ya preparadas que podemos usar directamente sin necesidad de instalarlas ni crear código adicional. Estas pipes permiten transformar datos fácilmente desde la propia plantilla HTML.

> [!warning] 
>
> **Importar `CommonModule`**
>
> Las pipes integradas de Angular (`uppercase`, `date`, `currency`, `json`, etc.) pertenecen a `CommonModule`.
> Si trabajamos con componentes standalone, debemos importar este módulo para poder usar las pipes en la plantilla.
>
> ```typescript
> import { CommonModule } from '@angular/common';
> @Component({
>   imports: [CommonModule]
> })
> ```
>
> Si no lo hacemos, Angular mostrará errores indicando que la pipe no existe.
>
> Además de pipes, `CommonModule` también incluye directivas muy utilizadas como `*ngIf`, `*ngFor`, `ngClass` o `ngStyle`.



## Ejemplos

Por ejemplo, podemos convertir un texto a mayúsculas usando `uppercase`:

```html
<p>{{ favorito.nombre | uppercase }}</p>
<!-- SALVADOR -->
```

También existe `lowercase` para minúsculas o `titlecase` para poner en mayúscula la primera letra de cada palabra.

Otra pipe muy útil es `json`, que muestra un objeto en formato JSON, algo especialmente cómodo durante el desarrollo y las pruebas:

```html
<pre>{{ favorito | json }}</pre>
<!--
{
  "id": 1,
  "nombre": "Salvador"
}
-->
```

La pipe `date` permite mostrar fechas con distintos formatos:

```html
<h5>{{ pelicula.fechaEstreno | date:'short' }}</h5>
<!-- 15/05/26, 18:30 -->

<h5>{{ pelicula.fechaEstreno | date:'yyyy-MM-dd' }}</h5>
<!-- 2026-05-15 -->
```

Para trabajar con números también existen pipes como `currency`, `percent` o `number`:

```html
<h5>{{ pelicula.recaudacion | currency:'EUR' }}</h5>
<!-- 1.500.000,00 € -->

<p>{{ descuento | percent }}</p>
<!-- 25% -->
```

Incluso podemos recortar textos o arrays usando `slice`. En este ejemplo mostraremos distintas partes de la palabra `JavaScript`:

```html
<p>{{ 'JavaScript' | slice:0:4 }}</p>
<!-- Java -->

<p>{{ 'JavaScript' | slice:4:10 }}</p>
<!-- Script -->
```

> El primer número indica desde qué posición empezar y el segundo hasta qué posición cortar.

Además, las pipes pueden encadenarse unas con otras:

```html
<h5>{{ pelicula.fechaEstreno | date:'fullDate' | uppercase }}</h5>
<!-- VIERNES, 15 DE MAYO DE 2026 -->
```

La sintaxis es siempre la misma: primero escribimos la expresión y después el símbolo de tubería (`|`) seguido del nombre del pipe.

Angular incluye bastantes más pipes además de estas. Puedes consultar la lista completa y la documentación oficial en:

https://angular.dev/guide/templates/pipes

## Localización

La pipe `date` utiliza por defecto la configuración regional `en-US`, por lo que muchas fechas aparecerán en inglés si no cambiamos el locale de la aplicación.

Vamos a verlo con un ejemplo.

**1. Crear una fecha en el componente**

```typescript
export class AppComponent {

  fechaActual = new Date();

}
```

La clase `Date` de JavaScript crea un objeto con la fecha y hora actuales.

**2. Mostrar la fecha en la vista**

```html
<p>{{ fechaActual | date:'fullDate' }}</p>
<!-- Friday, May 15, 2026 -->
```

Aunque nuestra aplicación esté en español, Angular mostrará la fecha en inglés porque usa el locale `en-US` por defecto.

**3. Registrar el locale español**

Para cambiar el idioma de las fechas, primero debemos importar y registrar los datos regionales de español.

En `main.ts`:

```typescript
// main.ts
import localeEs from '@angular/common/locales/es';
import { registerLocaleData } from '@angular/common';

registerLocaleData(localeEs);
```

**4. Configurar el locale de la aplicación**

Después indicamos a Angular que queremos usar el locale `es-ES`.

En `app.config.ts`:

```typescript
// app.config.ts
import { LOCALE_ID } from '@angular/core';

export const appConfig: ApplicationConfig = {
  providers: [
    {
      provide: LOCALE_ID,
      useValue: 'es-ES'
    }
  ]
};
```

**5. Resultado**

Ahora la misma pipe mostrará la fecha en español:

```html
<p>{{ fechaActual | date:'fullDate' }}</p>
<!-- viernes, 15 de mayo de 2026 -->
```

> [!important]
>
> Este cambio también afecta a otras pipes relacionadas con formatos regionales, como `currency`, `percent` o `number`.

# Pipes personalizados

Angular incluye muchas pipes integradas, pero también podemos crear las nuestras propias cuando necesitemos una transformación específica.

En este ejemplo vamos a crear una pipe llamada `estado` que transformará un valor booleano en un texto más amigable para el usuario.

Por ejemplo:

- `true` → "Activo"
- `false` → "Inactivo"

## 1. Crear la carpeta de pipes

Lo habitual es crear una carpeta para guardar todas las pipes de la aplicación: `src/app/pipes`

## 2. Crear el archivo de la pipes

Dentro de esa carpeta crearemos un archivo llamado: `estado-pipe.ts`

> [!tip]
>
> Se puede hacer directamente con el comando:
>
> ```shell
> ng generate pipe pipes/estado
> ```
>
> Esto generaría la carpeta `pipes`, y en su interior, un archivo llamado `estado-pipe.ts` con el siguiente contenido:
> ```typescript
> import { Pipe, PipeTransform } from '@angular/core';
> 
> @Pipe({
>     name: 'estado',
> })
> export class EstadoPipe implements PipeTransform {
>     transform(value: unknown, ...args: unknown[]): unknown {
>        return null;
>     }
> }
> ```

## 3. Crear la pipe

```typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'estado'
})
export class EstadoPipe implements PipeTransform {

  transform(value: boolean): string {
    if (value) {
      return 'Activo';
    }
    return 'Inactivo';
  }

}
```

La propiedad `name` será el nombre que utilizaremos desde el HTML.

El método siempre debe llamarse `transform()` y recibirá como parámetro el valor que queremos transformar.

En este caso el valor será un booleano (`true` o `false`) y devolveremos un texto distinto según su contenido.

## 4. Importar la pipe

Si usamos componentes standalone, podemos importar directamente la pipe dentro del componente:

```typescript
// ...
import { EstadoPipe } from '../../../pipes/estado-pipe';

@Component({
    imports: [EstadoPipe]
    // ...
})
export class HeroesList {
    // ...
}
```

> [!caution]
>
> En aplicaciones antiguas basadas en módulos (`NgModule`), la pipe se añadiría dentro de `declarations`.

## 5. Usar la pipe en la vista

```html
<p>Estado: {{hero.active | estado }}</p>
<!-- Estado: Activo -->
```

Angular tomará el valor de `usuario.activo`, lo enviará al método `transform()` y mostrará el texto devuelto por la pipe.

En lugar de ver `true` o `false`, el usuario verá un texto mucho más claro y amigable en pantalla.

> [!note]
>
> Claro que podríamos hacer `{{hero.active ? 'Activo' : 'Inactivo' }}` con el mismo resultado sin crear un pipe.
>
> ¿Y si decidimos usar “Vivo” o “Muerto” en su lugar?. Con un pipe sería mucho más eficiente cambiarlo.



