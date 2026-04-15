[TOC]

# Introducción

![Representación gráfica de una vista en angular dejando claro la separación entre la lógica (TS) y la vista (HTML)](img/03-vistas/vistas.png){.rounded}

En Angular, una **vista es la parte de la aplicación que el usuario ve en pantalla**. Es la representación visual de un componente y normalmente se define en un archivo HTML y CSS.

Cada componente tiene asociada una vista, que se encarga de mostrar la información y permitir la interacción con el usuario.

Angular sigue una separación clara entre:

- 🧠 **Lógica** → definida en el archivo TypeScript del componente  
- 🖼️ **Vista** → definida en el archivo HTML y CSS  

Esta separación facilita el mantenimiento del código y permite trabajar de forma más organizada.

Cuando Angular carga un componente, automáticamente enlaza su lógica con su vista. Esto significa que cualquier dato definido en el componente puede mostrarse en el HTML, y cualquier interacción del usuario puede ser gestionada desde el TypeScript.

Una de las características más importantes de Angular es que la vista se actualiza automáticamente cuando cambian los datos del componente, sin necesidad de manipular el DOM manualmente (como habría que hacer con JS *a pelo*). Esto permite crear interfaces dinámicas de forma sencilla y eficiente.

# Tipos de binding en Angular

Angular ofrece diferentes formas de establecer la comunicación entre la **vista (HTML)** y el **modelo (TypeScript)**.

A estos mecanismos se les conoce como **tipos de binding**, y permiten definir cómo fluye la información entre ambos lados del componente.

En función del sentido de la comunicación, podemos distinguir varios tipos:

- 🟦 Binding de propiedad  
- 🟨 Binding de interpolación (expresiones)  
- 🟩 Binding de eventos  
- 🟪 Doble binding (banana in a box)  

Cada uno de ellos tiene una finalidad concreta y se utiliza en distintos escenarios dentro de una aplicación Angular.

![Diagrama con el resumen del flujo de información entre la vista y el modelo y la sintaxis](img/03-vistas/esquema-binding-1776247992304-3.png)

A continuación veremos cada tipo con ejemplos prácticos de uso.



## 🟦 Binding de propiedad [ ]

El binding de propiedad permite enlazar un valor del modelo con una propiedad de un elemento HTML o componente.

Se utiliza la sintaxis:

```html
[propiedad]="valor"
```

**Ejemplos:**

```html
<img [src]="imagenUrl" />
```

En este caso, la propiedad `src` de la imagen toma su valor desde una variable del componente (`imagenUrl`).

```html
<a [href]="homeUrl">Pulsa aquí para volver</a>
```

Lo mismo pasa con la propiedad `href` del enlace. Su valor estará definido por la variable `homeUrl` en el componente.

---

## 🟨 Binding de interpolación {{ }}

La interpolación permite mostrar datos del modelo directamente en la vista como texto. 

La diferencia respecto a las propiedades es que **se usarán mayormente como contenido de los elementos HTML**, no como propiedad o su valor. Por ejemplo, el contenido de un párrafo o el texto de un encabezado.

Se utiliza la sintaxis:

```html
{{ expresión }}
```

**Ejemplos:**

```html
<h1>{{ titulo }}</h1>
```

Aquí, el valor de `titulo` se muestra dentro del encabezado.

```html
<p>Precio total: {{ precio + iva }} €</p>
```

Se sustituye las `{{}}` por el resultado de sumar las dos variables. El resto se queda tal cual.

> [!note]
>
> La interpolación no se limita a mostrar variables simples. Lo que realmente hace Angular es **evaluar una expresión**, igual que se haría en cualquier lenguaje de programación.  
> Esto significa que dentro de `{{ }}` se pueden usar operaciones, llamadas a métodos o combinaciones de datos, y Angular mostrará el resultado final de esa evaluación.

> [!caution]
>
> Estas dos líneas pueden producir el mismo resultado. ¿Pero es lo mismo?
>
> ```html
> <a href="{{homeUrl}}">Pulsa aquí</a>
> <a [href]="homeURL">Pulsa aquí</a>
> ```
>
> En ambos casos, Angular termina asignando un valor al atributo `href`.
>
> Sin embargo, es importante entender que la interpolación siempre **evalúa una expresión y la convierte a string**, mientras que el binding de propiedad trabaja directamente con la **propiedad del elemento del DOM**.
>
> Por este motivo, aunque en casos simples puedan parecer equivalentes, **no siempre es recomendable usar interpolación en atributos HTML**.
>
> En general, cuando se trabaja con propiedades que no son de tipo string (por ejemplo objetos, booleanos o elementos del DOM), o cuando se quiere un comportamiento más fiel al modelo del navegador, es preferible utilizar **binding de propiedad `[ ]`**.
>
> ```html
> <button disabled="{{isDisabled}}">Enviar</button>
> ```
>
> ```typescript
> isDisabled=false
> ```
>
> Se traduciría a:
>
> ```html
> <button disabled="false">Enviar</button>
> ```
>
> Y en HTML, si el atributo `disabled` **existe** (da igual si es `true` o `false`), se deshabilita el botón. Por lo que la forma correcta debería ser:
>
> ```html
> <button [disabled]="isDisabled">Enviar</button>
> ```

> [!important]
>
> **Solo usaremos la interpolación `{{ }}` para los contenidos** de los elementos HTML y no para las propiedades de dichos elementos, que para eso usaremos el binding de propiedades `[ ]`.

## 🟩 Binding de eventos ( )

El binding de eventos permite responder a acciones del usuario como clics, teclas o cambios.

Se utiliza la sintaxis:

```html
(evento)="metodo()"
```

**Ejemplo:**

```html
<button (click)="saludar()">Pulsar</button>
```

Cuando el usuario hace clic, se ejecuta el método `saludar()` del componente.

Se puede escribir directamente una instrucción básica, una asignación o algo simple, pero se recomienda encapsular la lógica en una función, para hacer la aplicación más escalable.

```html
<button (dblclick)="contador=0">Reset</button> <!-- Funciona, pero ta feo -->
<button (dblclick)="reset()">Reset</button> <!-- Mejor así -->
```

**Ejemplo:**

```html
<input type="text" (keyup)="comprobar($event)" />
```

Al lanzar el evento `keyup` se ejecuta el método `comprobar()` al cual le pasamos un objeto (`$event`) que contiene toda la información del evento, incluyendo la tecla que se pulsó para lanzar el evento.

```typescript
comprobar(event) {
    if (event.key === 'Enter') {
        console.log('Se ha pulsado Enter');
    }
}
```

> [!note]
>
> 🤓El objeto `event` es de tipo `KeyboardEvent` y sería buena idea definir el parámetro con este tipo, así al escribir <kbd>event.</kbd> el propio IDE nos mostrará las propiedades del objeto, en lugar de ir a ciegas. Otro motivo para definir los tipos de datos con los que trabajamos.
>
> ```typescript
> comprobar(event: KeyboardEvent): void {
>       //...
> }
> ```



## 🟪 Doble binding [( )]

El doble binding permite sincronizar automáticamente los datos entre la vista y el modelo en ambas direcciones.


Se utiliza la sintaxis conocida como **banana in a box**🍌📦`[()]`:

```html
[(ngModel)]="variable"
```

> [!note]
>
> La notación es curiosa, porque realmente hace referencia a que se usa a la vez el enlace de propiedades `[ ]` y el de eventos `( )` conjuntamente.

<div style="
            display: flex;
            align-items: center;
            gap: 16px;
            background-color: #fff3b0;
            border: 2px solid #f4c430;
            border-radius: 12px;
            padding: 12px;
            margin-bottom: 2rem;
            ">
    <img src="img/03-vistas/banana-in-a-box.png" alt="Representación gráfica de Banana-in-a-box (el personaje está disfrazado de banana y está dentro de una caja de cartón" style="
    width: 10rem;
    height: auto;
    max-width: 100%;
    object-fit: contain;
    border-radius: 12px;
    flex-shrink: 0;
  " />
    <div style="
                flex: 1;
                color: #333;
                font-size: 14px;
                ">
        <h3>Banana in a box</h1>
        <p>Angular te aconseja que visualices una banana en una caja, para que recuerdes que primero van los corchetes (caja 📦) y dentro los paréntesis (banana 🍌).</p>
    </div>
</div>




**Ejemplo:**

```html
<input [(ngModel)]="nombre" />
```

En este caso:
- Si el usuario escribe en el input, el valor cambia en el modelo  
- Si el modelo cambia, el input se actualiza automáticamente  

Este tipo de binding es muy útil en formularios y entradas de usuario.

> [!caution]
>
> Para poder utilizar `[(ngModel)]`, es necesario importar el módulo de formularios de Angular (`FormsModule`), ya que esta funcionalidad no forma parte del núcleo básico del framework.
>
> A continuación vemos como hacerlo.

### Importar `FormsModule`

En AngularJS el binding era bidireccional por defecto, pero desde Angular 2 se adopta el *one-way binding* por motivos de rendimiento, dejando el *two-way binding* (doble binding) como una opción explícita mediante módulos.

El módulo `FormsModule` se debe importar en el componente que vaya a usar el doble binding, y en su código TS incluimos lo siguiente:

```typescript
import { Component } from '@angular/core';
import { FormsModule } from '@angular/forms'; //<------ Se añadirá automáticamente

@Component({
  selector: 'app-root',
  imports: [FormsModule],     //<------- Aquí añadimos el módulo a importar
  templateUrl: './app.html',
  styleUrl: './app.css'
})
export class App {
    //...
}
```

> [!important]
>
> - Lo único que tenemos que hacer es escribir `FormsModule` dentro de los corchetes de `imports`. 
> - El IDE añadirá la línea de import automáticamente al inicio del código.
> - Si hubiera otro módulo importado previamente, lo añadimos separándolo por comas. Ej: `import: [BrowserModule, FormsModule]`

# 🧪Ejercicio : Binding en Angular

En este ejercicio vas a crear un **nuevo proyecto de Angular** (sin routing ni tests) llamado `demo2-binding` y trabajarás directamente sobre el **componente raíz** para practicar los distintos tipos de binding vistos en clase.

> [!tip]
>
> 🤔¿Con qué comando se hacía el proyecto directamente sin routing y sin testing?

La idea es que implementes varios ejemplos sencillos en un mismo componente para reforzar el uso de la comunicación entre la vista y el modelo.

**🎯 Objetivo del ejercicio**

Practicar los siguientes tipos de binding:

- 🟨 Interpolación `{{ }}`
- 🟦 Binding de propiedad `[ ]`
- 🟩 Binding de eventos `( )`
- 🟪 Two-way binding `[(ngModel)]`

##  Tareas a realizar

### ⬜ 0. Preparación

1. Crea el proyecto `demo2-binding` con el Angular CLI.
2. Abre el proyecto con VSC.
3. Ejecuta la aplicación y abre la vista previa.
4. Borra el contenido del componente raíz

---

### 🟨 1. Interpolación

Crea una propiedad en el componente llamada `titulo` y asígnale un valor inicial (o directamente o con el constructor, como quieras).

Muestra ese valor en la vista dentro de un encabezado `<h1>` utilizando interpolación.

{{REVISAR DESDE AQUÍ y al final súbelo a stackblitz }}

---

### 🟦 2. Binding de propiedad

Crea una propiedad que puedas utilizar para modificar dinámicamente algún atributo HTML.

Por ejemplo, puedes trabajar con estilos, imágenes o atributos como `disabled`.

---

### 🟩 3. Binding de eventos

Añade dos botones que interactúen con el componente:

- Uno que modifique el valor del `titulo`, por ejemplo poniéndolo en otro idioma.
- Otro que lo restablezca a su valor original.

---

### 🟪 4. Two-way binding

Añade un cuadro de texto (`input`) vinculado al `titulo` mediante `[(ngModel)]`.

De esta forma, cualquier cambio en el input debe reflejarse automáticamente en el `<h1>` y viceversa.

---

### 🧠 Recomendaciones

- Trabaja todo en el **AppComponent**
- No crees componentes nuevos
- Reutiliza las propiedades del mismo modelo para ver cómo se comporta Angular
- Observa cómo cambia la vista automáticamente al modificar los datos

---

### 💡 Objetivo final

Al terminar, deberías tener una pequeña interfaz donde:

- Un título se muestra dinámicamente
- Puede cambiarse mediante botones
- Se modifica desde un input en tiempo real
- Y se actualizan propiedades HTML de forma reactiva
