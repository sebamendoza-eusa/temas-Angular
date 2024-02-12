# Comunicación HTML en Angular

## ¿Qué es una API en Angular?

Una **API** (Application Programming Interface) en Angular es una **interfaz de programación de aplicaciones** que permite la comunicación y el intercambio de datos entre diferentes componentes de software. En el contexto de Angular, una API generalmente se refiere a una **interfaz expuesta por un servidor web** que permite acceder a los recursos y funcionalidades de una aplicación.

En Angular, una API puede ser una **fuente externa de datos**, como una **API REST**,  que proporciona datos en formato **JSON**. También puede ser una **API personalizada** creada en el backend de nuestra aplicación para ofrecer funcionalidades específicas.

En conclusión: Las **APIs en Angular** permiten a nuestras aplicaciones web interactuar con servicios externos, obtener datos en tiempo real, enviar y recibir información, y realizar acciones en base a las respuestas recibidas.

## ¿Cómo se conecta una API en Angular?

Existen diferentes métodos para **consumir una API en Angular**. Algunas de las opciones más comunes son las siguientes:

1. **HTTPClient**: Angular proporciona el módulo `HttpClient` que facilita la realización de **peticiones HTTP** a una API. Este módulo ofrece métodos como `get`, `post`, `put`, o `delete`, entre otros, que nos permiten realizar operaciones CRUD (Crear, Leer, Actualizar y Eliminar) con la API. Además, el `HttpClient` también nos brinda opciones para manejar encabezados, parámetros y respuestas.
2. **Fetch API**: También es posible utilizar la **Fetch API**, una API nativa del navegador, para realizar peticiones HTTP a una API en Angular. Fetch API proporciona una interfaz para realizar peticiones y manejar las respuestas de manera más sencilla y flexible.
3. **Librerías de terceros**: Otra opción es utilizar **librerías de terceros**, como Axios o jQuery, que ofrecen funcionalidades adicionales y simplifican el consumo de una API en Angular. Estas librerías suelen tener características más avanzadas y proporcionar una sintaxis más concisa y fácil de usar.

### Uso de módulos y componentes para integrar la API

En Angular, la integración de una API se realiza principalmente a través de los propios **componentes**.

Para integrar una API en Angular, generalmente seguimos los siguientes pasos:

1. **Importar el módulo provideHTTPClient**: Si trabajamos en modo *standalone* tendremos que añadir el proveedor correspondiente en el fichero `app.config.ts`. Importamos el módulo `provideHttpClient` desde `@angular/common/http` y lo agregamos a la lista de *providers*.
2. **Crear un servicio**: Creamos un servicio que utilizará el `HttpClient` para realizar las peticiones a la API REST. En este servicio, definimos los métodos y funcionalidades necesarios para interactuar con la API y manejar las respuestas.
3. **Consumir el servicio en un componente**: En un componente específico, importamos y utilizamos el servicio creado para realizar las llamadas a la API. Podemos llamar a los métodos del servicio y utilizar los datos obtenidos en la lógica y presentación del componente.

El primer paso no tiene mucha más explicación, al menos en una primera aproximación. Vamos a centrarnos en los pasos segundo y tercero.

#### Crear un servicio que gestione la conexión con la API

Ya hemos visto en un apartado anterior qué es y cómo funciona un servicio en Angular. Uno de los usos más habituales es la gestión de conexiones con APIs externas.

Lo habitual para, por ejemplo, gestionar flujo de datos con una API REST es generar un servicio y añadir en él los métodos de clase que necesitemos. Veamos el siguiente código de ejemplo, muy sencillo:

```ts
import { Injectable } from '@angular/core';
import { HttpClient } from "@angular/common/http";
import { Observable } from "rxjs";

// Definir inline o en archivo externo la interface que proporcione tipado a los datos procedentes de la API
// dataInterface

@Injectable({
  providedIn: 'root'
})
export class ApiRestService {

  private data: dataInterface = { /* definir propiedades */ };
  private urlBase = "https://..."; // URL to web api

  // inyectamos el servicio de conexión HTTPClient a través del objeto http
  constructor(private http: HttpClient) { } 

  public getData(): Observable<dataInterface> {
    return this.http.get<dataInterface>(this.urlBase); // se implementa un método GET
  }
}
```

Este código pertenece a un servicio con una clase `ApiRestService`. Vamos a analizar sus características principales:

- En la zona de importación incorporamos los módulos principales para trabajar con comunicación HTML: `HttpClient` se encarga de la comunicación en sí, `Injectable` nos permite que otros componentes consuman este servicio, `Observable` nos permite definir el flujo de datos desde la API REST como un observable
- Hay que tener en cuenta definir una interface que proporcione un tipado a los datos que provengan de la API
- En la clase del servicio se ha definido una propiedad que servirá para guardar información proveniente de la API REST; la URL que conecta con el *end point*; el constructor que permite consumir las características del módulo `HttpClient`  y por último un método asociado a una operación básica GET que permite una operación de lectura de datos en formato JSON.

A partir del ejemplo anterior se pueden desarrollar métodos más complejos que interactúen con la API REST e implementen operaciones POST, UPDATE o DELETE.

Una cuestión muy importante es la definición del método `getData` como un `Observable`. Esto nos hace configurar un flujo de datos continuo entre la API y nuestra aplicación, gestionando la comunicación asíncrona entre ambas.

#### Consumir el servicio en un componente

Una vez creado el servicio, lo siguiente es consumirlo en un componente determinado. Esto lo haremos a través de una *inyección de dependencia*. Para ello será necesario importar en la cabecera del componente el servicio en cuestión, así como la interface que fuese necesaria.

Un ejemplo de código de componente consumidor del servicio anterior sería algo como lo siguiente:

```ts
import { Component, OnInit } from '@angular/core';
import { ApiService } from "../api.service";
import { dataInterface } from "../dataInterface";

@Component({
  selector: 'app-verData',
  standalone: true,
  imports: [],
  templateUrl: './verData.component.html',
  styleUrl: './verData.component.css'
})
ex
port class VerDataComponent implements OnInit {
  public data: dataInterface = { /* definir propiedades */ };
  
  constructor(private apiservice: ApiService) {} // Inyector del servicio en el objeto apiservice

  ngOnInit(): void {
    this.apiservice.getData().subscribe(data => (this.data = data));
  }
}
```

Vemos cómo a través del constructor inyectamos el servicio `ApiService`, para después, en la sección `ngOnInit()` usar sus métodos.

Como el servicio se ha configurado como un observable, tendremos que usar el método `subscribe()` con la correspondiente función *callback*. Así conseguimos un comportamiento reactivo y que el método `getData()` recoja de manera asíncrona los cambios en el flujo de datos.

### Usar el JSON Server

El proceso de conexión a una API REST que hemos descrito en el apartado anterior puede funcionar en situaciones donde estemos interesados en consumir datos en modo lectura. Sin embargo, lo normal es que también tengamos que desarrollar mecanismos de escritura y actualización de los datos en el servidor. Para ello tendremos que tener un backend que lo permita. Angular incorpora un servidor JSON de desarrollo que nos permite emular un backend sencillo y poder implementar todos los métodos habituales en una conexión HTTP. Podemos instalar este servidor desde el Angular CLI del modo siguiente:

```powershell
npm install -g json-server
```

Una vez que tengamos instalado el componente, lo único que necesitaremos es definir un fichero `db.json` que contenga los datos con los que vamos a trabajar. Un ejemplo de este fichero podría ser el siguiente:

```json
{
    "articulos": [
      {
        "codigo": 1,
        "descripcion": "patata",
        "precio": 12.33
      },
      {
        "codigo": 2,
        "descripcion": "manzana",
        "precio": 54.25
      },
      {
        "codigo": 3,
        "descripcion": "sandía",
        "precio": 14.67
      }
    ]
}
```

Si ahora ejecutamos en la consola el siguiente comando:

```powershell
json-server --watch db.json
```

Tendremos disponible el servidor JSON en la URL local http://localhost:3000/articulos

> Puedes encontrar más información sobre el servidor de desarrollo JSON proporcionado por Angular aquí: https://www.npmjs.com/package/json-server

A partir de aquí podríamos de nuevo configurar un servicio de conexión a esta API REST cambiando la URL de conexión.

También sería interesante implementar el resto de métodos propios de un CRUD en el servicio para completar la funcionalidad de la aplicación. En el siguiente código, y siguiendo con el ejemplo anterior, tienes una muestra para ver como podrían implementarse el resto de funcionalidades

```javascript

// Ver todos los artículos
public getArticulos(): Observable<any> {
  return this.http.get<any>(this.urlBase); // se implementa un método GET en una URL
}

// borrar un registro
  deletePost(id:number){
    this.postService.delete(id).subscribe(res => {
         this.posts = this.posts.filter(item => item.id !== id);
    })
    
    
public getEmployeeDetailById(model: any): Observable<any> {
  return this.webApiService.get(httpLink.getEmployeeDetailById + '?employeeId=' + model);
}
public saveEmployee(model: any): Observable<any> {
  return this.webApiService.post(httpLink.saveEmployee, model);
}  
```

