**Angular** is a design framework for building SPAs(Single-Page-Applications) using HTML and TypeScript.
 Angular is written in TypeScript(although the first version, i.e.AngularJS, was written in JS).
 It is built upon a set of TypeScript libraries imported into our app; these libraries implement 
 all the core functionalities required for an app to run.

### Terminologies
Before diving into the tutorial, we will get familiar with the necessary terminologies used
across all the Angular applications.
#### 1. Components
Components are the most fundamental building blocks of any Angular app.
The *Component* is a *class* with a `@Component` decorator that associates
it with the respective template on which the data needs to be rendered.
Together, the *Component* class and *Template*(HTML here) file define a *view*.

- The *Component* class is responsible for exposing all the data and handling its related logic
through different data binding methods.

#### 2. Modules
Angular apps are modular, meaning the application's code 
 can be split up into many modules and has its modularity called 
 *NgModules*. NgModules are like containers that hold a segment of code 
 dedicated to an application workflow, and every app has at least one NgModule.
 
 - NgModules can contain *Component* files and other code files too, whose scope is defined by the NgModule in which they are defined.
 - Every Angular app has the *root* module named `AppModule`,
 which resides in the file `app.module.ts` in the `src` folder.
 - Angular apps can have one or more modules depending on 
 the application's versatility.

To understand more about *Modules and Components*. Refer to this [link](https://angular.io/guide/architecture).

### Data Binding

Data Binding is the means through which the TypeScript code communicates
with the *template*(HTML). There might be instances where one might need to
fetch data from a server/perform some calculations; the user need not be 
aware of all these details. The user is more concerned about the content 
that gets displayed. In cases, like these Data Binding comes in handy. 

Data Binding offers us different ways of communication:
- To Output data from our TypeScript code into the template, we could use 
String Interpolation and Property Binding.
- To get a user's reaction through the template into the TypeScript code,
we could use Event Binding.
- Two-way Binding combines both the abilities of the mentioned above.

Now let's look at how each of the data binding methods work.

### String Interpolation
String Interpolation provides us the provision of including *calculated strings* into marked-up text between HTML element tags.

It uses double curly braces(`{{..}}`) as its delimiter.

```html
<div class="container-fluid">
  <div class="jumbotron">
    <h1 class="display-4">Data Binding with Angular</h1>
    <hr>
    <div class="card" style="width: 18rem;">
      <div class="card-body">
        <h6 class="card-title">Student Details</h6>
        <hr>
        <h6 class="card-subtitle mb-2">Student ID : {{ studentId  }}</h6>
        <h6 class="card-subtitle mb-2">Student Name : {{ getStudentName()  }}</h6>
      </div>
    </div>
  </div>
</div>

```

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-string-interpolation',
  templateUrl: './string-interpolation.component.html',
  styleUrls: ['./string-interpolation.component.css']
})

export class StringInterpolationComponent {
  studentId = 100282;
  studentName = 'ALEXA';

  getStudentName(): string {
    return this.studentName;
  }
}
```

These produce an output, as shown below:
![String Interpolation](engineering-education/articles/data-binding-with-angular/string_Interpolation.png)
 
From the above output, the value of `studentId` from the TypeScript code gets displayed onto the template through
the delimiter `{{ }}`.

We can also pass methods(here, `getStudentName()`) as an expression to the delimiter as
described in the above code. It converts all the expressions into strings and  interpolates 
the result to an element or component.

Not everything that we insert into the delimiter gets interpolated.
A few exceptions come with string interpolation i.e. if the expression includes:

- assignment operators like `=`, `+=`, `-=`, and so on.
- increment `++` and decrement `--` operators.
- operators like `new`, `instanceof`, etc.

Also, note that the same variable cannot be used more than once in any
other interpolation delimiter within the same template file.

### Property Binding
Property Binding allows us to bind a variable's value from the *component* to a property in the *template*.

Here's an example that explains the working of property binding:

```html
<button class="btn btn-primary"
        [disabled]="!addNewStudent">
          Add new Student
</button>
```

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-property-binding',
  templateUrl: './property-binding.component.html',
  styleUrls: ['./property-binding.component.css']
})

export class PropertyBindingComponent{
  addNewStudent = false;

  //The button gets enabled after 2000ms.
  constructor() {
    setTimeout(() => {
      this.addNewStudent = true;
    }, 2000);
  }
}
```

![property-binding-disabled](engineering-education/articles/data-binding-with-angular/property-binding-disabled.png)

![property-binding-enabled](engineering-education/articles/data-binding-with-angular/property-binding-enabled.png)

Here, in the code above. The `disabled` *element property* of a button is bound to the value of the *component* variable `addNewStudent`.
- An *element property* enclosed between `[..]` identifies itself as the *target property*,
 that binds to a component variable.
- There is an alternative syntax too, where `bind-` prefix must be added before the *target property*.

```html
<button class="btn btn-primary"
        bind-disabled="!addNewStudent">
      Add new Student
</button>
```

- Although *element property* are more common targets, we can also give custom property names. Angular first checks if the name is a property declared within the component and the built-in ones next; if it fails to find any, it gives an `unknown directive` error.

An example of such would be:

```typescript
// src/app/app.component.ts

newStudent= 'Keanu Reeves';
```
```html
<!--  src/app/app.component.html-->

<app-student [studentName]="newStudent"></app-student>
<!-- studentName acts as a custom property here-->    
```
```typescript
// src/app/student/student.component.ts

@Input() studentName: string;  
//Through @Input() we can now use the value of newStudent.
```

- The `[..]` are indeed necessary to evaluate the template expression. If omitted, Angular
initializes the *target property* to the variable's type declared inside the component.


Event Binding allows us to register user actions like mouse clicks, keystrokes, and
touches. The *target name* is enclosed within `(..)`, which is the syntax.

```html
<!--component.html-->

<input type="text"
       class="form-control"
       (input)="onUpdateStudent($event)"/>
<div>
  <p>{{ studentID }}</p>
</div>
<button class="btn btn-primary"
        [disabled]="!addNewStudent"
        (click)="onAddStudent()">
  Add new Student
</button>
<p *ngIf="clicked">{{ status }}</p>
```

```typescript
//ts file

export class PropertyBindingComponent{
  addNewStudent = false;
  status = '';
  studentID = '-';
  clicked = false;

  onAddStudent(): void {
    this.status = 'Student with ID-' + this.studentID + ' has been created';
    this.addNewStudent = false;
    this.clicked = true;

    // the button gets disabled once clicked by the user, and the ID gets displayed.
  }

  onUpdateStudent(event: Event): void {
    this.studentID = (event.target as HTMLInputElement).value;
  }
}
```
![event-binding](engineering-education/articles/data-binding-with-angular/event-binding.gif =600x400)

From the above code, it can be seen that there are indeed two event binding listeners.
- First is the *target event*`(input)` in the *input* element. When the user enters data in the *template*, triggers `onUpdateStudent()` in the *component* file.
- The `$event` passed as a parameter holds the information about the binding. This information can include data values like an *object*,  *number* or *string*.
  If the *target event* is a native DOM element event, then `$event` is a *DOM event object* with `target` and `target.value` behaving as properties for that event object[1].
- In the `onUpdateStudent($event)`, we assign `studentID` a new value entered by the user.
- After entering, when the user clicks on the *Add new student* button, the event `(click)` gets registered and `onAddStudent()` is triggered from the *component* file.
- Like with *Property Binding*, there's an alternative syntax as well, `on-` prefix without the braces must be added before the *target element*.

For example:

```html
<input type="text"
       class="form-control"
       on-input="onUpdateStudent($event)"/>
```

The *element property* can be custom events too. Angular first checks whether the registered event is a name of a custom event and then checks for the built-in one, if it fails it will throw an `unknown directive` error.

### Two-way Binding
Two-way binding is a special kind of data binding method as it combines the techniques of both *property binding* & *event binding*.

It does two things:
- Listen to the changes of an event element.
- Set a value to that element property.

The Two-way binding uses the *Banana in a box* syntax `[(..)]`. The same example used in the event-binding will be used here, with some modifications in the *template* file and *component* file.

```html
<!--component.html-->

<input type="text"
       class="form-control"
       [(ngModel)]="studentID" />
<div>
  <p>{{ studentID }}</p>
</div>
<button class="btn btn-primary"
        [disabled]="!addNewStudent"
        (click)="onAddStudent()">
  Add new Student
</button>
<p *ngIf="clicked">{{ status }}</p>
```

```typescript
// ts file

export class PropertyBindingComponent{
  addNewStudent = true;
  status = '';
  studentID = '-';
  clicked = false;

  onAddStudent(): void {
    this.status = 'Student with ID-' + this.studentID + ' has been created';
    this.addNewStudent = false;
    this.clicked = true;
  }

  onUpdateStudent(event: Event): void {
    this.studentID = (event.target as HTMLInputElement).value;
  }
}
```

![two-way-binding](engineering-education/articles/data-binding-with-angular/two-way-binding.gif =600x400)

- The NgModel used here is a [*directive*](https://angular.io/guide/built-in-directives#ngModel) that displays the data property and updates the property when the user makes the changes. It requires *FormsModule* to be imported before using it.
- The `studentID` variable is registered to user input changes and gets updated simultaneously.

#### Additional Resources
- [1] [Event Binding](https://angular.io/guide/event-binding)
- To access my complete code. Click [here.](https://github.com/MahanteshR/dataBinding.git)
- [Angular Docs](https://angular.io/docs).