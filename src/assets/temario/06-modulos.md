[TOC]

# Introducción ¿Qué es un módulo?

![Metáfora sobre los módulos que se representan como cajas que agrupan distintas partes de la aplicación](img/06-modulos/image-20260422125801195.png){.rounded-4}

En Angular, un módulo es una forma de **organizar y agrupar partes de la aplicación** como componentes, directivas, pipes o servicios. Tradicionalmente, Angular se ha basado en esta estructura para poder dividir una aplicación en bloques más manejables.

Un módulo se define mediante el decorador `@NgModule` y actúa como una “caja” donde se registran los elementos que pertenecen a esa parte de la aplicación. Esto permite a Angular saber qué piezas forman parte de cada funcionalidad y cómo se relacionan entre sí.

Durante mucho tiempo, los módulos han sido una pieza fundamental en la arquitectura de Angular, ya que **toda la aplicación debía estar estructurada obligatoriamente a través de ellos**.

Sin embargo, es importante entender que su papel ha cambiado con la llegada de los **componentes *standalone*** (Angular 14+), que permiten crear componentes sin necesidad de declararlos dentro de un módulo. Aun así, los módulos siguen existiendo dentro del ecosistema de Angular y en muchos proyectos reales todavía están presentes.

# Angular moderno y componentes standalone

Con las versiones recientes de Angular, la forma de construir aplicaciones ha cambiado respecto al enfoque tradicional basado en módulos (`NgModules`).

Antes de este cambio, Angular obligaba a que todo estuviera organizado dentro de módulos, que actuaban como el centro de la aplicación. Con la introducción de los componentes *standalone*, esto cambia y se permite una forma de trabajar más directa y flexible.

🏗️ **Antes (basado en módulos)**

- Todo componente debía declararse en un módulo.
- Existía un módulo raíz llamado `app.module.ts`.
- Las dependencias se gestionaban desde el módulo.
- La estructura del proyecto giraba alrededor de los NgModules.

⚡ **Ahora (standalone)**

- Los componentes pueden funcionar de forma independiente.
- No necesitan estar declarados en un módulo.
- Cada componente gestiona sus propias dependencias.

> [!warning]
>
> **Cómo identificar rápidamente el tipo de proyecto en Angular**
>
> 🔍 **Truco 1: Busca el archivo principal**
>
> - Si existe `app.module.ts` ➡️ el proyecto está basado en **módulos (NgModules)**
> - Si NO existe y ves `app.config.ts` ➡️ el proyecto es **standalone**
>
> 🔍  **Truco 2: Mira el bootstrap de la app**
>
> - `platformBrowserDynamic().bootstrapModule(AppModule)` ➡️ módulos
> - `bootstrapApplication(AppComponent)` ➡️ standalone

> [!tip]
>
> En Angular 17+, los proyectos nuevos se crean por defecto con enfoque *standalone*, como todos los que hemos creado en el curso hasta ahora. 
>
> Este comportamiento puede variar en un futuro según la configuración del CLI al generar el proyecto, igual que ocurrió cuando se pasó de una estructura basada en módulos a otra centrada en componentes standalone.



## Componentes standalone

Los componentes *standalone* son una forma moderna de crear componentes en Angular que no dependen de un `NgModule` para funcionar.

Esto significa que el componente es **autónomo**, es decir, puede existir y utilizarse sin necesidad de ser declarado dentro de un módulo.

**Características principales**

- ⚡ No necesitan estar en `declarations` de ningún módulo.
- 📦 Se pueden usar directamente en rutas (*routing*, ya lo veremos) o en otros componentes.
- 🔗 Gestionan sus propias dependencias mediante `imports`.
- 🧱 Reducen la necesidad de estructuras intermedias como módulos.
- 🪪 Se identifican por la propiedad `standalone:true` en el `@Component`.

> [!caution]
>
> - En proyectos modernos (Angular 17+), por defecto, todos los componentes ya se generan como *standalone* automáticamente.
> - Por eso, en muchos casos no verás `standalone: true` escrito explícitamente.
> - Si aparece, simplemente sirve para **hacer explícito su comportamiento**, no para activarlo manualmente ni porque sea obligatorio.
> - De hecho, todos los componentes que hemos creado hasta ahora son *standalone* y ninguno tenía la propiedad `standalone: true`.

**Ejemplo:**

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-saludo',
  standalone: true,
  imports: [],
  template: `
    <h2>👋 Hola Angular</h2>
    @if (mostrarMensaje) {
    <p>
      Este es un componente standalone funcionando sin NgModule.
    </p>
    }
    <button (click)="alternarMensaje()">
      Mostrar / Ocultar mensaje
    </button>
  `
})
export class SaludoComponent {
  mostrarMensaje = true;

  alternarMensaje(): void {
    this.mostrarMensaje = !this.mostrarMensaje;
  }
}
```

> [!warning]
>
> 🤯No te líes:
>
> - Este componente es exactamente igual que cualquiera que hayamos visto (aunque tenga el HTML en el TS, ya vimos que se podía).
> - Solo está indicando de forma explícita (cosa que no hace falta) que es un componente *standalone* con la propiedad `standalone: true`.
> - Todos los componentes por defecto ya son *standalone*, aunque no tenga `standalone: true`.



## ¿Cuándo seguimos usando módulos?

Aunque Angular moderno trabaja principalmente con componentes *standalone*, los módulos (`NgModules`) siguen siendo útiles en algunos escenarios concretos. No son obligatorios en la mayoría de casos, pero sí aparecen en situaciones reales de desarrollo o integración.

1. 📦 **Integración de librerías externas**
   Algunas librerías aún están basadas en módulos y requieren ser importadas desde un `NgModule`. En estos casos, se suele crear un módulo propio para agrupar e importar todo de forma ordenada. Lo veremos al integrar librerías UI (*user interface*) como PrimeNG o Angular Material.
2. 🧩 **Agrupación de funcionalidades**
   Los módulos pueden usarse para agrupar componentes relacionados dentro de una misma funcionalidad, facilitando la organización del proyecto y evitando duplicación de imports.
3. ⚡**Lazy loading (carga perezosa o diferida)**
   Siguen siendo una forma habitual de estructurar rutas que se cargan solo cuando el usuario accede a ellas, mejorando el rendimiento en aplicaciones grandes. Lo veremos más adelante.
4. 🏗️ **Proyectos existentes o legacy**
   En aplicaciones ya creadas con una arquitectura basada en módulos, es habitual seguir utilizándolos para mantener la coherencia del proyecto o evitar migraciones innecesarias.

> [!important]
>
> 🎯Los módulos no han desaparecido, pero han dejado de ser obligatorios en la mayoría de casos. Se siguen usando principalmente para organización, retrocompatibilidad y casos específicos como librerías o *lazy loading*.

# 🧪Crear un módulo paso a paso

Aunque Angular moderno trabaja principalmente con componentes *standalone*, los módulos siguen siendo importantes para entender proyectos antiguos y ciertos patrones de organización que todavía podemos encontrar en aplicaciones reales.

{{ Aquí veremos como crear un módulo. Hay que pensar un ejemplo que se vea útil y no sea muy complejo. Creo que podemos crear varios componentes (tipo cabecera, contenido y footer) y crear componentes sueltos (botón, imagen, cositas) y agruparlo todo en un módulo para no tener que importar los componentes por separado en cada uno de los componentes (cabecera, contenido y footer) }}

{{Al hacer el ejemplo, explicar lo de crear una carpeta llamada components }}



# Ejercicio: Módulos





