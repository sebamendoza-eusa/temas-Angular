# Observables en Angular

## Programación reactiva vs. programación tradicional

Como ya se ha hablado en alguna ocasión durante el curso, en la *programación tradicional* las instrucciones se ejecutan una detrás de otra. Por tanto, si realizamos un cálculo con dos variables y obtenemos un resultado, aunque las variables usadas para hacer el cálculo cambien en el futuro, el cálculo ya se realizó y por tanto el resultado no cambiará.

```javascript
let a = 1;
let b = 3;
let resultado = a + b; // resultado vale 4
// Más tarde en las instrucciones... 
a = 7; // Asignamos otro valor a la variable a
// Aunque se cambie el valor de "a", resultado sigue valiendo 4,
```

El anterior código ilustra el modo de trabajo de la programación tradicional **y la principal diferencia con respecto a la programación reactiva**. Aunque pueda parecer magia, **en programación reactiva la variable resultado habría actualizado su valor al alterarse las variables con las que se realizó el cálculo**.

### Programación reactiva y los flujos de datos

Para facilitar el cambio de comportamiento entre la programación tradicional y la programación reactiva, en ésta última se usan intensivamente los flujos de datos. **La programación reactiva es la programación con flujos de datos asíncronos**.

En programación reactiva se pueden crear flujos (streams) a partir de cualquier cosa, como podría ser los valores que una variable tome a lo largo del tiempo. Todo puede ser un flujo de datos, como los clics sobre un botón, cambios en una estructura de datos, una consulta para traer un JSON del servidor, un feed RSS, el listado de tuits de las personas a las que sigues, etc.

En la programación reactiva se tienen muy en cuenta esos flujos de datos, creando sistemas que son capaces de consumirlos de distintos modos, fijándose en lo que realmente les importa de estos streams y desechando lo que no. Para ello se dispone de diversas herramientas que permiten filtrar los streams, combinarlos, crear unos streams a partir de otros, etc. En última instancia, la programación reactiva se ocupa de lanzar diversos tipos de eventos sobre los flujos:

- La aparición de algo interesante dentro de ese flujo
- La aparición de un error en el stream
- La finalización del stream

Como programadores, mediante código, podemos especificar qué es lo que debe ocurrir cuando cualquiera de esos eventos se produzca.

> Puedes obtener una introducción mucho más detallada en el artículo [The introduction to Reactive Programming you've been missing](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754).

## Observables y programación reactiva

El patrón *observable* no es más que **un modo de implementación de la programación reactiva**, que básicamente pone en funcionamiento a diversos actores para producir los efectos deseados.

Los componentes principales de este patrón son:

- **Observable**: Es aquello que queremos observar, que será implementado mediante una colección de eventos o valores futuros. Un observable puede ser creado a partir de eventos de usuario derivados del uso de un formulario, una llamada HTTP, un almacén de datos, etc. Mediante el observable nos podemos suscribir a eventos que nos permiten hacer cosas cuando cambia lo que se esté observando.
- **Observador**: Es el actor que se dedica a observar. Básicamente se implementa mediante una colección de funciones callback que nos permiten escuchar los eventos o valores emitidos por un observable. Las callbacks permitirán especificar código a ejecutar frente a un dato en el flujo, un error o el final del flujo.
- **Sujeto**: es el emisor de eventos, que es capaz de crear el flujo de eventos cuando el observable sufre cambios. Esos eventos serán los que se consuman en los observadores.

Existen diversas librerías para implementar programación reactiva que hacen uso del patrón observable. Una de ellas es **RxJS**, que es la que se usa en Angular.

En conclusión: En Angular, **los observables proporcionan un modo más para pasar mensajes entre partes de la aplicación**. Se utilizan con frecuencia y son la técnica recomendada para el manejo de eventos, la programación asíncrona y el manejo de múltiples valores.

## La librería RxJS

***Reactive Extensions (Rx)*** es una librería desarrollada por Microsoft para implementar programación reactiva, creando aplicaciones que son capaces de usar el patrón observable para gestionar operaciones asíncronas. Por su parte RxJS es la implementación en JavaScript de *Reactive Extensions*.

RxJS nos ofrece una base de código JavaScript muy interesante para programación reactiva, no solo para producir y consumir streams, sino también para manipularlos. Como es JavaScript la puedes usar en cualquier proyecto en este lenguaje, tanto del lado del cliente como del lado del servidor.

Angular se apoya en RxJS para implementar la programación reactiva, permitiendo mejorar sensiblemente el desempeño de las aplicaciones que realicemos con este framework.

## Promesas y Observables.

Un patrón de diseño que recuerda al uso de observables es el de manejo de *promesas*, que ya vimos en JS. Ambos patrones tienen similitudes y diferencias que nos ayudan a entender mejor la naturaleza de los observables. En cuanto a sus similitudes podemos mencionar las siguientes:

- **Manejo de operaciones asíncronas**: Promesas y observables son utilizados para manejar operaciones asíncronas en Angular, permitiendo que el código no se bloquee mientras espera la finalización de una operación.
- **Manejo de errores**: Tanto las promesas como los observables proporcionan mecanismos para manejar errores.

Pero entre ambos patrones también encontramos diferencias importantes:

- Mientras las **promesas** trabajan con un único valor que será resuelto o rechazado, los **observables** pueden representar una secuencia de valores que se emiten a lo largo del tiempo, permitiendo la transmisión de múltiples valores.
- Mientras que las **promesas** no se puede cancelar, los **observables** sí que son cancelables. Esto significa que se puede cancelar una suscripción antes de que se complete la operación asíncrona.
- Las **promesas** son más simples y adecuadas para operaciones asíncronas únicas, mientras que los **observables** son más potentes y flexibles, especialmente cuando se trata de manejar secuencias de eventos a lo largo del tiempo. Son preferidos en situaciones más complejas y en el desarrollo de aplicaciones reactivas.

## Uso y términos básicos

Cuando sea necesario, se puede crear una instancia de la clase `Observable` que defina una función *de suscriptor*. Esta función se ejecutará cuando un consumidor llame al método `subscribe()`. La función de suscriptor define cómo obtener o generar valores o mensajes que se van a publicar.

Para ejecutar el observable que ha creado y comenzar a recibir notificaciones, ha de invocarse el método `subscribe()`, pasando un *observador*. Se trata de un objeto JavaScript que define cómo va a manejar las notificaciones que reciba. La llamada a `subscribe()` devuelve un objeto `Subscription`, que a su vez tiene un método `unsubscribe()`, al que se llama para dejar de recibir notificaciones.

A continuación, se muestra un ejemplo de código que muestra cómo implementar un *observable* muy sencillo:

```ts
import { Observable } from 'rxjs';

// Crear una función que devuelve un observable
const miObservable = new Observable((observer) => {
  // Lógica para notificar eventos al observador
  observer.next('Hola');   // Emitir un valor
  setTimeout(() => {
    observer.next('Mundo');
  }, 2000); // Emitir otro valor después de un tiempo
  observer.complete(); // Indicar que el observable ha terminado

  // Manejar un error (opcional)
  // observer.error('Ocurrió un error');
});
```

## Definición de observadores

El controlador que recibe las notificaciones del *obeservable* implementa la interfaz `Observer`. Es un objeto que define métodos de devolución de llamada para controlar los tres tipos de notificaciones que puede enviar un observable. Se detallan en la siguiente tabla:

| TIPO DE NOTIFICACIÓN | DESCRIPCIÓN                                                  |
| :------------------- | :----------------------------------------------------------- |
| `next`               | Obligatorio. Un controlador para cada valor entregado. Se llama cero o más veces después de que se inicie la ejecución. |
| `error`              | Opcional. Controlador de una notificación de error. Un error detiene la ejecución de la instancia observable. |
| `complete`           | Opcional. Un controlador para la notificación de ejecución completa. Los valores retrasados se pueden seguir entregando al siguiente controlador una vez completada la ejecución. |

Un objeto observador puede definir cualquier combinación de estos controladores. Si no se proporcionara un controlador para un tipo de estos tipos de notificación en concreto, el observador omitiría las notificaciones de ese tipo.

## Suscribirse a un observable

Una objeto del tipo `Observable` comienza a publicar valores solo cuando algo se suscribe a él. La suscripción se realiza llamando al método `subscribe()` de la instancia y pasando un objeto observador para recibir las notificaciones.

> Para mostrar cómo funciona la suscripción, necesitamos crear un nuevo observable. Hay un constructor que se usa para crear nuevas instancias, pero a modo de ilustración, podemos usar algunos métodos de la biblioteca RxJS que crean observables simples de tipos de uso frecuente:
>
> - `of(...items)`: devuelve un objeto `Observable` que entrega de forma sincrónica los valores proporcionados como argumentos.
> - ``from(iterable)`: convierte su argumento en un objeto `Observable`. Este método se usa comúnmente para convertir una matriz en un observable.

A continuación, volvemos a mostrar otro ejemplo de creación de un observable, pero en esta ocasión añadimos la suscripción al mismo:

```ts
// Creamos un observable que emite tres valores
const myObservable = of(1, 2, 3);

// Creamos el objeto observador
const myObserver = {
  next: x => console.log('Observer got a next value: ' + x),
  error: err => console.error('Observer got an error: ' + err),
  complete: () => console.log('Observer got a complete notification'),
};

// Execute with the observer object
myObservable.subscribe(myObserver);
// Logs:
// Observer got a next value: 1
// Observer got a next value: 2
// Observer got a next value: 3
// Observer got a complete notification
```

Como alternativa, el método `subscribe()` puede aceptar definiciones de funciones callback el línea, para los manejadores `next`, `error` y `complete`. Por ejemplo, la siguiente llamada es la misma que la que especifica el observador predefinido:

```ts
myObservable.subscribe(
  x => console.log('Observer got a next value: ' + x),
  err => console.error('Observer got an error: ' + err),
  () => console.log('Observer got a complete notification')
);
```

En cualquier caso, el manejador `next` es obligatorio. Los manejadores `error` y `complete` son sin embargo opcionales.

Hay que tener en cuenta que una función `next()` puede recibir, por ejemplo, cadenas de mensajes u objetos de evento, valores numéricos o estructuras, según el contexto. Como término general, nos referimos a los datos publicados por un observable como un ***flujo***. Cualquier tipo de valor se puede representar con un observable, publicándose los los valores en forma de secuencia.

## Creación de observables

Puede usarse el constructor de la clase `Observable` para crear una secuencia observable de cualquier tipo. El constructor toma como argumento que la función de suscriptor y se ejecutará cuando se ejecute el método`subscribe()` del observable. Una función de suscriptor recibe un objeto `Observer` y puede publicar valores en el método `next()` del observador.

Por ejemplo, para crear un observable equivalente al del ejemplo anterior mediante el constructor de clase tendríamos que hacer lo siguiente:

```ts
// This function runs when subscribe() is called
function sequenceSubscriber(observer) {
  // synchronously deliver 1, 2, and 3, then complete
  observer.next(1);
  observer.next(2);
  observer.next(3);
  observer.complete();

  // unsubscribe function doesn't need to do anything in this
  // because values are delivered synchronously
  return {unsubscribe() {}};
}

// Create a new Observable that will deliver the above sequence
const sequence = new Observable(sequenceSubscriber);

// execute the Observable and print the result of each notification
sequence.subscribe({
  next(num) { console.log(num); },
  complete() { console.log('Finished sequence'); }
});

// Logs:
// 1
// 2
// 3
// Finished sequence
```

Para llevar este ejemplo un poco más lejos, podemos crear un observable que publique eventos. En este ejemplo, la función de suscriptor se define en línea. Veamos cómo:

```ts
function fromEvent(target, eventName) {
  return new Observable((observer) => {
    const handler = (e) => observer.next(e);

    // Add the event handler to the target
    target.addEventListener(eventName, handler);

    return () => {
      // Detach the event handler from the target
      target.removeEventListener(eventName, handler);
    };
  });
}
```

Ahora se podría usar esta función para crear, por ejemplo, un observable que publique eventos `keydown`:

```ts
const ESC_KEY = 27;
const nameInput = document.getElementById('name') as HTMLInputElement;

const subscription = fromEvent(nameInput, 'keydown')
  .subscribe((e: KeyboardEvent) => {
    if (e.keyCode === ESC_KEY) {
      nameInput.value = '';
    }
  });
```

## Manejo de errores

Dado que los observables producen valores de forma asincrónica, la estructura habitual `try/catch` no detectará errores de forma eficaz. En su lugar, los errores se controlan especificando una función callback de `error` en el observador. La producción de un error también hace que el observable anule sus suscripciones y deje de producir valores. Un observable puede producir valores (llamando a la función callback `next`) o puede completarse, llamando a las funciones `complete` o `error`

```ts
Observable.subscribe({
  next(num) { console.log('Next num: ' + num)},
  error(err) { console.log('Received an error: ' + err)}
});
```
