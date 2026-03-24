# Temar.io — Manual para crear un nuevo temario

Esta es tu guía de referencia: cuando vuelvas dentro de meses y necesites replicar la aplicación con un nuevo temario, aquí está todo lo que tienes que saber.

Si quieres ver en funcionamiento este repositorio, ve a https://temario-base.netlify.app

## Clonar y ejecutar localmente

```bash
git clone https://github.com/borilio/temar.io
cd temar.io
npm install
npm run start
```

Abre `http://localhost:4200` en el navegador. La app estará lista para editar.

> [!TIP]
>
> 1. Abre el proyecto con Visual Studio Code, y ya podrás editarlo localmente. 
> 2. Podrás subirlo a un nuevo repositorio.
> 3. Despliega ese repo en Netlify y ya estaría funcionando. Solo queda editar su contenido.

## Estructura de carpetas

Solo tocas estos lugares:

```
src/
├── assets/
│   ├── temario/              ← TUS archivos Markdown aquí
│   │   ├── *.md              ← (ejemplo: intro.md, modulo1.md)
│   │   └── img/              ← Imágenes del temario
│   │       ├── intro/        ← (subcarpetas por tema)
│   │       ├── modulo1/
│   │       └── ...
│   ├── json/
│   │   ├── temario.json      ← EDITAR: estructura y temas
│   │   └── config.json       ← EDITAR: configuración global
│   └── themes/
│       ├── markdown/         ← CSSs de markdown (ya están)
│       └── highlight/        ← CSSs de código (ya están)
└── ...
```

> [!IMPORTANT]
> Las rutas deben ser exactas (mayúsculas, espacios, caracteres especiales importan, especialmente en Linux).

## Paso 1: Añadir contenidos Markdown e imágenes

1. Copia tus archivos `.md` a `src/assets/temario/`:
   ```
   src/assets/temario/intro.md
   src/assets/temario/modulo1.md
   src/assets/temario/modulo2.md
   ```

2. Coloca las imágenes en subcarpetas dentro de `src/assets/temario/img/`. Si hiciste el contenido con Typora ya tendrá la estructura adecuada: 
   ```
   src/assets/temario/img/intro/logo.png
   src/assets/temario/img/modulo1/diagrama.png
   ```

3. En `temario.json` harás referencia a estos nombres de archivo (verás cómo más adelante).

## Paso 2: Editar `temario.json`

Abre `src/assets/json/temario.json`. Este archivo organiza la estructura completa del temario. Los dos JSON trabajan juntos:
- **`temario.json`**: estructura, temas y archivos MD.
- **`config.json`**: apariencia, preset, temas visuales, logos, enlaces.

### Estructura base de `temario.json`

```json
{
  "tituloGeneral": "Mi Curso",
  "descripcion": "Descripción corta del curso",
  "img": "img/logo-general.png",
  "mostrarBloquesEnMenu": true,
  "bloques": [
    {
      "id": "bloque-1",
      "orden": 1,
      "titulo": "Bloque 1: Introducción",
      "mostrarTitulo": true,
      "descripcion": "Primera parte del temario",
      "expandible": true,
      "contraido": false,
      "habilitado": true,
      "temas": [
        {
          "id": "intro-1",
          "orden": 1,
          "titulo": "Introducción",
          "descripcion": "Primera lección",
          "archivoMd": "intro.md",
          "img": "img/intro/logo.png",
          "habilitado": true
        }
      ]
    }
  ]
}
```

### Explicación de campos por nivel

**Nivel raíz (`temario.json`)**

| Campo | Tipo | Requerido | Efecto |
|-------|------|-----------|--------|
| `tituloGeneral` | string | Sí | Título mostrado en el índice. Si falta, encabezado vacío. |
| `descripcion` | string | No | Texto descriptivo bajo el título. Si falta/vacío, no se muestra. |
| `img` | string | No | Imagen general (ruta relativa a `src/assets/temario/`). Si falta/ruta errónea, no se muestra imagen. |
| `mostrarBloquesEnMenu` | boolean | No | Si `true`: menú agrupa temas por bloques. Si `false` u omitido: menú plano con separadores. |
| `bloques` | array | Sí | Array de bloques (cada uno contiene temas). |

**Nivel bloque**

| Campo | Tipo | Requerido | Efecto |
|-------|------|-----------|--------|
| `id` | string | Sí | Identificador único del bloque. Usado internamente. |
| `orden` | number | Sí | Número para ordenar bloques en menú e índice. Menor primero. |
| `titulo` | string | Sí | Texto mostrado en la cabecera del panel del bloque. |
| `mostrarTitulo` | boolean | Sí | Si `false`: cabecera invisible. Si `true`: se muestra. |
| `descripcion` | string | Sí | Texto bajo el título en el panel. |
| `expandible` | boolean | No | Si `true`: bloque se puede colapsar. Si `false` u omitido: no se puede colapsar (siempre abierto). |
| `contraido` | boolean | No | Si `true`: bloque comienza colapsado. Si `false` u omitido: comienza abierto. (Solo tiene efecto si `expandible: true`.) |
| `habilitado` | boolean | No | Si `false`: bloque deshabilitado en menú. Si `true` u omitido: habilitado. |
| `temas` | array | Sí | Array de temas dentro de este bloque. |

**Nivel tema**

| Campo | Tipo | Requerido | Efecto |
|-------|------|-----------|--------|
| `id` | string | Sí | Identificador único. Usado en URL `/tema/{id}`. |
| `orden` | number | Sí | Orden dentro del bloque. |
| `titulo` | string | Sí | Texto del botón. |
| `descripcion` | string | No | Aparece en tooltip al pasar ratón. |
| `archivoMd` | string | **Sí** | Nombre del archivo Markdown (ej: `intro.md`). Debe existir en `src/assets/temario/`. Si falta o es incorrecto: **error al cargar contenido**. |
| `img` | string | No | Imagen del tema (ruta en `src/assets/temario/`). Si omites: se busca `emoji`, si no: se busca `icon`. |
| `emoji` | string | No | Emoji alternativo si no hay `img`. |
| `icon` | string | No | Icono de PrimeNG (ej: `pi pi-star`). Se usa si no hay `img` ni `emoji`. |
| `severity` | string | No | Color del botón (`primary`, `success`, `warning`, `danger`, etc.). |
| `outlined` | boolean | No | Si `true`: botón con borde. Si `false` u omitido: botón relleno. **Fallback a `true` si se omite.** |
| `habilitado` | boolean | No | Si `false`: tema deshabilitado (gris, no navegable). Si `true` u omitido: habilitado. |

> [!CAUTION]
> **Errores comunes:**
> - Olvidas `archivoMd` o pones un nombre incorrecto → la app muestra error de carga.
> - `habilitado` omitido en temas → se muestra deshabilitado por defecto.
> - Rutas de imágenes con mayúsculas incorrectas → imagen no carga.
> - `mostrarBloquesEnMenu` omitido → por defecto muestra bloques agrupados.

## Paso 3: Editar `config.json`

Abre `src/assets/json/config.json`. Este archivo controla la apariencia y comportamiento global.

### Estructura base de `config.json`

```json
{
  "preset": "MiPresetSky",
  "defaultThemeId": "github-light",
  "mostrarLigadurasFuentes": true,
  "devMode": false,
  "data": {
    "copyright": "Copyright 2025 - Mi Curso",
    "logos": [
      { "img": "img/logo1.png", "link": "https://ejemplo.com", "alt": "Logo 1" }
    ]
  },
  "themes": [
    {
      "id": "github-light",
      "name": "GitHub Claro",
      "baseMode": "light",
      "markdownThemeFile": "github-markdown-light.css",
      "highlightThemeFile": "github.css"
    }
  ],
  "enlacesExternos": {
    "texto": "Enlaces útiles del curso",
    "enlaces": [
      {
        "icon": "pi pi-link",
        "desc": "Documentación",
        "href": "https://docs.ejemplo.com"
      }
    ]
  },
  "caducidad": {
    "fecha": "2099-12-31",
    "aviso": 30,
    "mensajeAviso": "El curso cierra pronto",
    "mensajeCerrado": "El curso ya no está disponible"
  }
}
```

### Explicación de campos

**`preset`** (string)
- Nombre del preset de colores definido en `src/app/shared/presets-aura/preset-colores.ts`.
- Si pones un nombre inexistente: la app usa fallback `MiPresetIndigo`.
- Ejemplos: `MiPresetSky`, `MiPresetNoir`, `MiPresetTeal`.

**`defaultThemeId`** (string)
- `id` del tema que se carga al abrir la app.
- Debe coincidir exactamente con algún `themes[].id`.
- Si es inválido: `ThemeService` falla y la app no carga estilos correctamente.
- **Asegúrate de que existe.**

**`devMode`** (boolean)

- Muestra opciones extra (Labs) en la configuración, como por ejemplo mostrar u ocultar ligaduras.
- Si `false` u omitido: modo producción. No se muestra la pestaña Labs.

**`mostrarLigadurasFuentes`** (boolean)

- Si `true`: tipografía con ligaduras por defecto.
- Si `false`: tipografía sin ligaduras por defecto.
- Si está `devMode` en true, se podrá modificar en tiempo real en Labs dentro de configuración.

**`data`** (objeto)

- `copyright`: texto mostrado en el footer.
- `logos`: array de objetos `{img, link, alt}`. Aparecen en pie.
  - Si vacío: no se muestran logos.
  - Rutas relativas a `src/assets/temario/`.

**`themes`** (array de objetos)
- Cada objeto es un tema visual disponible.
- Campos necesarios:
  - `id`: identificador único (usado en `defaultThemeId` y selector de temas).
  - `name`: nombre mostrado en el selector.
  - `baseMode`: `light` o `dark`.
  - `markdownThemeFile`: nombre CSS en `src/assets/themes/markdown/`. Es el que aplicará estilo al markdown.
  - `highlightThemeFile`: nombre CSS en `src/assets/themes/highlight/`. Es el que aplicará estilo al código.
- Si un archivo CSS no existe: navegador intenta cargar, falla (404), y la UI se ve rota.
- **Verifica que los archivos CSS existan.**

**`enlacesExternos`** (objeto)

Son los enlaces que se mostrarán en la página de “Enlaces externos” dentro de una tabla.

- `texto`: descripción mostrada en la página de "Enlaces", justo encima de la tabla.
- `enlaces`: array de objetos `{icon, desc, descLarga?, href, disabled?}`.
  - `icon`: clase de icono (PrimeNG, ej: `pi pi-link`).
  - `desc`: descripción corta.
  - `descLarga`: tooltip (aparece al pasar ratón).
  - `href`: URL del enlace. Si vacío o `disabled: true`: se muestra deshabilitado.
  - `disabled`: si `true`: fuerza deshabilitado (botón gris). Es opcional. Para deshabilitar también se puede dejar vacío el `href` (por si quieres deshabilitar sin borrar el `href`).

**`caducidad`** (objeto)

- Usada por `ExpirationGuard` para proteger el temario.
- `fecha`: fecha de cierre en formato ISO (ej: `2099-12-31`). Si es fecha pasada: acceso bloqueado → redirige a página "/cerrado".
- `aviso`: número de días antes del cierre para mostrar warning.
- `mensajeAviso`: mensaje mostrado en el toast de aviso.
- `mensajeCerrado`: mensaje mostrado en `/cerrado` cuando ya ha caducado.
- Si `fecha` es inválida: guardia puede fallar. **Define siempre una fecha válida.**

> [!CAUTION]
> **Errores comunes:**
>
> - `defaultThemeId` no existe en `themes[]` → error al inicializar temas.
> - Archivos CSS (`markdownThemeFile`, `highlightThemeFile`) con rutas incorrectas → 404 en consola, estilos no aplicados.
> - `caducidad.fecha` en formato incorrecto → El guard no funcionará.
> - `preset` inexistente → Por defecto se usa `MiPresetIndigo`.

## Paso 4: Probar en local

1. Asegúrate de que todos los archivos Markdown están en `src/assets/temario/`.
2. Verifica que las imágenes referenciadas en `temario.json` existen en `src/assets/temario/img/`.
3. Revisa que `defaultThemeId` existe en `config.json` dentro de `themes[]`.
4. Abre la consola del navegador (F12) para buscar errores.

Ejecuta:
```bash
npm run start
```

Navega a `http://localhost:4200` y:
- Ve al índice, verifica que se ve tu `tituloGeneral` y estructura.
- Haz clic en un tema y comprueba que carga el contenido.
- Abre la consola: no debe haber errores 404 de imágenes ni CSS.

> [!TIP]
> **Tips de depuración:**
> - Si un tema no carga: revisa que `archivoMd` existe y la ruta es exacta.
> - Si falta una imagen: comprueba mayúsculas y espacios en la ruta.
> - Si el tema visual es incorrecto: verifica que `markdownThemeFile` y `highlightThemeFile` existen.
> - Usa el inspector del navegador para ver los errores 404.

## Checklist final

Antes de publicar:

- [ ] ¿Existen todos los archivos `.md` en `src/assets/temario/`?
- [ ] ¿Están las imágenes en `src/assets/temario/img/`?
- [ ] ¿Todos los `archivoMd` en `temario.json` coinciden con nombres reales?
- [ ] ¿Todos los temas tienen `habilitado: true` si quieres que aparezcan?
- [ ] ¿`defaultThemeId` existe en `themes[]` de `config.json`?
- [ ] ¿Los archivos CSS (`markdownThemeFile`, `highlightThemeFile`) existen?
- [ ] ¿`caducidad.fecha` es una fecha válida (formato ISO)?
- [ ] ¿Probaste en local y no hay errores en consola?
- [ ] ¿Las rutas funcionan correctamente (sin errores 404)? 

# Temario.io — Modificar la app base 

A continuación se explican algunas cosas que puedes necesitar recordar para editar la app base, sobre todo si hace mucho tiempo que creaste la app y cuando vuelvas ya no te acuerdas de nada. Para eso está tu yo del pasado que te ayuda a recordar ahora que lo tiene todo fresquito.

## Uso de PrimeNG

### Añadir el componente al import compartido

Todas los módulos (componentes) de PrimeNG se van añadiendo al archivo `shared/primeng.imports.ts`. Aquí estarán todos los módulos que usemos de PrimeNG.

```typescript
import { ButtonModule } from 'primeng/button';
import { ToastModule } from 'primeng/toast';
//1. Primero se copia aquí la línea

export const PRIMENG_IMPORTS = [
    ButtonModule,
    ToastModule,
    //2. Luego se añade aquí el módulo
    
];
```

### Añadir el import al componente de Angular

Hacemos el import del archivo `shared/primeng.imports.ts` en el componente que queramos usar los "componentes" de PrimeNG.

```typescript
import { Component } from '@angular/core';
import { PRIMENG_IMPORTS } from '../../shared/primeng.imports';

@Component({
  selector: 'app-pie',
  imports: [PRIMENG_IMPORTS],
  templateUrl: './pie.html',
  styleUrl: './pie.css'
})
export class Pie {
  
}
```

Hay que importarlo en todos los componentes que tenga nuestra aplicación y que queramos usar componentes de primeNG.

## Uso de json internos de configuración

Es la idea principal sobre la que se basa la aplicación. No tener que tocar código, solo archivos json.

De esta forma, puedo editar fácilmente el contenido y funcionamiento de la aplicación sin tener que tocar código. Por ejemplo, el footer mostrará tantas imágenes haya en el JSON.

Para usar la información de los JSON en el código, se añadió una línea al `tsconfig.app.json`, concretamente la de `"resolveJsonModule": true`.

```json
{
  "extends": "./tsconfig.json",
  "compilerOptions": {
    "outDir": "./out-tsc/app",
    "types": [],
    "resolveJsonModule": true
  },
  "include": [
    "src/**/*.ts"
  ],
  "exclude": [
    "src/**/*.spec.ts"
  ]
}
```

Ahora ya podemos volcar archivos json externos en memoria de una forma simple sin hacer peticiones http ni funciones asíncronas.

1️⃣En el archivo `shared/constantes.ts` tenemos el siguiente contenido:

```typescript
import configJson from "../../../public/assets/json/config.json";
import { ConfigModel } from "../models/config.model";

// Creamos una constante de tipo ConfigModel con el contenido del json
// Para usarla, simplemente importa la constante CONFIG en donde quieras
export const CONFIG: ConfigModel = configJson;
```

2️⃣En el componente que deseemos usarlo importamos la constante:

````typescript
import { Component } from '@angular/core';
import { ConfigModel } from '../../models/config.model';
import { CONFIG } from '../../shared/constantes';

@Component({
  selector: 'app-pie',
  imports: [],
  templateUrl: './pie.html',
  styleUrl: './pie.css'
})
export class Pie {
  // Objeto de configuración que incluye información del footer (entre otras)
  public config: ConfigModel = CONFIG ;

}
````

3️⃣En el HTML, podemos usar el contenido de esa constante para crear contenido “dinámico”. Por ejemplo, se generan tantas imágenes como elementos haya en el array del JSON. O bien mostramos un simple string en el último div. 

```html
<div class="row justify-content-center">

    @for (logo of config.footer.logos; track logo) {
    <div class="col mb-2">
        <a [href]="logo.link">
            <img [src]="logo.img" [alt]="logo.alt" class="footer-logo img-fluid" />
        </a>
    </div>
    }
</div>

<div class="row text-center p-2">
    <span><i class="bi bi-c-circle"></i> {{config.footer.copyright}}</span>
</div>
```

> [!tip]
>
> Así no tenemos que editar archivos `html` de un temario a otro, si no solo editar archivos `json`.

## Temas

La aplicación te permite elegir un tema prediseñado desde el selector de temas.

En el `config.json`se puede ver la siguiente estructura:

```json
// config.json
{
    "preset": "MiPresetTeal",
    "defaultThemeId": "github-light",
    "footer": {...},
    "themes": [
        {
            "id": "github-light",
            "name": "GitHub Claro",
            "baseMode": "light",
            "markdownThemeFile": "github-markdown-light.css",
            "highlightThemeFile": "github.css"
        },
        {
            "id": "github-dark",
            "name": "GitHub Oscuro",
            "baseMode": "dark",
            "markdownThemeFile": "github-markdown-dark.css",
            "highlightThemeFile": "github-dark.css"
        },
        {...}       
    ]
}
```

En `themes`, hay un array de objetos. Cada objeto representa un tema de la aplicación.

Los atributos sirven para lo siguiente:

- `id`: El nombre interno que se usará. 

- `name`: El nombre que se mostrará al usuario.

- `baseMode`: Indica si la interfaz completa se mostrará en claro u oscuro. Sus posibles valores son `dark` o `light`.

- `markdownThemeFile`: El css que se aplicará al contenido generado desde el archivo markdown, independientemente del `baseMode`.

- `highlightThemeFile`: El css que se aplicará a los bloques de código, independientemente del `baseMode`.

- En `defaultThemeId` se indicará la `id` del tema por defecto que se aplicará al iniciar.

### Crear un tema

El proyecto tiene la siguiente estructura:

```
/src
└── assets/
    └── themes/
        ├── highlight/
        │   └── ... (archivos css)
        └── markdown/
            ├── github-markdown-dark.css
            └── github-markdown-light.css
```

Puedes crear nuevos temas a elegir siguiendo los siguientes pasos:

1️⃣Copia los archivos css en su respectiva carpeta (una para el Mark Down y otra para los bloques de código). 

> [!important]
>
> Asegúrate que el css del Mark Down esté todo encapsulado con la clase `.markdown-body`, si no no funcionará. Así funciona la librería del markdown-it.

> [!note]
>
> La librería `highlight.js` ya contiene más de 500 archivos css para aplicar a los bloques, no creo que necesites más. En el repositorio habrá unas cuantas, pero en `node_modules/highlight.js` están todas. 



2️⃣ Crea un objeto en el `config.json`. El atributo `themes` contiene un array con los temas. Solo tienes que crear un objeto más con la combinación que quieras y ya aparecerá automáticamente en el selector de temas.

```json
"themes": [
    {
        "id": "github-light",
        "name": "GitHub Claro",
        "baseMode": "light",
        "markdownThemeFile": "github-markdown-light.css",
        "highlightThemeFile": "github.css"
    },
]
```

> [!note]
>
> En `baseMode` solo puede poner `light` o `dark`.

> [!tip]
>
> No tienes que editar nada de código, tan solo copiar el css a su sitio, y crear el tema en el `config.json`.

### Presets de colores de un tema

Ya se han predefinido 18 presets de colores distintos, por lo que no creo que necesites añadir más, de todas formas, está todo documentado por si necesitas añadir o modificar presets nuevos.

#### Crear un nuevo preset

1️⃣ Crea el nuevo preset:

Abre el archivo de presets `src/app/shared/presets-aura/preset-colores.ts` y genera una nueva constante por cada preset nuevo:

```typescript
// Estos dos import ya deberían estar en el archivo
import { definePreset } from '@primeuix/themes';
import Aura from '@primeuix/themes/aura';

// -------------->
// Esto es lo que hay que generar.
export const MiPresetTurquesa = definePreset(Aura, {
  semantic: {
    primary: {
      50:  '#e0fcf9',
      100: '#b3f7ef',
      200: '#80f2e5',
      300: '#4deedb',
      400: '#1ae9d1',
      500: '#00cfb7', // Color principal
      600: '#00a393',
      700: '#00776f',
      800: '#004b4b',
      900: '#002f2f',
      950: '#001a1a'
    }
  }
});
```

Puedes elegir un color central (el 500) y pídele a alguna IA que te genere el preset como el de arriba, pero con un color concreto.

2️⃣ Añade el preset al objeto PRESETS, que está en el mismo archivo `src/app/shared/presets-aura/preset-colores.ts`, abajo del todo.

```typescript
export const PRESETS = {
  MiPresetLight,
  MiPresetBlue,
  MiPresetNoir,
  MiPresetViolet,
  MiPresetEmerald,
  MiPresetOrange,
  MiPresetTeal,
  MiPresetSky,
  MiPresetIndigo,
  MiPresetPurple,
  MiPresetFuchsia,
  MiPresetPink,
  MiPresetRose,
  MiPresetTurquesa // <-- Añade aquí tu nuevo preset
};
```

3️⃣ Y listo. Ya hemos hecho que el `app.config.ts` cargue automáticamente el preset indicado en el `config.json` o el `MiPresetIndigo` por defecto, por lo que no hay que modificar nada más.

#### Elegir un preset

En el archivo `public/assets/json/config.json` , escribir el nombre del preset existente en el campo `preset` y listo. Todo se cargará automáticamente.

```json
{
  "preset": "MiPresetTurquesa",
  "footer": {
    // ...
  }
}
```

