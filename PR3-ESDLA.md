# PR3. Proyecto ESDLA

## Desarrollar una aplicación básica que permita gestionar un listado de personajes de la serie El Señor de los Anillos (ESDLA)

#### Indicaciones

1. Crear un nuevo proyecto Angular en la carpeta donde estemos trabajando los distintos proyectos. Llama al proyecto `esdla`
2. En la aplicación raíz utiliza estos estilos:

```css
/* Application-wide Styles */
h1 {
  color: #369;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 250%;
}
h2, h3 {
  color: #444;
  font-family: Arial, Helvetica, sans-serif;
  font-weight: lighter;
}
body {
  margin: 2em;
}
body, input[type="text"], button {
  color: #333;
  font-family: Cambria, Georgia;
}
/* everywhere else */
* {
  font-family: Arial, Helvetica, sans-serif;
}
```

3. Crear un componente que se llame `personaje` como ya has visto y conectar el componente con la `app-root`
4. Crear una interfaz para un personaje. Tiene que contener un id, un nombre, y un nivel
5. Introduce una propiedad en el componente `personaje` que defina las características de un personaje, basado en la interfaz que has definido
6. Muestra en la plantilla los detalles del personaje que has definido en el componente
7. Vamos a usar un concepto nuevo: un **pipe**. Usamos el pipe incorporado *uppercase*

> Un [pipe](https://docs.angular.lat/guide/pipes) ("pipe") Es adecuado para formatear cadenas, importes monetarios, fechas y otros datos de visualización. Angular viene con múltiples pipes incorporadas, aunque puedes crear las tuyas propias.

​	El código de la plantilla del componente personaje quedaría así:

```html
<h2>{{personaje.nombre | uppercase}} Detalles:</h2>
<div><span>id: </span>{{personaje.id}}</div>
<div><span>Nombre: </span>{{personaje.nombre}}</div>
<div><span>Nivel: </span>{{personaje.nivel}}</div>
```

8. Introduce la posibilidad de que el usuario edite el nombre del personaje. Para ello introduce el HTML que defina el formulario correspondiente. El código sería el siguiente:

```html
<div>
  <label>name:
    <input [(ngModel)]="hero.name" placeholder="name"/>
  </label>
</div>
```

Con `[(ngModel)]="hero.name"` hemos introducido lo que en Angular se denomina ***enlace de datos bidireccional***

> Un enlace bidireccional vincula la propiedad al cuadro de texto HTML, por lo que puede pasar datos *en ambas direcciones* desde la propiedad al cuadro de texto y desde el cuadro de texto a la propiedad

Si aparece un error en la aplicación es porque no hemos hecho las importaciones correspondientes. Importa en los metadatos del componente raíz `FormsModule` 

9. El siguiente paso es crear un listado de personajes. Para ello crearemos un array de objetos con el interfaz `personaje` en un archivo `datosPersonajes.ts` en el raíz de `app`

10. Habrá que añadir la propiedad `personajes ` al componente `personaje` para cargar los datos. (Cuidado con las importaciones...)

11. Ahora ya podemos añadir el HTML correspondiente a la plantilla del componente `personaje`. Usaremos la directiva `*ngFor`, como ya hemos visto antes.

    ```html
    <ul>
      <li *ngFor="let hero of heroes"
        <span>{{personaje.id}}</span> {{personaje.nombre}}
      </li>
    </ul>
    ```

Otro tema que tenemos que aprender bien es a desarrollar estructuras **maestro/detalle**

> Una estructura **maestro/detalle** permite presentar datos con una serie reducida de atributos en forma de lista, de modo que cuando hacemos clic en algún registro de la lista se posibilita el acceso a conjunto completo de atributos de dicho registro

12. Primero tendremos que añadir en el componente el método que asignamos al evento de clic en la plantilla. Para ello podemos usar este código:

    ```ts
    personajeSel: Personaje; // declara la propiedad personajeSel para guardar el personaje sobre el que hacemos clic
    
    onSelect(personaje: Personaje): void {
      this.personajeSel = personaje;
    }
    ```

13. Ahora ya podemos añadir el gestor de evento `(clic)` a la plantilla HTML

14. Para visualizar los detalles del personaje sobre el que hemos hecho clic podemos añadir este código al final de la plantilla HTML del componente `personaje`

    ```html
    <h2>Detalles de personaje</h2>
    <div><span>id: </span>{{personajeSel.id}}</div>
    <div>
      <label>Nombre:
        <input [(ngModel)]="personajeSel.nombre" placeholder="nombre"/>
      </label>
    </div>
    <div><span>Nivel: </span>{{personaje.nivel}}</div>
    ```

15. La aplicación falla porque al iniciarse, la conexión con la propiedad `personajeSel ` no se ha establecido. Sólo ocurre cuando hacemos clic en el listado. Para resolver esto podemos usar la otra directiva que también ya conocemos: `*ngIf*`, de modo que sólo se tenga en cuenta esa parte del HTML cuando `personjeSel` tenga un valor. Bastaría envolver el código anterior en un `<div *ngIf="personajeSel">`

16. Por último podemos trabajar un poco los estilos, sobre todo al seleccionar un personaje del listado. Aquí tienes un contenido CSS para añadir a los estilos privados del componente `personaje`. Ya sabes como hacerlo:

    ```css
    /* PersonajeComponent's private CSS styles */
    .personajes {
      margin: 0 0 2em 0;
      list-style-type: none;
      padding: 0;
      width: 15em;
    }
    .personajes li {
      cursor: pointer;
      position: relative;
      left: 0;
      background-color: #EEE;
      margin: .5em;
      padding: .3em 0;
      height: 1.6em;
      border-radius: 4px;
    }
    .personajes li:hover {
      color: #607D8B;
      background-color: #DDD;
      left: .1em;
    }
    .personajes li.selected {
      background-color: #CFD8DC;
      color: white;
    }
    .personajes li.selected:hover {
      background-color: #BBD8DC;
      color: white;
    }
    .personajes .badge {
      display: inline-block;
      font-size: small;
      color: white;
      padding: 0.8em 0.7em 0 0.7em;
      background-color:#405061;
      line-height: 1em;
      position: relative;
      left: -1px;
      top: -4px;
      height: 1.8em;
      margin-right: .8em;
      border-radius: 4px 0 0 4px;
    }
    ```

17.  También tienes unos estilos definidos en el archivo CSS de la aplicación raíz. Intenta aplicar los estilos que ves en ese fichero en la medida de lo posible
