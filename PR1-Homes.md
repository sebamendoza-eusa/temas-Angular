# PR1. *Homes*

#### Desarrollar una aplicación básica que gestiona inmuebles y permite el contacto de usuarios para ampliar información. La aplicación implementa filtros y una estructura listado-detalle

##### Indicaciones:

Se trataría de una aplicación que gestionaría el filtrado de inmuebles de una inmobiliaria. Este ejemplo está basado en el tutorial de iniciación de angular que puedes encontrar aquí (https://angular.dev/tutorials/first-app/). 

Empezaríamos creando un nuevo proyecto como ya se ha explicado. En la raíz de la carpeta `src` encontramos el fichero `index.html` y `styles.css` (el punto de entrada de la aplicación), a los que añadiríamos estos contenidos

```html
<!-- app.component.html -->

<!doctype html>
<html lang="en">
    <head>
      <meta charset="utf-8">
      <title>Homes</title>
      <base href="/">
      <meta name="viewport" content="width=device-width, initial-scale=1">
      <link rel="icon" type="image/x-icon" href="favicon.ico">
      <link href="https://fonts.googleapis.com/css2?family=Be+Vietnam+Pro:ital,wght@0,400;0,700;1,400;1,700&display=swap" rel="stylesheet">
    </head>
        <body>
          <app-root></app-root>
        </body>
</html>
```

Vemos el punto de inserción del selector `<app-root>` del componente raíz

```css
/* app.component.css */

* {
  margin: 0;
  padding: 0;
  }

body {
  font-family: 'Be Vietnam Pro', sans-serif;
}
:root {
  --primary-color: #605DC8;
  --secondary-color: #8B89E6;
  --accent-color: #e8e7fa;
  --shadow-color: #E8E8E8;
}

button.primary {
  padding: 10px;
  border: solid 1px var(--primary-color);
  background: var(--primary-color);
  color: white;
  border-radius: 8px;
}
```

Descarga los ficheros de este enlace (https://github.com/eusa-daw-ec/angular-101/tree/main/first-app/src/assets) en la carpeta `assets` de `src`. Harán falta en algún momento del ejercicio.

Con todo lo anterior podríamos probar lanzar la aplicación con `ng serve` para ver que efectivamente todo funciona.

### Primer componente: El componente `home`

Recordemos que en Angular, la clase de cada componente se acompaña de un **decorador** en el que se definen los metadatos que lo configuran. En este caso los metadatos a tener en cuenta son:

- `selector`: la referencia del componente en la plantilla HTML
- `standalone`: Para configurar el componente sin necesidad de la pertenencia a un módulo
- `imports`: Array en el que figuran los componentes que necesita para poder funcionar
- `template`: Enlace a la URL del fichero de plantilla HTML del componente
- `styleUrls`: Array que contiene los enlaces a las URL de los ficheros de hoja de estilo que se aplicarán al componente

> Puedes tener más información al respecto del decorador `@Component()` aquí: https://angular.dev/api/core/Component

Para crear un componente nuevo, desde la carpeta del proyecto, **en la consola,** podemos usar Angular CLI

```powershell
ng generate component home // Se puede abreviar como ng g c home
```

Se pueden ver los archivos correspondientes en la carpeta `src/app/home`. De estos archivos tendremos que configurar correctamente la plantilla HTML (*home.component.html*) añadiendo el siguiente código:

```html
<section>
    <form>
    	<input type="text" placeholder="Filtrar por localidad" />
        <button class="primary" type="button">Buscar</button>
	</form>
</section>
```

Con ello añadimos un formulario para el filtro. La hoja de estilos puede completarse añadiendo el siguiente código:

```css
.results {
  display: grid;
  column-gap: 14px;
  row-gap: 14px;
  grid-template-columns: repeat(auto-fill, minmax(400px, 400px));
  margin-top: 50px;
  justify-content: space-around;
}
input[type="text"] {
  border: solid 1px var(--primary-color);
  padding: 10px;
  border-radius: 8px;
  margin-right: 4px;
  display: inline-block;
  width: 30%;
}
button {
  padding: 10px;
  border: solid 1px var(--primary-color);
  background: var(--primary-color);
  color: white;
  border-radius: 8px;
}
@media (min-width: 500px) and (max-width: 768px) {
  .results {
      grid-template-columns: repeat(2, 1fr);
  }
  input[type="text"] {
      width: 70%;
  }   
}
@media (max-width: 499px) {
  .results {
      grid-template-columns: 1fr;
  }    
}
```

Por último tenemos que comprobar que todo esté bien en el archivo TypeScript del componente `home`. Hay que repasar los metadatos del decorador y las importaciones necesarias. Una vez hecho esto podemos seguir con el siguiente paso, que es conectar este componente con el componente raíz de la aplicación (*app.component*).

Para ello tendremos que hacer dos cosas desde el *app.component*

- Importar el componente `home` en *app.component.ts*, en la sección correspondiente del decorador y de la cabecera
- Introducir el selector en la plantilla HTML en el fichero *app.component.html*

Esto último lo podemos hacer con el siguiente código:

```html
<!-- app.component.html -->

<main>
    <header class="brand-name">
        <img class="brand-logo" src="/assets/logo.svg" alt="logo" aria-hidden="true" />
    </header>
	<section class="content">
    	<app-home></app-home>
	</section>
</main>
```

### Segundo componente: El componente `housing-location`

Vamos ahora con un segundo componente que integraremos con el componente `home` desarrollado en el apartado anterior.

De nuevo nos ayudaremos de Angular CLI, y desde la consola haremos

```powershell
ng generate component housing-location
```

Podemos comprobar en el navegador (http://localhost:4200) que todo está bien.

Se trata ahora de conectar este componente con el componente `home`. Para hacerlo operaríamos de forma parecida a como lo hicimos en el apartado anterior:

- Desde el fichero *home.component.ts* importaríamos el *HousingLocationComponent*
- Para insertar el componente en la plantilla HTML sustituiríamos el código en el fichero *home.component.html* por este:

```html
<section>
    <form>
        <input type="text" placeholder="Filter by city" />
        <button class="primary" type="button">Search</button>
	</form>
</section>
<section class="results">
    <app-housing-location></app-housing-location>
</section>
```

Continuamos ahora terminando con el componente `housing-location`. Primero añadiendo código a su hoja de estilos:

```css
/* housing-location.component.css */

.listing {
  background: var(--accent-color);
  border-radius: 30px;
  padding-bottom: 30px;
}
.listing-heading {
  color: var(--primary-color);
  padding: 10px 20px 0 20px;
}

.listing-photo {
  height: 250px;
  width: 100%;
  object-fit: cover;
  border-radius: 30px 30px 0 0;
}

.listing-location {
  padding: 10px 20px 20px 20px;
}

.listing-location::before {
  content: url("/assets/location-pin.svg") / "";
}

section.listing a {
  padding-left: 20px;
  text-decoration: none;
  color: var(--primary-color);
}

section.listing a::after {
  content: "\203A";
  margin-left: 5px;
}
```

y segundo, añadiendo código HTML a su plantilla. Pero antes de esto vamos a centrarnos antes en otro aspecto de nuestra aplicación.

### Creando una interface para los datos de los inmuebles

Recordemos que los componentes de Angular se escriben TypeScript. Una posibilidad que nos brindaba TS es la creación de tipos customizados a partir de la definición de ***interfaces***. En esta aplicación vamos a trabajar con objetos (inmuebles) que, de alguna manera, vamos a tener que manipular.

> Puedes recordar lo que vimos acerca de las interfaces en TS aquí: https://www.typescriptlang.org/docs/handbook/interfaces.html

Para crear una nueva interface en Angular también podemos usar Angular CLI. Bastaría con hacer desde la consola, en la ubicación de nuestro proyecto lo siguiente:

```powershell
ng generate interface housinglocation
```

La interface se colocará en la ubicación `src/app` en forma de fichero .ts

Podemos acceder al fichero y añadir el siguiente código:

```ts
export interface HousingLocation {
  id: number;
  nombre: string;
  localidad: string;
  provincia: string;
  foto: string;
  udsDisponibles: number;
  wifi: boolean;
  terraza: boolean;
}
```

Con esto ya tendríamos definido un nuevo tipo disponible en la aplicación: el tipo `HousingLocation`, que podremos usar convenientemente.

Para poner en práctica el nuevo tipo creado vamos a definir un inmueble de prueba. Para ello definiremos un objeto literal como valor de una propiedad `housingLocation` en la clase `HomeComponent` del fichero *home.component.ts*. Añadimos a la clase el siguiente código:

```ts
@Component({
	// ...
})
export class HomeComponent {
    
  readonly baseUrl = 'https://angular.dev/assets/tutorials/common'; // ubicación remota de la foto del inmueble
    
  housingLocation: HousingLocation = {
    id: 9999,
    nombre: 'Test Home',
    localidad: 'Test city',
    provincia: 'ST',
    foto: `${this.baseUrl}/example-house.jpg`,
    udsDisponibles: 99,
    wifi: true,
    terraza: false,
  };
}
```

Es importante tener en cuenta que para que se reconozca el tipo `HousingLocation`, hay que importar la interface en la cabecera del fichero:

```javascript
import {HousingLocation} from '../housinglocation';
```

### Compartir datos entre componentes: el decorador @input()

El decorador @input() permite compartir datos entre un componente padre y sus componentes hijos. En nuestro caso vamos a definir las propiedades de este decorador en el componente `housingLocation` para que nos permita definir las características de los datos renderizados por el componente.

> Puedes acceder a más información sobre el decorador @input() aquí: https://angular.dev/api/core/Input y aquí: https://angular.dev/guide/components/inputs

Para proceder a usar @input() tendremos que importar la clase Input en el componente `housing-location`. También importaremos el interface `HousingLocation`. El código de la clase quedará como sigue:

```ts
import {Component, Input} from '@angular/core';
import {CommonModule} from '@angular/common';
import {HousingLocation} from '../housinglocation';

// ...

export class HousingLocationComponent {
  @Input() housingLocation!: HousingLocation;
}
```

Con esto estamos diciendo a la clase que puede aceptar datos de un selector HTML denominado `housingLocation`. Con el modificador `!` indicamos al compilador de TS que el valor de la propiedad no puede ser nulo o indefinido.

### Enlazar propiedades entre distintos componentes

De nuevo volvemos al componente `home` para insertar el selector de la propiedad que va a recoger el valor de la propiedad `housingLocation` de la clase `HousingLocation` . Para ello sustituiremos el código de la plantilla HTML por el siguiente:

```ts
<!-- home.component.html -->

<section>
    <form>
    	<input type="text" placeholder="Filtrar por localidad" />
        <button class="primary" type="button">Buscar</button>
	</form>
</section>
<section class="results">
    <app-housing-location [housingLocation]="housingLocation"></app-housing-location>
</section>
```

Vemos como añadimos el parámetro `[housingLocation]="housingLocation"` en el selector del componente `housingLocation`.

Usamos la sintaxis `[atributo] = "valor"` para avisar a Angular que el valor asignado debe tratarse como una propiedad de la clase del componente y no como un valor `string`, consiguiendo **el enlazado con la propiedad** en cuestión (*binding property*)

### Interpolación de las propiedades del componente en la plantilla HTML

Veamos ahora cómo es posible renderizar propiedades de una clase en una plantilla HTML. Esto se consigue mediante un mecanismo que denominados **interpolación**.

> Puedes encontrar más información sobre interpolación de propiedades aquí: https://angular.dev/guide/templates/interpolation

Para interpolar las propiedades de un inmueble en la plantilla correspondiente tendremos que añadir en el fichero *housing-location.component.html* el siguiente código:

```ts
<!-- housing-location.component.html -->

<section class="listing">
	<img
        class="listing-photo"
        [src]="housingLocation.photo"
        alt="Exterior photo of {{ housingLocation.name }}"
        crossorigin
      />
      <h2 class="listing-heading">{{ housingLocation.name }}</h2>
      <p class="listing-location">{{ housingLocation.city }}, {{ housingLocation.state }}</p>
</section>
```

Vemos como para interpolar propiedades usamos las dobles llaves `{{ prop }}`

### Directiva ngFor

Como ya se ha comentado antes, en Angular, `ngFor` es un tipo específico de **directiva** que usada en una plantilla puede repetir datos concretos. Es similar a un bucle `for` en JS. Podemos usar `ngFor` para iterar sobre listas e incluso sobre valores asíncronos.

> Puedes acceder a una explicación más detallada sobre directivas aquí: https://angular.dev/guide/directives#ngFor

Para ver en acción la directiva `ngFor` vamos a cambiar el objeto que definía un inmueble individual y era asignado a la propiedad `housingLocation` en la clase `HomeLocation` por un array de objetos. Añadimos el siguiente código en el fichero *home.component.ts*:

```javascript
export class HomeComponent {
  readonly baseUrl = 'https://angular.dev/assets/tutorials/common';
  housingLocationList: HousingLocation[] = [
    {
      id: 0,
      nombre: 'Acme Fresh Start Housing',
      localidad: 'Chicago',
      provincia: 'IL',
      foto: `${this.baseUrl}/bernard-hermant-CLKGGwIBTaY-unsplash.jpg`,
      udsDisponibles: 4,
      wifi: true,
      terraza: true,
    },
    {
      id: 1,
      nombre: 'A113 Transitional Housing',
      localidad: 'Santa Monica',
      provincia: 'CA',
      foto: `${this.baseUrl}/brandon-griggs-wR11KBaB86U-unsplash.jpg`,
      udsDisponibles: 0,
      wifi: false,
      terraza: true,
    },
    {
      id: 2,
      nombre: 'Warm Beds Housing Support',
      localidad: 'Juneau',
      provincia: 'AK',
      foto: `${this.baseUrl}/i-do-nothing-but-love-lAyXdl1-Wmc-unsplash.jpg`,
      udsDisponibles: 1,
      wifi: false,
      terraza: false,
    },
    {
      id: 3,
      nombre: 'Homesteady Housing',
      localidad: 'Chicago',
      provincia: 'IL',
      foto: `${this.baseUrl}/ian-macdonald-W8z6aiwfi1E-unsplash.jpg`,
      udsDisponibles: 1,
      wifi: true,
      terraza: false,
    },
    {
      id: 4,
      nombre: 'Happy Homes Group',
      localidad: 'Gary',
      provincia: 'IN',
      foto: `${this.baseUrl}/krzysztof-hepner-978RAXoXnH4-unsplash.jpg`,
      udsDisponibles: 1,
      wifi: true,
      terraza: false,
    },
    {
      id: 5,
      nombre: 'Hopeful Apartment Group',
      localidad: 'Oakland',
      provincia: 'CA',
      foto: `${this.baseUrl}/r-architecture-JvQ0Q5IkeMM-unsplash.jpg`,
      udsDisponibles: 2,
      wifi: true,
      terraza: true,
    },
    {
      id: 6,
      nombre: 'Seriously Safe Towns',
      localidad: 'Oakland',
      provincia: 'CA',
      foto: `${this.baseUrl}/phil-hearing-IYfp2Ixe9nM-unsplash.jpg`,
      udsDisponibles: 5,
      wifi: true,
      terraza: true,
    },
    {
      id: 7,
      nombre: 'Hopeful Housing Solutions',
      localidad: 'Oakland',
      provincia: 'CA',
      foto: `${this.baseUrl}/r-architecture-GGupkreKwxA-unsplash.jpg`,
      udsDisponibles: 2,
      wifi: true,
      terraza: true,
    },
    {
      id: 8,
      nombre: 'Seriously Safe Towns',
      localidad: 'Oakland',
      provincia: 'CA',
      foto: `${this.baseUrl}/saru-robert-9rP3mxf8qWI-unsplash.jpg`,
      udsDisponibles: 10,
      wifi: false,
      terraza: false,
    },
    {
      id: 9,
      nombre: 'Capital Safe Towns',
      localidad: 'Portland',
      provincia: 'OR',
      foto: `${this.baseUrl}/webaliser-_TPTXZd9mOo-unsplash.jpg`,
      udsDisponibles: 6,
      wifi: true,
      terraza: true,
    },
  ];
}
```

Y ahora ya podemos cambiar la plantilla del componente `home` para añadir la directiva y renderizar todos los inmuebles

```html
<!-- home.component.html -->
<section>
      <form>
        <input type="text" placeholder="Filter by city" />
        <button class="primary" type="button">Search</button>
      </form>
</section>
<section class="results">
      <app-housing-location
        *ngFor="let housingLocation of housingLocationList"
        [housingLocation]="housingLocation"
      ></app-housing-location>
</section>
```

### Servicios en Angular

Los **servicios** en Angular proporcionan una manera de separar, en una aplicación, datos y funciones que van a ser utilizados en múltiples componentes de la misma. Para que esto sea posible, se dice que un servicio debe ser *inyectable*. Los servicios que son inyectables y son usados por un componente **se convierten en dependencias** de ese componente. Quiere decir que el componente dependerá de esos servicios y no podrá funcionar sin ellos.

En este sentido, se entiende la ***inyección de dependencias*** como aquel mecanismo que gestiona las dependencias de los componentes de una aplicación y los servicios que los otros componentes pueden usar.

Así pues vamos a crear un servicio en nuestra aplicación. Escribiremos el siguiente código en consola:

```javascript
ng generate service housing --skip-tests
```

Añadiremos algunos datos al nuevo servicio creado, que encontraremos `/src/app` con el nombre *housing.service.ts*. Desplazaremos el array de datos del componente `home` hasta este fichero, de modo que nos quedaría que:

```ts
// housing.service.ts

import {Injectable} from '@angular/core';
import {HousingLocation} from './housinglocation';
@Injectable({
  providedIn: 'root',
})
export class HousingService {
  readonly baseUrl = 'https://angular.dev/assets/tutorials/common';
  protected housingLocationList: HousingLocation[] = [
    {
      id: 0,
      nombre: 'Acme Fresh Start Housing',
      localidad: 'Chicago',
      provincia: 'IL',
      foto: `${this.baseUrl}/bernard-hermant-CLKGGwIBTaY-unsplash.jpg`,
      udsDisponibles: 4,
      wifi: true,
      terraza: true,
    },
    {
      id: 1,
      nombre: 'A113 Transitional Housing',
      localidad: 'Santa Monica',
      provincia: 'CA',
      foto: `${this.baseUrl}/brandon-griggs-wR11KBaB86U-unsplash.jpg`,
      udsDisponibles: 0,
      wifi: false,
      terraza: true,
    },
    {
      id: 2,
      nombre: 'Warm Beds Housing Support',
      localidad: 'Juneau',
      provincia: 'AK',
      foto: `${this.baseUrl}/i-do-nothing-but-love-lAyXdl1-Wmc-unsplash.jpg`,
      udsDisponibles: 1,
      wifi: false,
      terraza: false,
    },
    {
      id: 3,
      nombre: 'Homesteady Housing',
      localidad: 'Chicago',
      provincia: 'IL',
      foto: `${this.baseUrl}/ian-macdonald-W8z6aiwfi1E-unsplash.jpg`,
      udsDisponibles: 1,
      wifi: true,
      terraza: false,
    },
    {
      id: 4,
      nombre: 'Happy Homes Group',
      localidad: 'Gary',
      provincia: 'IN',
      foto: `${this.baseUrl}/krzysztof-hepner-978RAXoXnH4-unsplash.jpg`,
      udsDisponibles: 1,
      wifi: true,
      terraza: false,
    },
    {
      id: 5,
      nombre: 'Hopeful Apartment Group',
      localidad: 'Oakland',
      provincia: 'CA',
      foto: `${this.baseUrl}/r-architecture-JvQ0Q5IkeMM-unsplash.jpg`,
      udsDisponibles: 2,
      wifi: true,
      terraza: true,
    },
    {
      id: 6,
      nombre: 'Seriously Safe Towns',
      localidad: 'Oakland',
      provincia: 'CA',
      foto: `${this.baseUrl}/phil-hearing-IYfp2Ixe9nM-unsplash.jpg`,
      udsDisponibles: 5,
      wifi: true,
      terraza: true,
    },
    {
      id: 7,
      nombre: 'Hopeful Housing Solutions',
      localidad: 'Oakland',
      provincia: 'CA',
      foto: `${this.baseUrl}/r-architecture-GGupkreKwxA-unsplash.jpg`,
      udsDisponibles: 2,
      wifi: true,
      terraza: true,
    },
    {
      id: 8,
      nombre: 'Seriously Safe Towns',
      localidad: 'Oakland',
      provincia: 'CA',
      foto: `${this.baseUrl}/saru-robert-9rP3mxf8qWI-unsplash.jpg`,
      udsDisponibles: 10,
      wifi: false,
      terraza: false,
    },
    {
      id: 9,
      nombre: 'Capital Safe Towns',
      localidad: 'Portland',
      provincia: 'OR',
      foto: `${this.baseUrl}/webaliser-_TPTXZd9mOo-unsplash.jpg`,
      udsDisponibles: 6,
      wifi: true,
      terraza: true,
    },
  ];
  getAllHousingLocations(): HousingLocation[] {
    return this.housingLocationList;
  }
  getHousingLocationById(id: number): HousingLocation | undefined {
    return this.housingLocationList.find((housingLocation) => housingLocation.id === id);
  }
    
}
```

Añadimos también los dos métodos: `getAllHousingLocations()` y `getHousingLocationById` para recuperar la lista completa de inmuebles y un inmueble concreto perteneciente a la lista respectivamente.

Hemos importado, como se puede comprobar, la interface `housingLocation`

Por último tendremos que **inyectar** este nuevo servicio en el componente `home`, añadiendo la clase `inject` del core de Angular e importando el servicio explícitamente. La clase `HomeComponent` pasaría a tener este código:

```javascript
export class HomeComponent {
  housingLocationList: HousingLocation[] = [];
  housingService: HousingService = inject(HousingService);
    
  constructor() {
    this.housingLocationList = this.housingService.getAllHousingLocations();
  }
}
```

Vemos como el constructor de la clase hace uso del servicio inyectado

### Enrutamiento

El enrutamiento es la capacidad de navegar entre componentes de una aplicación Angular. En los casos de aplicaciones en una sola página (SPA), solo algunas partes de la página son actualizadas a petición del usuario.

El *router* de Angular permite así declarar ubicaciones en las que específicamente va a mostrarse un componente cuando esa ruta sea solicitada por la aplicación.

Es muy común usar enrutamiento en situaciones en las que la información de la aplicación se presenta en forma de listado-detalle

> Puedes encontrar más información sobre enrutamiento en Angular aquí: https://angular.dev/guide/routing

Para armar una estructura de vista listado-detalle en nuestra aplicación tendremos que generar primero un componente de detalle. Lo hacemos como se ya ha explicado:

```powershell
ng generate component details
```

Una vez creado este componente podemos centrarnos en el enrutado

Para ello tendremos que trabajar con el fichero `routes.ts`, situado en `/src/app`. También tendremos que activar el enrutamiento en el fichero `main.ts`, importando el módulo `provideRouter` de @angular/router y `routeConfig` del fichero `routes.ts`

Además tendremos que actualizar la llamada a `bootstrapApplication` con el siguiente código

```ts
// main.ts

// (...)

bootstrapApplication(AppComponent, {
  providers: [provideProtractorTestingSupport(), provideRouter(routeConfig)],
}).catch((err) => console.error(err));
```

El componente raíz (*app.component.ts*) también tendrá que ser actualizado para activar el enrutamiento. Para ello importaremos `RouterModule` desde @angular/router, y lo añadiremos a los datos del decorador @Component en la sección *imports*. El fichero quedaría así:

```ts
// app.component.ts

import {Component} from '@angular/core';
import {HomeComponent} from './home/home.component';
import {RouterLink, RouterOutlet} from '@angular/router';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [HomeComponent, RouterLink, RouterOutlet],
  template: './app.component.html',
  styleUrls: ['./app.component.css'],
})
export class AppComponent {
  title = 'homes';
}
```

Por último, actualizaremos la plantilla app.component.html, sustituyendo el código presente por este otro:

```html
<!-- app.component.html -->

<main>
    <a [routerLink]="['/']">
        <header class="brand-name">
            <img class="brand-logo" src="/assets/logo.svg" alt="logo" aria-hidden="true" />
        </header>
    </a>
    <section class="content">
        <router-outlet></router-outlet>
    </section>
</main>
```

#### Añadir una ruta al nuevo componente `details`

Para crear una nueva ruta para el componente que presenta los detalles tendremos que importar los componentes `home` y `details` en el fichero *routes.ts*, así como el interface `Routes` desde @angular/router

Ahora definimos una variable llamada `routeConfig` con el tipo `Routes` para definir las rutas en nuestra aplicación

```ts
// routes.ts

import {Routes} from '@angular/router';
import {HomeComponent} from './home/home.component';
import {DetailsComponent} from './details/details.component';

const routeConfig: Routes = [
  {
    path: '',
    component: HomeComponent,
    title: 'Inicio',
  },
  {
    path: 'details/:id',
    component: DetailsComponent,
    title: 'Detalles',
  },
];

export default routeConfig;
```

El objeto `routeConfig` maneja un parámetro `:id` que va a depender de la propiedad `id` del inmueble (tipo *HousingLocation*) en cuestión y que permitirá conectar el detalle con su lugar en el listado.

Para añadir un enlace desde el listado lo haremos desde la plantilla del componente `housing-location`, usando la directiva `routerLink`

```html
<!-- housing-location.component.html -->

<section class="listing">
      <img
        class="listing-photo"
        [src]="housingLocation.photo"
        alt="Exterior photo of {{ housingLocation.name }}"
        crossorigin
      />
      <h2 class="listing-heading">{{ housingLocation.name }}</h2>
      <p class="listing-location">{{ housingLocation.city }}, {{ housingLocation.state }}</p>
      <a [routerLink]="['/details', housingLocation.id]">Más detalles</a>
</section>
```

Ahora es el momento de pasar la ruta parametrizada al componente `details`. Primeramente tendremos que hacer la importaciones correspondientes, y editar la clase del componente y después retocar convenientemente la plantilla HTML

El fichero *details.component.ts* tendría que quedarnos de este modo:

```ts
// details.component.ts

import {Component, inject} from '@angular/core';
import {CommonModule} from '@angular/common';
import {ActivatedRoute} from '@angular/router';
import {HousingService} from '../housing.service';
import {HousingLocation} from '../housinglocation';

@Component({
  selector: 'app-details',
  standalone: true,
  imports: [CommonModule],
  template: './details.component.html',
  styleUrls: ['./details.component.css'],
})
export class DetailsComponent {
    route: ActivatedRoute = inject(ActivatedRoute);
    housingLocationId = -1;
    constructor() {
        this.housingLocationId = Number(this.route.snapshot.params['id']);
    }
}
```

De este modo, el componente mostrará la información correcta

Por su parte, el fichero de plantilla HTML quedaría más o menos así:

```html
<!-- details.component.html -->

<article>
      <img
        class="listing-photo"
        [src]="housingLocation?.photo"
        alt="imagen exterior de {{ housingLocation?.name }}"
        crossorigin
      />
   <section class="listing-description">
        <h2 class="listing-heading">{{ housingLocation?.name }}</h2>
        <p class="listing-location">{{ housingLocation?.city }}, {{ housingLocation?.state }}</p>
   </section>
   <section class="listing-features">
       <h2 class="section-heading">Características del inmueble:</h2>
       <ul>
           <li>Unidades disponibles: {{ housingLocation?.availableUnits }}</li>
           <li>¿Tiene red wifi?: {{ housingLocation?.wifi }}</li>
           <li>¿Existe terraza exterior?: {{ housingLocation?.laundry }}</li>
       </ul>
    </section>
</article>

```

Por último, se puede añadir una hoja de estilos propia al componente

```css
// details.component.css

.listing-photo {
  height: 600px;
  width: 50%;
  object-fit: cover;
  border-radius: 30px;
  float: right;
}
.listing-heading {
  font-size: 48pt;
  font-weight: bold;
  margin-bottom: 15px;
}
.listing-location::before {
  content: url('/assets/location-pin.svg') / '';
}
.listing-location {
  font-size: 24pt;
  margin-bottom: 15px;
}
.listing-features > .section-heading {
  color: var(--secondary-color);
  font-size: 24pt;
  margin-bottom: 15px;
}
.listing-features {
  margin-bottom: 20px;
}
.listing-features li {
  font-size: 14pt;
}
li {
  list-style-type: none;
}
.listing-apply .section-heading {
  font-size: 18pt;
  margin-bottom: 15px;
}
label, input {
  display: block;
}
label {
  color: var(--secondary-color);
  font-weight: bold;
  text-transform: uppercase;
  font-size: 12pt;
}
input {
  font-size: 16pt;
  margin-bottom: 15px;
  padding: 10px;
  width: 400px;
  border-top: none;
  border-right: none;
  border-left: none;
  border-bottom: solid .3px;
}
@media (max-width: 1024px) {
  .listing-photo {
    width: 100%;
    height: 400px;
  }
}
```

### Formularios en Angular

Vamos a ver ahora cómo añadir un formulario que recoja información del usuario que esté usando nuestra aplicación. En este caso, los datos recogidos serán enviados al servicio correspondiente de la aplicación, el cual los escribirá en la consola. Dejamos para más adelante el uso de API REST para enviar y recibir los datos de un formulario.

De lo primero que debemos ocuparnos es de añadir un **método** al servicio de la aplicación que, tras recibir los datos, los envíe a su destino. Para ello nos vamos al fichero *housing.service.ts* y añadimos el siguiente código tras los métodos ya declarados

```javascript
//  housing.service.ts

// (...)

submitApplication(firstName: string, lastName: string, email: string) {
    console.log(
      `Datos recibidos: Nombre: ${firstName}, Apellidos: ${lastName}, email: ${email}.`,
    );
  }
```

 En nuestro caso los datos son enviados a la consola de JS.

Ahora vamos a implementar un formulario que recoja los datos de una persona que esté interesado en un inmueble determinado. Para ello tendremos que hacer las importaciones correspondientes en el fichero *details.component.ts*

El componente nos quedaría así:

```ts
// details.component.ts

import {Component, inject} from '@angular/core';
import {CommonModule} from '@angular/common';
import {ActivatedRoute} from '@angular/router';
import {HousingService} from '../housing.service';
import {HousingLocation} from '../housinglocation';
import {FormControl, FormGroup, ReactiveFormsModule} from '@angular/forms';

@Component({
  selector: 'app-details',
  standalone: true,
  imports: [CommonModule, ReactiveFormsModule],
  template: './details.component.html',
  styleUrls: ['./details.component.css'],
})
export class DetailsComponent {
  route: ActivatedRoute = inject(ActivatedRoute);
  housingService = inject(HousingService);
  housingLocation: HousingLocation | undefined;
  
  constructor() {
    const housingLocationId = parseInt(this.route.snapshot.params['id'], 10);
    this.housingLocation = this.housingService.getHousingLocationById(housingLocationId);
  }
    
  applyForm = new FormGroup({
    firstName: new FormControl(''),
    lastName: new FormControl(''),
    email: new FormControl(''),
  });
 
  submitApplication() {
    this.housingService.submitApplication(
      this.applyForm.value.firstName ?? '',
      this.applyForm.value.lastName ?? '',
      this.applyForm.value.email ?? '',
    );
  }
}

```

En Angular, `FormGroup` y `FormControl` son tipos predefinidos que nos permiten construir formularios. El tipo `FormControl` nos provee un valor por defecto y tipear el dato del formulario . En nuestro caso, `firstName` es un `string` y el valor por defecto es una cadena vacía.

El método `submitApplication()` hace uso del servicio *housing.service.ts* para enviar la información a su destino.

Por último nos quedará añadir el formulario en sí a la plantilla del componente `details`. Lo hacemos reemplazando el código ya existente por el siguiente:

```html
<!-- details.component.html -->

<article>
    <img
        class="listing-photo"
        [src]="housingLocation?.photo"
        alt="Exterior photo of {{ housingLocation?.name }}"
        crossorigin
    />
	<section class="listing-description">
        <h2 class="listing-heading">{{ housingLocation?.name }}</h2>
        <p class="listing-location">{{ housingLocation?.city }}, {{ housingLocation?.state }}</p>
	</section>
	<section class="listing-features">
        <h2 class="section-heading">About this housing location</h2>
        <ul>
          <li>Units available: {{ housingLocation?.availableUnits }}</li>
          <li>Does this location have wifi: {{ housingLocation?.wifi }}</li>
          <li>Does this location have laundry: {{ housingLocation?.laundry }}</li>
        </ul>
    </section>
    
<!-- Código del formulario de contacto -->    
    
    <section class="listing-apply">
        <h2 class="section-heading">Apply now to live here</h2>
        <form [formGroup]="applyForm" (submit)="submitApplication()">
          <label for="first-name">Nombre</label>
          <input id="first-name" type="text" formControlName="firstName" />
          <label for="last-name">Apellidos</label>
          <input id="last-name" type="text" formControlName="lastName" />
          <label for="email">Email</label>
          <input id="email" type="email" formControlName="email" />
          <button type="submit" class="primary">Más información</button>
        </form>
    </section>
    
</article>
```

### Añadir la funcionalidad de búsqueda a nuestra aplicación

Queremos ahora que nuestra aplicación filtre los contenidos a través del formulario que ya se integró al principio del desarrollo.

Para ello tendremos que actualizar las propiedades de la clase del componente `home`. Añadimos una propiedad nueva: `filteredLocationList`, del tipo HousingLocation y que inicializaremos con un valor `[]`.

Esta propiedad recogerá los valores que coincidan con los criterios de búsqueda introducidos en el formulario. Inicialmente recogerá todo el listado de inmuebles existente en el array de objetos.

Ahora tendremos que añadir el método `filterResults()`,  que retendrá en el array `filteredLocationList` sólo los inmuebles que cumplan con el criterio de búsqueda de estar en la localidad elegida.

```javascript
  filterResults(text: string) {
    if (!text) {
      this.filteredLocationList = this.housingLocationList;
    }
    this.filteredLocationList = this.housingLocationList.filter((housingLocation) =>
      housingLocation?.city.toLowerCase().includes(text.toLowerCase()),
    );
  }
```

Por último, habremos de conectar el formulario en la plantilla del componente `home` para incluir una variable que recoja el contenido del input del formulario:

```html
<!-- home.component.html -->

<input type="text" placeholder="Filtrar por localidad" #filter>
```

Aquí estamos usando lo que en Angular se denomina *variable de referencia en plantilla*. Puedes encontrar más información aquí: https://angular.dev/guide/templatess

También tendremos que asociar el evento clic del botón del formulario al método correspondiente:

```html
<!-- home.component.html -->

<button class="primary" type="button" (click)="filterResults(filter.value)">Buscar</button>
```

Por último, tendremos que modificar la directiva `ngFor` para que itere sobre el array `filteredLocationList`

```html
<!-- home.component.html -->

<app-housing-location *ngFor="let housingLocation of filteredLocationList" [housingLocation]="housingLocation"></app-housing-location>
```

### Comunicación HTTP

El último apartado del proyecto será simular la lectura de datos desde un servidor externo. Nos conectaremos a un servidor mediante HTTP y obtendremos los datos en formato JSON.

#### Configuración del servidor JSON en Angular

Primero instalaremos un módulo de Node.JS: **json-server**. Lo haremos mediante npm

```powershell
npm install -g json-server
```

Ahora tendremos que crear en la raíz del proyecto un fichero `db.json`, que actuará como almacén de datos que usará el servidor JSON instalado. Ese archivo contendrá los datos de los inmuebles en formato JSON

```json
// db.json

{
         "locations": [
             {
                 "id": 0,
                 "name": "Acme Fresh Start Housing",
                 "city": "Chicago",
                 "state": "IL",
                 "photo": "${this.baseUrl}/bernard-hermant-CLKGGwIBTaY-unsplash.jpg",
                 "availableUnits": 4,
                 "wifi": true,
                 "laundry": true
             },
             {
                 "id": 1,
                 "name": "A113 Transitional Housing",
                 "city": "Santa Monica",
                 "state": "CA",
                 "photo": "${this.baseUrl}/brandon-griggs-wR11KBaB86U-unsplash.jpg",
                 "availableUnits": 0,
                 "wifi": false,
                 "laundry": true
             },
             {
                 "id": 2,
                 "name": "Warm Beds Housing Support",
                 "city": "Juneau",
                 "state": "AK",
                 "photo": "${this.baseUrl}/i-do-nothing-but-love-lAyXdl1-Wmc-unsplash.jpg",
                 "availableUnits": 1,
                 "wifi": false,
                 "laundry": false
             },
             {
                 "id": 3,
                 "name": "Homesteady Housing",
                 "city": "Chicago",
                 "state": "IL",
                 "photo": "${this.baseUrl}/ian-macdonald-W8z6aiwfi1E-unsplash.jpg",
                 "availableUnits": 1,
                 "wifi": true,
                 "laundry": false
             },
             {
                 "id": 4,
                 "name": "Happy Homes Group",
                 "city": "Gary",
                 "state": "IN",
                 "photo": "${this.baseUrl}/krzysztof-hepner-978RAXoXnH4-unsplash.jpg",
                 "availableUnits": 1,
                 "wifi": true,
                 "laundry": false
             },
             {
                 "id": 5,
                 "name": "Hopeful Apartment Group",
                 "city": "Oakland",
                 "state": "CA",
                 "photo": "${this.baseUrl}/r-architecture-JvQ0Q5IkeMM-unsplash.jpg",
                 "availableUnits": 2,
                 "wifi": true,
                 "laundry": true
             },
             {
                 "id": 6,
                 "name": "Seriously Safe Towns",
                 "city": "Oakland",
                 "state": "CA",
                 "photo": "${this.baseUrl}/phil-hearing-IYfp2Ixe9nM-unsplash.jpg",
                 "availableUnits": 5,
                 "wifi": true,
                 "laundry": true
             },
             {
                 "id": 7,
                 "name": "Hopeful Housing Solutions",
                 "city": "Oakland",
                 "state": "CA",
                 "photo": "${this.baseUrl}/r-architecture-GGupkreKwxA-unsplash.jpg",
                 "availableUnits": 2,
                 "wifi": true,
                 "laundry": true
             },
             {
                 "id": 8,
                 "name": "Seriously Safe Towns",
                 "city": "Oakland",
                 "state": "CA",
                 "photo": "${this.baseUrl}/saru-robert-9rP3mxf8qWI-unsplash.jpg",
                 "availableUnits": 10,
                 "wifi": false,
                 "laundry": false
             },
             {
                 "id": 9,
                 "name": "Capital Safe Towns",
                 "city": "Portland",
                 "state": "OR",
                 "photo": "${this.baseUrl}/webaliser-_TPTXZd9mOo-unsplash.jpg",
                 "availableUnits": 6,
                 "wifi": true,
                 "laundry": true
             }
         ]
     }
```

#### Configuración del servicio para usar el servidor JSON en vez de los datos estáticos

En el fichero *housing.service.ts* tendremos que hacer algunos cambios. Primero suprimir el array de datos estáticos y sustituirlos por una propiedad `url` que apuntará al servidor JSON. En segundo lugar tendremos que cambiar las funciones  `getAllHousingLocations`  y `getHousingLocationById`. Veámoslo en el siguiente código.

```ts
// housing.service.ts
// (...)

export class HousingService {
  url = 'http://localhost:3000/locations';
  async getAllHousingLocations(): Promise<HousingLocation[]> {
    const data = await fetch(this.url);
    return (await data.json()) ?? [];
  }
  async getHousingLocationById(id: number): Promise<HousingLocation | undefined> {
    const data = await fetch(`${this.url}/${id}`);
    return (await data.json()) ?? {};
  }
  submitApplication(firstName: string, lastName: string, email: string) {
    // tslint:disable-next-line
    console.log(firstName, lastName, email);
  }
}
```

Por su parte, también habrá que retocar los constructores de las clases de los componentes `home` y `details` para que trabajen con las nuevas funciones asíncronas:

```ts
// home.component.ts 

constructor() {
    this.housingService.getAllHousingLocations().then((housingLocationList: HousingLocation[]) => {
		this.housingLocationList = housingLocationList;
    	this.filteredLocationList = housingLocationList;
    });
}
```

 y 

```ts
// details.component.ts

constructor() {
    const housingLocationId = parseInt(this.route.snapshot.params['id'], 10);
    this.housingService.getHousingLocationById(housingLocationId).then((housingLocation) => {
		this.housingLocation = housingLocation;
	});
}
```
