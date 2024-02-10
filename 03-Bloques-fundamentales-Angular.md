# Bloques fundamentales de Angular

¿Cuáles son los pilares fundamentales de Angular? Ya hemos tratado uno de ellos en el apartado anterior, el componente. Pero además existen más bloques fundamentales en Angular

Podríamos decir que Angular se compone de **cinco bloques o pilares fundamentales**: 

- Componentes
- Rutas
- Directivas
- Servicios
- Módulos

## Componentes

Ya lo hemos visto antes: Cada aplicación de Angular tiene al menos un componente, el *componente raíz* que conecta una jerarquía de componentes con el modelo de objetos del documento de la página (DOM). Cada componente define una clase que contiene datos y lógica de la aplicación, y está asociado con una *plantilla* HTML que define una vista que se mostrará en un entorno de destino.

El decorador `@Component()` identifica la clase inmediatamente debajo de ella como un componente, y proporciona la plantilla y los metadatos específicos del componente relacionado.

Los **componentes**, vistos así, no son más que bloques de código formados por fragmentos de HTML y CSS en combinación con uno o varios ficheros TypeScript, usualmente en forma de contenedores de clases. La idea es que los componentes sean bloques pequeños y simples en cuanto a código, de tal modo que la aplicación sea una suma de componentes. Es muy importante entender que un componente es **un elemento reutilizable**. 

## Directivas, plantillas y enlaces de datos

Las **directivas** son una parte muy importante del núcleo de Angular. Con ellas se pueden extender las funcionalidades del HTML usando para ello **una nueva sintaxis**. Esta nueva sintaxis nos permitirá incorporar nueva lógica que será ejecutada en el DOM. Las directivas pueden ser de tres tipos: **Directivas de Atributo**, que alteran la apariencia o comportamiento de un elemento del DOM y son usadas como atributos de los elementos; **directivas estructurales**, que permiten añadir, manipular o eliminar elementos del DOM; y las **directivas de componente**, que incorporan plantillas de código HTML que se pueden incorporar en nuestros componentes.

Una plantilla combina HTML con **un lenguaje de marcas propio de Angular** que puede modificar elementos HTML antes de que se muestren. Las *directivas* de la plantilla proporcionan la lógica de programa y el *enlace markup* conecta los datos de tu aplicación y el DOM. Hay dos tipos de enlace de datos:

- *Manejador de eventos* permite que tu aplicación responda a la entrada del usuario en el entorno objetivo actualizando los datos de tu aplicación.
- *Vincular propiedades* te permite interpolar valores que se calculan a partir de los datos de tu aplicación en HTML.

Antes de mostrar una vista, Angular evalúa las directivas y resuelve la sintaxis de enlace en la plantilla para modificar los elementos HTML y el DOM, de acuerdo con los datos y la lógica de tu programa. Angular soporta *enlace de datos en dos sentidos*, lo que significa que los cambios en el DOM, como las elecciones del usuario, también se reflejan en los datos de su programa.

Tus plantillas pueden usar ***pipes*** para mejorar la experiencia del usuario mediante la transformación de valores para mostrar. Por ejemplo, usa pipes para mostrar fechas y valores de moneda que sean apropiados para la configuración regional de un usuario. Angular proporciona pipes predefinidas para transformaciones comunes, y también puedes definir tus propias pipes.

## Enrutamiento

En Angular, las **rutas** pueden entenderse como maneras de conectar con los diferentes componentes de nuestro proyecto. Estos se mostrarían **según sea la URL que se especifique desde el cliente**

El módulo `Router` de Angular proporciona un servicio que te permite definir una ruta de navegación entre los diferentes estados de la aplicación y ver sus jerarquías. Se basa en las convenciones frecuentes de navegación del navegador:

- Cuando se ingresa una URL en la barra de direcciones el navegador irá a la página correspondiente.
- Cuando se haga clic en los enlaces de una página, el navegador nos llevará a una nueva página.
- Cuando hagamos clic en los botones atrás y adelante del navegador, el navegador irá hacia atrás y hacia adelante a través del historial de las páginas que has visto.

El enrutador de Angular mapea rutas (similares a URLs) para navegar por las vistas de los componentes, (en lugar de páginas). Cuando un usuario realiza una acción, como hacer clic en un enlace, que cargaría una nueva página en el navegador, el enrutador intercepta el comportamiento del navegador y muestra u oculta las plantillas HTML de los componentes implicados.

El enrutador interpreta una URL de enlace de acuerdo con las reglas de navegación de visualización de la aplicación y el estado de los datos. Puedes navegar a las nuevas vistas cuando el usuario hace clic en un botón o selecciona desde un cuadro desplegable, o en respuesta a algún otro estímulo de cualquier fuente. El enrutador registra la actividad en el historial del navegador, por lo que los botones de retroceso y avance también funcionan.

Para definir reglas de navegación, **se deben asociar *rutas de navegación* a los componentes**. Una ruta utiliza una sintaxis similar a una URL que integra los datos del programa, de la misma manera que la sintaxis de la plantilla integra dichos datos. Luego puedes aplicar la lógica del programa para elegir qué vistas mostrar u ocultar, en respuesta a la entrada del usuario y a tus propias reglas de acceso.

## Servicios e inyección de dependencia

Para los datos o la lógica que no están asociados con una vista específica y que deseas compartir entre componentes, se puede crear **una clase *servicio***. Una definición de clase servicio está inmediatamente precedida por el decorador `@Injectable()`. El decorador proporciona los metadatos que permiten **inyectar** otros proveedores como dependencias en su clase.

La *Inyección de Dependecia* (ID) es el modo en que se integran los distintos servicios en los respectivos componentes. La *ID* permite mantener la clase de un componente más legible y la hace más eficiente. Evitan tener que reprogramar métodos al obtenerlos directamente del servicio inyectado. Por ejemplo, obtener y trabajar con datos de un servidor o validar la autenticación de usuarios suelen ser tareas delegadas a los servicios.

En resumen, los **servicios** pueden entenderse como lugares centralizados de información, que permiten crear código reutilizable para incorporar en cada componente que lo necesite. Los servicios tienen forma de clases TypeScript. Un patrón de diseño interesante en Angular es hacer recaer la **gestión de los datos** siempre que se pueda en los servicios y no en los componentes. Estos últimos deberían centrarse sobre todo **en presentar los datos al usuario**.

## Módulos

Los **módulos** (*NgModules*) organizan la aplicación en **bloques funcionales**. En versiones anteriores de Angular, cada aplicación tenía, como mínimo, un módulo (el *AppModule*). Dentro de un módulo se declaran los componentes y, si hace falta, se importan otros módulos necesarios para su funcionamiento. A partir de la versión 16 de Angular es posible crear componentes ***standalone***, los cuales no necesitan tener un módulo asociado. Durante este curso seguiremos el patrón de diseño *standalone* siempre que sea posible.

Los *NgModules* de Angular difieren y complementan los módulos JavaScript (ES2015). Un NgModule declara un contexto de compilación para un conjunto de componentes que está dedicado a un dominio de aplicación, un flujo de trabajo o un conjunto de capacidades estrechamente relacionadas. Un NgModule puede asociar sus componentes con código relacionado, como servicios, para formar unidades funcionales.

Cada aplicación en Angular, si no se declara `standalone=true`,  tiene un *módulo raíz*, convencionalmente nombrado `AppModule`, que proporciona el mecanismo de arranque que inicia la aplicación. Una aplicación generalmente contiene muchos módulos funcionales.

Como en los módulos de JavaScript, los NgModules pueden importar la funcionalidad de otros, y permiten que su propia funcionalidad sea exportada y utilizada por otros NgModules. Por ejemplo, para utilizar el servicio de enrutamiento en su aplicación, importa el NgModule `Router`.

Organizar tu código en distintos módulos funcionales ayuda a gestionar el desarrollo de aplicaciones complejas, y en el diseño para su reutilización. Además, esta técnica te permite aprovechar la *carga diferida*—es decir, cargar módulos bajo demanda—para minimizar la cantidad de código que debe cargarse al inicio.

---

Hemos señalado los conceptos básicos acerca de los bloques de construcción de una aplicación en Angular. El siguiente diagrama muestra cómo se relacionan estos conceptos básicos.

![overview](H:\Mi unidad\Classroom\DAW-EC_PRE\apuntes\Angular\images\overview2.png)

Vemos como:

- Un componente y una plantilla definen una vista en Angular.
- Un decorador en una clase componente agrega los metadatos, incluido un apuntador a la plantilla asociada.
- Las directivas y el enlace markup en la plantilla de un componente modifican las vistas basadas en los datos y la lógica del programa.
- El inyector de dependencia proporciona servicios a un componente, como el servicio de enrutamiento que le permite definir la navegación entre vistas.
