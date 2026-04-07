# JSON Server en Node.js

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
  }],
  "heroes": [{
      "id": "dc-batman",
      "superhero": "Batman",
      "publisher": "DC Comics",
      "alter_ego": "Bruce Wayne",
      "first_appearance": "Detective Comics #27",
      "characters": "Bruce Wayne"
    },
    {
      "id": "dc-wonder",
      "superhero": "Wonder Woman",
      "publisher": "DC Comics",
      "alter_ego": "Princess Diana",
      "first_appearance": "All Star Comics #8",
      "characters": "Princess Diana"
    },
    {
      "id": "marvel-captain",
      "superhero": "Captain America",
      "publisher": "Marvel Comics",
      "alter_ego": "Steve Rogers",
      "first_appearance": "Captain America Comics #1",
      "characters": "Steve Rogers"
    },
    {
      "id": "marvel-iron",
      "superhero": "Iron Man",
      "publisher": "Marvel Comics",
      "alter_ego": "Tony Stark",
      "first_appearance": "Tales of Suspense #39",
      "characters": "Tony Stark"
    }
  ]
}

```

Este ejemplo tendría 2 'tablas' y serían `usuarios` y `heroes`. Cada una de ellas tendrían sus propios campos. 

Para que NodeJS empiece a servir estos datos localmente tendremos que escribir abrir una terminal en la carpeta donde tengamos el archivo `db.json` y escribir lo siguiente:

```bash	
json-server --watch db.json
```

Esto abrirá algo parecido a:

```bash
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

Ya que el servidor es local y la respuesta será muy rápida, se puede añadir un retardo artificial en la respuesta del backend para probar tiempos de carga y demás. En el siguiente ejemplo le añadiríamos 1500ms a lo que ya tarde la respuesta. 

```bash
json-server --watch --delay 1500 db.json 
```

### Parámetros en la petición

#### Buscar una cadena en algunos de sus campos 

```bash
http://localhost:3000/heroes?q=super
```

Mostrará todos los 'registros' que tengan en algún campo (el que sea), el texto 'super'.

#### Limitar el número de registros devueltos

```bash
http://localhost:3000/heroes?q=super&_limit=5
```

Limitará a 5 elementos el resultado de la consulta anterior. Recordar que el primero los parámetros se empieza con el símbolo `?` y cada parámetro adicional va con el símbolo `&`. Se pueden poner en cualquier orden.

## My JSON Server

Usando la web https://my-json-server.typicode.com/, se puede hacer lo mismo pero además funcionará remotamente y sin necesidad de instalar nada. Tan sólo habrá que colocar el archivo `db.json` (ahora si debe llamarse así) en la raíz de un repositorio **público** de GitHub y acceder a la url siguiente usando tu nombre de usuario de GitHub y el nombre del repositorio donde esté el archivo `db.json`.

https://my-json-server.typicode.com/tunombredeusuario/turepositorio

Aquí se puede ver un ejemplo funcionando, de un archivo json subido a un repositorio:

 https://my-json-server.typicode.com/borilio/curso-angular
