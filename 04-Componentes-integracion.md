# Avanzando con componentes: Integración

Ya hemos visto cuáles son los elementos principales de un componente en Angular.

En Angular, los componentes pueden conectarse de dos maneras: Como componentes ***standalone***, o mediante **módulos.**

## Componentes *standalone*

Son componentes que directamente pueden importar otros componentes, módulos o directivas para su uso. El equipo de Angular recomienda, en la medida de lo posible, usar este tipo de componentes en los nuevos desarrollos. Para definir un componente *standalone* es necesario declarar el metadato `standalone: true` en su decorador, aunque a partir de la versión 17, ni siquiera es necesario, ya que es el modo de trabajo por defecto.

Juntos, **un componente y su plantilla definen una *vista***. Un componente puede contener una *jerarquía de vistas* que permite definir áreas arbitrariamente complejas de la pantalla.

![View hierarchy](H:\Mi unidad\Classroom\DAW-EC_PRE\carpeta_clase\Apuntes\view-hierarchy.png)

Cuando se crea un componente, se asocia directamente con una sola vista, llamada *vista host*. La vista host puede ser la raíz de una jerarquía de vistas, que puede contener *vistas incrustadas*, que a su vez son las vistas de host de otros componentes.

## *NgModules*

Es la otra manera de integrar componentes en una aplicación Angular. Esta manera era la utilizada en las versiones más antiguas de Angular. Los `NgModules` son contenedores de código dedicado a un dominio concreto de aplicación, un flujo de trabajo o un conjunto de capacidades estrechamente relacionadas. Los módulos en Angular pueden contener componentes, proveedores de servicios y otros archivos de código cuyo alcance está definido por el propio `NgModule` que los contiene. Pueden importar la funcionalidad que se exporta desde otros módulos y exportar la funcionalidad seleccionada para que la utilicen otros módulos.

Si un proyecto se declara con el modificador `standalone=False`,  la aplicación Angular incorporará una clase llamada convencionalmente `AppModule` y que se ubicará en un archivo llamado `app.module.ts`. La mayoría de las aplicaciones de tamaño medio suelen incorporar varios *módulos de funcionalidad*.

Un `NgModule` , al igual que los componentes, se define por una clase y un decorador. Los metadatos más importantes de un `NgModule` son:

- `declarations`: Los componentes, *directivas*, y *pipes* que pertenecen a este NgModule.
- `exports`: El subconjunto de declaraciones que deberían ser visibles y utilizables en las *plantillas de componentes* de otros NgModules.
- `imports`: Otros módulos cuyas clases exportadas son necesarias para las plantillas de componentes declaradas en *este* NgModule.
- `providers`: Creadores de [servicios](https://docs.angular.lat/guide/architecture-services) que este NgModule aporta a la colección global de servicios; se vuelven accesibles en todas las partes de la aplicación. (También puedes especificar proveedores a nivel de componente, que a menudo es preferido).
- `bootstrap`: La vista principal de la aplicación, llamado el *componente raíz*, que aloja todas las demás vistas de la aplicación. Sólo el *NgModule raíz* debe establecer la propiedad `bootstrap`.

Los módulos proporcionan un *contexto de compilación* para sus componentes. Un `NgModule` raíz siempre tiene un componente raíz que se crea durante el inicio del proyecto, pero cualquier `NgModule` puede incluir cualquier cantidad de componentes adicionales, que compartirán un contexto de compilación.

![Component compilation context](H:\Mi unidad\Classroom\DAW-EC_PRE\carpeta_clase\Apuntes\compilation-context.png)

