# PR4. CRUD Blog

## Se trata de desarrollar un módulo CRUD para las publicaciones de un blog. Se tienen que implementar funcionalidades de lista, vista, inserción, actualización y eliminación de los post. Por simplificar el backend, usaremos la API del servicio web JSONPlaceholder.

#### Indicaciones:

##### Inicio del proyecto

1. Crear un nuevo proyecto Angular en un repositorio GitHub. Llama al proyecto `blog`. Trabajarás en modo `standalone`

2. Añade Bootstrap (versión 5) a través de sus CDN. Ten en cuenta al añadir BS5 que el alcance ha de ser **global a todo el proyecto**

3. **Crea componentes para cada acción del CRUD:** listado, vista individual, creación y actualización y englóbalos en una carpeta dentro del proyecto a la que denominarás `post`

4. **Crea las rutas del proyecto**. Recuerda que el enrutamiento se configura desde el archivo `app.routes.ts`

   También habrá que cambiar el fichero `app.config.ts` para añadir el servicio `provideHttpClient`, tal y como hemos visto en el caso de conexiones con servidores HTTP.

##### Modelo de datos

5. **Vamos con el modelo.** Crea la interfaz para los objetos principales de nuestro proyecto, que son los *posts*. Llama al archivo de la interfaz `post.ts` y ubícalo dentro de la carpeta `post`. El objeto `post` puede tener la siguiente estructura:

```ts
Post {
    id: number;
    title: string;
    body: string;
}
```

6. Lo siguiente es **configurar el servicio de conexión a la base de datos**. Este servicio incorporará todas las funciones del CRUD. Se usará como proveedor de datos la web de  https://jsonplaceholder.typicode.com.

7. **Genera un servicio `post` dentro de la carpeta `post`**. El servicio tiene que tener una estructura parecida a la siguiente:

```ts
import { Injectable } from '@angular/core';
import { HttpClient } from '@angular/common/http';
     
import {  Observable } from 'rxjs';
  
// Importar aquí las interfaces necesarias
  
@Injectable({
  providedIn: 'root'
})
export class PostService {
  
  private apiURL = // URL API
    
// Http Header Options
    
  httpOptions = {
    headers: new HttpHeaders({
      'Content-Type': 'application/json'
    })
  }
   
 // Constructor

  constructor(private httpClient: HttpClient) { }
    
  // Metodos
    
 // GET
	getAll(): Observable<any> {  
    return this.httpClient.get(this.apiURL + '/posts/')
    )
  } // usar adecuadamente las interfaces
    
// CREATE
    
  create(post:Post): Observable<any> {
  
    return this.httpClient.post(this.apiURL + '/posts/', JSON.stringify(post), this.httpOptions)
  }  
    
// BUSCAR
    
  find(id:number): Observable<any> {
  
    return this.httpClient.get(this.apiURL + '/posts/' + id)
  }
    
// ACTUALIZAR
    
  update(id:number, post:Post): Observable<any> {
    return this.httpClient.put(this.apiURL + '/posts/' + id, JSON.stringify(post), this.httpOptions)

  }
       
// BORRAR
  delete(id:number){
    return this.httpClient.delete(this.apiURL + '/posts/' + id, this.httpOptions)
  }
      
}
```

##### Desarrollo de componentes

8. **Primero desarrolla el componente que se encarga del listado de posts**. La plantilla del componente tendrá que tener un aspecto parecido a esto

![image-20240204191400841](H:\Mi unidad\Classroom\DAW-EC_PRE\practicas-angular\image-20240204191400841.png)

9. En la plantilla tendrás que usar directivas de bucle para construir la tabla. Por otro lado, ten en cuenta que los botones de vista y de edición son **links** a otros componentes. Lo mismo se aplica al botón de creación de post nuevo.

10. El componente de listado tendrá que incorporar en su clase la lógica necesaria para listar y para añadir los botones de borrado, vista y edición

	Un esquema del componente podría ser este:

```ts
// zona de imports (recuerda el servicio de conexión con la API)

// Decorador
// Clase (implementa onInit)

// Declaración/Inicialización de objetos

// constructor (servicio API)

// Método onInit que incorpora la funcionalidad de listar los posts

// Método de borrado (buscar el id del post y llamar al método correspondiente del servicio de conexión con la API)
```

11. Vamos ahora con el componente de **creación de posts**. Se trata de usar un formulario (de tipo reactivo) que añada datos al servidor usando el servicio de gestión de la API.
12. La estructura del componente podría ser algo así

```javascript
// zona de imports (recuerda el servicio de conexión con la API)
// Añadir los imports necesarios para manejar formularios reactivos:
import { ReactiveFormsModule, FormGroup, FormControl, Validators } from '@angular/forms';

// Decorador
// Clase (implementa onInit)

// Declaración de objetos (en este caso un objeto form)

// constructor (servicio API y servicio de enrutamiento)

// Método onInit que incorpora la creación de un objeto form de la clase FromGroup

// Método que recupere campos del formulario

// Método submit (llamar al método correspondiente del servicio de conexión con la API)
```

13. En cuanto a la plantilla, tendríamos que conseguir algo como esto:

![image-20240204194801281](H:\Mi unidad\Classroom\DAW-EC_PRE\practicas-angular\image-20240204194801281.png)

14. Puedes usar un código parecido a este para el formulario en la plantilla. Esto es solo una muestra para un campo (`nombreCampo`). Intenta hacer tú el resto:

```html
<div class="form-group">
    <label for="nombreCampo">nombreCampo</label>
    <input 
        formControlName="nombreCampo"
        id="nombreCampo" 
        type="text" 
        class="form-control">
         <div *ngIf="f['nombreCampo'].touched && f['nombreCampo'].invalid" class="alert alert-danger">
    <div *ngIf="f['nombreCampo'].errors && 	f['nombreCampo'].errors['required']"> nombreCampo no puede estar vacío </div>
</div>
```

15. Ahora toca el turno del componente **de edición de posts**. El componente es similar al de creación de posts, con algunos matices. El más importante es que este componente tiene que incorporar un parámetro que viene a través de la URL. Por ello tendrá que importar el módulo `ActivatedRoute`. También tendrá que declarar una variable `id`, que sirva para guardar el código del post que se quiere editar. 
16. La clave se encuentra en el método `ngOnInit()`. En él tendremos que incorporar la lógica que localice el post que queremos editar y la de los validadores.
17. Por último tendremos que añadir el método `submit()` que guarde los cambios en la base de datos.
18. En cuanto **a la plantilla del componente de edición**, esta será muy parecida a la de creación de posts, con la salvedad de que ha de incorporar una conexión bidireccional `[(ngModel)]` para visualizar los datos a editar.
19. El último componente del que habrá que ocuparse es el **componente de visualización del post**. En este caso, el componente tendrá una lógica muy parecida a la del componente de edición, sólo que más sencilla, en tanto que no hay que incorporar la funcionalidad de escritura en la base de datos.
20. En cuanto a la plantilla, no plantea especial complicación ya que sólo hay que pasar las variables correspondientes para que se visualicen. El resultado podría ser más o menos así:

![image-20240204201845300](H:\Mi unidad\Classroom\DAW-EC_PRE\practicas-angular\image-20240204201845300.png) 

## Mejoras

- Mejoras estéticas y estructurales
- Sistema de paginación en la página de listados
- Uso de servidor de JSON interno de Angular para manejar persistencia

## Instrucciones de entrega

- Subir el proyecto a repositorio GitHub público
- Preparar un ***codespace*** para poder ejecutar el proyecto. **Comprobar que funciona**
- Incluir README.md con una pequeña memoria del proyecto
