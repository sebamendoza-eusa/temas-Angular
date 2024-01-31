# Enlace de datos

El enlace de datos (*data binding*) en Angular es un proceso que permite a las aplicaciones mostrar valores de datos a un usuario y responder a sus acciones (como clics, toques y pulsaciones de teclas).

En un enlace de datos, se declara **la relación entre un widget HTML y una fuente de datos** y Angular hace el resto.

Existen varios tipos de enlace en que se pueden integrar en la sintaxis de plantilla de Angular. Veamos cuáles son:

## Interpolación

Ya hemos visto algo sobre esta forma de enlace de datos en los primeros apartados. La interpolación permite incorporar expresiones evaluadas de JS entre etiquetas de elementos HTML y dentro de las asignaciones de atributos. Para evaluar las expresiones, estas deben aparecer entre dobles llaves `{{ ... }}`.  Cualquier texto entre dobles llaves en una plantilla HTML de Angular es una expresión que primero se evalúa y después se transforma en una cadena de texto. Por ejemplo:

```html
<p>El empleado dentro de 5 años tendrá:{{edad+5}}</p>
```

También cabe la posibilidad de utilizar la interpolación como valor en propiedades de elementos HTML. Por ejemplo:

```html
<p>Puede visitar el sitio ingresando <a href="{{sitio}}">aquí</a></p>
```

## Enlace de propiedad (*property binding*)

El *property binding* es una herramienta poderosa en Angular que nos permite **mantener sincronizadas las propiedades de un componente con las propiedades de elementos del DOM**, facilitando así la creación de aplicaciones dinámicas e interactivas.

Para establecer un enlace de propiedad situamos entre corchetes la propiedad del selector HTML en cuestión y la igualamos al atributo de la clase del componente que deseemos. El enlace se establecerá de forma dinámica. Por ejemplo:

```html
<input type="text" [value]="nombre">
```

mostrará el valor del atributo `nombre` de la clase del componente en el contenido del elemento de texto del formulario. Si el atributo cambia, también lo hará el texto del cuadro del formulario.

El enlace de propiedad tiene una similitud muy grande con la interpolación y ciertamente pueden usarse indistintamente. Cuando se quiere representar valores de datos como cadenas, no hay razón técnica para preferir una forma a la otra, aunque la legibilidad tiende a favorecer la interpolación. Sin embargo, **cuando queramos establecer un elemento a un valor de datos que no sea de cadena, debe usarse el *property binding***.

## Enlace de evento

Otra actividad muy común en una aplicación es la captura de eventos. La presión de un botón, la presión de una tecla, el desplazamiento de la flecha del mouse etc. son eventos que podemos capturar. Vimos en el proyecto de ejemplo como hemos de proceder para capturar los eventos:

```html
<button (click)="incrementaContador()">+1</button>
<button (click)="decrementaContador()">-1</button>
```

Vemos que cuando se declara un enlace de evento es necesario tener asociado un método de la clase del componente.

En un enlace de evento, Angular configura un controlador de eventos para el evento de destino. Cuando se genera el evento, el controlador ejecuta la instrucción de la plantilla. La instrucción de plantilla suele implicar a un receptor, que realiza una acción en respuesta al evento. El enlace transmite información sobre el evento. Esta información puede incluir valores de datos como un objeto de evento `$event`, una cadena o un número.

Como en JS, un evento de destino determina las características del objeto `$event`. Si el evento de destino es un evento del propio DOM, entonces se producirá un [objeto de evento DOM](https://developer.mozilla.org/en-US/docs/Web/Events), con propiedades, por ejemplo, como `target` o `target.value`

## Enlace bidireccional

El enlace bidireccional también posibilita a la aplicación una forma de compartir datos entre una clase de componente y la plantilla, pero con un matiz: Primero establece una propiedad de elemento específica y después escucha un evento de cambio de elemento.

La sintaxis de un enlace bidireccional es `[( ... )]` y combina los corchetes de enlace de propiedad, `[]`, con el paréntesis de vinculación de eventos, `()`.

Un caso de uso habitual del enlace bidireccional es el la directiva `NgModel`. Esta directiva permite mostrar un atributo y actualizarlo cuando el usuario hace determinados cambios. Por ejemplo:

```html
<label for="example-ngModel">[(ngModel)]:</label>
<input [(ngModel)]="currentItem.name" id="example-ngModel">
```

Puede verse más profundamente este concepto en el apartado de formularios con Angular

## Enlace de atributo

Establece el valor de un atributo directamente con un **enlace de atributo**. Por lo general, lo normal es establecer una propiedad de elemento con un **enlace de propiedad**. Sin embargo, a veces no hay ninguna propiedad de elemento para vincular, por lo que se acude a este tipo de enlace

La sintaxis de enlace de atributo se parece al enlace de propiedad, pero en lugar de una propiedad de elemento entre paréntesis, comienza con el prefijo `attr`, seguido de un punto (`.`) y el nombre del atributo. Luego establecerá el valor del atributo, utilizando una expresión que se resuelve en un tipo `string`, o eliminará el atributo cuando la expresión se resuelva en `null`.

Uno de los casos de uso principales para el enlace de atributos es establecer atributos ARIA, como en este ejemplo:

```html
<!-- create and set an aria attribute for assistive technology -->
<button [attr.aria-label]="actionName">{{actionName}} with Aria</button>
```
