# Comunicarse con servidores de backend mediante HTTP en Angular

La mayoría de las aplicaciones frontend necesitan comunicarse con un servidor a través del protocolo HTTP para descargar o cargar datos y acceder a otros servicios backend. Angular proporciona una API HTTP de cliente simplificada: la clase de servicio `HttpClient` ubicada en  `@angular/common/http`

El servicio de cliente HTTP ofrece, entre otras, las siguientes funcionalidades:

- La capacidad de solicitar objetos de tipo *response*
- Un manejo de errores optimizado
- Interceptación de solicitudes y respuestas

Antes de poder usar `HttpClient` en la aplicación, se debe configurar mediante una **inyección de dependencia**. Para ello usaremos el método `provideHttpClient`, que la mayoría de las aplicaciones incluyen la sección `providers` de la aplicación principal `main.ts`

```ts
boostrapApplication(App, {providers: [
  provideHttpClient(),
]});
```

Si la aplicación en cuestión  usa un sistema de módulos del tipo `NgModule`,  se podrá incluir el método en la sección  `providers` del `NgModule` de la app.

```ts
@NgModule({
  providers: [
    provideHttpClient(),
  ],
  // ... other application configuration
})
export class AppModule {}
```

A continuación, ya se podría inyectar el servicio `HttpClient` como una dependencia dentro de cualquier componente , servicios u otro tipo de clases:

```ts
@Injectable({providedIn: 'root'})
export class ConfigService {
  constructor(private http: HttpClient) {
    // This service can now make HTTP requests via `this.http`.
  }
}
```

`provideHttpClient` acepta una lista de configuraciones de características opcionales, para habilitar o configurar el comportamiento de diferentes aspectos del cliente. Puedes encontrar más información al respecto aquí: https://angular.dev/guide/http/setup#configuring-features-of-httpclient

## Solicitud de datos JSON a un servidor

Podemos usar el método `HTTPClient.get()` para obtener datos de un servidor. El método asincrónico envía una solicitud HTTP y devuelve un *observable* que emite los datos solicitados cuando se recibe la respuesta. El tipo de valor devuelto variará en función de los valores de `observe` y `responseType` que se pasen a la llamada.

El método `get()` toma dos argumentos: la dirección URL del punto de conexión desde la que se va a traer la información y un objeto `options` que puede usar para configurar la solicitud. Dicho objeto puede tener una forma como esta:

```javascript
options: {
    headers?: HttpHeaders | {[header: string]: string | string[]},
    observe?: 'body' | 'events' | 'response',
    params?: HttpParams|{[param: string]: string | string[]},
    reportProgress?: boolean,
    responseType?: 'arraybuffer'|'blob'|'json'|'text',
    withCredentials?: boolean,
  }
```

Se puede ver como entre las opciones se incluyen las propiedades `observe` y `responseType`.

Como hemos visto durante el curso, las aplicaciones a menudo solicitan datos JSON de un servidor. Vamos a poner en práctica lo visto para crear un servicio que reciba datos en formato JSON desde una API Rest externa. Usaremos para ello la API-Rest https://chiquitadas.es/api/quotes/avoleorrr que proporciona frases “celebres” de Chiquito de la Calzada “a voleo”

Tras generar el proyecto y configurar correctamente `app.config.ts` para incorporar el método `provideHttpClient` así:

```ts
import { ApplicationConfig } from '@angular/core';
import { provideRouter } from '@angular/router';
import { provideHttpClient } from '@angular/common/http';

import { routes } from './app.routes';

export const appConfig: ApplicationConfig = {
  providers: [provideRouter(routes), provideHttpClient()]
};

```

crearemos el componente `verfrase`, que será el que devuelva las frases de Chiquito desde la API. Una vez creado este componente, crearemos a su vez una interface que sirva para tipear los datos que provienen de dicha API. Definimos un fichero `frase.ts` con el siguiente contenido

```ts
export interface Frase {
    id: string; // Identificador la frase aleatoria
  	quote: string; // Contiene la frase
}
```

Ahora tendremos que afrontar la cuestión más importante y central de este apartado: **Crear un servicio que gestione la comunicación con la API externa**

Podemos generar un servicio con Angular CLI al que llamaremos “api”

```ts
ng g service api
```

Este servicio tendrá un código como este más o menos:

```ts
import { Injectable } from "@angular/core";
import { Frase } from "../frase";
import { HttpClient } from "@angular/common/http";
import { Observable } from "rxjs";

@Injectable({
  providedIn: "root"
})
export class ApiService {
  private frase: Frase = { id: "", quote: ""};
  private ChiquitoUrl = "https://chiquitadas.es/api/quotes/avoleorrr"; // URL to web api

  constructor(private http: HttpClient) {}

  public getFrase(): Observable<Frase> {
    return this.http.get<Frase>(this.ChiquitoUrl);
  }
}
```

Ya hemos visto en un apartado anterior que los métodos **observables** permiten intercambiar mensajes entre un publicador y los suscriptores, y que son de especial interés para la gestión de eventos y la programación asíncrona.

Los métodos observables son funciones **declarativas**, se definen para publicar valores pero no se ejecutan hasta que un consumidor se suscribe a ellas, desde ese momento recibe notificaciones hasta que finalice la función observable o finalice la suscripción de forma programada.

Por otro lado, **RxJS** es una librería que nos permite crear observables de manera sencilla.

Ahora ya tenemos todo preparado para trabajar sobre el componente `verFrase`. Primero el fichero de la clase, que quedaría así:

```ts
import { Component, OnInit } from "@angular/core";
import { ApiService } from "../../servicios/api.service";
import { Frase } from "../../modelos/frase";

@Component({
  selector: "app-verfrase",
  templateUrl: "./verfrase.component.html",
  styleUrls: ["./verfrase.component.css"]
})
export class VerfraseComponent implements OnInit {
  public frase: Frase = { id: "", quote: ""};

  constructor(private apiservice: ApiService) {
   
  }

  ngOnInit() {
    this.apiservice.getFrase().subscribe(frase => (this.frase = frase));
    console.log("ngOnInit. FIN");
  }
}

```

Y el fichero de plantilla, algo tan sencillo como esto:

```html
<p>{{ frase.quote }}</p>
```



