# Angular Interview Questions and Answers

## 1. What is Angular?

**Answer:**
Angular is a TypeScript-based open-source web application framework developed and maintained by Google. It's a complete rewrite of AngularJS (Angular 1.x) and was first released in September 2016.

Angular provides a comprehensive solution for building single-page applications. It includes:
- A component-based architecture
- A collection of well-integrated libraries
- A suite of developer tools to help develop, build, test, and update code

Angular helps developers build applications that live on the web, mobile, or desktop by providing features like two-way data binding, dependency injection, declarative templates, end-to-end tooling, and more.

## 2. What are the key features of Angular?

**Answer:**
Angular comes with several key features that make it powerful for web development:

- **Component-Based Architecture**: Everything in Angular is organized into components, which are self-contained pieces of UI with their own logic and styling.

- **TypeScript Integration**: Angular uses TypeScript, which offers features like static typing, classes, interfaces, and compile-time error checking.

- **Two-Way Data Binding**: Angular provides automatic synchronization between the model and view components.

- **Dependency Injection**: Angular has a built-in dependency injection system that helps manage dependencies and enables more testable code.

- **Directives**: Angular extends HTML with directives, allowing you to create custom HTML elements and attributes.

- **Templates**: Angular uses HTML-based templates to define the UI of an application.

- **Routing**: Angular's router enables navigation from one view to another without page reloads.

- **Testing**: Angular was designed with testing in mind and provides tools for both unit and integration testing.

- **Forms Handling**: Angular provides two approaches to handle form inputs: Template-driven forms and Reactive forms.

- **HTTP Client**: Angular includes a powerful HTTP client for making API calls and handling responses.

## 3. Explain Angular components.

**Answer:**
Components are the fundamental building blocks of Angular applications. A component controls a portion of the screen—a view—and consists of:

1. **Component Class**: A TypeScript class that handles data and functionality. It contains properties and methods that can be accessed from the component's template.

2. **HTML Template**: Defines the UI for the component. It's a mix of HTML and Angular-specific syntax.

3. **Styles**: CSS that applies specifically to the component's template.

4. **Metadata (Decorator)**: Information that tells Angular how to process the class. The most common decorator is `@Component`.

Here's a basic example of an Angular component:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-hello',
  template: '<h1>Hello, {{name}}!</h1>',
  styles: ['h1 { color: blue; }']
})
export class HelloComponent {
  name = 'World';
}
```

In this example:
- The `@Component` decorator marks the class as an Angular component
- `selector` defines how the component is used in templates (`<app-hello></app-hello>`)
- `template` defines the HTML structure
- `styles` defines the CSS rules for this component
- The class contains a property `name` that is bound to the template

Components support lifecycle hooks like `ngOnInit()`, `ngOnChanges()`, and others that allow you to tap into important events in the component's lifecycle.

## 4. What is the difference between AngularJS and Angular?

**Answer:**
AngularJS (Angular 1.x) and Angular (Angular 2+) are quite different, despite sharing a similar name:

| Feature | AngularJS (1.x) | Angular (2+) |
|---------|----------------|--------------|
| **Language** | JavaScript | TypeScript (primarily) |
| **Architecture** | MVC (Model-View-Controller) | Component-based |
| **Expression Syntax** | Uses `ng-directives` (e.g., `ng-model`) | Uses `[]` for property binding and `()` for event binding |
| **Mobile Support** | Limited | Better, with mobile-oriented features |
| **Dependency Injection** | Based on named functions | Based on hierarchical dependency injectors |
| **Data Binding** | Two-way by default | Unidirectional by default, with option for two-way using `[(ngModel)]` |
| **Routing** | `ngRoute` module | More powerful `@angular/router` |
| **Speed & Performance** | Slower due to digest cycle | Faster with improved change detection |
| **Structure** | Less structured | More structured and modular |
| **CLI Support** | No official CLI | Angular CLI for project scaffolding |
| **Testing** | Uses different libraries | Integrated testing setup |

Angular (2+) was a complete rewrite of AngularJS, designed to be more modern, faster, and better suited for mobile development. It addressed many limitations of AngularJS and introduced a more component-focused architecture.

## 5. What is Angular CLI?

**Answer:**
Angular CLI (Command Line Interface) is a powerful command-line tool that simplifies the creation, development, and maintenance of Angular applications. It automates many routine development tasks, making development more efficient.

Key features of Angular CLI include:

- **Project Creation**: Initialize new Angular projects with a standardized structure
  ```bash 
  ng new my-app
  ```

- **Component/Service Generation**: Generate new components, services, modules, etc. with proper boilerplate code
  ```bash 
  ng generate component my-component
  # or shorter
  ng g c my-component
  ```

- **Development Server**: Run a local development server with live reloading
  ```bash 
  ng serve
  ```

- **Building**: Compile the application for production with optimizations
  ```bash 
  ng build --prod
  ```

- **Testing**: Run unit tests and end-to-end tests
  ```bash 
  ng test
  ng e2e
  ```

- **Linting**: Check code quality with TSLint
  ```bash 
  ng lint
  ```

- **Updates**: Update Angular dependencies
  ```bash 
  ng update
  ```

Angular CLI follows best practices and helps maintain consistency across the project. It also supports workspaces that can contain multiple projects, which is useful for larger applications or component libraries.

## 6. What is a module in Angular?

**Answer:**
An Angular module is a container for a cohesive block of code dedicated to an application domain, a workflow, or a closely related set of capabilities. Modules help organize an application into functional units.

Every Angular application has at least one module, the root module (conventionally named `AppModule`). As your application grows, you can organize code into feature modules.

A typical Angular module consists of:

1. **Declarations**: Components, directives, and pipes that belong to this module
2. **Imports**: Other modules whose exported classes are needed by components in this module
3. **Exports**: The subset of declarations that should be visible and usable in component templates of other modules
4. **Providers**: Services that the module contributes to the global collection of services
5. **Bootstrap**: The main application view, called the root component, which hosts all other app views

Here's an example of a basic Angular module:

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppComponent } from './app.component';
import { FeatureComponent } from './feature/feature.component';

@NgModule({
  declarations: [
    AppComponent,
    FeatureComponent
  ],
  imports: [
    BrowserModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

Types of Angular modules:
- **Feature modules**: Organize code related to a specific application feature
- **Shared modules**: Contain components, directives, and pipes used throughout the application
- **Core module**: Contains singleton services that are used application-wide
- **Routing modules**: Separate modules dedicated to setting up routes

## 7. Explain data binding in Angular.

**Answer:**
Data binding is a mechanism that allows communication between the component and its template. It provides a way to define interactive applications without having to worry about pushing and pulling data between the component and template.

Angular supports four types of data binding:

1. **Interpolation (One-way, Component to View)**:
   Uses double curly braces `{{}}` to display component property values in the template.
   ```html
   <h1>Hello, {{userName}}!</h1>
   ```

2. **Property Binding (One-way, Component to View)**:
   Binds a property of a DOM element to a component property using square brackets `[]`.
   ```html
   <img [src]="userProfileImage">
   <button [disabled]="isSubmitting">Submit</button>
   ```

3. **Event Binding (One-way, View to Component)**:
   Responds to DOM events like clicks using parentheses `()`.
   ```html
   <button (click)="saveData()">Save</button>
   <input (input)="onInputChange($event)">
   ```

4. **Two-way Binding**:
   Combines property and event binding using the banana-in-a-box syntax `[()]`.
   ```html
   <input [(ngModel)]="userName">
   ```
   This is equivalent to:
   ```html
   <input [value]="userName" (input)="userName = $event.target.value">
   ```

Two-way binding requires the `FormsModule` to be imported in your Angular module to use `ngModel`.

Data binding is an essential part of Angular's change detection system, which efficiently updates the DOM when the state of your application changes. It reduces the need for manual DOM manipulation and makes your code more declarative and easier to understand.

## 8. What are Angular directives? Explain the different types.

**Answer:**
Directives are instructions in the DOM (Document Object Model) that tell Angular how to render a template. They extend HTML with new functionality.

There are three main types of directives in Angular:

### 1. Component Directives
Components are directives with a template. They are the most common directive type.
```typescript
@Component({
  selector: 'app-user-profile',
  template: '<div>{{user.name}}</div>',
})
export class UserProfileComponent {
  user = { name: 'John' };
}
```

### 2. Structural Directives
These directives change the structure of the DOM by adding, removing, or manipulating elements. They are prefixed with an asterisk (*).

- **ngIf**: Conditionally includes a template based on a boolean expression
  ```html
  <div *ngIf="isLoggedIn">Welcome back, user!</div>
  ```

- **ngFor**: Repeats a template for each item in an iterable
  ```html
  <li *ngFor="let item of items; let i = index">{{i}} - {{item.name}}</li>
  ```

- **ngSwitch**: Switches between different templates based on a condition
  ```html
  <div [ngSwitch]="userRole">
    <div *ngSwitchCase="'admin'">Admin Panel</div>
    <div *ngSwitchCase="'user'">User Dashboard</div>
    <div *ngSwitchDefault>Login Please</div>
  </div>
  ```

### 3. Attribute Directives
These directives change the appearance or behavior of an existing element.

- **ngClass**: Adds or removes CSS classes
  ```html
  <div [ngClass]="{'active': isActive, 'disabled': isDisabled}">Content</div>
  ```

- **ngStyle**: Sets inline styles
  ```html
  <div [ngStyle]="{'color': textColor, 'font-size': fontSize + 'px'}">Styled text</div>
  ```

- **ngModel**: Implements two-way data binding
  ```html
  <input [(ngModel)]="name">
  ```

### Creating Custom Directives
You can create your own directives to extend Angular with custom functionality:

```typescript
import { Directive, ElementRef, HostListener, Input } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  @Input() highlightColor = 'yellow';

  constructor(private el: ElementRef) {}

  @HostListener('mouseenter') onMouseEnter() {
    this.highlight(this.highlightColor);
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.highlight(null);
  }

  private highlight(color: string | null) {
    this.el.nativeElement.style.backgroundColor = color;
  }
}
```

Usage:
```html
<p appHighlight highlightColor="lightblue">Highlight me!</p>
```

Directives are powerful tools that keep your templates declarative and focused on business logic rather than DOM manipulation.

## 9. What are services in Angular?

**Answer:**
Services in Angular are singleton objects that provide shared functionality across the application. They are used to organize and share business logic, data, or functions with different components.

Key characteristics of Angular services:

1. **Reusability**: Code that needs to be reused across components is placed in services.

2. **Separation of Concerns**: Services help keep components lean by moving data access and business logic out of components.

3. **Dependency Injection**: Angular's DI system makes services available to components that need them.

4. **Singleton by Default**: When provided at the root level, services are created as singletons, meaning there's only one instance of the service throughout the application.

Creating a basic service:
```typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root' // Makes it available application-wide
})
export class UserService {
  private users = [
    { id: 1, name: 'John' },
    { id: 2, name: 'Jane' }
  ];

  getUsers() {
    return this.users;
  }

  getUserById(id: number) {
    return this.users.find(user => user.id === id);
  }
}
```

Using the service in a component:
```typescript
import { Component, OnInit } from '@angular/core';
import { UserService } from './user.service';

@Component({
  selector: 'app-user-list',
  template: '<ul><li *ngFor="let user of users">{{user.name}}</li></ul>'
})
export class UserListComponent implements OnInit {
  users: any[] = [];

  constructor(private userService: UserService) { }

  ngOnInit() {
    this.users = this.userService.getUsers();
  }
}
```

Common types of services include:
- **Data services**: Handle data operations and communication with backends
- **Utility services**: Provide helper functions used across the application
- **Feature services**: Contain business logic for specific features
- **State management services**: Manage the state of the application

Services can be provided at different levels:
- At the root level for application-wide access
- At a module level for access within a specific module
- At a component level for access only within that component and its children

## 10. What is dependency injection in Angular?

**Answer:**
Dependency Injection (DI) is a design pattern and core concept in Angular that allows classes to receive dependencies from external sources rather than creating them internally. This makes code more modular, reusable, and easier to test.

### How Angular's DI works:

1. **Provider Registration**: You register a class or value as a provider, which tells Angular how to create/obtain the dependency.

2. **Injector Hierarchy**: Angular maintains a hierarchical injector system that follows the component tree.

3. **Dependency Resolution**: When a component or service requests a dependency, Angular looks up the appropriate provider to create or retrieve the dependency.

4. **Instance Delivery**: Angular delivers the instance to the requesting component or service through its constructor.

### Key components of Angular's DI system:

- **Injectable**: A decorator that marks a class as available for injection
- **Injector**: The service responsible for creating instances of dependencies
- **Provider**: Tells the injector how to create an instance of a dependency
- **Token**: The key used to identify a dependency (usually the class itself)

### Example of dependency injection:

1. Create a service with `@Injectable()`:
```typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'  // Registers at root injector level
})
export class LoggerService {
  log(message: string) {
    console.log(`LOG: ${message}`);
  }
}
```

2. Inject it into a component or another service:
```typescript
import { Component } from '@angular/core';
import { LoggerService } from './logger.service';

@Component({
  selector: 'app-example',
  template: '<button (click)="logMessage()">Log Message</button>'
})
export class ExampleComponent {
  constructor(private logger: LoggerService) { }
  
  logMessage() {
    this.logger.log('Button clicked!');
  }
}
```

### Provider scopes:

- **Root-level providers** (`providedIn: 'root'`): Available application-wide
- **Module-level providers** (provided in a module's `providers` array): Available to all components within the module
- **Component-level providers** (provided in a component's `providers` array): A new instance is created for the component and its children

### Benefits of DI:

1. **Testability**: Makes it easy to mock dependencies in tests
2. **Maintainability**: Components don't need to know how to create their dependencies
3. **Flexibility**: Dependencies can be swapped without changing the consuming code
4. **Reusability**: Services can be reused across components
5. **Singleton Management**: Angular handles the lifecycle of services

### Advanced DI features:

- **InjectionToken**: For injecting non-class dependencies (like configuration objects)
- **Multi Providers**: Register multiple providers for the same token
- **Optional Dependencies**: Mark dependencies as optional
- **Forward References**: Handle circular dependencies
- **Value Providers**: Inject literal values instead of service instances

## 11. What is the Angular Router and how does it work?

**Answer:**
The Angular Router is a powerful module that enables navigation between different components/views in an Angular application without page reloads, creating a Single Page Application (SPA) experience.

### Basic Router Setup:

1. **Import RouterModule**:
```typescript
import { RouterModule, Routes } from '@angular/router';
```

2. **Define Routes**:
```typescript
const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'products', component: ProductListComponent },
  { path: 'products/:id', component: ProductDetailComponent },
  { path: 'contact', component: ContactComponent },
  { path: '**', component: PageNotFoundComponent }  // Wildcard route for 404
];
```

3. **Register Router Module**:
```typescript
@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

4. **Add Router Outlet** in your template:
```html
<router-outlet></router-outlet>
```

### Key Concepts:

1. **Routes**: Configuration objects that define a mapping between a URL path and a component.

2. **Router Outlet**: A directive that serves as a placeholder where the router displays the component for the active route.

3. **Router Links**: Directives for navigating between routes:
```html
<a routerLink="/products">Products</a>
<a [routerLink]="['/products', product.id]">{{ product.name }}</a>
```

4. **Programmatic Navigation**:
```typescript
constructor(private router: Router) {}

navigateToProducts() {
  this.router.navigate(['/products']);
}
```

5. **Route Parameters**: Pass data between routes:
```typescript
// In component with link
<a [routerLink]="['/products', product.id]">View Details</a>

// In target component
import { ActivatedRoute } from '@angular/router';

constructor(private route: ActivatedRoute) {
  this.route.paramMap.subscribe(params => {
    const productId = params.get('id');
    // Fetch product with this ID
  });
}
```

6. **Query Parameters**:
```html
<a [routerLink]="['/products']" [queryParams]="{category: 'electronics'}">Electronics</a>
```

7. **Route Guards**: Protect routes based on conditions:
   - **CanActivate**: Controls if a route can be activated
   - **CanDeactivate**: Controls if a user can leave a route
   - **CanLoad**: Controls if a module can be loaded lazily

8. **Child Routes**: Nest routes under parent routes:
```typescript
const routes: Routes = [
  { 
    path: 'admin', 
    component: AdminComponent,
    children: [
      { path: 'dashboard', component: DashboardComponent },
      { path: 'users', component: UsersComponent }
    ]
  }
];
```

9. **Lazy Loading**: Load modules on demand to improve initial load time:
```typescript
const routes: Routes = [
  { path: 'products', loadChildren: () => import('./products/products.module').then(m => m.ProductsModule) }
];
```

10. **Router Events**: Subscribe to router events for analytics or loading indicators:
```typescript
constructor(private router: Router) {
  router.events.subscribe(event => {
    if (event instanceof NavigationStart) {
      // Show loading indicator
    }
    if (event instanceof NavigationEnd) {
      // Hide loading indicator
    }
  });
}
```

The Angular Router is highly configurable and provides numerous features for complex navigation scenarios, making it a powerful tool for building sophisticated single-page applications.

## 12. What are lifecycle hooks in Angular?

**Answer:**
Lifecycle hooks are special methods that give you the ability to tap into specific moments in the lifecycle of an Angular component or directive. They allow you to perform actions at specific times, such as initializing data, responding to changes, or cleaning up resources.

### Main Lifecycle Hooks (in order of execution):

1. **ngOnChanges**
   - Called when input properties change
   - Called before ngOnInit and whenever input property values change
   - Receives a SimpleChanges object with current and previous values
   
   ```typescript
   ngOnChanges(changes: SimpleChanges) {
     console.log('Input properties changed:', changes);
     for (const propName in changes) {
       const change = changes[propName];
       console.log(`${propName}: ${change.previousValue} => ${change.currentValue}`);
     }
   }
   ```

2. **ngOnInit**
   - Called once after the first ngOnChanges
   - Best place to initialize the component
   - Good for setting up subscriptions and fetching initial data
   
   ```typescript
   ngOnInit() {
     console.log('Component initialized');
     this.loadInitialData();
   }
   ```

3. **ngDoCheck**
   - Called during every change detection run
   - Use for custom change detection logic
   - Called frequently, so keep it lightweight
   
   ```typescript
   ngDoCheck() {
     // Custom change detection
     if (this.previousName !== this.currentName) {
       console.log('Name changed from detection');
       this.previousName = this.currentName;
     }
   }
   ```

4. **ngAfterContentInit**
   - Called once after the first ngDoCheck
   - Called after content (ng-content) has been projected into the component
   
   ```typescript
   ngAfterContentInit() {
     console.log('Content has been projected into the component');
     console.log('Projected content:', this.projectedContent);
   }
   ```

5. **ngAfterContentChecked**
   - Called after every check of projected content
   - Called after ngAfterContentInit and every subsequent ngDoCheck
   
   ```typescript
   ngAfterContentChecked() {
     console.log('Projected content checked');
   }
   ```

6. **ngAfterViewInit**
   - Called once after the first ngAfterContentChecked
   - Called after the component's view and child views are initialized
   - Safe place to interact with ViewChild references
   
   ```typescript
   @ViewChild('someElement') element: ElementRef;
   
   ngAfterViewInit() {
     console.log('View initialized');
     console.log('Element:', this.element.nativeElement);
   }
   ```

7. **ngAfterViewChecked**
   - Called after every check of the component's view and child views
   - Called after ngAfterViewInit and every subsequent ngAfterContentChecked
   
   ```typescript
   ngAfterViewChecked() {
     console.log('View checked');
   }
   ```

8. **ngOnDestroy**
   - Called just before Angular destroys the component
   - Ideal for cleanup work like unsubscribing from observables
   - Prevents memory leaks
   
   ```typescript
   ngOnDestroy() {
     console.log('Component being destroyed');
     this.subscription.unsubscribe();
   }
   ```

### Best Practices:

1. **Use ngOnInit for initialization** rather than the constructor for better readability and separation of concerns.

2. **Clean up in ngOnDestroy** to prevent memory leaks:
   ```typescript
   private subscription: Subscription;
   
   ngOnInit() {
     this.subscription = this.dataService.getData().subscribe(data => {
       this.data = data;
     });
   }
   
   ngOnDestroy() {
     if (this.subscription) {
       this.subscription.unsubscribe();
     }
   }
   ```

3. **Be careful with ngDoCheck, ngAfterContentChecked, and ngAfterViewChecked** as they run frequently and can impact performance.

4. **Implement interfaces** to make your code more readable and maintainable:
   ```typescript
   import { Component, OnInit, OnDestroy } from '@angular/core';
   
   @Component({...})
   export class MyComponent implements OnInit, OnDestroy {
     ngOnInit() {...}
     ngOnDestroy() {...}
   }
   ```

5. **Use ngOnChanges with SimpleChanges** to properly track changes to input properties.

Lifecycle hooks give you fine-grained control over your components and directives, allowing you to build more responsive and efficient applications.

## 13. What is AOT compilation in Angular?

**Answer:**
Ahead-of-Time (AOT) compilation is a process in Angular where the framework compiles the application during the build phase, before the browser downloads and runs the code, as opposed to Just-in-Time (JIT) compilation where the application is compiled in the browser at runtime.

### How AOT Works:

1. The Angular AOT compiler converts your Angular HTML templates and TypeScript code into efficient JavaScript code during the build phase.

2. The compiler extracts metadata from the application's components, generates factory classes, and creates instructions for the component rendering.

3. This pre-compiled code is then bundled, minified, and delivered to the browser.

### Benefits of AOT Compilation:

1. **Faster Rendering**
   - No need to compile in the browser, so applications load and render more quickly
   - The browser downloads a pre-compiled version of the application
   - Rendering begins immediately as the browser doesn't wait for compilation

2. **Smaller Application Size**
   - The Angular compiler is not needed in the browser, reducing the application payload
   - Tree shaking (dead code elimination) works more effectively with AOT, removing unused code

3. **Better Security**
   - HTML templates are compiled before they reach the browser
   - Reduces the risk of injection attacks because potentially dangerous HTML isn't dynamically created

4. **Earlier Error Detection**
   - Template errors are caught during the build process rather than at runtime
   - Type checking for templates (with the --strictTemplates flag)
   - Developers can fix issues before deploying the application

5. **Better Compatibility**
   - Works better with Content Security Policy (CSP) as it doesn't require the browser to generate code through eval()

### AOT vs. JIT:

| Feature | AOT (Ahead-of-Time) | JIT (Just-in-Time) |
|---------|---------------------|-------------------|
| Compilation Timing | During build | In browser at runtime |
| Application Size | Smaller (no compiler) | Larger (includes compiler) |
| Loading Speed | Faster | Slower |
| Build Time | Longer | Shorter |
| Error Detection | At build time | At runtime |
| Development Mode | Less suitable (longer builds) | More suitable (faster builds) |
| Production Mode | More suitable | Less suitable |

### Enabling AOT Compilation:

AOT is enabled by default in production builds with Angular CLI:

```bash
ng build --prod
```

For development builds, you can explicitly enable it:

```bash
ng build --aot
```

### Potential Limitations and Considerations:

1. **Build Time**: AOT compilation can increase build times, especially for large applications.

2. **Dynamic Content**: Some dynamic coding patterns may not work with AOT, such as using eval() or dynamically generating components.

3. **Meta-programming**: Some libraries that use meta-programming techniques might face challenges with AOT.

4. **Debugging**: Debugging can be more challenging as the executed code is compiled and potentially different from the source code.

In modern Angular applications (version 9+), the default build engine Ivy has made AOT compilation more efficient and is now recommended for both development and production environments.

## 14. What is the difference between template-driven forms and reactive forms?

**Answer:**
Angular provides two approaches for handling forms: Template-driven forms and Reactive forms. Each has its own strengths, use cases, and implementation patterns.

### Template-Driven Forms

Template-driven forms rely on directives in the template to create and manipulate the underlying form object. They're built with ngModel and form directives like ngForm.

**Key Characteristics:**
1. **More Angular-like HTML approach** - Most of the form logic lives in the template
2. **Easier to set up** - Less code required for simple forms
3. **Two-way data binding** with ngModel
4. **Implicit form creation** - Angular creates form controls behind the scenes
5. **Asynchronous validation**

**Implementation Example:**

```typescript
// In module
import { FormsModule } from '@angular/forms';

@NgModule({
  imports: [FormsModule],
})
export class AppModule { }
```

```typescript
// In component
@Component({
  selector: 'app-profile-editor',
  template: `
    <form #profileForm="ngForm" (ngSubmit)="onSubmit(profileForm.value)">
      <div>
        <label for="name">Name</label>
        <input 
          type="text" 
          id="name" 
          name="name" 
          [(ngModel)]="profile.name" 
          required 
          #name="ngModel">
        <div *ngIf="name.invalid && name.touched">
          Name is required
        </div>
      </div>
      
      <div>
        <label for="email">Email</label>
        <input 
          type="email" 
          id="email" 
          name="email" 
          [(ngModel)]="profile.email" 
          email 
          #email="ngModel">
        <div *ngIf="email.invalid && email.touched">
          Please enter a valid email
        </div>
      </div>
      
      <button type="submit" [disabled]="profileForm.invalid">Submit</button>
    </form>
  `
})
export class ProfileEditorComponent {
  profile = {
    name: '',
    email: ''
  };
  
  onSubmit(formValue) {
    console.log('Form submitted with:', formValue);
    // Process the form
  }
}
```

### Reactive Forms

Reactive forms are created programmatically in the component class. They provide direct access to the underlying form object and use reactive programming patterns.

**Key Characteristics:**
1. **More explicit** - Form controls are created in the component class
2. **More flexible** - Greater control over form validation and dynamic form elements
3. **Synchronous validation** by default, with support for async validation
4. **Better for complex forms** and dynamic forms
5. **Better for unit testing** - Logic is in the component, not the template

**Implementation Example:**

```typescript
// In module
import { ReactiveFormsModule } from '@angular/forms';

@NgModule({
  imports: [ReactiveFormsModule],
})
export class AppModule { }
```

```typescript
// In component
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

@Component({
  selector: 'app-profile-editor',
  template: `
    <form [formGroup]="profileForm" (ngSubmit)="onSubmit()">
      <div>
        <label for="name">Name</label>
        <input type="text" id="name" formControlName="name">
        <div *ngIf="profileForm.get('name').invalid && profileForm.get('name').touched">
          <div *ngIf="profileForm.get('name').errors?.required">Name is required</div>
        </div>
      </div>
      
      <div>
        <label for="email">Email</label>
        <input type="email" id="email" formControlName="email">
        <div *ngIf="profileForm.get('email').invalid && profileForm.get('email').touched">
          <div *ngIf="profileForm.get('email').errors?.email">Please enter a valid email</div>
        </div>
      </div>
      
      <button type="submit" [disabled]="profileForm.invalid">Submit</button>
    </form>
  `
})
export class ProfileEditorComponent implements OnInit {
  profileForm: FormGroup;
  
  constructor(private fb: FormBuilder) {}
  
  ngOnInit() {
    this.profileForm = this.fb.group({
      name: ['', Validators.required],
      email: ['', [Validators.email]]
    });
  }
  
  onSubmit() {
    console.log('Form submitted with:', this.profileForm.value);
    // Process the form
  }
}
```

### Comparison Between Template-Driven and Reactive Forms:

| Feature | Template-Driven Forms | Reactive Forms |
|---------|----------------------|---------------|
| **Form Model Creation** | Created implicitly by directives | Created explicitly in component |
| **Data Model** | Mutable, two-way binding | Immutable, one-way binding |
| **Form Validation** | Directives in template | Functions in component |
| **Testing** | More difficult (template-based) | Easier (component-based) |
| **Predictability** | Less predictable (async) | More predictable (sync) |
| **Scalability** | Better for simple forms | Better for complex forms |
| **Dynamism** | Less suitable for dynamic forms | More suitable for dynamic forms |
| **Complexity** | Lower initial setup complexity | Higher initial setup complexity |
| **Control** | Less fine-grained control | More fine-grained control |
| **Error Handling** | Template-based error handling | Programmatic error handling |

### When to Use Each:

**Use Template-Driven Forms when:**
- Building simple forms with basic validation
- Working on smaller applications
- Preferring a more HTML-centric approach
- Needing to quickly implement a form with minimal code

**Use Reactive Forms when:**
- Building complex forms with dynamic controls
- Needing extensive or custom validation
- Working with forms that change values based on user interaction
- Working in a team environment and need better testing capabilities
- Implementing more complex form scenarios like multi-step forms or wizards

## 15. How does change detection work in Angular?

**Answer:**
Change detection is the process by which Angular keeps the view in sync with the application state. It's one of the most important features of Angular, determining when and how to update the DOM based on changes in the application data.

### Core Concepts:

1. **Zone.js and NgZone**:
   - Angular uses Zone.js to intercept asynchronous operations
   - Zone.js patches browser APIs like setTimeout, addEventListener, and Promise
   - This allows Angular to know when asynchronous operations complete
   - Angular runs change detection after these operations

2. **Change Detection Tree**:
   - Components form a hierarchical tree
   - Change detection flows from root to leaves (top to bottom)
   - Each component has its own change detector

3. **Change Detection Strategies**:
   - **Default (CheckAlways)**: Checks the entire component tree whenever a change is detected
   - **OnPush**: Only checks when:
     - Input references change (not their properties)
     - An event originated from the component or its children
     - Change detection is explicitly triggered
     - Using async pipe that emits a new value

### Default Change Detection:

By default, Angular performs change detection on all components whenever any of the following happens:
- Events (click, submit, etc.)
- HTTP requests
- Timers (setTimeout, setInterval)
- Promises
- Other async operations

```typescript
@Component({
  selector: 'app-default',
  template: `<div>{{data.value}}</div>`,
  // No changeDetection property means CheckAlways
})
export class DefaultComponent {
  data = { value: 'Initial' };
  
  constructor() {
    setTimeout(() => {
      // This change will be detected automatically
      this.data.value = 'Updated';
    }, 2000);
  }
}
```

### OnPush Change Detection:

OnPush strategy reduces the number of checks performed, improving performance:

```typescript
import { Component, ChangeDetectionStrategy, Input } from '@angular/core';

@Component({
  selector: 'app-child',
  template: `<div>{{data.value}}</div>`,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class ChildComponent {
  @Input() data: any;
}

@Component({
  selector: 'app-parent',
  template: `<app-child [data]="childData"></app-child>
             <button (click)="updateChildData()">Update Data</button>`,
})
export class ParentComponent {
  childData = { value: 'Initial' };
  
  // This will NOT update the view in the child component
  updateIncorrectly() {
    this.childData.value = 'Updated';
  }
  
  // This WILL update the view in the child component
  updateChildData() {
    this.childData = { value: 'Updated' }; // New reference
  }
}
```

### Manual Change Detection Control:

For advanced cases, you can control change detection explicitly:

```typescript
import { Component, ChangeDetectorRef, NgZone } from '@angular/core';

@Component({
  selector: 'app-manual',
  template: `<div>{{counter}}</div>`,
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class ManualComponent {
  counter = 0;
  
  constructor(
    private cdr: ChangeDetectorRef,
    private ngZone: NgZone
  ) {
    // Run outside Angular's zone to avoid triggering change detection
    this.ngZone.runOutsideAngular(() => {
      setInterval(() => {
        this.counter++;
        
        // Manually trigger change detection
        this.cdr.detectChanges();
        
        // Or mark for check (will be checked in next cycle)
        // this.cdr.markForCheck();
      }, 1000);
    });
  }
  
  detach() {
    // Completely detach change detector
    this.cdr.detach();
  }
  
  reattach() {
    // Reattach change detector
    this.cdr.reattach();
  }
}
```

### Performance Optimization Strategies:

1. **Use OnPush Strategy**: Apply it to components that don't need frequent updates

2. **Immutable Data Patterns**: Use immutable objects for @Input properties

3. **Observable + Async Pipe**:
```html
<div>{{ data$ | async }}</div>
```

4. **Run Outside NgZone**:
```typescript
this.ngZone.runOutsideAngular(() => {
  // Code that doesn't need to trigger change detection
});
```

5. **Detach Change Detector**: For manual control over change detection

6. **TrackBy Function**: Optimize ngFor directive rendering:
```html
<div *ngFor="let item of items; trackBy: trackByFn">{{item.name}}</div>
```
```typescript
trackByFn(index, item) {
  return item.id; // Unique identifier
}
```

7. **Pure Pipes**: Use pure pipes instead of methods in templates

Change detection is central to Angular's performance. Understanding and optimizing it is crucial for building efficient applications, especially as they grow in size and complexity.

## 16. What are pipes in Angular? Give examples of built-in pipes.

**Answer:**
Pipes in Angular are a way to transform data within templates. They take input data and transform it into the desired output format for display, without modifying the original data. Pipes can be chained together to create complex transformations and can be used directly in templates.

### Syntax:

```html
{{ value | pipeName:parameter1:parameter2:... }}
```

### Built-in Pipes:

1. **DatePipe**: Formats dates according to specified formats
   ```html
   <!-- Output: May 10, 2025 -->
   {{ today | date:'longDate' }}
   
   <!-- Output: 05/10/2025 -->
   {{ today | date:'MM/dd/yyyy' }}
   
   <!-- Output: 10-May-2025 -->
   {{ today | date:'dd-MMM-yyyy' }}
   ```

2. **UpperCasePipe**: Converts text to uppercase
   ```html
   <!-- Output: HELLO WORLD -->
   {{ 'Hello World' | uppercase }}
   ```

3. **LowerCasePipe**: Converts text to lowercase
   ```html
   <!-- Output: hello world -->
   {{ 'Hello World' | lowercase }}
   ```

4. **TitleCasePipe**: Converts text to title case
   ```html
   <!-- Output: Hello World -->
   {{ 'hello world' | titlecase }}
   ```

5. **CurrencyPipe**: Formats numbers as currency values
   ```html
   <!-- Output: $1,234.56 -->
   {{ 1234.56 | currency }}
   
   <!-- Output: €1,234.56 -->
   {{ 1234.56 | currency:'EUR' }}
   
   <!-- Output: £1,234.56 -->
   {{ 1234.56 | currency:'GBP':'symbol':'1.2-2' }}
   ```

6. **DecimalPipe**: Formats numbers according to specified formats
   ```html
   <!-- Output: 1,234.5 -->
   {{ 1234.5678 | number }}
   
   <!-- Output: 1,234.57 -->
   {{ 1234.5678 | number:'1.2-2' }}
   
   <!-- Output: 01,234.567 -->
   {{ 1234.5678 | number:'5.1-3' }}
   ```

7. **PercentPipe**: Formats numbers as percentages
   ```html
   <!-- Output: 42% -->
   {{ 0.42 | percent }}
   
   <!-- Output: 42.0% -->
   {{ 0.42 | percent:'1.1-1' }}
   ```

8. **SlicePipe**: Extracts a slice from a string or array
   ```html
   <!-- Output: lo Wo -->
   {{ 'Hello World' | slice:3:8 }}
   
   <!-- Output: [3, 4, 5] -->
   {{ [1, 2, 3, 4, 5, 6] | slice:2:5 }}
   ```

9. **JsonPipe**: Converts object to a JSON string
   ```html
   <!-- Output: {"name":"John","age":30} -->
   {{ {name: 'John', age: 30} | json }}
   ```

10. **AsyncPipe**: Handles Observables and Promises
    ```html
    <!-- Automatically subscribes to dataObservable and updates when it emits -->
    {{ dataObservable | async }}
    
    <!-- Automatically resolves dataPromise and updates when resolved -->
    {{ dataPromise | async }}
    ```

11. **KeyValuePipe**: Transforms an object or Map into an array of key-value pairs
    ```html
    <div *ngFor="let item of object | keyvalue">
      {{item.key}}: {{item.value}}
    </div>
    ```

### Custom Pipes:

You can create your own custom pipes for specific transformations:

```typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'truncate'
})
export class TruncatePipe implements PipeTransform {
  transform(value: string, limit: number = 25, completeWords: boolean = false, ellipsis: string = '...'): string {
    if (!value) return '';
    
    if (value.length <= limit) return value;
    
    if (completeWords) {
      limit = value.substring(0, limit).lastIndexOf(' ');
    }
    
    return `${value.substring(0, limit)}${ellipsis}`;
  }
}
```

Usage:
```html
<!-- Output: This is a long text... -->
{{ 'This is a long text that needs to be truncated' | truncate:20 }}

<!-- Output: This is a long... -->
{{ 'This is a long text that needs to be truncated' | truncate:15:true }}
```

### Pure vs. Impure Pipes:

1. **Pure Pipes** (default):
   - Only execute when input value changes
   - More efficient as they don't run on every change detection cycle
   - Best for most scenarios

2. **Impure Pipes**:
   - Execute on every change detection cycle
   - Useful when transformations depend on internal state or other data changes
   - Should be used carefully as they can affect performance

```typescript
@Pipe({
  name: 'filterByStatus',
  pure: false // Makes this an impure pipe
})
export class FilterByStatusPipe implements PipeTransform {
  transform(items: any[], status: string): any[] {
    if (!items || !status) return items;
    return items.filter(item => item.status === status);
  }
}
```

### Best Practices:

1. **Chain pipes** for complex transformations:
   ```html
   <!-- First converts to lowercase, then takes a slice -->
   {{ 'Hello World' | lowercase | slice:0:5 }}
   ```

2. **Create reusable custom pipes** instead of having transformation logic in components

3. **Prefer pure pipes** for better performance

4. **Use AsyncPipe** to handle observables and automatically manage subscriptions

5. **Avoid complex logic in pipes** - they should be focused on simple transformations

Pipes help keep templates clean and maintainable by separating presentation logic from business logic, making them an essential part of Angular's templating system.

## 17. What is lazy loading in Angular and how is it implemented?

**Answer:**
Lazy loading is a design pattern in Angular that allows you to load JavaScript components only when they are needed. Instead of loading the entire application at once, modules are split into chunks and loaded on demand, typically when a user navigates to a specific route.

### Benefits of Lazy Loading:

1. **Faster Initial Load Time**: Only the essential modules are loaded initially, reducing the startup time and allowing users to interact with the application more quickly.

2. **Reduced Resource Usage**: Less memory is required since only necessary components are loaded.

3. **Better Performance**: Smaller initial bundle size results in faster parsing and execution time.

4. **Improved User Experience**: Users don't have to wait for the entire application to load before interacting with it.

5. **Better Caching**: Smaller chunks can be more efficiently cached by browsers.

### Implementation Steps:

#### 1. Structure Your Application:

Organize your code into feature modules, where each feature represents a section of your application that can be loaded independently.

```
src/
├── app/
│   ├── home/                  // Eagerly loaded
│   │   ├── home.component.ts
│   │   └── home.module.ts
│   ├── admin/                 // Lazily loaded
│   │   ├── admin.component.ts
│   │   ├── admin.module.ts
│   │   └── admin-routing.module.ts
│   ├── products/              // Lazily loaded
│   │   ├── products.component.ts
│   │   ├── products.module.ts
│   │   └── products-routing.module.ts
│   ├── app.component.ts
│   ├── app.module.ts
│   └── app-routing.module.ts
```

#### 2. Create Feature Modules with Their Own Routing:

Each lazy-loaded module should have its own routing configuration.

```typescript
// products/products-routing.module.ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { ProductsComponent } from './products.component';
import { ProductDetailComponent } from './product-detail.component';

const routes: Routes = [
  {
    path: '',  // Note: Empty path since the parent already has 'products'
    component: ProductsComponent
  },
  {
    path: ':id',
    component: ProductDetailComponent
  }
];

@NgModule({
  imports: [RouterModule.forChild(routes)],
  exports: [RouterModule]
})
export class ProductsRoutingModule { }
```

```typescript
// products/products.module.ts
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';
import { ProductsRoutingModule } from './products-routing.module';
import { ProductsComponent } from './products.component';
import { ProductDetailComponent } from './product-detail.component';

@NgModule({
  declarations: [
    ProductsComponent,
    ProductDetailComponent
  ],
  imports: [
    CommonModule,
    ProductsRoutingModule
  ]
})
export class ProductsModule { }
```

#### 3. Configure the Main Routing Module:

In your app-routing.module.ts, set up lazy loading using the loadChildren property:

```typescript
// app-routing.module.ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { HomeComponent } from './home/home.component';

const routes: Routes = [
  { path: '', component: HomeComponent },
  { 
    path: 'products',
    loadChildren: () => import('./products/products.module').then(m => m.ProductsModule)
  },
  { 
    path: 'admin',
    loadChildren: () => import('./admin/admin.module').then(m => m.AdminModule)
  },
  { path: '**', redirectTo: '' }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

The `loadChildren` property uses the dynamic import() syntax which returns a Promise. The `then()` method extracts the Module class from the loaded module file.

#### 4. Ensure No Import References to Lazy Modules:

Ensure that lazily loaded modules are not imported directly in the AppModule or other eagerly loaded modules. Doing so would defeat the purpose of lazy loading.

```typescript
// app.module.ts
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { HomeComponent } from './home/home.component';

// DO NOT import ProductsModule or AdminModule here!

@NgModule({
  declarations: [
    AppComponent,
    HomeComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

### Preloading Strategies:

Angular provides strategies to preload lazy modules in the background after the app has finished loading:

1. **No Preloading (Default)**: Only loads modules when needed
   ```typescript
   RouterModule.forRoot(routes)
   ```

2. **Preload All Modules**: Loads all lazy modules after the app loads
   ```typescript
   import { PreloadAllModules } from '@angular/router';

   RouterModule.forRoot(routes, { preloadingStrategy: PreloadAllModules })
   ```

3. **Custom Preloading**: Selectively preload certain modules
   ```typescript
   // Define custom preloading strategy
   import { Injectable } from '@angular/core';
   import { PreloadingStrategy, Route } from '@angular/router';
   import { Observable, of } from 'rxjs';

   @Injectable()
   export class CustomPreloadingStrategy implements PreloadingStrategy {
     preload(route: Route, load: () => Observable<any>): Observable<any> {
       return route.data && route.data.preload ? load() : of(null);
     }
   }

   // In app-routing.module.ts
   const routes: Routes = [
     // ...
     { 
       path: 'products',
       loadChildren: () => import('./products/products.module').then(m => m.ProductsModule),
       data: { preload: true }
     },
     // ...
   ];

   RouterModule.forRoot(routes, { preloadingStrategy: CustomPreloadingStrategy })
   ```

### Monitoring Lazy Loading:

You can verify that lazy loading is working by:

1. Checking the Network tab in browser DevTools to see chunks being loaded when navigating
2. Using the Angular CLI command `ng build --stats-json` and then analyzing the output with tools like Webpack Bundle Analyzer

### Best Practices:

1. **Group Related Functionality**: Each lazy-loaded module should represent a cohesive feature area
2. **Balance Module Size**: If a module is too large, consider splitting it further
3. **Shared Module**: Use a shared module for common components, directives, and pipes
4. **Consider Preloading**: For better UX, preload modules that are likely to be needed
5. **Analyze Bundles**: Regularly check bundle sizes to ensure effective code splitting

Lazy loading is an essential technique for optimizing Angular applications, particularly for larger applications with multiple feature areas, where it can significantly improve initial load time and overall user experience.

## 18. What is RxJS and how is it used in Angular?

**Answer:**
RxJS (Reactive Extensions for JavaScript) is a library for reactive programming using Observables, providing a powerful way to manage asynchronous data and events. Angular deeply integrates with RxJS to handle asynchronous operations, event handling, and state management throughout the framework.

### Core Concepts of RxJS:

1. **Observable**: Represents a stream of data or events over time.
2. **Observer**: Consumes values delivered by an Observable through next(), error(), and complete() callbacks.
3. **Subscription**: Represents the execution of an Observable and allows you to cancel the execution.
4. **Operators**: Pure functions that enable functional programming style transformations of data.
5. **Subject**: A special type of Observable that allows values to be multicasted to many Observers.
6. **Schedulers**: Control when subscription begins and when notifications are delivered.

### How Angular Uses RxJS:

#### 1. HTTP Client:
Angular's HttpClient returns Observables for HTTP requests, allowing for reactive handling of responses:

```typescript
import { HttpClient } from '@angular/common/http';
import { Injectable } from '@angular/core';
import { Observable } from 'rxjs';
import { catchError, retry } from 'rxjs/operators';

@Injectable({
  providedIn: 'root'
})
export class DataService {
  private apiUrl = 'https://api.example.com/data';
  
  constructor(private http: HttpClient) { }
  
  getData(): Observable<any[]> {
    return this.http.get<any[]>(this.apiUrl).pipe(
      retry(2),
      catchError(this.handleError)
    );
  }
  
  private handleError(error: any) {
    console.error('An error occurred:', error);
    // Return a safe error for the component
    return throwError(() => new Error('Something went wrong; please try again later.'));
  }
}
```

#### 2. Reactive Forms:
Angular's Reactive Forms provide observables to track form values and validation status:

```typescript
import { Component, OnInit } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';
import { debounceTime, distinctUntilChanged } from 'rxjs/operators';

@Component({
  selector: 'app-search',
  template: `
    <form [formGroup]="searchForm">
      <input type="text" formControlName="searchTerm">
    </form>
    <div *ngIf="results$ | async as results">
      <!-- Display results -->
    </div>
  `
})
export class SearchComponent implements OnInit {
  searchForm: FormGroup;
  results$: Observable<any[]>;
  
  constructor(
    private fb: FormBuilder,
    private searchService: SearchService
  ) { }
  
  ngOnInit() {
    this.searchForm = this.fb.group({
      searchTerm: ['', Validators.required]
    });
    
    this.results$ = this.searchForm.get('searchTerm').valueChanges.pipe(
      debounceTime(300),
      distinctUntilChanged(),
      switchMap(term => this.searchService.search(term))
    );
  }
}
```

#### 3. Router Events:
Angular Router emits events as observables:

```typescript
import { Router, NavigationStart, NavigationEnd } from '@angular/router';
import { filter } from 'rxjs/operators';

@Component({
  selector: 'app-root',
  template: `
    <div *ngIf="loading">Loading...</div>
    <router-outlet></router-outlet>
  `
})
export class AppComponent implements OnInit {
  loading = false;
  
  constructor(private router: Router) { }
  
  ngOnInit() {
    this.router.events.pipe(
      filter(event => event instanceof NavigationStart || event instanceof NavigationEnd)
    ).subscribe(event => {
      if (event instanceof NavigationStart) {
        this.loading = true;
      } else if (event instanceof NavigationEnd) {
        this.loading = false;
      }
    });
  }
}
```

#### 4. Component Communication:
RxJS Subjects enable communication between components:

```typescript
// Shared service
import { Injectable } from '@angular/core';
import { Subject } from 'rxjs';

@Injectable({
  providedIn: 'root'
})
export class MessageService {
  private messageSubject = new Subject<string>();
  
  // Observable stream that components can subscribe to
  public messages$ = this.messageSubject.asObservable();
  
  // Method to send messages
  sendMessage(message: string) {
    this.messageSubject.next(message);
  }
}

// Sender component
@Component({
  selector: 'app-sender',
  template: `<button (click)="sendMessage()">Send Message</button>`
})
export class SenderComponent {
  constructor(private messageService: MessageService) { }
  
  sendMessage() {
    this.messageService.sendMessage('Hello from sender!');
  }
}

// Receiver component
@Component({
  selector: 'app-receiver',
  template: `<p>Latest message: {{ latestMessage }}</p>`
})
export class ReceiverComponent implements OnInit, OnDestroy {
  latestMessage: string;
  private subscription: Subscription;
  
  constructor(private messageService: MessageService) { }
  
  ngOnInit() {
    this.subscription = this.messageService.messages$.subscribe(
      message => this.latestMessage = message
    );
  }
  
  ngOnDestroy() {
    this.subscription.unsubscribe();
  }
}
```

#### 5. State Management:
RxJS can be used to implement simple state management:

```typescript
import { BehaviorSubject, Observable } from 'rxjs';
import { map } from 'rxjs/operators';

interface AppState {
  user: User | null;
  isLoading: boolean;
  error: string | null;
}

@Injectable({
  providedIn: 'root'
})
export class StateService {
  private initialState: AppState = {
    user: null,
    isLoading: false,
    error: null
  };
  
  private stateSubject = new BehaviorSubject<AppState>(this.initialState);
  private state$ = this.stateSubject.asObservable();
  
  // Selectors
  getUser(): Observable<User | null> {
    return this.state$.pipe(map(state => state.user));
  }
  
  getIsLoading(): Observable<boolean> {
    return this.state$.pipe(map(state => state.isLoading));
  }
  
  getError(): Observable<string | null> {
    return this.state$.pipe(map(state => state.error));
  }
  
  // Actions
  setLoading(isLoading: boolean) {
    const currentState = this.stateSubject.getValue();
    this.stateSubject.next({
      ...currentState,
      isLoading
    });
  }
  
  setUser(user: User | null) {
    const currentState = this.stateSubject.getValue();
    this.stateSubject.next({
      ...currentState,
      user
    });
  }
  
  setError(error: string | null) {
    const currentState = this.stateSubject.getValue();
    this.stateSubject.next({
      ...currentState,
      error
    });
  }
}
```

### Commonly Used RxJS Operators in Angular:

1. **map**: Transforms values from source observable
   ```typescript
   import { map } from 'rxjs/operators';
   
   this.http.get<User[]>('/api/users').pipe(
     map(users => users.map(user => user.name))
   );
   ```

2. **filter**: Filters values based on a predicate
   ```typescript
   import { filter } from 'rxjs/operators';
   
   this.userService.getUsers().pipe(
     filter(user => user.isActive)
   );
   ```

3. **switchMap**: Cancels previous subscriptions when a new value is emitted
   ```typescript
   import { switchMap } from 'rxjs/operators';
   
   this.searchForm.get('term').valueChanges.pipe(
     debounceTime(300),
     switchMap(term => this.searchService.search(term))
   );
   ```

4. **catchError**: Handles errors in observable sequences
   ```typescript
   import { catchError } from 'rxjs/operators';
   
   this.http.get('/api/data').pipe(
     catchError(error => {
       console.error('Error:', error);
       return of([]); // Return a safe value
     })
   );
   ```

5. **tap**: Performs side effects without modifying the stream
   ```typescript
   import { tap } from 'rxjs/operators';
   
   this.userService.getUser(id).pipe(
     tap(user => console.log('Fetched user:', user))
   );
   ```

6. **debounceTime**: Waits until a specified time has passed before emitting values
   ```typescript
   import { debounceTime } from 'rxjs/operators';
   
   this.searchInput.valueChanges.pipe(
     debounceTime(300) //