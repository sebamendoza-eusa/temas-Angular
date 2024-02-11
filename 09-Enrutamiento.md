# Enrutamiento (*routers*)

El sistema de rutas es una parte fundamental en cualquier aplicación de Angular. Permite la navegación entre diferentes vistas y componentes, lo que es crucial para crear una experiencia de usuario fluida y amigable.

El enrutamiento en Angular es la **capacidad de segmentar o dividir nuestra aplicación en múltiples vistas**. La renderización de estas vistas se vinculará a una URL asignada a cada una de ellas. Según la URL en donde nos encontremos, o donde nos redireccione la aplicación, se renderizará el componente enlazado a esta ruta.

El enrutamiento no es obligatorio en Angular, pero es la manera más óptima que tenemos de dividir nuestras vistas y nuestro trabajo en componentes, segmentarlo y poder renderizar el contenido según lo que le queramos mostrar al usuario.

**Además, es posible enviar y recibir valores entre rutas según la necesidad y el comportamiento deseado dentro de la aplicación.**

También en una aplicación de una sola página (SPA), en lugar de ir al servidor para obtener una nueva página, se puede cambiar la vista del usuario mostrando u ocultando partes de la pantalla que corresponden a componentes concretos.

Para manejar la navegación entre vistas se utiliza **la API Angular `Router`**. Con ello se habilita la navegación interpretando una URL del explorador como una instrucción para cambiar la vista. Cuando creamos la aplicación con Angular CLI se nos generan los archivos básicos para trabajar con rutas.

## Enrutamiento básico

Para **definir una ruta básica** es necesario primeramente comprobar que en el archivo `app.config.ts` está definida la función `provideRoute` en la sección `providers`. Nos quedaría un fichero con esta forma más o menos:

```ts
import { ApplicationConfig } from '@angular/core';
import { provideRouter } from '@angular/router';
import { routes } from './app.routes';

export const appConfig: ApplicationConfig = {
  providers: [provideRouter(routes)]
};
```

A partir de aquí tendremos que hacer tres cosas:

1. Definir un array de rutas
2. Definir las distintas rutas dentro del array
3. Añadir las rutas en la aplicación

Los dos primeros puntos pueden verse en el siguiente código, perteneciente al archivo `app.routes.ts`:

```ts
import { Routes } from '@angular/router';
const routes: Routes = [
  { path: 'first-component', component: FirstComponent },
  { path: 'second-component', component: SecondComponent },
];
```

Cada ruta en el array es un objeto JS que contiene dos propiedades. La primera propiedad, `path`, define la URL de la ruta. La segunda propiedad, `component`, define el componente que Angular usará en la correspondiente ruta.

### Agregar una ruta predeterminada

Cuando se inicia la aplicación, la barra de direcciones del navegador apunta a la raíz del sitio web. Normalmente, eso no coincide con ninguna ruta existente, por lo que el enrutador no navegaría ninguna parte.

Para que la aplicación navegue a un componente por defecto de manera automática, habría que agregar la siguiente ruta al array de rutas en el fichero `app.routes.ts`:

```ts
import { Routes } from '@angular/router';
const routes: Routes = [
  { path: 'first-component', component: FirstComponent },
  { path: 'second-component', component: SecondComponent },
  { path: '', redirectTo: 'first-component', pathMatch: 'full' },
];
```

### Añadiendo rutas a la aplicación

Antes de añadir los enlaces dentro de la aplicación hay que insertar la directiva `RouterOutlet`. Este selector le dice al enrutador dónde tiene que colocar las vistas enrutadas. Colocaremos el selector en la plantilla HTML del componente en el que elijamos presentar las distintas vistas enrutadas. A partir de aquí pasaremos a conectar dichas vistas a partir de las rutas URL y no a través de los selectores de los distintos componentes asociados.

Además de lo anterior hay que tener en cuenta que es necesario importar los módulos relacionados con el enrutamiento en el componente desde que se añadirán los enlaces. Estos módulos serán normalmente:

- **RouterOutlet**: Módulo que permite usar el selector <router-outlet> en la vista HTML para indicar a Angular donde deben colocarse las vistas enlazadas
- **RouterLink**: Módulo que nos permite insertar los enlaces a otras vistas desde nuestra vista actual. El `routerLink` es el selector para la directiva `RouterLink`, que convierte los clics del usuario en navegaciones del enrutador.
- **RouterLinkActive**: Permite usar la directiva `routerLinkActive`, que rastrea qué enlace está activo actualmente y que permite aplicar CSS concreto para distinguirlo de otros enlaces. El nombre de la clase se proporciona en la propia directiva.

Ahora sí, para añadir las rutas a la aplicación, en primer lugar, hay que agregar los enlaces a las vistas que deseemos. Agrega la ruta al atributo `routerLink` y añade la directiva `routerLinkActive` si lo ves oportuno.

Veamos todo en acción en el siguiente código:

```html
<h1>Angular Router App</h1>
<nav>
  <ul>
    <li><a routerLink="/first-component" routerLinkActive="active">First Component</a></li>
    <li><a routerLink="/second-component" routerLinkActive="active">Second Component</a></li>
  </ul>
</nav>
<!-- The routed views render in the <router-outlet>-->
<router-outlet></router-outlet>
```

Recordemos que por supuesto será necesario importar en el archivo de componente `Routerlink`, `RuterLinkActive` y `RouterOutlet`

> Puedes obtener más información acerca de tareas comunes de enrutamiento aquí: https://angular.dev/guide/routing/common-router-tasks

## Obtener información sobre una ruta y paso de parámetros

Existe una situación muy típica que se da en aplicaciones web que manejan estructura maestro-detalle, que es la **generación de rutas que aceptan parámetros**.

Una cuestión importante radica, además de crear los enlaces que envían los parámetros, **en tratar de recibir esos parámetros en el componente de destino**. Además de ello, hay que saber detectar de modo asíncrono cuando los parámetros cambian.

El primer paso será declarar en nuestro listado de rutas la posibilidad de recibir parámetros. Para ello usaremos la siguiente notación:

```ts
{ path: 'first-component/:parametro', component: FirstComponent },
```

Vemos como la ruta se compone de varios segmentos separados por `/`. Cuando en un segmento colocamos los dos puntos (`:`)  delante, estaremos indicando que estamos ante un parámetro y no ante una cadena estática. Hay que tener en cuenta que aunque podemos indicar cualquier número de parámetros en una declaración de ruta, **una ruta nunca puede comenzar por un parámetro**. Por ejemplo, la siguiente declaración sería perfectamente válida:

```ts
{ path: 'coches/:marca/:modelo', component: CochesComponent }
```

Algo a tener en cuenta es que **el número de parámetros definidos en las rutas debe ser exacto**. Es decir, una ruta no aceptará un número de parámetros que no esté definido convenientemente en el fichero de rutas.

### Generar enlaces a rutas enviando parámetros

El siguiente paso es generar enlaces a una ruta, enviando parámetros. Para ello sólo tendremos que indicar la ruta a la que enviaremos los valores que se deseen como parámetros.

```html
<a routerLink="first-component/parametro1">Enlace a vista</a>
```

Además de este modo, podemos crear las rutas mediante una notación de array, lo que tendrá mucha más utilidad cuando integremos propiedades dinámicas del componente en la ruta. Veamos el siguiente ejemplo:

```html
<a [routerLink]="['/first-component', param1 ]">Enlace a vista dinámica</a>
```

En este caso, por pasar un array como valor a la directiva *routerLink*, estamos obligados a colocar el nombre de la directiva entre corchetes, ya que el valor asignado no es un simple literal de cadena. Vemos como, cada segmento de la ruta corresponde con una casilla del array y `param1` sería una propiedad del componente y no una cadena de texto.

### Recibir valores de los parámetros

Una vez enviado el parámetro mediante la ruta toca recuperarlo en el componente de destino. Esta parte puede ser más o menos compleja según el caso y lo mejor es tratar cada uno por separado. Vamos a estudiar las dos alternativas en función de su dificultad. En cualquier caso, para recibir los valores de los parámetros en el componente al que nos dirige la ruta **tendremos que usar un objeto del sistema de enrutamiento llamado `ActivatedRoute`**. Este objeto nos ofrece diversos detalles sobre la ruta actual, entre ellos los parámetros que contiene.

Para usar este objeto tenemos que importarlo primero. Lo haremos en el componente donde vas a recibir los parámetros.

```javascript
import { ActivatedRoute, Params } from '@angular/router';
```

Luego tendrás que inyectar ese objeto en el constructor de la clase.

```ts
constructor(private rutaActiva: ActivatedRoute) { }
```

Una vez inyectado ya lo puedes usar dentro del componente, para obtener los datos que necesitas. Dada la declaración anterior, la propiedad del componente `rutaActiva` será la que contenga el objeto que vamos a necesitar para recuperar las rutas.

#### Primera opción: recibir valores de parámetros con el *snapshot*

La manera más sencilla de recuperar parámetros es a través del método *snapshot*. Una vez inyectado el objeto `ActivatedRoute` accederemos a los parámetros de la ruta a través del método `ngOnInit()` (que a su vez habrá que implementar en la declaración de clase). Bastaría con añadir este código

```javascript
this.rutaActiva.snapshot.params.<prop>
```

El snapshot proporciona los parámetros al componente en el instante que se consulten. El objeto `params` contendrá todas las propiedades según los parámetros recibidos.

Supongamos un ejemplo en el que estamos pasando la marca y el modelo de un coche a través de una ruta, desde la vista de un componente `listadoCoches` hacia un componente denominado `detalleCoche`. El código de la clase de este componente podría quedar así:

```ts
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute } from '@angular/router';

@Component({
  selector: 'app-detalle-coche',
  templateUrl: './detalle-coche.component.html',
  styleUrls: ['./detalle-coche.component.css']
})
export class DetalleCocheComponent implements OnInit {
  coche: {marca: string, modelo: string}; // declaración de interface inline
  constructor(private rutaActiva: ActivatedRoute) { }

  ngOnInit() {
    this.coche = {
      marca: this.rutaActiva.snapshot.params.marca,
      modelo: this.rutaActiva.snapshot.params.modelo
    };
  }

}
```

Vemos en acción todo lo explicado anteriormente.

- El import de `ActivatedRoute`
- La declaración de la propiedad coche del componente.
- La inyección del `ActivatedRoute` en el constructor
- La inicialización de la propiedad `coche` dentro de `ngOnInit()`
- El acceso al método `snapshot`  y la asignación de cada uno de los parámetros recibidos como propiedades del objeto coche.

A partir de aquí ya podemos usar la propiedad coche y sus atributos dentro de la plantilla HTML del componente como ya se sabe.

Hay un pequeño problema con el método anterior de aceptación de parámetros, y que el componente **podría no ser consciente de que los parámetros han cambiado en un momento dado**. Esto ocurre porque la inicialización de los parámetros la hemos colocado en el `ngOnInit()` , de modo que cuando el componente pasa su etapa de inicialización ya no se vuelven a recibir los parámetros con el *snapshot*.

Veamos como podemos resolver esta cuestión:

#### Segunda opción: recibir valores de parámetros con un observable

Para resolver el problema anterior usaremos los observables de Angular y RxJS. **Los observables nos permiten suscribirnos a eventos que se recibirán de manera asíncrona, mediante programación reactiva y manteniendo un alto rendimiento.**

Hemos visto que en el objeto de tipo `AtivatedRoute` tenemos una propiedad llamada `params` **que es un observable** y que nos sirve para suscribirnos a cambios en los parámetros enviados al componente.

Las suscripciones a los cambios en los observadores se realizan mediante el método `suscribe()`. Ese método recibe varios parámetros y el que nos interesa de momento es solo el primero, que consiste en una función callback que se ejecutará con cada cambio de aquello que se está observando.

Es decir: `this.rutaActiva.params` **es el observable** y `this.rutaActiva.params.subscribe()` **es el método para suscribirnos a los cambios**. A dicho método enviaremos una función callback, la cual recibirá el nuevo valor, y que se ejecutará cada vez que cambie.

La función callback recibirá un objeto de clase `Params`. Ese objeto también hay que importarlo desde `@angular/route`.

```javascript
import { ActivatedRoute, Params } from '@angular/router';

// El código de la suscripción es el siguiente.

this.rutaActiva.params.subscribe(
    (params: Params) => {
      this.coche.modelo = params.modelo;
      this.coche.marca = params.marca;
    }
  );
```

Veamos ahora cómo quedaría el código de nuestro componente. En este caso el se nos complica algo más, pero si entendemos bien los observables en Angular no tenemos por qué tener mayores problemas.

```ts
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute, Params } from '@angular/router';

@Component({
  selector: 'app-detalle-coche',
  templateUrl: './detalle-coche.component.html',
  styleUrls: ['./detalle-coche.component.css']
})
export class DetalleCocheComponent implements OnInit {
  coche: {marca: string, modelo: string}; // declaración de interface inline
  constructor(private rutaActiva: ActivatedRoute) { }
  
  ngOnInit() {
    this.coche = {
      marca: this.rutaActiva.snapshot.params.marca,
      modelo: this.rutaActiva.snapshot.params.modelo
    };
    this.rutaActiva.params.subscribe(
      (params: Params) => {
        this.coche.modelo = params.modelo;
        this.coche.marca = params.marca;
      }
    );
  }

}
```

