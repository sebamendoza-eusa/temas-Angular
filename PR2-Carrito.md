# PR2. Carrito de la compra

## Desarrollar una aplicación básica de comercio electrónico que implemente un catálogo, un carrito de la compra y un formulario de pedido.

#### Indicaciones

1. Crear un nuevo proyecto Angular en la carpeta donde estemos trabajando los distintos proyectos. Llama al proyecto `mystore`
2. Para crear un tipo personalizado de datos para los productos de la tienda define una interface y guardarla en `src/app`. Llámala `productosInterface.ts` 

```ts
export interface Producto {
    nombre: string;
    precio: number;
	descripcion: string;
}
```

3. Añadir un archivo `productos.ts` en la raíz de `src/app` para que nos sirva como contenedor de datos. Tiene que tener la forma:

```ts
const productos: Producto[] = [
    {
        // Datos del producto 1
    },
    //...
] 
```

4. Si quieres, puedes usar esta hoja de estilos globales para el proyecto:

```css
/* Global Styles */

* {
    font-family: 'Roboto', Arial, sans-serif;
    color: #616161;
    box-sizing: border-box;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }
  
  body {
    margin: 0;
  }
  
  .container {
    display: flex;
    flex-direction: row;
  }
  
  router-outlet + *  {
    padding: 0 16px;
  }
  
  /* Text */
  
  h1 {
    font-size: 32px;
  }
  
  h2 {
    font-size: 20px;
  }
  
  h1, h2 {
    font-weight: lighter;
  }
  
  p {
    font-size: 14px;
  }
  
  /* Hyperlink */
  
  a {
    cursor: pointer;
    color: #1976d2;
    text-decoration: none;
  }
  
  a:hover {
    opacity: 0.8;
  }
  
  /* Input */
  
  input {
    font-size: 14px;
    border-radius: 2px;
    padding: 8px;
    margin-bottom: 16px;
    border: 1px solid #BDBDBD;
  }
  
  label {
    font-size: 12px;
    font-weight: bold;
    margin-bottom: 4px;
    display: block;
    text-transform: uppercase;
  }
  
  /* Button */
  .button, button {
    display: inline-flex;
    align-items: center;
    padding: 8px 16px;
    border-radius: 2px;
    font-size: 14px;
    cursor: pointer;
    background-color: #1976d2;
    color: white;
    border: none;
  }
  
  .button:hover, button:hover {
    opacity: 0.8;
    font-weight: normal;
  }
  
  .button:disabled, button:disabled {
    opacity: 0.5;
    cursor: auto;
  }
  
  /* Fancy Button */
  
  .fancy-button {
    background-color: white;
    color: #1976d2;
  }
  
  .fancy-button i.material-icons {
    color: #1976d2;
    padding-right: 4px;
  }
  
  /* Top Bar */
  
  app-top-bar {
    width: 100%;
    height: 68px;
    background-color: #1976d2;
    padding: 16px;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
  }
  
  app-top-bar h1 {
    color: white;
    margin: 0;
  }
  
  /* Checkout Cart, Shipping Prices */
  
  .cart-item, .shipping-item {
    width: 100%;
    min-width: 400px;
    max-width: 450px;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    padding: 16px 32px;
    margin-bottom: 8px;
    border-radius: 2px;
    background-color: #EEEEEE;
  }
```

4. También puedes usar el siguiente enlace en el `index.html` para tener iconos y fuentes de google

```html
<link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet" />
```

5. Genera un componente para la cabecera de la aplicación. Llámalo `barra-superior`
6. Genera un componente para la lista de productos. Llámalo `lista-productos`
7. Conecta ambos componentes al componente principal `app` haciendo las importaciones correspondientes
8. Ahora intenta mostrar la lista de productos en la aplicación usando el componente `lista-productos`. (pista: Para usar directivas estructurales es necesario importar `CommonModule`)
9. En la lista de productos haz que se muestre la descripción **solo si existe esta última** (Puedes usar la directiva de Angular correspondiente)
10. Intenta añadir un enlace a cada producto de modo que cuando te poses sobre cualquiera de ellos aparezca una descripción del tipo “Detalles de...” y el nombre del producto
11. Añade a cada producto un botón “Compartir”, de tal modo que cuando hagamos clic en él, se muestre el mensaje “El producto se ha compartido” a través de una función Alert() de JS

Ahora vamos a crear una alerta que tome un producto de la lista como entrada. La alerta tiene que chequear el precio, de modo que si el precio es mayor de 700 presente un botón con el texto “Avísame...”. Este botón tiene que permitir a los usuarios registrarse para ser notificados de la salida a la venta  del producto en cuestión.

12. Primero vamos a crear un componente nuevo. Lo llamaremos `alerta-producto`
13. El componente ha de recibir un producto como input. Añade el decorador `@Input` a la clase para conseguirlo (recuerda las importaciones...)
14. En la plantilla HTML de este nuevo componente añade el botón “Avisame...”, pero utiliza la directiva correspondiente para ocultarlo o no según el precio del producto.
15. Ahora conecta y añade el punto de entrada del componente `alerta-producto` en el componente `lista-productos` para que pueda verse.
16. Haz que el botón “Avísame...” funcione. Tendrás que llevar a cabo dos pasos:
    1. Usa un decorador @Output y la función `EventEmitter()` como ya lo hemos hecho en otros proyectos
    2. Implementa el evento clic correspondiente en el botón
17. Por último añade el método onAviso() en el componente padre `lista-productos`

