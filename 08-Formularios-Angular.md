# Formularios en Angular

Angular proporciona dos enfoques diferentes para manejar formularios:

- Formularios basados en plantillas
- Formularios reactivos

Ambos capturan los eventos de entrada del usuario desde la vista, crean un modelo de formulario y un modelo de datos para actualizar y proporcionan una forma de rastrear los cambios.

Tanto los formularios reactivos como los formularios basados en plantillas procesan y administran los datos de los formularios de manera diferente. Cada enfoque ofrece sus ventajas.

| Enfoque               | Detalles                                                     |
| :-------------------- | :----------------------------------------------------------- |
| Reactivos             | Proporcionan acceso directo y explícito al modelo de objetos del formulario subyacente. En comparación con los formularios basados en plantillas, son más robustos: son más escalables, reutilizables y comprobables. Si los formularios son una parte clave de la aplicación, o si ya está usando patrones reactivos para compilar la aplicación, mejor usar formularios reactivos. |
| Basados en plantillas | Confían en las directivas para crear y manipular el modelo de objetos subyacente de modo implícito. Son útiles para agregar un formulario simple a una aplicación, como por ejemplo un formulario de registro de lista de correo electrónico. Son fáciles de agregar a una aplicación, pero no se escalan tan bien como los formularios reactivos. Si los requisitos del formulario son muy básicos y la lógica se puede administrar en la plantilla, los formularios basados en plantillas son una buena opción. |

En la tabla siguiente se resumen las principales diferencias entre los formularios reactivos y los basados en plantillas.

|                                                              | Reactivos                                   | Basado en plantillas             |
| :----------------------------------------------------------- | :------------------------------------------ | :------------------------------- |
| [Configuración del modelo de formulario](https://angular.dev/guide/forms#setting-up-the-form-model) | Explícito, creado en la clase de componente | Implícito, creado por directivas |
| [Modelo de datos](https://angular.dev/guide/forms#mutability-of-the-data-model) | Estructurado e inmutable                    | Desestructurado y mutable        |
| [Flujo de datos](https://angular.dev/guide/forms#data-flow-in-forms) | Síncrono                                    | Asíncrono                        |
| [Validación de formularios](https://angular.dev/guide/forms#form-validation) | Funciones                                   | Directivas                       |

Vamos a ejemplificar ambos enfoque con el desarrollo de un formulario típico de validación de usuario. Introduciremos y gestionaremos la siguiente información.

- Nombre de pila de la persona que se registra.
- Email de registro.
- Password.
- Repetir Password.

## Formularios basados en plantillas

Comencemos con el código del componente asociado al formulario

```ts
import { Component } from "@angular/core";

interface IRegisterForm {
  name: string;
  email: string;
  password: string;
  repeatPass: string;
}

@Component({
  selector: "app-template-forms",
  templateUrl: "./template-forms.component.html",
  styleUrls: ["./template-forms.component.css"]
})
export class TemplateFormsComponent {
  register: IRegisterForm = {
    name: "",
    email: "",
    password: "",
    repeatPass: ""
  };
    
  constructor() {}
    
  submit() {
    // Llegados a este punto, ya hemos podido validar todo excepto las contraseñas y ya recibimos los datos
    console.log("Datos de inicio de sesión");
    console.log(this.register.name);
    console.log(this.register.email);
    console.log(this.register.password);
    console.log(this.register.repeatPass);

    // Controlar si el password y el password verificado son iguales
    if (this.register.password !== this.register.repeatPass) {
      // Emitir alerta POR NO SER IGUALES Y NO DEJAR ENVIAR DATOS
      console.log(
        "Hay que introducir las dos contraseñas iguales para validarlo"
      );
      // Echar un mensaje de alerta
      return;
    }

    // Con estos datos ya todo OK, los enviamos al servidor para comprobar si el login es OK o NO
  }
}
```

Podemos identificar claramente las partes dentro del componente: La interface, las propiedades y los métodos utilizados

A partir de ahora viene lo más complejo: El trabajo con la plantilla HTML del componente.

Para crear el apartado del formulario en la plantilla, debemos de añadir lo siguiente:


```html
<div class="container">
	<form (ngSubmit)="submit()" #forma="ngForm">
	
	</form>
</div>
```

Hemos de tener en cuenta lo siguiente:

- **ngSubmit**“ es el evento que se ejecuta cuando pulsamos el botón de submit (que todavía no está creado).
- **#forma**: es la referencia por plantilla, donde tendremos la información de si ese formulario es válido o no
- **“ngForm”**: Valor que se asignada a la referencia por template para usar esa clase que se encuentra dentro de `FormsModule`.

Ahora habría que ir trabajando los campos de manera individual. Para el nombre tendríamos:

```html
<label for="name"><b>Nombre</b></label>
    <input
		type="text"
        #name="ngModel"
        [(ngModel)]="register.name"
        placeholder="Introduzca su nombre de pila"
        name="name"
        minlength="5"
        class="form-control"
        required
        [ngClass]="{'is-valid': !name.invalid, 'is-invalid': name.invalid}"
        />
```

Repasemos algunas de las propiedades del elemento `input`

- **type: [text | number | email]**: Tipo de dato que es. En este caso al ser un nombre de pila, usaremos “text”.
- **#name**: Variable de plantilla relacionada con el campo del formulario correspondiente al nombre de pila. Es un valor que tenemos que tener en cuenta para las validaciones, para saber si ese valor es válido o inválido, por lo que es importante elegir un nombre acorde a lo que se va a introducir.
- **[(ngModel)]**: Para asignar el valor según la propiedad que hemos inicializado en el componente.
- **[ngClass]=“{‘valid’…}”**: Se determina por condición, si ese campo es válido o no, para que se aplique una clase CSS dinámicamente. Lo que en este caso corresponde a “name” es lo que hemos definido como variable de plantilla.

Vamos a ver como se gestionan los errores. Para ello habría que añadir un código como el siguiente:

```html
<div class="invalid-feedback" *ngIf="name.invalid && (name.dirty || name.touched)">
    <div *ngIf="name.errors.minlength">
        Introduzca un nombre de pila con 5 caracteres mínimo
    </div>
    <div *ngIf="name.errors.required">
        Introduzca un nombre de pila válido por favor
    </div>
</div>
```

Este trozo del código se activa siempre y cuando “name” (del valor de la variable de plantilla que sería #name) detecta que es inválido, por no cumplir unos requerimientos mínimos que hemos establecido, como que es obligatorio (required) y tenemos que añadir un mínimo de 5 caracteres (minlength = 5).

El resto del formulario se trabajaría de forma parecida. Veámoslo a continuación al completo

```html
<div class="container">
	<form (ngSubmit)="submit()" #forma="ngForm">
		<h3>Inicio de sesión - Formulario por template</h3>
		<hr />
		<label for="name"><b>Nombre</b></label>
		<input
            type="text"
            #name="ngModel"
            [(ngModel)]="register.name"
            placeholder="Introduzca su nombre de pila"
            name="name"
            minlength="5"
            class="form-control"
            required
            [ngClass]="{'is-valid': !name.invalid, 'is-invalid': name.invalid}"
          />

		<div class="invalid-feedback" *ngIf="name.invalid && (name.dirty || name.touched)">
			<div *ngIf="name.errors.minlength">
				Introduzca un nombre de pila con 5 caracteres mínimo
			</div>
			<div *ngIf="name.errors.required">
				Introduzca un nombre de pila válido por favor
			</div>
		</div>
		
		<label for="email"><b>Correo electrónico</b></label>
		<input
            type="email"
            #email="ngModel"
            [(ngModel)]="register.email"
            placeholder="Introduzca la correo electrónico"
            name="email"
            class="form-control"
            required
            [ngClass]="{'is-valid': !email.invalid, 'is-invalid': email.invalid}"
            pattern="^[a-zA-Z0-9._-]+@[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?(?:\.[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?)*$"
          />

		<div class="invalid-feedback" *ngIf="email.invalid && (email.dirty || email.touched)">
			<div *ngIf="email.errors.required">
				Introduzca un correo electrónico válido por favor {{ email.value }}
			</div>
			<div *ngIf="email.errors && email.errors.pattern">
				El correo electrónico no sigue el formato correcto. Sigue este ejemplo: nombre@dominio.com
				{{ email.value }}
			</div>
		</div>

		<label for="psw"><b>Contraseña</b></label>
		<input
            type="password"
            placeholder="Introduzca la contraseña"
            #password="ngModel"
            [(ngModel)]="register.password"
            name="password"
            class="form-control"
            required
            [ngClass]="{'is-valid': !password.invalid, 'is-invalid': password.invalid}"
          />
                
		<div class="invalid-feedback" *ngIf="password.invalid && (password.dirty || password.touched)">
			<div *ngIf="password.errors.required">
				Introduzca una contraseña por favor
			</div>
		</div>

		<label for="psw"><b>Repetir Contraseña</b></label>
		<input
            type="password"
            placeholder="Repetir la contraseña"
            #repeatPass="ngModel"
            [(ngModel)]="register.repeatPass"
            name="repeatPass"
            class="form-control"
            required
            [ngClass]="{'is-valid': !repeatPass.invalid, 'is-invalid': repeatPass.invalid}"
          />
                
		<div class="invalid-feedback" *ngIf="repeatPass.invalid && (repeatPass.dirty || repeatPass.touched)">
			<div *ngIf="repeatPass.errors.required">
				Introduzca una contraseña por favor
			</div>
		</div>

		<br>
		<div class="row">
			<div class="col-lg">
				<button type="submit" [disabled]="!forma.valid" class="signup-btn">Registro</button>
			</div>
		</div>
	</form>
</div>
```

Al final de todos los campos, tenemos el elemento **“button”** de tipo **“submit”** y dentro de él tenemos la directiva `[disabled]` que hará que esté o no esté disponible, dependiendo de si el formulario es válido.

Dentro de ese valor tenemos **“!forma.valid”** que querrá decir, que mientras el formulario no sea válido, se activará la directiva **“[disabled]”** y eso hace que el botón de envío no esté disponible.

Cuando finalmente se envíe el formulario, se hará invocando al método `submit()` de la clase del componente. Este método gestionará las acciones sucesivas, que en nuestro caso será sacar la información enviada por consola.

## Formularios reactivos

Una vez que hemos trabajado en los formularios controlados por plantillas donde la mayor parte de la interacción se producía en la plantilla, vamos a usar el enfoque reactivo, en el cual **la mayor parte de la interacción se produce en el componente**.

Vamos a trabajar con el mismo ejemplo base anterior: un formulario de registro de un usuario.

Empezaremos a trabajar en el componente, que es donde más lógica vamos a implementar. El flujo de datos, la validación o su disponibilidad tendrán que insertarse aquí. El código inicial sería el siguiente más o menos:

```ts
import { Component, OnInit } from "@angular/core";
import { FormGroup, Validators, FormBuilder } from "@angular/forms";

@Component({
  selector: "app-reactive-forms",
  templateUrl: "./reactive-forms.component.html",
  styleUrls: ["./reactive-forms.component.css"]
})
export class ReactiveFormsComponent implements OnInit {
  registerForm: FormGroup;
  submitted = false;

  constructor(private formBuilder: FormBuilder) {}

  ngOnInit() {
    this.registerForm = this.formBuilder.group(
      {
        name: ["", Validators.required],
        email: ["", [Validators.required, Validators.email]],
        password: ["", [Validators.required, Validators.minLength(6)]],
        repeatPass: ["", Validators.required]
      },
      {
        validator: this.MustMatch("password", "repeatPass") // Validando
      }
    );
  }
```

En el código anterior pueden diferenciarse dos bloques principales.

El primero corresponde a las inicializaciones principales de lo que se necesita para empezar a trabajar.

- Aquí se añade la propiedad **`registerForm: FormGroup`**, es es donde tendremos todas las propiedades de todos los campos que van a componer ese formulario como aspectos de si son obligatorios, longitud mínima o tipo de campo
- Añadimos una **propiedad `submitted`** para hacer un control de si se ha pulsado o no el botón de enviar, con idea de gestionar la información generada
- Finalmente, en el constructor inyectamos la clase `FormBuilder`, que será la encargada de construir el formulario con sus datos por defecto y la configuración acerca de si son obligatorios o no, las validaciones u otras opciones que sean necesarias. Una vez construido, lo asignamos a `registerForm`, y así añadirlo en la plantilla del componente.

En el segundo apartado, ya más centrados en el agrupamiento de los campos, se detallan las configuraciones individuales como valor por defecto, si es requerido, tipo de dato, etc.

Dentro del segundo bloque de código podemos distinguir a su vez dos partes diferenciadas:

- Por un lado, los campos que compondrán el formulario. Cada uno con un apartado donde se añaden las opciones básicas: Valor por defecto, y validaciones.
- En segundo lugar, se añaden otras opciones, en este caso una validación personalizada, como comprobar que dos contraseñas son iguales.

La función que define la validación de las contraseñas se implementa más abajo como un método de clase. Veamos el código:

```ts
MustMatch(controlName: string, matchingControlName: string) {
    return (formGroup: FormGroup) => {
      const control = formGroup.controls[controlName];
      const matchingControl = formGroup.controls[matchingControlName];

      if (matchingControl.errors && !matchingControl.errors.mustMatch) {
        // return if another validator has already found an error on the matchingControl
        return;
      }

      // set error on matchingControl if validation fails
      if (control.value !== matchingControl.value) {
        matchingControl.setErrors({ mustMatch: true });
      } else {
        matchingControl.setErrors(null);
      }
    };
```

Por último, para terminar el componente, habrá que añadir tres funciones más que son necesarias para trabajar con la plantilla. El código es el siguiente:

```ts
get form() {
    return this.registerForm.controls;
  }

onSubmit() {
	this.submitted = true;

// stop here if form is invalid
	if (this.registerForm.invalid) {
  		return;
	}

    // display form values on success
    alert(
      "SUCCESS!! :-)\n\n" + JSON.stringify(this.registerForm.value, null, 4)
    );
}

onReset() {
	this.submitted = false;
	this.registerForm.reset();
}
```

- **`get form()`**: Es un *getter* que se usa para simplificar la propiedad del formulario de registro cuando se vaya a usar en el control de los diferentes campos. Así se consigue que, en la plantilla, **en vez de escribir *registerForm.controls*, con *form*, se tenga el mismo resultado**.
- **`onSubmit()`**: Función donde se controla la validez del formulario. En el caso de ser válido, se seguirá hacia delante para enviar los datos al backend.
- **`onReset()`**: Resetea el formulario.

Llegados a este punto podemos centrar ahora el trabajo **en la plantilla del componente**.  Se trata de elaborar una estructura base visual del formulario.

El código de la plantilla sería el siguiente:

```html
<div class="container">
    
	<form (ngSubmit)="onSubmit()" [formGroup]="registerForm">

		<h3>Inicio de sesión - Formulario reactivo</h3>
        
		<label>Nombre</label>
		<input type="text" formControlName="name" class="form-control" [ngClass]="{ 'is-invalid': form.name.errors }" />
		<div *ngIf="form.name.errors" class="invalid-feedback">
			<div *ngIf="form.name.errors.required">El nombre es obligatorio</div>
		</div>
        
		<label>Email</label>
		<input type="text" formControlName="email" class="form-control" [ngClass]="{ 'is-invalid': form.email.errors }" />
		<div *ngIf="form.email.errors" class="invalid-feedback">
			<div *ngIf="form.email.errors.required">El email es obligatorio</div>
			<div *ngIf="form.email.errors.email">El email debe de ser válido</div>
		</div>

		<label>Contraseña</label>
		<input type="password" formControlName="password" class="form-control" [ngClass]="{ 'is-invalid': form.password.errors }" />
		<div *ngIf="form.password.errors" class="invalid-feedback">
			<div *ngIf="form.password.errors.required">Se requiere una contraseña</div>
			<div *ngIf="form.password.errors.minlength">La contraseña tiene que tener al menos 6 caracteres</div>
		</div>

		<label>Repetir contraseña</label>
		<input type="password" formControlName="repeatPass" class="form-control" [ngClass]="{ 'is-invalid': form.repeatPass.errors }" />
		<div *ngIf="form.repeatPass.errors" class="invalid-feedback">
			<div *ngIf="form.repeatPass.errors.required">Es necesario repetir contraseña</div>
			<div *ngIf="form.repeatPass.errors.mustMatch">Las contraseñas deben de coincidir</div>
		</div>
		<div class="text-center">
			<button class="btn btn-primary mr-1" [disabled]="registerForm.invalid">Registro</button>
			<button class="btn btn-secondary" type="reset" (click)="onReset()">Cancelar</button>
		</div>
	</form>
</div>
```

Vamos a comentar el código anterior:

- **Selector `form`**: Se añade como en el caso del formulario basado en plantilla, pero se usa el enlace de propiedad para vincular con la propiedad `registerForm` del componente.
- **Evento `ngSubmit `**: Funcionará igual que en el caso del formulario basado en plantillas.
- **Botones de registro y cancelar**: Funcionan igual que en el ejemplo por plantillas
- **Apartados para los distintos campos**: Se desarrollan uno a uno, comenzando por el campo `name`. Comentemos este caso de forma más detallada, ya que los demás bloques son similares. En el `input` tenemos varios elementos que tenemos que analizar. Todos los elementos excepto `formControlName` los conocemos de usarlos en el formulario por plantilla:
  - **`type`**: Puede ser un campo de texto (text), de tipo email,…
  - **`formControlName`**: Nombre que hemos asignado en el componente, dentro de los campos. Importante escribirlo igual, respetando minúsculas y mayúsculas.
  - **`class`**: Usamos la clase `form-control` para aplicar a los controles de formulario. Se trabaja con Bootstrap.
  - **`[ngClass]`**: Se asigna clase válida o inválida, dependiendo de si es válido o no ese campo.
- **Apartados de *feedback* de los errores**: Se trata de mostrar el error (o errores) en el caso de no ser correcto el valor. Por ejemplo, en el caso del email, se tiene que cumplir que se ha escrito algo y que sea de tipo email, si no, nos dará un error.

En el caso que introduzcamos todos los datos y se validen correctamente ya se activará el botón con la opción de *Registro*. En ese momento que hacemos click, se accederá al componente a las acciones subsiguientes. En este caso la de mostrar un `alert` con la información del registro, que a su vez, podríamos enviar al backend correspondiente.

Por último insistir en el hecho de que, aunque los formularios basados por plantillas eran la única posibilidad antes de la versión 11 de Angular y siguen existiendo, **el equipo de Angular recomienda para los nuevos desarrollos el uso de formularios reactivos**.
