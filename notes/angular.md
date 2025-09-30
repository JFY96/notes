# Angular

- application design framework and development platform for creating efficient & sophisticated SPAs
- (like other front end frameworks) is generally used to create single page apps (SPAs) that run on the client, but can be used to create full stack apps by making HTTP requests to a backend server
- Can be run on the server side with something like Angular Universal

## Why Angular?

- Create dynamic frontend apps & UIs
- full featured frameworks (router, http, etc)
- Integrated TypeScript
- RxJS - efficient, asynchronous programming
- Test-friendly
- Popular in enterprise (more strict and standardised than something like React)

## Angular Components

- Components are pieces of the UI including the tempalte (html), logic, and styling
- Components are reusable and can be embedded into the template as an XML-like tag

```ts
// Component Declartion
@Component({
  selector: 'app-food-list', // (html) tag for the component
  templateUrl: './food-list.component.html', // html file
  styleUrls: ['./food-list.component.css'], // style sheets for component
  providers: [ FoodService ]
})
// Class for properties, methods
export class FoodListComponent implements OnInit {
    /* ... */
}
```

## Angular Services

- Angular distinguishes components from services to increase modularity and reusability
- By separating a component's view-related functionality from other kinds of processing, you can make your component classes lean and efficient
- A component can delegate certain tasks to services, such as fetching data from the server, validating user input, or logging directly to the console

## Angular CLI

- Standard tooling for Angular development
- Command line interface for creating Angular apps
- Dev server and easy production build
- Commands to generate components, services, etc

```bash
npm install -g @angular/cli

ng new my-app
```

## Base File Structure

- `package.json`
- `angular.json`
  - change options like `options.outputPath` for build path
  - add `assets`, `styles`, `scripts` to be loaded in globally
- `src`
  - `index.html` - single page that is loaded
  - `main.ts` - entry point to angular, imports app modules
  - `styles.css` - global styling
  - `app` - main application and components, services
    - `app.module.ts`
      - `@NgModule` - ecorator that marks a class as an NgModule and supplies configuration metadata
        - `declarations` - set of components that belong to this module
        - `imports` - set of NgModules whose exported declarables are available to templates in this module
        - `providers` - global services/providers (injectable objects)
        - `entryComponents` - set of components to compile when this NgModule is defined, so they can be dynamically loaded into view
        - `bootstrap` - set of components that are bootstrapped when this module is bootstrapped. Any components listed here are automatically added to `entryComponents`
    - `app.component.ts` - main class and config
      - ```ts
        import { Component } from '@angular/core'; // required for each component
        @Component({
          selector: 'app-root', // tag for the component i.e. this will be <app-root></app-root> in index.html
          templateUrl: './app.component.html', // html template file
          styleUrls: ['./app.component.css'] // style sheets
        })
        // Class with properties and methods (custom or lifecycle methods)
        export class AppComponent {
          title = 'task-tracker';
        }
        ```
    - `app.component.html` - template file for view
      ```html
      <h1>{{title.toUpperCase()}}</h1> <!-- {{}} to use class properties, or calculate anything (uses js) -->
      ```
    - `app.component.css` - styling file for component
    - `app.component.spec.ts` - testing
    - `components`
      - create them using cli e.g.
        ```bash
        ng generate component components/header
        ```
      - 4 files created similar to `app.component`, but for this specific component
      - also automatically added to `declarations` in `app.module.ts`
      - can now use it in e.g. `app.component.html`:
        ```html
        <app-header></app-header>
        ```

## Passing Properties to Component

```html
<!-- header - passing in properties -->
<app-button color="green" text="Add"></app-button>

<!-- Then in button component, can use directive:  -->
<button [ngStyle]="{ 'background-color': color }">
```
Then to use the inputs,
```ts
import { Component, Input } from '@angular/core';
// ...
export class ButtonComponent implements OnInit {
  @Input() text: string;
  @Input() color: string;
  // ...
}
```

## Events
```html
<button (click)="onClick()">{{text}}</button>
```
```ts
import { Component, Output, EventEmitter } from '@angular/core';
//...
export class ButtonComponent implements OnInit {
  @Output() btnClick = new EventEmitter();
  // ...
  onClick() {
    this.btnClick.emit();
  }
}
```
Then to use it in some other component:
```html
<app-button (btnClick)="doAction()"></app-button>
```
```ts
// the component class ...

doAction() {
  console.log('do action');
}
```

- Note the `Output` and `EventEmitter` was only required as the method comes from a component that uses the button. If the functionality was fine in `ButtonComponent`, then the `onClick` method would just contain the logic

- You can also pass in arguments
```html
<button (click)="onDelete(task)"></button>
```

- if the params not know, use `$event`
```html
<button (onAddTask)="addTask($event)"></button>
```

## Template directives

- `[]` for input
- `()` for output (events)
- `[()]` for two way data binding

- For loop:
```html
<div *ngFor="let task of tasks">{{ task.text }}</div>
```

- Conditional class:
```html
<div [ngClass]="{ reminder: task.reminder }"></div>
```

- Conditional
```html
<div *ngIf="show"></div>
```

## Services

Generates service `TaskService` in `services` directory:
```bash
ng generate service services/task
```

Note the use of Observables here (rxjs).

```ts
@Injectable({
  providedIn: 'root'
})
export class TaskService {

  constructor() { }

  getTasks(): Observable<Task[]> {
    const tasks = of(TASKS);
    return tasks;
  }
}
```

To use a service in a component, need to add it as a provider in a constructor:
```ts
  // pass services as argument, then can use this.taskService in the class
  constructor(private taskService: TaskService) { }

  ngOnInit(): void {
    this.taskService.getTasks().subscribe((tasks) => {
      this.tasks = tasks;
    });
  }
```

## Angular HTTP

In `app.module.ts`:
```ts
import { HttpClientModule } from '@angular/common/http';
// ...
@NgModule({
  // ...
  imports: [
    // ...
    HttpClientModule,
  ],
  // ...
})
```

In your service:
```ts
import { HttpClient, HttpHeaders } from '@angular/common/http';

export class TaskService {
  private apiUrl = 'http://localhost:5000/tasks'; // obviously dont hardcode this here

  constructor(private http: HttpClient) { }

  getTasks(): Observable<Task[]> {
    return this.http.get<Task[]>(this.apiUrl);
  }
}
```
# Forms

In `app.module.ts`:
```ts
import { FormsModule } from '@angular/forms';
// ...
@NgModule({
  // ...
  imports: [
    // ...
    FormsModule,
  ],
  // ...
})
```

To use in a template:
```html
<form (ngSubmit)="onSubmit()">
  <input
    type="text"
    name="day"
    [(ngModel)]="day"
    id="day"
    placeholder="Add Day & Time"
  />
  <!-- ... -->
</form>
```
```ts
export class AddTaskComponent implements OnInit {
  day: string;
  // ...
  onSubmit() {
    // code when pressing form submit button
  }
}
```

## Subjects

- Subjects involves taking the notifications from a single, source observable, and forwarding them to one or more destination observers
- Useful to use instead of passing around values or events (similar to 'prop-drilling' in react)
```ts
import { Observable, Subject } from 'rxjs';
// ...
export class UiService {
  private showAddButton: boolean = false;
  private subject = new Subject<any>();

  constructor() { }

  toggleAddButton(): void {
    this.showAddButton = !this.showAddButton;
    this.subject.next(this.showAddTask);
  }

  onToggle(): Observable<any> {
    return this.subject.asObservable();
  }
}
```
```ts
export class HeaderComponent implements OnInit {
  showAddButton: boolean = false;
  subscription: Subscription;

  constructor(private uiService: UiService) {
    this.subscription = this.uiService
      .onToggle()
      .subscribe(val => this.showAddButton = val);
  }

  ngOnInit(): void {
  }

  toggleAddButton() {
    this.uiService.toggleAddButton();
  }

}
```
`AddComponent` (not a child of `HeaderComponent`):
```ts
export class AddComponent implements OnInit {
  // ...
  showAddButton: boolean = false;
  subscription: Subscription;

  constructor(private uiService: UiService) {
    this.subscription = this.uiService
      .onToggle()
      .subscribe(val => this.showAddButton = val);
  }
  // ...
```
`AddComponent` template. Will respond to changes even though the click that controls the show/hide is in a different component.
```html
<form *ngIf="showAddTask">
  <!-- ... -->
</form>
```

## Routing

- Note: this can be set up automatically when creating the app using the CLI
- Manually:
`app.module.ts`:
```ts
import { RouterModule, Routes } from '@angular/router';
// ...

// (or put in seperate file)
const appRoutes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'about', component: AboutComponent },
];

@NgModule({
  // ...
  imports: [
    // ...
    // enableTracing helps debug, there are other options too
    RouterModule.forRoot(appRoutes, { enableTracing: true }),
  ],
  // ...
})
```
In `app.module.html` put `router-outlet` to handle routing:
```html
<div class="container">
    <app-header></app-header>
    <router-outlet></router-outlet>
    <app-footer></app-footer>
</div>
```
Then to use routing, redirects etc, can use `routerLink` in an `a` tag:
```html
<footer>
  <p>Copyright &copy; 2022</p>
  <a routerLink="/about">About</a>
</footer> 
```
To access the current route (e.g. in a `Header` component):
```ts
import { Router } from '@angular/router';
//...
export class HeaderComponent implements OnInit {
  // ... 
  constructor(private router: Router) { }
  // ... 
  hasRoute(route: string) {
    return this.router.url == route;
  }
}
```