[TOC]

# Backend en Node.js

## JSON Server

Podremos instalar un servidor local de datos en formato JSON que acepta peticiones HTTP de tipo `GET, POST, PUT, PATCH, DELETE y OPTIONS`.

La página oficial y todas las instrucciones actualizadas están en https://www.npmjs.com/package/json-server.

Para instalarlo, debemos escribir en la consola lo siguiente: 

```shell
npm install -g json-server@0.17.4
```

> [!important]
>
> Esa es la última versión que incluía la opción de retraso artificial. Si deseas instalar la última versión disponible de json-server ejecuta `npm install -g json-server` sin indicar la versión concreta, pero no tendrás la opción de retraso.

Tendremos que crear un archivo llamado  `db.json` (puede llamarse de otra forma, pero se recomienda ese nombre) con los datos de la siguiente forma :

```json
{
  "usuarios": [{
    "id": 1,
    "usuario": "John Doe",
    "email": "john.doe@gmail.com"
  },
  {
    "id": 2,
    "usuario": "Jane Smith",
    "email": "jane.smith@yahoo.com"
  },
  {
    "id": 3,
    "usuario": "Carlos García",
    "email": "carlos.garcia@gmail.com"
  },
  {
    "id": 4,
    "usuario": "María López",
    "email": "maria.lopez@hotmail.com"
  }],
  "heroes": [{
      "id": "dc-batman",
      "nombre": "Batman",
      "alter_ego": "Bruce Wayne",
      "poder": 85,
      "activo": true
    },
    {
      "id": "dc-wonder",
      "nombre": "Wonder Woman",
      "alter_ego": "Diana Prince",
      "poder": 90,
      "activo": true
    },
    {
      "id": "dc-superman",
      "nombre": "Superman",
      "alter_ego": "Clark Kent",
      "poder": 100,
      "activo": true
    },
    {
      "id": "marvel-captain",
      "nombre": "Captain America",
      "alter_ego": "Steve Rogers",
      "poder": 80,
      "activo": true
    },
    {
      "id": "marvel-iron",
      "nombre": "Iron Man",
      "alter_ego": "Tony Stark",
      "poder": 88,
      "activo": true
    },
    {
      "id": "marvel-spider",
      "nombre": "Spider-Man",
      "alter_ego": "Peter Parker",
      "poder": 75,
      "activo": true
    }
  ]
}

```

Este ejemplo tendría 2 'tablas' y serían `usuarios` y `heroes`. Cada una de ellas tendrían sus propios campos. 

Para que NodeJS empiece a servir estos datos localmente tendremos que abrir una terminal en la carpeta donde tengamos el archivo `db.json` y escribir lo siguiente:

```shell
json-server --watch db.json
```

Esto abrirá algo parecido a:

```shell
  \{^_^}/ hi!

  Loading db.json
  Done

  Resources
  http://localhost:3000/usuarios
  http://localhost:3000/heroes

  Home
  http://localhost:3000

  Type s + enter at any time to create a snapshot of the database
  Watching...
```

Y así tendremos un backend montado fácilmente en 30 segundos. 

### Retraso artificial

Ya que el servidor es local y la respuesta será muy rápida, se puede añadir un retraso artificial en la respuesta del backend para probar tiempos de carga y demás. En el siguiente ejemplo le añadiríamos 1500ms a lo que ya tarde la respuesta. 

```shell
json-server --watch --delay 1500 db.json
```

### Parámetros en la petición

#### Buscar una cadena en algunos de sus campos

```http
GET http://localhost:3000/heroes?q=super
```

Mostrará todos los 'registros' que tengan en algún campo (el que sea), el texto 'super'.

#### Limitar el número de registros devueltos

```http
GET http://localhost:3000/heroes?q=super&_limit=5
```

Limitará a 5 elementos el resultado de la consulta anterior. Recordar que para el primero de los parámetros se usa el símbolo `?` y cada parámetro adicional va con el símbolo `&`. Se pueden poner en cualquier orden.

## My JSON Server

Usando la web https://my-json-server.typicode.com/, se puede hacer lo mismo pero además funcionará remotamente y sin necesidad de instalar nada. Tan sólo habrá que colocar el archivo `db.json` (ahora si debe llamarse así) en la raíz de un **repositorio público de GitHub** y acceder a la url siguiente usando tu nombre de usuario de GitHub y el nombre del repositorio donde esté el archivo `db.json`.

https://my-json-server.typicode.com/tunombredeusuario/turepositorio

Aquí se puede ver un ejemplo funcionando, de un archivo ``json`` subido a un repositorio:

 https://my-json-server.typicode.com/borilio/curso-angular

> [!caution]
>
> **Diferencia importante:** Instalar json-server localmente permite hacer cambios persistentes (POST, PUT, PATCH, DELETE) que se guardarán en el archivo `db.json`. En cambio, my-json-server remoto acepta estas peticiones pero **no las persiste**; los cambios se pierden al reiniciar el servidor o al volver a acceder, ya que los datos se cargan desde el archivo en GitHub pero no se modifican.
