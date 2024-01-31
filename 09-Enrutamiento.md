# Enrutamiento (*routers*)

El sistema de rutas es una parte fundamental en cualquier aplicación de Angular. Permite la navegación entre diferentes vistas y componentes, lo que es crucial para crear una experiencia de usuario fluida y amigable.

El enrutamiento en Angular es la capacidad de segmentar o dividir nuestra aplicación en múltiples vistas. La renderización de estas vistas se vinculará a una URL asignada a cada una de ellas. Según la URL en donde nos encontremos o donde nos redireccione la aplicación se renderizará el componente enlazado a esta ruta.

El enrutamiento no es obligatorio en Angular, pero es la manera más óptima que tenemos de dividir nuestras vistas y nuestro trabajo en componentes, segmentarlo y poder renderizar el contenido según lo que le queramos mostrar al usuario.

Además, es posible enviar y recibir valores entre rutas según la necesidad y el comportamiento deseado dentro de la aplicación.

También en una aplicación de una sola página (SPA), en lugar de ir al servidor para obtener una nueva página, se puede cambiar la vista del usuario mostrando u ocultando partes de la pantalla que corresponden a componentes concretos.

Para manejar la navegación entre vistas se utiliza el método Angular **[`Router`](https://angular.dev/api/router/Router)**. Con ello se habilita la navegación interpretando una URL del explorador como una instrucción para cambiar la vista. Cuando creamos la aplicación con Angular CLI se nos generan los archivos básicos para trabajar con rutas.

## Enrutamiento básico

Para **definir una ruta básica** es necesario primeramente comprobar que en el archivo `app.config.ts` está definida la función `provideRoute` en la sección `providers`. A partir de aquí tendremos que hacer tres cosas:

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

Para añadir las rutas a la aplicación, en primer lugar, agrega los enlaces a los dos componentes. Asigna la etiqueta de anclaje a la que hay que agregar la ruta al atributo `routerLink`. Después habrá que establecerse el valor del atributo en el componente que se mostrará cuando un usuario haga clic en cada vínculo. A continuación, actualiza la plantilla de componente para incluir `<router-outlet>` . Este elemento informa a Angular que actualice la vista de la aplicación con el componente para la ruta seleccionada.

Veamos todo en acción en el siguiente código:

```html
<h1>Angular Router App</h1>
<nav>
  <ul>
    <li><a routerLink="/first-component" routerLinkActive="active" ariaCurrentWhenActive="page">First Component</a></li>
    <li><a routerLink="/second-component" routerLinkActive="active" ariaCurrentWhenActive="page">Second Component</a></li>
  </ul>
</nav>
<!-- The routed views render in the <router-outlet>-->
<router-outlet></router-outlet>
```

Por supuesto será necesario importar en el archivo de componente `Routerlink`, `RuterLinkActive` y `RouterOutlet`

> Puedes obtener más información acerca de tareas comunes de enrutamiento aquí: https://angular.dev/guide/routing/common-router-tasks

## Obtener información sobre una ruta y paso de parámetros

(pendiente)

## Captura de ruta no existente

(pendiente)

## Rutas anidadas

(pendiente)
