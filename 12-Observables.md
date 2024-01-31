# Observables en Angular

En Angular, los observables proporcionan un modo más para pasar mensajes entre partes de la aplicación. Se utilizan con frecuencia y son la técnica recomendada para el manejo de eventos, la programación asíncrona y el manejo de múltiples valores.

El patrón de observador **es un patrón de diseño de software** en el que un objeto, llamado *sujeto*, mantiene una lista de sus objetos dependientes, llamados *observadores*, y les notifica automáticamente los cambios de estado.

Los **observables son declarativos**, es decir, se define una función para publicar valores, pero no se ejecuta hasta que un consumidor se suscribe a ella. A continuación, el consumidor suscrito recibe notificaciones hasta que se completa la función o hasta que cancela la suscripción.

Un observable puede entregar varios valores de cualquier tipo: literales, mensajes o eventos, según el contexto. La API para recibir valores es la misma si los valores se entregan de forma sincrónica o asincrónica. Dado que la lógica de configuración y desmontaje se controlan mediante el observable, el código de la aplicación solo tiene que preocuparse por suscribirse para consumir valores y, cuando haya terminado, cancelar la suscripción. Ya sea que la transmisión haya sido pulsaciones de teclas, una respuesta HTTP o un temporizador de intervalos, la interfaz para escuchar valores y dejar de escuchar es la misma.

Debido a estas ventajas, los observables se utilizan ampliamente dentro de Angular y también se recomiendan para el desarrollo de aplicaciones en general.

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

A continuación, se muestra un ejemplo de código que muestra cómo implementar un *observable* para proporcionar actualizaciones de geolocalización.

```ts
// Create an Observable that will start listening to geolocation updates
// when a consumer subscribes.
const locations = new Observable((observer) => {
  let watchId: number;

  // Simple geolocation API check provides values to publish
  if ('geolocation' in navigator) {
    watchId = navigator.geolocation.watchPosition((position: Position) => {
      observer.next(position);
    }, (error: PositionError) => {
      observer.error(error);
    });
  } else {
    observer.error('Geolocation not available');
  }

  // When the consumer unsubscribes, clean up data ready for next subscription.
  return {
    unsubscribe() {
      navigator.geolocation.clearWatch(watchId);
    }
  };
});

// Call subscribe() to start listening for updates.
const locationsSubscription = locations.subscribe({
  next(position) {
    console.log('Current Position: ', position);
  },
  error(msg) {
    console.log('Error Getting Location: ', msg);
  }
});

// Stop listening for location after 10 seconds
setTimeout(() => {
  locationsSubscription.unsubscribe();
}, 10000);
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

Una objeto del tipo `Observable` comienza a publicar valores solo cuando algo se suscribe a ella. La suscripción se realiza llamando al método `subscribe()` de la instancia y pasando un objeto observador para recibir las notificaciones.

> Para mostrar cómo funciona la suscripción, necesitamos crear un nuevo observable. Hay un constructor que se usa para crear nuevas instancias, pero a modo de ilustración, podemos usar algunos métodos de la biblioteca RxJS que crean observables simples de tipos de uso frecuente:
>
> - `of(...items)`: devuelve un objeto `Observable` que entrega de forma sincrónica los valores proporcionados como argumentos.
> - ``from(iterable)`: convierte su argumento en un objeto `Observable`. Este método se usa comúnmente para convertir una matriz en un observable.

A continuación, se muestra un ejemplo de creación y suscripción a un observable simple, con un observador que registra el mensaje recibido en la consola:

```ts
// Create simple observable that emits three values
const myObservable = of(1, 2, 3);

// Create observer object
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
