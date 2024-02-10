# Antes de empezar...

Al igual que la mayoría de de frameworks actuales, para trabajar con Angular es importante conocer bien HTML, CSS y JavaScript. Además de esto es preciso que estar familiarizados con el trabajo con clases en JS y entender los fundamentos de TypeScript.

Angular aporta una herramienta muy interesante: la **Command Line Interface (CLI)**, que ayuda al desarrollo de los componentes y realiza funciones de compilación. Vamos a mencionar algunos conceptos importantes de Angular que iremos desarrollando más adelante

## Componentes

Los componentes son el bloque fundamental de Angular para el desarrollo de aplicaciones. Al aprovechar una arquitectura de componentes, Angular busca proporcionar una estructura para organizar el proyecto en partes manejables y organizadas, cada una con sus responsabilidades claras. Con ello conseguimos un código que sea mantenible y escalable.

Los **componentes** son bloques de código formados por fragmentos de HTML y CSS en combinación con uno o varios ficheros TypeScript, usualmente en forma de contenedores de clases. La idea es que los componentes sean bloques pequeños y simples en cuanto a código, de tal modo que la aplicación sea una suma de componentes. Es muy importante entender que un componente es **un elemento reutilizable**. 

Un componente angular **se puede identificar por el sufijo**:`.component.ts` y en el diferenciaremos dos partes:

- Un **decorador** para definir las opciones de configuración para los distintos aspectos del componente, como por ejemplo el selector que define cuál es el nombre de la etiqueta cuando se hace referencia a un componente desde una plantilla o la propia plantilla HTML que controla lo que se representa en el explorador

  > Un concepto íntimamente asociado a los componentes es el de ***decorador***. Un decorador es una implementación de un patrón de diseño de software que permite **extender una función dentro de otra función**, sin modificar la original de la que se está extendiendo. En términos simples, **un decorador nos permite *decorar* una función a través de unos metadatos con los que se informa sobre la función y sus comportamientos.**

- Una **clase de TypeScript** que define el comportamiento del componente. Por ejemplo: el control de la entrada del usuario, la gestión del estado, o la definición de métodos entre otros

Por ejemplo, un componente muy básico que implemente una lista de tareas podría tener esta forma:

```ts
@Component({
  standalone: true,
  imports: [CommonModule, RouterOutlet],
  selector: 'todo-list-item',
  template: ` <li>(TODO) Aprender Angular</li> `,
  styles: ['li { color: blue; }'],
})
export class TodoListItem {
  /* El comportamiento del componente se define aquí */
}
```

### Estado del componente

Podemos definir los datos que desea que administre el componente declarándolos como propiedades de la clase (https://www.typescriptlang.org/docs/handbook/2/classes.html#fields).

Por ejemplo. En un componente *TodoList* podríamos definir un par de propiedades referentes a las tareas a completar

```ts
@Component({ ... })
export class TodoList {
  taskTitle: string = '';
  isComplete: boolean = false;
}
```

### Métodos

Se pueden definir funciones para un componente declarando métodos dentro de la clase de componente. Siguiendo con el ejemplo anterior:

```ts
// todo-list-item.component.ts

@Component({ ... })
export class TodoList {
  taskTitle = '';
  isComplete = false;

  updateTitle(newTitle: string) {
    this.taskTitle = newTitle;
  }

  completeTask() {
    this.isComplete = true;
  }
}
```

## Plantillas

Cada componente tiene una plantilla HTML que define lo que ese componente representa en el DOM.

Las plantillas HTML se pueden definir como una plantilla en línea dentro de la clase TypeScript o en archivos independientes con la propiedad `templateUrl`. Para obtener más información, puedes consultar [los documentos sobre la definición de plantillas de componentes](https://angular.io/guide/component-overview#defining-a-components-template).

### Representación de datos dinámicos

Cuando se necesita mostrar contenido dinámico en la plantilla, entre otros, **Angular utiliza la sintaxis de doble llave para distinguir entre contenido estático y dinámico**.

```ts
@Component({
  template: ` <p>Title: {{ taskTitle }}</p> `,
})
export class TodoListItem {
  taskTitle = 'Aprender Angular';
}
```

Esta sintaxis declara una **interpolación** entre la propiedad de datos dinámicos dentro del HTML. Como resultado, cada vez que los datos cambien, Angular actualizará automáticamente el DOM reflejando el nuevo valor de la propiedad.

### Propiedades y atributos dinámicos

Cuando se necesita **establecer dinámicamente el valor de los atributos en un elemento HTML**, la propiedad de destino se ajusta entre corchetes. Esto vincula el atributo con los datos dinámicos deseados informando a Angular que el valor declarado debe interpretarse como una declaración similar a JavaScript ([con algunas mejoras de Angular](https://angular.io/guide/understanding-template-expr-overview)) en lugar de una cadena simple.

```html
<button [disabled]="hasPendingChanges"></button>
```

En este ejemplo, la propiedad *disabled* está vinculada a la variable que Angular esperaría encontrar dentro del estado del componente `hasPendingChanges`.

### Control de eventos

Se pueden enlazar manejadores de eventos especificando el nombre del evento entre paréntesis e invocando un método de la clase en el lado derecho del signo igual:

```html
<button (click)="saveChanges()">Save Changes</button>
```

En ocasiones es interesante pasar el objeto asociado al evento como parámetro del método que se dispara. Angular proporciona una variable implícita que se puede usar dentro de la llamada de función que se denomina `$event`

```html
<button (click)="saveChanges($event)">Save Changes</button>
```

## Estilos

Cuando se necesita aplicar estilo a un componente, hay dos propiedades opcionales que se pueden configurar dentro del decorador.`@Component`

De forma similar a las plantillas de componentes, puede administrar los estilos de un componente en el mismo archivo que la clase TypeScript o en archivos independientes con las propiedades.`styleUrls`

Opcionalmente, los componentes pueden incluir una lista de estilos CSS que se aplican al DOM de ese componente:

```ts
content_copy@Component({
  selector: 'profile-pic',
  template: `<img src="profile-photo.jpg" alt="Your profile photo" />`,
  styles: [
    `
      img {
        border-radius: 50%;
      }
    `,
  ],
})
export class ProfilePic {
  /* Your code goes here */
}
```

**De forma predeterminada, el estilo de un componente solo se aplicará a los elementos de la plantilla de ese componente para limitar los efectos secundarios.**

Para obtener más información, se pueden consultar [los documentos sobre el estilo de los componentes](https://angular.io/guide/component-styles).
