# Pipes

Las *pipes* también llamadas **tuberías o filtros** son funciones que se llaman en una vista (HTML) y que tienen por objetivo transformar un dato a mostrar para mejorar la experiencia del usuario.

Angular trae en el *core* una cantidad reducida de *pipes* de las que podemos hacer uso en cualquier vista de nuestros componentes. Pero lo más interesante es que **podemos crear nuestras propias pipes**.

La llamada a estas funciones tiene una sintaxis muy distinta a la tradicional y su objetivo es mejorar la legibilidad del código

Por ejemplo para mostrar el contenido de una variable toda en mayúsculas en la plantilla usaríamos la siguiente sintaxis:

```html
<p>Nombre del cliente: {{ nombre | uppercase }}</p>
```

Considerando que en el componente hayamos definido la propiedad:

```ts
nombre = 'Juan Carlos';
```

La salida en el navegador será:

```
Nombre del cliente: JUAN CARLOS
```

Como vemos en la interpolación de la propiedad 'nombre' llamamos a la *pipe* después del carácter `|`.

En algunas *pipes* podemos pasar parámetros:

```
<p>El saldo es:{{ saldo | currency:'$'}}</p>
```

Pasamos el parámetro seguido del nombre de la *pipe*, disponiéndolo antecedido por el caracter `:`.

Podemos aplicar una *pipe* directamente a un valor indicado en la vista:

```
<p>{{ 'Hola' | uppercase }}</p>
```

Como se trata de un `string` debemos indicarlo entre comillas. Luego cuando ejecutamos en el navegador obtendremos como resultado 'HOLA'.

## Crear *pipes* personalizadas

Para marcar una clase como *pipe*,  y proporcionar metadatos de configuración se debe aplicar el decorador `@Pipe` a la clase correspondiente. Para el nombre de la clase de la tubería es mejor usar la convención *UpperCamelCase*. Para el nombre en la plantilla usaremos el convenio *camelCase*.

Por ejemplo, mira este código, esta sería la forma de declarar una *pipe* correctamente

```ts
import { Pipe } from '@angular/core';
@Pipe({
  name: 'greet',
  standalone: true,
})
export class GreetPipe {}
```

Para conseguir la transformación deseada con la *pipe*, es necesario implementar la interface `PipeTransform` en la clase. Posteriormente Angular invocará el método  `transform`  con el valor de un enlace como primer argumento, y el resto de enlaces como argumentos, todo ello en forma de lista. Finalmente retornará el valor transformado. Veamos como funciona esto en el siguiente ejemplo:

```ts
import { Pipe, PipeTransform } from '@angular/core';
@Pipe({
  name: 'greet',
  standalone: true,
})
export class GreetPipe implements PipeTransform {
  transform(value: string, param1: boolean, param2: boolean): string {
    return `Hello ${value}`;
  }
}
```
