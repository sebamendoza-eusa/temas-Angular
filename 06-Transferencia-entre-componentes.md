# Transferencia de información entre componentes

Hemos visto anteriormente como pueden comunicarse un componente y el DOM. De lo que se trata ahora es de entender cómo se puede pasar información entre componentes distintos. En angular todo componente puede tener descendientes o ascendientes, por lo cual existirán distintos modos de transferir información entre ellos.

## Pasar datos de un componente padre a un componente descendiente

La forma más sencilla de pasar los datos a un componente descendiente es hacerlo en el momento que introduzcamos el selector del mismo en la plantilla del componente padre. Por ejemplo:

```html
<app-dado valor="3"></app-dado>
```

Cuando declaramos la etiqueta `app-dado` definimos una propiedad llamada `valor` y le pasamos el dato a dicho componente. Es idéntico a lo que hacemos a cuando definimos etiquetas HTML con sus propiedades.

Para declarar que una propiedad procede de un componente padre tendremos que usar el decorador `@input` en la clase del componente hijo. La manera es la siguiente:

```ts
@Input() valor: string="";
```

Aquí estamos diciendo que la propiedad `valor` procede desde un componente padre. Así, aunque inicialicemos la propiedad con un `string` vacío,  posteriormente podrá cargarse un valor determinado cuando creemos un objeto de esta clase. 

Para definir el decorador` @Input()` **debemos importar la clase `Input` previamente**.

## Disparar eventos desde un componente hijo a un componente padre

En Angular también existe la posibilidad de implementar la otra dirección en la comunicación entre componentes: desde los hijos hacia los padres.

- **El componente hijo** será el encargado de escalar el evento hacia el padre, para avisarle de un suceso. Al avisarle, el hijo podrá comunicar un dato que el padre deba conocer, relacionado lógicamente con ese suceso.
- **El componente padre** será capaz de capturar el evento personalizado emitido por el hijo y recuperar aquel dato que fue enviado.

El trabajo empieza desde el componente hijo. Éste comunicará el suceso **mediante la emisión de un evento personalizado**. Así, un componente padre puede capturar un evento que le envía un componente hijo.

La clase de Angular que implementa objetos capaces de emitir un evento se llama `EventEmitter`. Pertenece al "core" de Angular, por lo que necesitamos asegurarnos de importarla debidamente. Para poder emitir eventos personalizados necesitaremos crear una propiedad en el componente, donde instanciar un objeto de esta clase. Una particularidad es que la clase `EventEmitter` hace uso de los *generics* de TypeScript, lo que quiere decir que el tipo del dato que nuestro evento personalizado escalará hacia el padre en su comunicación.

Para completar el paso de información de hijo a padre tendremos que usar el decorador `@Output()`. Para usar ese decorador tenemos que importarlo también. Igual que `EventEmitter`, la declaración de la función decoradora "Output" está dentro de "@angular/core"

Al final un ejemplo de este tipo de código sería el siguiente:

```ts
@Output() emisor = new EventEmitter<string>();
```

En este caso el atributo `emisor` se declara como *emisor* a través de la clase `EventEmitter`. Lógicamente, el tipo del dato que enviemos hacia el padre debe concordar con el que hayamos declarado en el genérico al instanciar la el objeto `EventEmitter`.

```ts
this.emisor.emit('Este dato viajará hacia el padre');
```

Para fijar ideas supongamos el siguiente supuesto: Desde el componente hijo queremos enviar el contenido de un cuadro de texto al componente padre. Entonces, en la plantilla del componente descendiente tendríamos el siguiente código:

```html
<p>
  <input type="text" [(ngModel)]="mensaje">
  <button (click)="onPropagar()">Enviar</button>
</p>
```

Cuando hagamos clic en el botón el contenido del cuadro input se guardará en la propiedad mensaje. Si en la clase del componente descendiente tenemos lo siguiente:

```javascript
export class PropagadorComponent {
  mensaje: string;

  @Output() propagar = new EventEmitter<string>();

  onPropagar() {
    this.emisor.emit(this.mensaje);
  }
}
```

Conseguiríamos tener el enlace con el componente padre a través de la propiedad `propagar` definida como un emisor de la propiedad `mensaje`.

Hasta aquí el trabajo que hay que realizar con el componente hijo. Vamos a ver **cómo hay que proceder con el componente padre**. Esta es una operativa bastante sencilla, ya que los eventos personalizados se reciben igual que los eventos que genera el navegador.

Al usar el componente hijo debemos especificar el correspondiente manejador de evento en la plantilla del padre. El nombre del evento personalizado será el mismo con el que se definió anteriormente la propiedad emisora de evento en el descendiente. Veamos como se hace: Desde la plantilla del componente padre tendríamos

```html
<dw-propagador
  (emisor)="procesaEmisor($event)"
></dw-propagador>
```

y finalmente en la clase del componente padre:

```ts
procesaEmisor(mensaje) {
  console.log(mensaje);
}
```

Con ello conseguiríamos que el mensaje de consola que presenta el componente padre provenga de una propiedad del componente descendiente.

## Llamar a métodos del componente hijo desde la plantilla del padre

Otra forma de comunicarnos del componente padre al componente hijo es la posibilidad de llamar a métodos de este último definiendo una variable en la correspondiente plantilla HTML del componente padre. Lo podríamos hacer así

```html
<app-dado #dado1></app-dado>
<button (click)="dado1.tirar()">Tirar</button>
```

Para definir una variable local le antecedemos el carácter `#` al nombre. Luego podemos llamar a métodos indicando el nombre de la variable y el método a llamar. Lo que debe quedar claro que debemos definir una variable en la etiqueta del selector del componente hijo, para a partir de ahí  llamar a métodos o inclusive acceder a propiedades de dicho componente.

En el ejemplo vemos como a través de la variable local `#dado1` podemos acceder a métodos del componente hijo (*app-dado*) desde la plantilla del componente padre.

Para que todo lo anterior funcione también será necesario **importar el componente hijo en el componente padre**.

