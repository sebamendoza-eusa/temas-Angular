# PR5. Países del mundo

## Se trata de desarrollar una SPA (Single Page Application) que muestre información sobre países del mundo. Usaremos enrutamiento para mostrar sólo las partes de la aplicación que estén usándose por parte del usuario y *lazy loading* para cargar sólo aquellos componentes activos.

#### Indicaciones:

##### 	Inicio del proyecto

1. Iniciamos un nuevo proyecto Angular de la manera habitual. Llamaremos al proyecto `countryApp`

2. Añadimos Bootstrap a través de su CDN para tener estilos globales en toda la aplicación

3. Vamos a añadir dos carpetas al proyecto: `countries` y `shared`. A su vez, dentro de `shared` vamos a colocar dos carpetas más: `component` y `pages`

4. Dentro de `pages` generaremos tres componentes, que corresponderán a tres páginas de la app: La página de inicio, una página *acerca de* y una página de *contacto* (*home-page*, *about-page* y *contact-page*)

   ##### Enrutamiento

5. Configura el enrutamiento para que según tecleemos en la URL la dirección principal terminada en *home*, en *about* o en *contact* nos presente cada uno de los componentes que hemos definido anteriormente. Por defecto cualquier URL debe llevarnos a *home*

6. Para lo anterior tendrás que tocar el fichero `app.route.ts` y tener en cuenta lo que hemos visto en clase y en los apuntes

   ##### Barra lateral

7. Vamos con el componente de **barra lateral**. Dentro de `shared/component` crea un componente que se llame `sidebar`. Configura su plantilla HTML con un título y una lista de enlaces a las páginas de *inicio* y *acerca de*. Usa clases Bootstrap.

8. Ahora configura los enlaces de la barra lateral para que apunten a los componentes *home-page*, *about-page* y *contact-page*. Tendrás que usar la directiva `routerLink`. Ten cuidado con las importaciones, tanto en el componente *sidebar* como en `app.routes.ts`. También ten cuidado con las rutas en la directiva routerLink

   ##### Nuevos componentes en `countries`

9. Vamos ahora a centrarnos en otro aspecto de la aplicación. En la carpeta `countries` vamos a crear otras cuatro subcarpetas: `components`, `interfaces`, `pages` y `services`

10. Dentro de la carpeta `pages` vamos a crear un serie de componentes nuevos. Al generarlos usa las opciones `--inline-style` y `--skip-tests` para no generar archivos de más. Los nombre de los componentes serán los siguientes: `byCapitalPage`, `byCountryPage`, `byRegionPage`, y `countryPage`.

11. Vamos a trabajar el enrutamiento añadiendo un nuevo archivo `country.routes.ts` , donde de forma parecida a como hemos hecho antes, añadiremos las rutas relacionadas con la carpeta `countries`. A su vez se tiene conectar este archivo con el de rutas globales `app.routes.ts`

    Usa **lazy-loading** para hacer la carga de las rutas del módulo `countries.routes.ts`. Puedes ver información al respecto aquí: https://angular.io/guide/lazy-loading-ngmodules

    Puedes probar que todo funciona introduciendo por ejemplo la URL `(...)/countries/by-capital`

12. Ahora añade los enlaces correspondientes en el componente `sidebar` de modo que tengamos acceso a las rutas incluidas en el archivo `country.routes.ts`

    ##### Búsqueda de países

13. Nos ocuparemos ahora del **componente de búsqueda de países**. Como este componente es un componente genérico de búsqueda lo ubicaremos en `/shared/components`. Genera el componente con las opciones `--inline-style` y `--skip-tests` para no generar archivos de más. Lo llamaremos `searchBox`

14. Este componente solo tiene que tener de momento un formulario que recoja el término de búsqueda. El código de la plantilla podría ser algo así:

    ```html
    <input type="text" placeholder="Introduce el término de búsqueda..." class="form-control">
    ```

15. Conecta este componente con el componente *by-capital-page*, para comprobar como queda. Haz que el atributo `placeholder` sea una propiedad que pueda recibirse como entrada a la clase del componente `searchBox` desde fuera del componente.

16. Por otro lado vamos a hacer que cuando escribamos un texto en la caja de búsqueda, este llegue a una propiedad del componente *by-capital-page*. Se trata de mandar una propiedad desde *search-box* mediante un *eventEmitter* y el decorador `@output()`. Comprueba todo ello a través de un método de la clase del componente *by-capital-page* que muestre el texto de la caja de búsqueda por consola.

    ##### Backend y servicio de conexión

17. Llegados a este punto ya tenemos conseguida la manera de conectar el termino de búsqueda con una consulta en a un backend. Vamos a centrarnos en ese punto ahora. Para ello usaremos una conexión HTTP a una API-Rest, en este caso de datos e información sobre países. Usamos la URL **restcountries.com** para ver de qué manera podemos acceder a la información sobre países del mundo. Accede a la web y estudia un poco cuales son los *endpoints* que podemos usar en nuestro caso para que se adapten a las distintas páginas de búsqueda, por capital, por país o por región.

18. Para montar la conexión con la API-Rest procederemos como ya hemos hecho en otras ocasiones: Definiremos la interface asociada al objeto correspondiente al modelo de datos, y implementaremos el servicio correspondiente. Ubica la interface `country.ts` en la carpeta *interfaces* dentro de la carpeta *countries*. Para ver las características  conéctate al *endpoint* correspondiente y fíjate en las propiedades del objeto que se devuelve.

    > Para facilitar la copia de estructuras del objeto JSON a nuestra interface usa la extensión de VSCode ***Paste JSON as Code***. Te facilitará mucho la tarea. (También puedes conseguir el mismo resultado en la web ***quicktype.io***)

19. Configura el servicio `country.service.ts` como hemos visto en los ejemplos de clase

    ##### Resultados por pantalla. Componente `country-table`

20. El siguiente paso consiste en presentar los resultados por pantalla. Para ello tendremos que inyectar el servicio en el componente correspondiente, en este caso el componente *by-capital-page*. Tras ello habrá que desarrollar la lógica necesaria en la clase para traernos los datos. Puedes basarte en los ejercicios de clase. Finalmente nos iremos a la plantilla HTML del componente y mediante una directiva `ngFor` presentar una lista de resultados básica interpolando las propiedades relacionadas.

21. Para presentar la lista de resultados de una manera más visual vamos a desarrollar un componente específico: lo llamaremos **country-table** y lo ubicaremos en la carpeta `countries`, subcarpeta `components`.

22. Este componente recibirá los datos de países mediante el correspondiente decorador `@input()`. Usa un array `countries` para almacenar los resultados de búsqueda

23. La plantilla del componente podría tener un código como éste:

    ```html
    <div
    *ngIf="countries.length === 0; else table"
    class="alert alert-warning text-center"
    >
    No hay países que mostrar...
    </div>
    
    <ng-template #table>
    <div class="table table-hover">
    <thead>
      <tr>
        <th>#</th>
        <th>Icono</th>
        <th>Bandera</th>
        <th>Nombre</th>
        <th>Capital</th>
        <th>Población</th>
        <th> <!-- deja un espacio en blanco de momento --></th>
      </tr>
    </thead>
    <tbody>
      <tr *ngFor="let country of countries; let i = index">
    	<!-- td's que presentan los distintos elementos de las columnas -->
        <td><a href="#">Ver más...</a></td>
      </tr>
    </tbody>
    </div>
    </ng-template>
    ```

24. Finalmente conecta el componente *country-table* con el componente *by-capital-page*, reemplazando en la plantilla HTML, el código que mostraba los nombres de los países por el selector del componente *country-table*. Será necesario que pases a través de este selector la propiedad `countries` para que el resultado en la tabla sea visible.

    ##### Manejo de errores de conexión a la API-Rest

25. Ahora vas a implementar el manejo de errores en el formulario de búsqueda y el resultado del observable. Si te das cuenta verás que si tipeas cualquier cadena en el formulario de búsqueda no ocurre nada. Esto puede causar confusión, y por ello es necesario manejar los errores que proceden del hecho de que el observable no mande ningún resultado. Lo más interesante es que manejes el error a nivel del servicio `country.service.ts` ya que ese servicio es común a todos los tipos de búsqueda.

26. Para capturar el error vas a usar un `.pipe()` de Angular. Sigue las siguientes indicaciones:

    - Sitúate en la clase del servicio

    - Reescribe el método `searchCapital()`, como sigue:

      ```ts
        searchCapital(term: string): Observable<Country[]> {
          const url = `${this.apiURL}/capital/${term}`;
          return this.http.get<Country[]>(url)
          .pipe(
            catchError(error => of([])) // Si hay un error se vacía el array de resultados
          );
        }
      ```

    - Tendrás que añadir `catcherror` y `of` dentro de la sección del módulo `'rxjs'` en la zona de importaciones

    > Hasta aquí ya tendríamos las búsquedas por capitales. Ahora **implementa siguiendo los pasos anteriores las búsquedas por región y por país**. Podrás aprovechar la mayor parte del código y sólo tendrás que hacer algunas adaptaciones.

    ##### Vista de detallada de la información. Paso de parámetros por URL

27. Para completar la aplicación de búsqueda acometeremos finalmente la implementación del enlace *ver más* que aparecía en la última columna del componente *country-table*. Se trata de desarrollar una estructura **listado-detalle**, pasando el parámetro del código del país del que vamos a ampliar la información a través de un parámetro en la URL.

28. Empezaremos por situarnos en el componente *country-page*, dentro de la carpeta `pages`, dentro de `countries`.  En la clase `CountryPageComponent` tendrás que añadir un constructor, además de implementar `OnInit`

    1. Añade el constructor `constructor(activatedRoute: ActivatedRoute){}`

    2. Usa el siguiente código dentro de `ngOnInit`

       ```javascript
         ngOnInit(): void {
           this.activatedRoute.params.subscribe( ({ id }) => {
             console.log({ params: id })
           })
         }
       ```

       Con esto conseguiremos ver por consola el parámetro que escribamos en la URL. Por ejemplo: si escribimos una URL que termina en `countries/id/HOLA`  en consola veremos la palabra *HOLA*.

29. Ahora necesitamos, una vez que somos capaces de pasar el parámetro por URL, añadir al servicio de conxión con la API-Rest un nuevo método que consulte los datos de un país dado su código *id*. Para hacerlo añade un método que llamaremos `searchCountryByAlphaCode()` , el cual implementarás para que lea los datos del *endpoint* `https://restcountries.com/v3.1/alpha/<id>`, donde <id> es el código de país que pasaremos por parámetro en la URL.

30. El código de este método tiene que quedar así:

    ```ts
    searchCountryByAlphaCode( code: string ): Observable<Country | null> {
    
      const url = `${ this.apiUrl }/alpha/${ code }`;
    
        return this.http.get<Country[]>( url )
          .pipe(
            map( countries => countries.length > 0 ? countries[0]: null ),
            catchError( () => of(null) ) // cuidado con la importación de map en `rxjs`
          );
      }
    ```

31. Ahora ya podemos inyectar el servicio en el componente *country-page*, vía el constructor correspondiente: `constructor(private countriesService: CountryService) {}`. Cambia el código del apartado (28) por el siguiente:

    ```javascript
    ngOnInit(): void {
      this.activatedRoute.params
      .pipe(
      switchMap( ({ id }) => this.countriesService.searchCountryByAlphaCode( id )),
        )
          .subscribe( country => {
            if ( !country ) return this.router.navigateByUrl('');
            return this.country = country;
          });
    }
    ```

    Para que todo funcione, tendrás que añadir el constructor `constructor(private router: Router) {}` e importar `switchMap` desde `rxjs`, y el módulo `router`. Las importaciones de este componente quedarán así:

    ```javascript
    import { Component, OnInit } from '@angular/core';
    import { ActivatedRoute, Router } from '@angular/router';
    import { CountriesService } from '../../services/countries.service';
    import { switchMap } from 'rxjs';
    import { Country } from '../../interfaces/country';
    ```

    Con ello ya completamos el código del componente *country-page*

    ##### Plantilla del componente de detalle y enlace

32. Faltaría ahora diseñar la plantilla del componente. Usa el código siguiente y tendrás el resultado deseado cuando pases el código del país por parámetro en la URL

    ```html
    <ng-template #loading>
      <div class="alert alert-info text-center">
        Espere por favor
      </div>
    </ng-template>
    <div *ngIf="country; else loading">
      <div class="row">
        <div class="col-12">
          <h2>País: <strong>{{ country.name.common }}</strong></h2>
          <hr>
        </div>
      </div>
    
      <div class="row">
        <div class="col-4">
          <h3>Bandera</h3>
          <img [src]="country.flags.svg" [alt]="country.name.common" class="img-thumbnail">
        </div>
        <div class="col">
          <h3>Información</h3>
          <ul class="list-group">
            <li class="list-group-item">
              <strong>Población:</strong> {{ country.population | number }}
            </li>
            <li class="list-group-item">
              <strong>Código:</strong> {{ country.cca3 }}
            </li>
          </ul>
        </div>
      </div>
      <div class="row mt-2">
        <div class="col">
          <h3>Traducciones</h3>
          <div class="d-flex flex-wrap">
            <span class="badge bg-primary m-1">{{ country.translations['ara'].common }}</span>
            <span class="badge bg-primary m-1">{{ country.translations['bre'].common }}</span>
            <span class="badge bg-primary m-1">{{ country.translations['ces'].common }}</span>
            <span class="badge bg-primary m-1">{{ country.translations['cym'].common }}</span>
            <span class="badge bg-primary m-1">{{ country.translations['deu'].common }}</span>
            <span class="badge bg-primary m-1">{{ country.translations['est'].common }}</span>
            <span class="badge bg-primary m-1">{{ country.translations['fin'].common }}</span>
            <span class="badge bg-primary m-1">{{ country.translations['ita'].common }}</span>
            <span class="badge bg-primary m-1">{{ country.translations['kor'].common }}</span>
            <span class="badge bg-primary m-1">{{ country.translations['per'].common }}</span>
          </div>
        </div>
      </div>
    </div>
    ```

33. Por último hay que enlazar desde el componente *country-table* al detalle de cada país que hemos desarrollado en el apartado anterior. Para ello tendremos que situarnos en la plantilla del componente y editar el enlace *Ver más* añadiendo una directiva `[routerlink]`, que igualaremos a un array que nos proporciona la ruta deseada. Este array sería `['countries/by', country.cca3]` (`cca3` es la propiedad del código del país en el objeto JSON que nos traemos del servidor API-Rest). Para poder usar `[routerLink]` en la plantilla, habrá que importar el módulo en el fichero del componente.

## Mejoras

- Implementar refactorización de código si se ve necesario
- Añadir contenido a las páginas `Inicio`, `Acerca de` y `Contacto`
- Simular un formulario de contacto en la página de contacto

## Instrucciones de entrega

- Subir el proyecto a repositorio GitHub público
- Preparar un ***codespace*** para poder ejecutar el proyecto. **Comprobar que funciona**
- Incluir README.md con una pequeña memoria del proyecto
