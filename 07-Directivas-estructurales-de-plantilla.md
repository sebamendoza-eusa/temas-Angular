# Sintaxis de plantilla para estructuras condicionales y repetitivas: Directivas `@if`, `@for` y `@switch`

A partir de la versión 17 Angular ha introducido nuevas formas de controlar el flujo en las plantillas de los componentes.

Ya conocemos la estructura `*ngIf` para el renderizado condicional en nuestras plantillas, ahora se tiene una estructura más parecida a la sintaxis de JavaScript que nos permitirá tener esta misma funcionalidad en nuestras plantillas. Es una estructura con sintaxis de bloque, y con ella el compilador de angular la transforma en instrucciones de JavaScript.

¿Por qué son más interesantes estas nuevas estructuras? Por las siguientes razones

- Sintaxis más parecida a JavaScript, lo que hace que sea más familiar su uso
- Su uso está automáticamente disponible sin realizar importaciones adicionales
- Mejoras en el rendimiento

Esta nueva estructura funcionará para `*ngIf`, `*ngSwitch` y `*ngFor`, sustituyendo las directivas anteriores por `@if`, `@for` y `@switch`

Vamos a ver un ejemplo del cambio en el control de flujo. Hasta la versión 17, la forma de proceder era la siguiente:

```html
<div *ngIf="showTable; else showList">
  <table>
    <!-- full table -->
  </table>
</div>

<ng-template #showList>
  <ul>
    <li><!-- full list --></li>
  <ul>
</ng-template>
```

En este caso tenemos un código que dependiendo del valor de `showTable` mostrará o no la tabla html.

Con las nuevas estructuras, el código quedaría como sigue:

```html
<!-- *ngIf -->
@If (showTable) {
  <table>
    <!-- full table -->
  </table>
} @else {
  <ul>
    <li><!-- full list --></li>
  <ul>
}
```

Como puede comprobarse el código resultante es mucho más legible.

El uso con las otras estructuras es similar. Veamos algunos ejemplos:

Uso de `@for`

```html
@for (item of items; track item.id) {
  <p>{{ item }}</p>
}
```

La única salvedad en este caso es que **es obligatorio declarar la propiedad `track` para mejorar el rendimiento**

Para el caso de `@switch`, la mejora es mucho mayor en legibilidad ya que de un código del tipo 

```html
<div [ngSwitch]="color">
  <app-color *ngSwitchCase="red"/>
  <app-color *ngSwitchCase="blue"/>
  <app-default-color *ngSwitchDefault />
</div>
```

pasaríamos a uno del tipo:

```html
@switch (color) {
  @case ('red') {
    <app-red-color />
  }
  @case ('blue') {
    <app-blue-color />
  }
  @default {
    <app-default-color />
  }
}
```
