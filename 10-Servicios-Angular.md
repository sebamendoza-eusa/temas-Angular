# Servicios en Angular

*Servicio* es una categoría amplia que abarca cualquier valor, función o característica que sea consumida por una aplicación. Un servicio es típicamente una clase con un propósito limitado y bien definido. Debe hacer algo específico y hacerlo bien.

Angular distingue los componentes de los servicios para aumentar la modularidad y la reutilización. Al separar la funcionalidad relacionada con la vista de un componente de otros tipos de procesamiento, se pueden conseguir componentes más ágiles y eficientes.

Idealmente, **el trabajo de un componente es permitir la experiencia del usuario y nada más**. Un componente debe presentar propiedades y métodos para el enlace de datos, para mediar entre la vista (representada por la plantilla) y la lógica de la aplicación (que a menudo incluye alguna noción de *modelo* ).

Un componente puede delegar ciertas tareas a los servicios, como obtener datos de un servidor, validar la entrada del usuario o registrar información directamente en la consola. Al definir tales tareas de procesamiento en una *clase de servicio inyectable*, se consigue que dichas tareas se dispongan para cualquier componente.

En cualquier caso, Angular no *impone* estos principios. Angular te ayuda a *seguir* estos principios al facilitar la integración de la lógica de tu aplicación en los servicios y hacer que esos servicios están disponibles para los componentes a través de lo que se denomina *inyección de dependencia*.

Un ejemplo de servicio en Angular lo tenemos en el siguiente código:

```ts
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class ArticulosService {

  retornarArticulos() {
    return [
      {
        codigo: 1,
        descripcion: "papas",
        precio: 12.33
      },
      {
        codigo: 2,
        descripcion: "manzanas",
        precio: 54
      },
      {
        codigo: 3,
        descripcion: "sandía",
        precio: 14
      }
    ];
  }
}
```

Este código provee un array de objetos al resto de los componentes que lo consuman.

Tal es la importancia de los servicios en Angular que la herramienta Angular CLI nos provee la capacidad de crearlos. Para ello tendremos que ejecutar

```powershell
ng generate service <nombre-del-servicio>
```

## Inyección de dependencia

La *inyección de dependencia* es un concepto conectado al framework de Angular. Este mecanismo puede usarse para proporcionar nuevas funcionalidades a los componentes, bien sea a través de servicios o de otro elemento que necesiten. Así pues, **los componentes pueden consumir servicios**; o lo que es lo mismo: se puede *inyectar* un servicio en un componente, dándole acceso al servicio a dicho componente.

Para definir una clase como un servicio en Angular, usa el decorador `@Injectable ()` y así proporcionar los metadatos que permitan a Angular inyectarlo en un componente como una *dependencia* . De manera similar, usa el decorador `@Injectable()` para indicar que un componente u otra clase (como otro servicio, un pipeline o un NgModule) *tiene* una dependencia.

- El *inyector* es el mecanismo principal. Angular crea un inyector para toda la aplicación durante el proceso de arranque e inyectores adicionales según sea necesario. No es necesario crear inyectores.
- Un inyector crea dependencias y mantiene un *contenedor* de instancias de dependencia que reutiliza si es posible.
- Un *proveedor* es un objeto que le dice a un inyector cómo obtener o crear una dependencia.

Para cualquier dependencia que necesites en tu aplicación, **debes registrar un proveedor con el inyector de la aplicación**, con el fin de que el inyector pueda utilizar el proveedor para crear nuevas instancias. Para un servicio, el proveedor suele ser la propia clase de servicio.

Una dependencia no tiene que ser solamente un servicio, podría ser una función, o un valor, entre otras cosas

Cuando Angular crea una nueva instancia de una clase de componente, determina qué servicios u otras dependencias necesita ese componente al chequear los tipos de parámetros del constructor de la clase.

Por ejemplo. En este código:

```ts
constructor(private service: HeroService) { }
```

Tenemos que el constructor del componente necesita del servicio `HeroService` y que creará un objeto `service` para disponer de la funcionalidad del servicio.

Cuando Angular descubre que un componente depende de un servicio, primero verifica si el inyector tiene instancias existentes de ese servicio. Si una instancia de servicio solicitada aún no existe, el inyector crea una utilizando el proveedor registrado y la agrega al inyector antes de devolver el servicio a Angular.

Cuando todos los servicios solicitados se han resuelto y devuelto, Angular puede llamar al constructor del componente con esos servicios como argumentos.

En el ejemplo anterior, el proceso de inyección de `HeroService` se parece a esto:

![Service](./images/injector-injects.png)

## Proporcionar servicios

Se debe registrar al menos un *proveedor* de cualquier servicio que vayas a utilizar. El proveedor puede formar parte de los propios metadatos del servicio, haciendo que ese servicio esté disponible en todas partes, o se pueden registrar proveedores con módulos o componentes específicos. Los proveedores se registran en los metadatos del servicio (en el decorador `@Injectable()`), o en los metadatos de los decoradores `@NgModule()` o `@Component()`

Por defecto, el comando CLI de Angular [`ng generate service`](https://docs.angular.lat/cli/generate) registra un proveedor con el inyector raíz para tu servicio al incluir metadatos del proveedor en el decorador `@Injectable()`. 

```ts
@Injectable({
 providedIn: 'root',
})
```

Cuando se proporciona el servicio en el nivel raíz, Angular crea una instancia única compartida de dicho servicio y lo inyecta en cualquier clase que lo solicite. El registro del proveedor en los metadatos `@Injectable()` también permite a Angular optimizar una aplicación eliminando el servicio de la aplicación compilada si no se utiliza.

Cuando se registra un proveedor a nivel del componente, obtienes una nueva instancia del servicio con cada nueva instancia de ese componente. A nivel del componente, se puede registrar un proveedor de servicios en la propiedad `Providers` de los metadatos `@Component()`. Por ejemplo, en el siguiente código se registra una instancia del servicio `HeroService` dentro de un componente concreto

```ts
@Component({
  selector:    'app-hero-list',
  templateUrl: './hero-list.component.html',
  providers:  [ HeroService ]
})
```
