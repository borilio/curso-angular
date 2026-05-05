> [!caution]
>
> Este documento necesita una actualización. Está proceso...

# RECOGER PARAMÉTROS POR LA URL DESDE UN COMPONENTE

Podemos recoger parámetros que haya en la url desde el código TypeScript de un componente de la siguiente forma:

Imagina que ya tenemos montado que vamos a una url llamada `/proyecto/blog/articulo/143`. Esto nos debería mostrar un componente para editar el articulo cuya id sea `143`. Pero debemos capturar esa id desde el componente para poder consultarlo a la base de datos. Por lo que deberíamos hacer en el archivo ts de nuestro componente lo siguiente:

1. Primero, en el routing debemos declarar que en esa url, va a ir una variable por la url.

```typescript
//routing
...
{ 
    path: "proyecto/blog/articulo/:id", 
    component: EditarArticuloComponent
},
```

2. Inyectar en el constructor los siguientes servicios y clases en el componente `EditarArticuloComponent`:

```typescript
constructor(
    private _route: ActivatedRoute, 
    private _router: Router, 
    private parametros: Params
) {
    //Inicializamos el atributo this.parametros con los parámetros de la url
    this._route.params.subscribe(params => {
      this.parametros = params;
    });
}
```
3. Importar las correspondientes clases de `@angular/router`.
4. Al pasar por el constructor, nos inicializará una variable de tipo `Params`, llamada `parametros` donde habrá una colección de tipo map con pares clave-valor, con el nombre y valor de la variable. El nombre será el indicado en el routing en el punto 1.
5. Y ahora desde el método que queramos, podemos extraer los parámetros de la siguiente manera:

```typescript
ngOnInit(): void {
    //Usamos el servicio para extraer el artículo que buscamos...
    let consulta = this._articuloService.getArticulo(this.parametros['id']);
	consulta.subscribe(...);
     ...
}
```



