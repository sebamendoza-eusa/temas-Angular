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

Para establecer un enlace de propiedad situamos entre corchetes la propiedad del selector HTML en cuestión y la igualamos a la propiedad o método de la clase del componente correspondiente. El enlace se establecerá de forma dinámica. Por ejemplo:

```html
<input type="text" [value]="nombre">
```

mostrará el valor de la propiedad `nombre` de la clase del componente en el contenido del elemento de texto del formulario. Si el atributo cambia, también lo hará el texto del cuadro del formulario.

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

![angular 2 component data binding](H:\Mi unidad\Classroom\DAW-EC_PRE\apuntes\Angular\images\_MUVVuYjPS_01JoHwNvGKkctpOF3mbfiWx4yqGMKlkXPMl0dCx8ez9nbb72MeDeqnZZ-O8AxYw=s400)

![angular 2 parent-child data binding](H:\Mi unidad\Classroom\DAW-EC_PRE\apuntes\Angular\images\uyVMPR1P3Uo9Ptf5rU99uJv1Suwbjbm0DlQtAbH0bnZgnQRZn6NOKss-YCixCZEMejQ3G2UcIw=s400)



## Enlace bidireccional

El enlace bidireccional también posibilita a la aplicación una forma de compartir datos entre una clase de componente y la plantilla, pero con un matiz: Primero establece una propiedad de elemento específica y después escucha un evento de cambio de elemento.

La sintaxis de un enlace bidireccional es `[( ... )]` y combina los corchetes de enlace de propiedad, `[]`, con el paréntesis de vinculación de eventos, `()`.

Un caso de uso habitual del enlace bidireccional es el la directiva `NgModel`. Esta directiva permite mostrar un atributo y actualizarlo cuando el usuario hace determinados cambios. Por ejemplo:

```html
<input [(ngModel)]="todo.subject">
```

En este caso, el valor de la propiedad fluye a la caja de *input* como en el caso *property binding*, pero los cambios del usuario también fluyen de vuelta al componente, actualizando el valor de dicha propiedad. Este concepto puede verse más detalladamente en el apartado de formularios con Angular

## Enlace de atributo

en Angular, a menudo usamos la palabra "atributo" como sinónimo de "propiedad". En muchos casos podría valer meter todo en el mismo saco, pero lo cierto es que no siempre es lo mismo y Angular no trata a ambos de la misma manera. En el contexto de HTML entendemos atributo como los modificadores que acompañan a etiquetas y normalmente, cuando se escribe código HTML, existe una correspondencia directa entre lo que sería un atributo de HTML y lo que sería una propiedad del DOM. Por ejemplo el atributo "id" del HTML corresponde directamente con la propiedad "id" del objeto del DOM y por lo general, lo normal es establecer una propiedad de elemento con un **enlace de propiedad**. 

Pero existen atributos que no tienen un mapeo directo en el DOM, como es el caso de *colspan*. También existen atributos de HTML que mapean a propiedades del DOM que no se llaman ni funcionan igual, como *class* y *classList*.

Es en estos casos donde se hace necesario usar el **enlace de atributo**. La sintaxis de enlace de atributo se parece al enlace de propiedad, pero en lugar de una propiedad de elemento entre paréntesis, comienza con el prefijo `attr`, seguido de un punto (`.`) y el nombre del atributo. Luego establecerá el valor del atributo, utilizando una expresión que se resuelve en un tipo `string`, o eliminará el atributo cuando la expresión se resuelva en `null`.

Un ejemplo interesante de uso son los denominados *data attribute*, los cuales no tienen mapeo a propiedades. En estos casos, si queremos enlazar a un *data attribute* del HTML5, tendremos que hacerlo con la sintaxis de un enlace de atributo. Mira el ejemplo siguiente:

```html
<div [attr.data-cliente]="cliente.nombre">{{cliente.nombre}}</div>
```

Aquí asignamos al atributo `data-cliente` el valor de la propiedad *nombre* del objeto *cliente* definido en el componente correspondiente.



