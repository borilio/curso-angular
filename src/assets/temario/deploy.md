[TOC]



# Desplegar una aplicación desde GitHub a Netlify 🚀

## Introducción

Netlify permite publicar proyectos web directamente desde un repositorio de GitHub. Cada vez que subas cambios al repositorio, Netlify puede generar una nueva versión automáticamente.

------

## Requisitos previos

- Tener la aplicación subida a GitHub.
- Tener una cuenta en GitHub.
- Tener una cuenta gratuita en Netlify.
- Que el proyecto funcione correctamente en local.

------

## Paso 1. Entrar en Netlify

1. Accede a la web oficial de Netlify.
2. Inicia sesión.
3. Puedes entrar usando tu cuenta de GitHub para hacerlo más rápido.

------

## Paso 2. Importar el proyecto desde GitHub

1. Dentro del panel, pulsa en **Add new site**.
2. Selecciona **Import an existing project**.
3. Elige **GitHub** como proveedor.
4. Autoriza a Netlify para acceder a tus repositorios si lo solicita.
5. Selecciona el repositorio donde está tu aplicación.

------

## Paso 3. Configurar el despliegue

Netlify detectará el tipo de proyecto, pero conviene revisar estos datos:

### Si es Angular

- **Build command:** `ng build`
- **Publish directory:** `dist/nombre-del-proyecto/browser`

> [!important]
> Sustituye `nombre-del-proyecto` por el nombre real configurado en `angular.json`.

### Si es otro proyecto frontend

Normalmente detectará automáticamente React, Vue, Vite u otros frameworks.

------

## Paso 4. Lanzar el despliegue

1. Pulsa en **Deploy site**.
2. Netlify descargará el repositorio.
3. Instalará dependencias.
4. Ejecutará la compilación.
5. Publicará la web.

Cuando termine, mostrará una URL pública similar a:

```url
https://tu-proyecto.netlify.app
```

------

## Paso 5. Actualizaciones automáticas

Cada vez que hagas:

- cambios en tu proyecto
- commit
- push a GitHub

Netlify detectará la nueva versión y volverá a desplegar automáticamente.

------

## Dominio personalizado (opcional)

Puedes cambiar la URL gratuita por un dominio propio desde:

**Site settings > Domain management**

------

## Problemas frecuentes ⚠️

### Error de rutas en Angular

Si usas rutas internas (`/inicio`, `/contacto`, etc.) puede ser necesario añadir un archivo `_redirects`.

Contenido:

```text
/* /index.html 200
```

### Fallo en la compilación

Revisa:

- Dependencias en `package.json`
- Versión de Node.js
- Errores previos en local

------

## Ventajas de Netlify ✅

- Gratis para proyectos personales.
- Integración directa con GitHub.
- HTTPS automático.
- Despliegue continuo.
- Muy rápido y sencillo.

------

## Resumen

GitHub guarda tu código y Netlify lo publica en internet. La combinación de ambos permite trabajar de forma profesional, rápida y automática. 🔥
