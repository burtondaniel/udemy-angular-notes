# udemy-angular-notes
https://www.udemy.com/the-complete-guide-to-angular-2
Notes from the above angular 2 course on Udemy.

## 11. A Basic Project Setup using Bootstrap for Styling
You can add libraries (css and js) using NPM.

npm install --save bootstrap


Which will install the library in the node_modules folder in your project, then add the css to the .angular-cli.json file:

![styles](./images/11-bootstrap-css-link.png)

## 13. How an Angular App gets Loaded and Started
index.html is served by the server - this is defined in the angular-cli file
There is an <app-root> element  in there by default. An app component can bind to it as follows:

![app-component](./images/11-app-component-binding.png)

Selector identifies the dom element to be replaced with the content of the component.

Ng serve creates script bundle and chucks them into the index.html file that cause angular to be bootstrapped.
Main.ts is the file which contains the JS / TS that is executed first.
Example:

![main.ts](./images/11-main-ts.png)

It loads the app.module file, which in turn declares components to load, imports, etc.

Hence:

![app-load-order](./images/11-app-load-order.png)

## 14. Components are Important!
Components - reusable pieces on a page, ie. a nav bar can be a component, main content can be a component

## 15. Creating a New Component
Generally you place new components under the app directory in the src folder.
Recommends putting components in their own folders

Naming convention:
<component-name>.component.ts

A component is just a TS class. You use decorators (annotations) to enable the angular specific functions. Components need to be imported from packages provided by angular.

Configuration / metadata for components are provided inside these decorators. 

selector: used to identify a dom element to use as the destination - start it with app-, ie. app-server
templateUrl: used to specify a HTML document to use as a template, relative to the TS file
```typescript
  import { Component } from ‘@angular/core’

  @Component({
    selector: ‘app-server’,
    templateUrl: ‘./server.component.html’
  })
  export class ServerComponent {

  }
```

## 16. Understanding the Role of AppModule and Component Declaration
Modules - used to bundle components. Mainly used in bigger applications.

AppModule - default module in an app

@NgModule decorator is used to define properties of the module.

declarations: register components here as an array (need to import the components first - omit the .ts extension on imports)
imports: allows import of other modules. - examples of default ones:
	BrowserModule
	FormsModule
	HttpModule
providers: not sure yet
bootstrap: - array of components to use for index.html (default components)

## 17. Using Custom Components
In the templateUrl file, create a dom element using the selector you defined in the corresponding component.

## 18. Creating Components with the CLI & Nesting Components
While `ng serve` is running

```
  ng generate component <component-name>
  ng g c <component-name>
```

It will create the .ts, .css, .spec and .html files for you, as well as add the imports to the app module. It creates the component in it’s own folder under the app folder.

The Selectors you use in a component can be placed into html files as many times as you like, and an instance of the component will be placed into each.

## 19. Working with Component Templates
Basically says you need one of either template or templateUrl

## 20. Working with Component Styles
Use the styleUrls or styles parameters in @Component argument to specify styles

If using styles, you can use backticks to have line breaks etc accepted by javascript.
```typescript
  styles: [`
    h3 {
        color: dodgerblue;
    }
  `]
```
## 21. Fully Understanding the Component Selector
You can use a subset of CSS selectors to find an element to bind the angular component to.
Classes:  .app-servers
Attributes: [app-servers]
Id tag wont work
Pseudo selectors (hover etc) won’t work

Best practice is to just create your own element and use that in the selector.

Seems to generate warnings if you don’t use app- to start the name of the selector.

## 22. What is Databinding?
Dynamically updated data (from your business logic) reflected in templates.
String interpolation:
	`{{ data }}`
Property binding:
	`[property] = “data”`

Typescript can react to user events via Event Binding 
Two way binding: bind typescript vars to user update of data

```
  [(ngModel)] = “data”
```
  
## 23. String Interpolation

Define vars in your component class:
```
  export class ServerComponent {
      serverId = 10;
      serverStatus: string = ‘offline’;
  }
```
Output it in the template:

```
  <p>Server with ID {{ serverId }} is {{ serverStatus }}</p>
```

Any condition (other than multiline / block expressions) which can be resolved to a string is acceptable inside the curly braces.
Ie: `{{ ‘server’ }}`

Can also return results of a method in the component:

```
  export class ServerComponent {
    serverStatus = 'offline';

    getServerStatus() {
        return this.serverStatus;
    }
  }
```
and in the template:
```
  {{ getServerStatus() }}
```

## 24. Property binding

### Binding to HTML element properties
Example given involves binding a variable to the disabled state of a button.

```
  <button class="btn btn-primary" [disabled]="allowNewServer">Add Server</button>
```

Adding code to the constructor to make the allowNewServer var change and highlight the binding:

```
  allowNewServer = false;
  
  constructor() {
    setTimeout(() => {
      this.allowNewServer = true;
    }, 2000);
  }
```

The binding occurs in the `[disabled]="allowNewServer"` code.

## 25. Property Binding vs String Interpolation

String interpolation:

`{{ allowNewServer }}`

Property binding:

`<p [innerText]="allowNewServer"></p>`

innerText is a property of all elements? It is used to refer to the text inside an element.

String interpolation and property binding cannot be mixed, ie. you can't use string interpolation inside of a property binding.

## 26. Event Binding

Suggests a convention of naming methods that are bound to events in the template as `onXxx()`

IN the template:

`(click)="onCreateServer()"`

where click is an event such as onClock, onMouseEnter etc., and it's value is a method defined in the component (or even define the method inline)

Final product:

```<button class="btn btn-primary" [disabled]="!allowNewServer" (click)="onCreateServer()">Add server</button>```

## 28. Passing and Using Data with Event Binding

input elements can bind to the input event.

```<input (input)="onUpdateServerName($event)"/>```

`$event` is a reserved variable name you can use in a template with event binding. It is the data associated with the triggering event.

COmponent code:

```typescript
onUpdateServerName(event: Event) {
  console.log(event.target.value)
}```

this code will trigger the event on each keystroke that occurs.

Can cast typescript:

```typescript
(<HtmlInputElement>event.target).value
```

## 29. Two-Way-Databinding

There is an easier way of performing event binding, using the `ngModel` directive.

Note: `FormsModule` from `@angular/forms` needs to be imported first in the `AppModule`.

```
  <input [(ngModel)]="serverName"
```

It will update the value of `serverName` in your component automatically, and vice-versa if the variable changes from another source.

## 31. Combining all forms of Databinding

Overview of the types of databinding covered so far.

## Assignment 2 - implementing databinding

app.component.html

```html
  <ol>
    <li>Add a Input field which updates a property ('username') via Two-Way-Binding</li>
    <input [(ngModel)]="username"/>
    <button class="btn btn-primary" [disabled]="isButtonDisabled()" (click)="onResetUsername()" >Reset username</button>
    <p>Username entered is {{ username }}</p>
    <li>Output the username property via String Interpolation (in a paragraph below the input)</li>
    <li>Add a button which may only be clicked if the username is NOT an empty string</li>
    <li>Upon clicking the button, the username should be reset to an empty string</li>
  </ol>
```

app.component.ts

```typescript
  import { Component } from '@angular/core';

  @Component({
    selector: 'app-root',
    templateUrl: './app.component.html',
    styleUrls: ['./app.component.css']
  })
  export class AppComponent {
      username: string = "";

      isButtonDisabled() {
          return this.username.trim().length === 0;
      }

      onResetUsername() {
          this.username = "";
      }
  }

```

## 32. Understanding Directives

Directives are **instructions in the DOM**.

Components are directives with a template.

Example of a directive:

`<p appTurnGreen>Receives a green background</p>`

Can bind to the attribute via a directive selector:

```typescript
@Directive({
  selector: '[appTurnGreen]'
})
export class TurnGreenDirective {
  ...
}
```

## 33. Using ngIf to output data conditionally

`ngIf` works like a regular `if` statement. 
It is a structural directive, hence requires different syntax, and takes an expression as it's value:

```html
<p *ngIf="someBooleanValue">someBooleanValue must be true if you can see this</p>
```

## 34. Enhancing ngIf with an Else Condition

Place a local reference (**#**) on the element, and make the element an ng-template

```
<p *ngIf="serverCreated; else noServer">Server was created, name is {{serverName}}</p>
<ng-template #noServer>
  <p>No server was created!
</ng-template>
```

## 35. Styling Elements Dynamically with ngStyle

Attribute directives

`ngStyle`

Allows you to dynamically add css styles 

Requires configuration to do anything - takes a javascript object as a parameter.

We use property binding to bind a value to the ngStyle directive. 

```
  <p [ngStyle]="{backgroundColor: getColour()}">Blah blah blah</p>
```

## 36. Applying CSS Classes Dynamically with ngClass

Attribute directive:

`ngClass`

Allows you to dynamically add or remove css classes. Also requires property binding.
Takes a property which is a javascript object, of key-values which are:

key: the css class
value: the condition under which it will apply

```
  <p [ngClass]="{online: server-status === 'online'}">Blah blah blah</p>
```

## 37. Outputting Lists with ngFor

ngFor

It is another structural directive - hence the need to prefix an *.

```
  <app-server *ngFor="let server of servers"></app-server>
```

Where `server` represents an item in the servers array.

## 38. Getting the Index when using ngFor

Use `index` (in a reserved keyword sense) in the ngFor definition.

`*ngFor="let logItem of log; let i = index"`

`i` will now be available to use in conditional statements.


### 39. Project discussion
## 40. Planning an app

Splitting an app up into features, and then components.

### Components
- Root component
- Header component

#### Shopping list
- Shopping List
- Shopping list edit

#### Recipe book (limited to displaying, no editing)
- Recipe containing:
- Recipe List
- Recipe Item

- Recipe Detail

### Model items
#### Shopping list
- Ingredient

#### Recipe book
- Recipe

## 41. Setting up the application
ng new projectname
npm install --save bootstrap

add bootstrap to the .angular-cli.json file under the apps.styles property:

```json
"styles": [
  "../node_modules/bootstrap/dist/css/bootstrap.min.css",
  "styles.css"
],```

Discusses the use of Emmet - Using tab to autocomplete to knock up html quickly

ie.

`div.container>.row>p`

pressing tab would then create associated HTML dom

## 42. Creating the components

Nested components (with spec disabled):

`ng g c recipes --spec false`

`ng g c recipes/recipe-list --spec false`

## 43. Using the components

Just adding html content to the components - nothing more than that.

## 44. Adding a navigation bar

More HTML - header nav based on bootstrap.

## 46. Creating a recipe model 

Just a TS class:

```typescript
export class Recipe {
  public name: string;
  public description: string;
  public imagePath: string;

  constructor(name: string, description: string, imagePath: string) {
    this.name = name;
    this.description = description;
    this.imagePath = imagePath;
  }
}
```

## 47. Adding content to the recipes component

Using instance of the recipe class as an array

`import { Recipe } from '../recipe.model';`

`...`

`recipes: Recipe[] = [];`

Populated the HTML with an ngFor where required to show the recipe on the page.

## 48. Outputting a List of Recipes with ngFor

for the img src, you have two options:

String interpolation: `src="{{ recipe.imagePath }}`

or

Property binding: `[src]="recipe.imagePath"`


## 49, 50

Same stuff -- only HTML changes

## 51. Creating Ingredient model

Creating data objects without verbosity - add visibility modifiers to the constructor params.

```typescript
export class Ingredient {

  constructor(public name: string, public amount: number) {
  }
}
```

## 52. Creating and Outputting the Shopping List

Add ngFor to the ul in shopping-list and populate ingredient details.

## 53, 54. Adding a Shopping List Edit Section and wrapping up the HTML stuff

.. HTML edits

# Section 4 - Debugging
## 55. Understanding Angular Error Messages
Straight-forward example of debugging stack trace in the browser console.

## 56. Debugging code in the browser using source maps

With enableProdMode() not called, you're in development mode. You get access to source maps in this mode.

Putting break points in the compiled js in the browser console will jump you to the associated ts file source.

Source TS is available at runtime in the browser under the sources path webpack/./src/app/

## 57. Using Augury to Dive into Angular Apps

A chrome dev tools extension that helps you debug angular apps.

Gives you visibility of the loaded components and their properties at runtime, as well as injection graphs etc.

# Section 5. Understanding Components and Databinding

## 59. Splitting Apps into Components

Starts with a small app that has everything in one component, and defines discreet parts that he then splits the app up into.

Involves moving the HTML and then the corresponding component code.

Discusses need for passing data between components.

## 60. Property & Event Binding Overview

Concept of passing data in the form of events between different elements on a page. 

Can be used on:
* HTML elements (native events and properties) 
* Directives (custom properties and events)
* Components (custom properties and events)

## 61. Binding to Custom Properties

_Summary: use @Input() to allow a property to be assigned to from another component_

In a low level Component (ie ServerComponent which is used in a list somewhere) define a property of type JS object

```typescript
export class ServerElementComponent {
  element: {type: string, name: string, content: string}
}
```

Then in the Component which will use it:

```typescript
export class AppComponent {
  serverElements = [{type: 'server', name: 'TestServer', content: 'Just a test!'}];
}
```
html:

```
<app-server-element
*ngFor="let serverElement of serverElements"
[element]="serverElement"></app-server-element>
```

This still won't work - the property in ServerElementComponent needs a __Decorator__.

```typescript
import { Component, Input } from '@angular/core';
...
@Input() element: {type: string....}
```

This now allows any component using this ServerElementComponent can bind to the `element` property.

## 62. Assigning an Alias to Custom Properties

Inside the @Input() decorator, you can provide an alias for the name of the property, that can then be used to bind to.

`@Input('srvElement') element: ...`

and to use it:

`[srvElement]="serverElement"`

## 63. Binding to Custom Events

In the selector tag for an embedded component, you can add an attribute that can be used to hook up the results of an event to a handler method in the parent component.

`<app-cockpit (serverCreated)="onServerAdded($event)"></app-cockpit>`

We would then expect the app-cockpit associated component to have a property called:

`serverCreated`

And make it an event that can be emitted using the EventEmitter (with the property name matching the reference in the parent template)s:

```import {EventEmitter, Output} from @angular/core'
...
@Output()
serverCreated = new EventEmitter<{serverName: string, serverContent: string>
... // and in the method that emits it
this.serverCreated.emit({serverName: 'servername', serverContent: 'newContent'});```

## 63. Assigning an alias to Custom Events

Easy:

```@Output('aliasName')```

## 66. Understanding View Encapsulation

Angular encapsulates CSS styles to the components they are defined within.

It achieves this through all elements within a component getting the same attribute assigned to them, in order to uniquely identify it.

## 67. More on View Encapsulation

You can override this behaviour by changin the encapsulation parameter in the @Component annotation

```
encapsulation: ViewEncapsulation.Native, 
ViewEncapsulation.None, 
ViewEncapsulation.Emulated
```
`None`:
Styles defined in the associated Component will then apply application wide.

`Native`:
Uses 'shadow dom' which is not supported in all browsers

`Emulated`:
default encapsulation

## 68. Using Local References (#) in Templates

Get the value of an element without using 2way binding, by using a local reference.

It can be placed on any HTML element. 

```angular
 <input #serverNameInput></input>
```

It holds a reference to the entire HTML element (type HTMLInputElement in the case of an input). It can be used anywhere in the template (not in the typescript code however).

(click)="onAddServer(serverNameInput)"

## 69. Getting Access to the Template and DOM with @ViewChild

@ViewChild decorator (annotation) is used on properties within a Component.

```typescript
@ViewChild('serverContentInput') serverNameInput: ElementRef;
```

Argument is required - the Local Reference is used here. Can be accessed in the template as follows:

```typescript
this.serverNameInput.nativeElement.value
```

This can then be accessed at any time.

Advises to not set the value of the elements through this method - use property binding or other methods that are better suited to this.

## 70. Projecting Content into Components with ng-content

Mark a place in a template that will have the contents inside of the components selector tag injected.

Given a component called HelloComponent
```html
<app-hello><p>Hello!</p></app-hello>
```

Inside the template for the component:
```html
  <ng-content></ng-content>
```

It will output `<p>Hello!</p>`

This is called 'projection' - good for generating generic things such as tabs.

## 71. Understanding the Component Lifecycle

You can hook into the phases of a component's lifecycle at the following points:

- `ngOnChanges` - called after a bound input property changes (`@Input`) - takes param of SimpleChanges
- `ngOnInit` - called once the component is initialized (before it's rendered)
- `ngDoCheck` - called during every change detection run (system by which angular determines if something has changed in a component - ie. property value, etc, or also even from events firing such as clicks), also from promises returning
- `ngAfterContentInit` - after content (ng-content) has been projected into view
- `ngAfterContentChecked` - after projected content has been checked
- `ngAfterViewInit` - called after the component's view and CHILD VIEWS have been initialized
- `ngAfterViewChecked` - called every time the view (and child views) have been checked
- `ngOnDestroy` - once a comonent is about to be destroyed

## 72. Seeing Lifecycle Hooks in Action

implement them by implementing the methods in a component, and making the component implement the interface of the same names (minus ng prefix).

ngOnChanges() - when called you get access to current value, previous value, etc

ngAfterContentInit() 

 
## 73. Lifecycle Hooks and Template Access
 
@ViewChild  - only available when afterViewInit is loaded.
 
`@ViewChild('heading') heading: ElementRef;`

`<div #heading></div>`

If you were to access heading in onInit it would be empty.

## 74. Getting access to ng-content with @ContentChild

@ContentChild('contentParagraph'): content ElementRef;

Can access an element that's projected inside of an ng-content block. - otherwise in the outer block it could be accessed via ViewChield. 


### end of section

# Section 6 - Components & Data Binding

## 78. Adding Navigation with Event Binding and ngIf

Uses same approach as in the assignment to do a hacky implementation of navigation.

## 79. Passing Recipe Data with Property Binding

As above - moved recipe list implementation into the list component, and used property binding to display the details in the recipe item component.

## 80. Passing Data with Event and Property Binding combined

Passing the results of clicking on a recipe in the list up 2 levels to the details component. 

## 81. Allowing the User to Add Ingredients to the Shopping List

Used @ViewChild to bind properties to view references (#blahInput attribute on an input element) of type ElementRef.

# Section 7 - Directives Deep Dive

## 82. Intro

Attribute directives vs Structural directives

Structural directives prefixed with * and affect existence of the element. Attribute directives only affect the element they exist on.

## 83. ngFor and ngIf recap

ngFor: 

`<div *ngFor="let number of numbers">
{{ number }}
</div>`

ngIf:

`<div *ngId="!onlyOdd">
...
</div>`

Nothing new here - just a note that you can't have two structural directives on the one element.

## 84. ngClass and ngStyle recap

ngClass:

`<li [ngClass]="{odd: true}"></li>`

ngStyle:

`<li [ngStyle]="{backgroundColor: 'yellow'}"></li>`

## 85. Creating a basic attribute directive

Creating a directive to highlight an element on hover, using an attribute: 'appBasicHighlight'

> basic-highlight.directive.ts

`@Directive({
    selector: '[appBasicHighlight]' // attribute selector
})
export class BasicHighlightDirective {
    constructor(private elementRef: ElementRef) {
    }
    
    ngOnInit() {
        this.elementRef.nativeElement.style.backgroundColor = 'green';
    }
}`

Injection provides a reference to the element the directive is on - of type ElementRef.

From it you can access it's  `nativeElement` property.

Can use OnInit to initialise data.

To then use it, you need to add the class in the `declarations` array in the module the directive is in.

## 86. Using the renderer to build a better attribute directive

Accessing elements directly like that is not advised - requires rendering in a browser, so there are other ways of doing this.

Injecting a renderer.

type: Renderer2 from @angular/core

Gives you access to set certain render properties.

`this.renderer.setStyle(this.elementRef.nativeElement, 'background-color', 'blue', false, false)`

Args being:

element to change 
css style attribute
attribute value
flags (options) - optional

This is a better approach because - angular isn't limited to running in the browser (ie. works with service workers, that doesnt have access to the DOM). 

## 88. Using HostListener to listen to host events

Reacting to events occuring in the element the directive sits on.

@HostListener decorator can assist with this. Takes a string arg of the event to listen for.

`@HostListener('mouseenter') mouseover(eventData: Event) {
    this.renderer.setStyle(this.elementRef.nativeElement, 'background-color', 'blue', false, false);
}

@HostListener('mouseleave') mouseleave(eventData: Event) {
    this.renderer.setStyle(this.elementRef.nativeElement, 'background-color', 'transparent', false, false);
}`

The directive will now react to the events occuring via the HostListener.

## 89. Using HostBinding to listen to host properties

Another decorator - `@HostBinding`

Can define which property of the host element we want to bind to.

`@HostBinding('style.backgroundColor') backgroundColor: string = 'transparent';`

On the element which this directive sits, allow us access to the given property.

Can then do:

`this.backgroundColor = 'blue';` 

instead of using the renderer.

## 90. Binding to directive properties

Using custom property binding - ie. allowing input values

`@Input() defaultColor: string = 'transparent';
@Input() highlightColor: string = 'blue';
@HostBinding('style.backgroundColor') backgroundColor: string;

ngOnInit() {
   this.backgroundColor = this.defaultColor;
}`

You then provide the input as follows

`<p appBetterHighlight [defaultColor]="'yellow'" [highlightColor]="'red'">`

Angular looks for directives with inputs matching the property binding, if it doesnt find a match it falls through looking for it on the element (p in this case)

Also supports use of Aliases in the @Input  - if you set the alias to the same as the selector, you can then do the following:

[appBetterHighlight]="'red'"

## 91. What happens behind the scenes on structural directives

Discussion of the `*` char in front of structural directives - it's simply used so that angular can detect them in the dom - and wrap the element in an ng-template

```<ng-template></ng-template>``` 

This is used to create elements that only exist for the purpose of angular

ie:

```<div *ngIf="!onlyOdd"></div>```

is translated to

`<ng-template [ngIf]="!onlyOdd">
    <div></div>
</ng-template>`

## 92. Building a structural directive

creating the opposite of the ngIf directive:

`ng g d unless`

Provide a property with a setter so that you can hook in to changes to the property:

`@Input() set appUnless(condition: boolean) {
    if (!condition) {
        this.vcRef.createEmbeddedView(this.templateRef);
    } else {
        this.vcRef.clear()
    }
}

constructor(private templateRef: TemplateRef<any>, private viewContainerRef: ViewContainerRef) {

}
`
TemplateRef - reference to the template
ViewContainerRef - place where we put the directive in a DOM

vcRef.clear() - remove from the dom

```
<div *appUnless="onlyOdd"></div>
```
Input name has to match the directive selector...

## 93. Understanding ngSwitch

Using it:

`<div [ngSwitch]="value">
    <p *ngSwitchCase="5"></p>
    <p *ngSwitchCase="10"></p>
    <p *ngSwitchCase="20"></p>
    <p *ngSwitchDefault="100"></p>
</div>`

## 94. Building and using a toggle dropdown directive

Simply adds / removes a class to a div.

Can bind to a class:

`@HostBinding('class.open') isOpen = false;`

Setting it to true will add the class, false removes it.


`import {Directive, HostBinding, HostListener} from '@angular/core';

@Directive({
  selector: '[appDropdown]'
})
export class DropdownDirective {

  @HostBinding('class.open') isOpen = false;

  @HostListener('click')
  click(eventData: Event) {
    this.isOpen = !this.isOpen;
  }

  constructor() {
  }

}
`








# Section 11 - Modules + routing

## 112. Intro

Adding URL binding to an app.


## 113. Why do we need a router

The need to have pages specifically for site functions - ie. admin page, server list page, etc.

Can register routers - url paths to these components - using the angular router

## 115. Setting up and loading routes

Currently, the app loads all components in the one module.

Routes are a core part of the app - as there's only one active URL at a time. Hence, the app module is the best place to begin informing Angular about the routes you want to have defined.

You define a route, and then an 'action' to perform at that route. Generally you use components here.

> app.module.ts

`
const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'users', component: UsersComponent },
  { path: 'servers', component: ServersComponent }
];`

and then

`
    import: [
        ...
        RouterModule.forRoot(routes)
    ]
`

You then need a __router outlet__ in the bootstrapped component template

It tells Angular where to render the component of the currently selected route.

## 116. Navigating with Router Links

Initially says you can use a href="/users" style links to point to router paths. 

This works, but it restarts the app on each click, and application state will be lost.

Can use the angular directive called `routerLink` instead.

`<a routerLink="/users">`

Can also use property binding to use arrays as args:

`<a [routerLink]="['/users']">`

Router link - 

1. catches link clicks
2. prevents default
3. Determines which route to load

## 117. Understanding navigation paths

Use of "/servers" vs "servers" in routerLink args.

Absolute vs Relative paths.

They are relative to the COMPONENTS themselves and where they are loaded in the app.

app.component.ts is at the root level - so thats the context of its urls

ServerComponent is only loaded when at the /servers context path. As such any relative links are below that context level of the URL path.

Can also use the `./` and `../` sequences as part of the paths.

## 118. styling active router links

`routerActiveLink='active'`

a directive that allows you to add classes based on the active route.

The directive analyses your current URL path, and also which links in the dom beneath that element contain links to the active path. If it contains the path, it will add the class that is the argument.

the directive can have config provided as follows:

`[routerLinkActiveOptions]="{
    exact: true
}"`

This is used to check for exact matches, as opposed to a contains match.

## 119. Navigating Programmatically

Allows you to perform navigation after other logic in a component.

Inject the router into a component, and then:

`this.router.navigate(['/users']);`

Routes defined as an array of the single elements of the desired url.

## 120. Using relative paths in programmatic navigation

Unlike routerLink, the navigate method doesn't know on which route you're currently on.

We can give context via relativeTo option as follows:

Getting a hook to the route which loaded this component - the ActivatedRoute: 

`constructor(private route: ActivatedRoute){}`

and then

`this.router.navigate(['/users'], {relativeTo: this.route});`

## 121. Passing parameters to routes

`
{ path: 'users/:id', component: UserComponent}
`

Colon prefix means its a dynamic part of the url (ie. parameter)

The parameter is then passed to the component.


## 122. Fetching route parameters 

Can get the parameters from the ActivatedRoute class - it has a param called snapshot which can be referred to.

`constructor(private route: ActivatedRoute) {}`

then in ngOnInit:

`
this.user = {
    id: this.route.snapshot.params['id']
}
`

## 123. Fetching route parameters reactively

Snapshot data only loads in the example above if we havent been on the component before. ngOnInit isn't called again.

We need to subscribe to the params parameter on the ActivatedRoute.

`this.route.params` => returns an Observable.

You can subscribe to the changes in the params:

`
ngOnInit() {
    this.user = {
      id: 0,
      name: ''
    }
    this.route.params.subscribe(
      (params: Params) => {
        this.user.id = params['id'];
        this.user.name = params['name'];
      }
    );
  }
`

## 124. An important note about route observables

Angular cleans up the route observable subscriptions for you when a component is destroyed.

Can implement the OnDestroy lifecycle hook to unsubscribe from the subscription manually.

## 125. Passing query parameters and fragments

URL query params

Given this:

`{ path: 'servers/:id/edit', component: EditServerComponent }`

and generating the link in a template:

`[routerLink]="['/servers', 5, 'edit']"
[queryParams]="{ allowEdit: '1' }"
fragment="loading"`

would generate:

`/servers/5/edit?allowEdit=1#loading`

and programmatically:

`this.router.navigate(['/servers', id, 'edit], {queryParams: {allowEdit: '1', fragment: 'loading'}})`

## 126. Retrieving query parameters and fragments

Using the activated route - can use the snapshot or again the observables

`this.route.snapshot.queryParams;
this.route.snapshot.fragment;`

or

`this.route.queryParams.subscribe();
this.route.fragment.subscribe();`

## 127. Practice and some common gotchas

params are always strings. if you pass them to a method that expects a number, you need to convert them to numbers. Put a + in front of the var to turn it into a number - or use `parseInt(string, 10);`

## 128. Setting up nested / child routes

can have child routes:

`{path: 'servers', component: ServersComponent, children: [
    {path: ':id', component: ServerComponent },
    {path: ':id/edit', component: EditServerComponent }
]}`

This requires a new `<router-outlet>` in the parent component, which is where the child will be rendered. 

## 130. Using query params

Loss of query params when navigating to child routes

## 131. Configuring the handling of query parameters

Pass another proeprty to the router.navigate method.

`this.router.navigate(['edit'], { queryParamsHandling: 'preserve'})`

other type is called `merge`, which allows you to combine with ones youre adding.

## 132. Redirecting and Wildcard routes

Error handling in routes (routes that dont exist - 404) by redirection

`{ path: 'not-found', component: PageNotFoundComponent },
{ path: 'something', redirectTo: '/not-found' }`

 /something will redirect to /not-found
 
 Use wildcard route path to send all matches 
 
 `{ path: '**', redirectTo: '/not-found' }`
 
 Location matters - must be the last route defined or it'll override other defined route paths.

## 133. Redirection path matching

By default, angular performs path matching by prefix. 

ie, the path '' would match every url, as every url is prefixed with nothing.

You can change this behaviour by using `pathMatch: 'full'` in the route definition.

## 134. Outsourcing the route configuration

Create an app-routing module to house complex route definitions.

`const appRoutes: Routes = {
    {path: '', component: HomeComponent}
}

@NgModule({
    imports: [
        RouterModule.forRoot(appRoutes)
    ],
    exports: [
        RouterModule
    ]
})
export class AppRoutingModule {
    
}`

Exports - tells angular from this module if i were to add this module to the imports of another module, what should be accessible in that context?

We export the RouterModule as it's what we've configured in this class.

Then import the AppRoutingModule in your AppModule.

## 135. An introduction to guards

Code executed before a route is loaded or when you want to leave a route.

One example of a guard is the canActivate guard.

## 136. Protecting routes with canActivate

Created a class called AuthGuard = implements CanActivate interface.

This interface forces you to have a canActivate method in the class. The method receives 2 parameters:

`canActivate(route: ActivatedRouteSnapshot,
state: RouterStateSnapshot): Observable<boolean> | Promise<boolean> | boolean`

Can return sync or async responses hence the return types.

Defers auth logic to a new class called AuthService which performs login, logout, and tells you whether the user is logged in or not.

in the canACtivate method, it checks if the user is authenticated, if so it returns true, otherwise it navigates away to the root route. This cancels the original navigation - up to you whether you want to return false afterwards or not.

You can then assign the guard to routes by:

`{ path: 'servers', canActivate: [AuthGuard], component: blah }`

## 137. Protecting Child Routes with canActivateChild

Other interface - `CanActivateChild` - which forces you to implement a method called canActivateChild with the same parameters as canActivate.

You then use this hook in the parent route definition.

`{ path: 'servers', canActivateChild: [AuthGuard] }`

## 138. Using a fake auth service

Just hooked up a login / logout button to the fake auth service.

## 139. Controlling navigation with canDeactivate

Lets you know whether you can leave a route or not.

ie. if you are editing a server, don't let the user leave the route unless they are finished.

Created a new service (thats actually an interface...) called 
`can-deactive-guard.service.ts`

Inside it is an interface: 
`export interface CanComponentDeactivate` that has a method: 

`canDeactivate: () => Observable<boolean> | Promise<boolean> | boolean;`

`export class CanDeactivateGuard implements CanDeactivate<CanComponentDeactivate> {
    canDeactivate(component: CanComponentDeactivate,
        currentRoute: ActivatedRouteSnapshot,
        currentState: RouterStateSnapshot,
        nextState?: RouterStateSnapshot): Observable<boolean> | Promise<boolean> | boolean {
        return component.canDeactivate();
    }
}`

We then implement the interface in our component and add the guard to our path:

>edit-server.component.ts

`canDeactivate(): Observable<boolean> | Promise<boolean> | boolean {
    if (!this.allowEdit) {
        return true;
    }
    
    if ((this.serverName !== this.server.name || this.serverStatus !== this.server.status) && !this.changesSaved) { 
        return confirm('Do you want to discard the changes?');
    } else {
        return true;
    }
}`


>app-routing.module.ts

`{ path: ':id/edit', component: EditServerComponent, canDeactivate: [CanDeactivateGuard] }`

You would now be prompted with the confirm dialog if you edit a server and attempt to navigate away without saving.


## 140. Passing static data to a route

When routes depend on data, they can be provided by another type of guard.

Example: `ErrorPageComponent`

It will display an errorMessage that will be provided to it at runtime.


>app-routing.module

`{ path: 'not-found', component: ErrorPageComponent, data: {
    message: 'Page not found!'
}}`


The data field allows you to pass data to the component via the route.

Then, in the ErrorPageComponent:

`ngOnInit() {
    this.errorMessage = this.route.snapshot.data['message'];
    // or
    this.route.data.subscribe(
        (data:Data) => {
            this.errorMessage = data['message'];
        }
    )
}`

## 141. Resolving dynamic data with the resolve guard

Need a 'resolver' to source data dynamically when a route is loaded. There is a Resolve interface provided in the angular router package.

> server-resolver.service.ts

`
@Injectable()
export class ServerResolver implements Resolve<Server> {
    constructor(private serverService: ServersService){}
    
    resolve(route: ActivatedRouteSnapshot, 
        state: RouterStateSnapshot): Observable<Server> | Promise<Server> | Server {
            this.serverService.getServer(+router.params['id']);    
        }
}`

To now add this to a route:

Add to providers in the app module

Add it on the route that you want to use it

`{ path: ':id' , component: ServerComponent, resolve: {
    server: ServerResolver
} }`

the 'server' property will now be available in the component to be loaded via the route data param.

> server.component.ts

`ngOnInit() {
    this.route.data.subscribe(
    (data: Data) => {
        this.server = data['server'] // needs to match name in the path resolve argument
    })
}`

## 142. Understanding Location Strategies

Server hosting your app in the real world needs to handle the case that when a 404 error occurs, it returns the index.html file containing your angular app.

Fallback is using the # sign in your route - has compatibility with older apps.

>app-routing.module.ts

`RouterModule.forRoot(appRoutes, {useHash: true})`

Forces webservers to ignore URL after the #, allowing angular to handle it.

Default mode is called HtmlHistory.

# Section 12 - Course projet - routing

Just applies previous learnings.

# Section 13 - Understanding Observables

## 159. Module Introduction

An observable is a data source. Provided by RxJS.

Can represent events emitted over time.

User input events, http requests, manual triggers in code, etc.

OTher part is the Observer (the subscribe function).

You have 3 ways to hook into an observer

1. Handle data received
2. Handle error received
3. Handle completion (some dont ever complete)

Data is provided asynchronously. 

## 160. Angular 6 and RxJS 6

Syntax differs with RxJS 6 (way you import things) - if upgrading to angular 5, you need to change the imports in your project. Otherwise you can import the following module:

`npm install --save rxjs-compat`

It exposes imports that match the RxJS way of doing it < 6

## 161. Analyzing a built-in angular observable

Looks at `route.params` 

`this.route.params.subscribe(
    (params: Params) => {
        this.id = +params['id'];
    },
    () => {},
    () => {}
)`

subscribe takes 3 args - first is on new data, second is on error, third is on completion.

## 162. Building & Using a first simple observable

An observable to emit ascending numbers at a fixed interval.

`import { Observable } from 'rxjs/Observable';

...

const myNumbers = Observable.interval(100);
myNumbers.subscribe(
    (number: number) => {
        console.log(number);
    }
);`

Observable.interval method takes param of how many ms to wait before emiting a nummber.

## 163. Building a custom observable

One that will fire after 2 and 4 seconds, then fail after 5 seconds.

`const myObservable = Observable.create((observer: Observer) => {
    
});`

create() takes a function as an argument, and should hold your async code.

We tell the Observer that's passed in to the anonymous function when to emit events.


`const myObservable = Observable.create((observer: Observer) => {
    setTimeout(() => {
        observer.next('first package');
    }, 2000);
    setTimeout(() => {
        observer.next('second package');
    }, 4000);
    setTimeout(() => {
        observer.error('this does not work');
    }, 5000);
});`

next() pushes the next data package into the observable.
error() pushes an error.

you can then subscribe to it:

`
myObservable.subscribe(
    (data: string) => { console.log(data); },
    (error: string) => { console.error(error); },
    () => { console.log('completed'); }
)`

Can also call complete() in the observable definition - this will complete the observable.

Need to unsubscribe too!! Don't forget

## 166. Using Subjects to pass AND listen to data























... skipped to next section.

# Section 14 - Handling Forms in Angular Apps

## 173. Template driven vs Reactive approach

Template driven - angular infers the form object from the DOM (simpler)

Reative - Form is created programmatically and synched with the DOM. (more complex)

## 174. An example form

Typical `<form>` where inputs have an id.

`<form>` in his example doesnt have action or method defined. This is because we want Angular to handle the form for us. 

## 175. Creating the form and registering the controls.

Import the `FormsModule` in your app - required for the template driven approach.

Angular will create a javascript representation of any form you create automatically.

However we still need to inform Angular about which input you want as controls. (controls meaning represented in javascript)

You add `ngModel` as an attribute on any element you want to exist as a control, and also provide a `name` value for the input.

eg.

`<form>
    <input type="text" name="email" ngModel>
</form>`

## 176. TD: Submitting and using the form

Hook up a method to listen to form submission events, via a directive called `ngSubmit`. 

`<form (ngSubmit)="onSubmit()">
</form>
`

where `onSubmit()` is a function defined in the component.

Can get a hook to the form as follows (setup a local reference to a form object that Angular creates for you automatically - ngForm):

`<form (ngSubmit)="onSubmit(f)" #f="ngForm">
</form>
`

and in the component:

`onSubmit(form: NgForm) {
    console.log(form);
}`

they are then available under the value attribute in the form (by key-value pairs keyed by the form element names).

# 177. TD: Understanding Form State

controls - shows which controls are registered in the form (that have ngModel applied)

Controls themselves have properties as well.

`dirty 
disabled
enabled
errors`

dirty - has the form input changed at all?
invalid - according to validators
touched - has there been any input (ie has the user tabbed into it at all)

## 178. TD: Accessing the forms with @ViewChild

Instead of passing a local reference

`@ViewChild('f') signupForm: NgForm`

Will give you the same reference in a nicer way.

## 179. TD: Adding Validation to check User Input

Doen in the template in TD forms - they mark an input / control / form as not being valid if they are not met

examples: 

`required` - built in directive in Angular

`email` - another Angular directive which makes sure the input is a valid email address

`pattern` - regex pattern validator

`ng-dirty`, `ng-touched`, `ng-valid` are all classes used by angular to mark the validity state of inputs.

## 180. Built in validators + HTML 5 validation

Which Validators do ship with Angular? 

Check out the Validators class: https://angular.io/docs/ts/latest/api/forms/index/Validators-class.html - these are all built-in validators, though that are the methods which actually get executed (and which you later can add when using the reactive approach).

For the template-driven approach, you need the directives. You can find out their names, by searching for "validator" in the official docs: https://angular.io/api?type=directive - everything marked with "D" is a directive and can be added to your template.

Additionally, you might also want to enable HTML5 validation (by default, Angular disables it). You can do so by adding the ngNativeValidate  to a control in your template.












# Section 24 - NGRX

## 302. Intro

Application state

The state of the data in your components is lost when the application refreshes in a traditional angular application.

NGRX provides a Store which contains the application state. Components and services receive state updates from the store. 

They can then update the store via dispatching Actions.

Actions are sent to Reducers (functions) that take an applications current state and payload, and immutably update the state.

> Lazy loading - only adds things to your store once particular modules are loaded.


## 303. Getting started with reducers

Reducer functions are triggered whenever an action is dispatched.

`npm install --save @ngrx/store`

Creating a reducer - `shopping-list.reducer.ts`

They take in 2 things - the current State of the app and an action (of type Action)

You can define a default state in the function's param by providing a default value.

The function should return the updated state of the application. Even if you simply returned the input param of the current state as the respone, NGRX would create a new instance of it as the new state. 

Actions themselves by default don't have a payload, only a type. You need to create your own Action types in order to define a payload.

A switch statement is usually used to handle an action by looking at it's `type` param, which is set when dispatching the action. It's typically a String. Defining it as a constant in the reducer (for export to other components that may create the actions) is a good practice.

Array spread operator is liberally used to copy the existing state (for immutability).

`
export const ADD_INGREDIENT = 'ADD_INGREDIENT';

const initialState = {
    ingredients: [
        new Ingredient('Apples', 5),
        new Ingredient('Tomatoes', 10),
    ]
}
export function shoppingListReducer(state = initialState, action: Action) {
    switch (action.type) {
        case ADD_INGREDIENT:
            return {
                ...state,
                ingredients: [...state.ingredients, action.??? todo]
            }
    }
    
    return state
}`


## 304. Adding Actions

Create a file that contains actions (could also place it in the reducers file if you'd like)

`shopping-list.actions.ts`

Move the files into a subfolder called `store` to keep the ngrx files together.

Create a class which implements the Action class / interface, and define it's type and payload:

`import { Action } from '@ngrx/store';

export const ADD_INGREDIENT = 'ADD_INGREDIENT';

export class AddIngredient implements Action {
    readonly type = ADD_INGREDIENT;
    payload: Ingredient;
}

export type ShoppingListActions = AddIngredient;
`

and export the actions in the last line of the class, for use in the reducer.

## 305. Finishing the first reducer

Update the reducer to use the typed references in the action class.

`
import * as ShoppingListActions from './shopping-list.actions';

const initialState = {
    ingredients: [
        new Ingredient('Apples', 5),
        new Ingredient('Tomatoes', 10),
    ]
}

export function shoppingListReducer(state = initialState, action: ShoppingListActions.ShoppingListActions) {
    switch (action.type) {
        case ShoppingListActions.ADD_INGREDIENT:
            return {
                ...state,
                ingredients: [...state.ingredients, action.payload]
            }
        default:
            return state;
    }
    
}`

Next step is to register the reducer and store in the app, and adding the ability to actually dispatch an action.

## 306. Registering the Application Store


Occurs in the app module.

forRoot - main application, eagerly loaded modules (not lazily loaded ones)
    - takes an argument, a JS object with all your reducers, and setup a store, register the reducers provided, and setup their initial state.

>app.module.ts

`
import { StoreModule } from '@ngrx/store';

...

imports: [
    ...,
    StoreModule.forRoot({shoppingList: shoppingListReducer})
]
`

## 307. Selecting data from State

Example of fetching store data from a component - important thing to note is that when you do this, you receive an Observable, not the type that you are storing. As such it makes the usage a little more complex.

Inject the Store service into the component, providing the generic type (seems to match the object listed in the StoreModule.forRoot() argument - shoppingList, and then the field in the state in the reducer - in this case ingredients) as well for the data you care about():

>shopping-list.component.ts

`constructor(private store: Store<{shoppingList: {ingredients: Ingredient[]}}>) {}`

and then use it as follows:

`
shoppingListState: Observable<{ingredients: Ingredient[]}>;

..


this.shoppingListState = this.store.select('shoppingList');
`

the select seems to match the thing listed in the StoreModule.forRoot() argument, which is the same as the first layer in the Store generic object as well.

and use it in a template as follows (becuase it's an observable) with the `async` pipe:

`<div *ngFor="let ingredient of (shoppingListState | async).ingredients;">`

## 308. Dispatch Actions

Autowire the store into the component that adds an ingredient:

`constructor(private store: Store<{shoppingList: {ingredients: Ingredient[]}}>)`

Then use the dispatch method on the Store to send an Action with a payload:

`import * as ShoppingListActions from '../store/shopping-list-actions'`

... and assuming that the AddIngredient action has a constructor arg called `payload` of type `Ingredient`

`this.store.dispatch(new ShoppingListActions.AddIngredient(newIngredient))`


## 309. Adding more actions

Add a new constant to the shopping-list.actions file - use the UNION operator to add it to the exported bundle:

`export type ShoppingListActions = AddIngredient | AddIngredients;`

Add it to the reducer:

`case ShoppingListActions.ADD_INGREDIENTS:
  return {
    ...state,
    ingredients: [...state.ingredients, ...action.payload]
  };`
  
Mentions replacing the existing service class' implementation with the ngrx store dispatch version, but then moves it all the way into the component. Not sure how i feel about that yet, seems to bring the implementation detail into the component, and all of your components would need to change if the data store implementation changes...

## 310. Dispatching update and delete actions

Update - payload is an index of an item plus the updated item. A little trickier to handle.

1. get the ingredient to handle
2. spread it's properties, then on top spread the properties of the updated ingredient
3. spread the existing array of ingredients into a new object
4. set the value of the ingredient at the index we are updating, to the newly created object representing that ingredient
5. return the new array in the state returned from the method.


`    case ShoppingListActions.UPDATE_INGREDIENT:
      const ingredient = state.ingredients[action.payload.index];
      const updatedIngredient = {
        ...ingredient,
        ...action.payload.ingredient
      };
      const ingredients = [...state.ingredients];
      ingredients[action.payload.index] = updatedIngredient;


      return {
        ...state,
        ingredients: ingredients
      };
`

and deleting - use the splice ES6 array method:

    case ShoppingListActions.DELETE_INGREDIENT:
      const updatedList = [...state.ingredients];
      updatedList.splice(action.payload, 1);
      return {
        ...state,
        ingredients: updatedList
      };
      
      
## 311. Expanding app state

Keeping track in the Store of which ingredient is currently being edited.


First step - export an interface representing the state:

>shopping-list.reducers

`export interface State {
    ingredients: Ingredient[];
    editedIngredient: Ingredient;
    editedIngredientIndex: number;
}`

Overall appstate is then represented in the same file:

`export interface AppState {
    shoppingList: State
}`

and the state itself has the new properties defined and it's type added:

`
const initialState: State = {
    ingredients: [
        new Ingredient('Apples', 5);
        new Ingredient('Tomatoes', 10);
    ],
    editedIngredient: null,
    editedIngredientIndex: -1
}
`
We can also now use these interfaces in place of any reference to the generic objects that we used before.


`import * as fromShoppingList from '../store/shopping-list.reducer';`


`constructor(private slService: ShoppingListService, private store: Store<fromShoppingList.AppState>) { }`

## 312. Editing the shopping list via NGRX

The beginning of Editing should be an action.

Create an action. and a reducer function for it (spreading the editing ingredient to get a copy of it):

    case ShoppingListActions.START_EDIT:
      const editedIngredient = {...state.ingredients[action.payload]};
      return {
        ...state,
        editedIngredient: editedIngredient,
        editedIngredientIndex: action.payload
      };
      
Dispatch an action as per usual.

Listening to the action, make sure your component has a `subscription: Subscription;` defined, and use the `store.select('sliceName').subsribe()` method to update relevant data as it flows into the component:

> shopping-edit.component.ts

  `ngOnInit() {
    this.subscription = this.store.select('shoppingList')
      .subscribe(
        data => {
          if (data.editedIngredientIndex > -1) {
            this.editedItem = data.editedIngredient;
            this.editMode = true;
            this.slForm.setValue({
              name: this.editedItem.name,
              amount: this.editedItem.amount
            })
          } else {
            this.editMode = false;
          }
        }
      );
    }`
      
and make sure the component implements OnDestroy, so that the component can manage it's own subscription:

`
  ngOnDestroy() {
    this.subscription.unsubscribe();
  }
`
## 313. Managing all relevant state (ie edit modes)

We require a STOP_EDIT action that can reset the active editing properties of the store whenever we navigate away from editing an ingredient.

In the edit component, in it's `ngOnDestroy()` method:

`
  ngOnDestroy() {
    this.store.dispatch(new ShoppingListActions.StopEdit());
    this.subscription.unsubscribe();
  }`
  
Also added the same logic that is in StopEdit to the update and delete actions.


## 314. Authentication and Side Effects - Introduction

Async operations (ie. HTTP / REST calls) can't be handled directly in reducers, as they need to operate synchronously. 


## 315. Setting up the Auth store files

Creating a new reducers file for the auth part of the app - means the AppState interface needs to be updated and taken out of the shopping list reducers file.

This can be done in a /store dir in the root of the app dir. It can be thought of as the global app-wide reducer.

New file - `store/app.reducers.ts`


`import * as fromShoppingList from '../shopping-list/store/shopping-list.reducer';
import * as fromAuth from '../auth/store/auth.reducers';

export interface AppState {
  shoppingList: fromShoppingList.State,
  auth: fromAuth.State;
}
`

## 316. The Reducer

Just created all the auth actions - login, logout, signup, set token.

## 317. Adding reducer logic and actions

Basically just implemented the following:

`import * as AuthActions from './auth.actions';

export interface State {
  token: string,
  authenticated: boolean
}

const initialState: State = {
  token: null,
  authenticated: false
};

export function authReducer(state = initialState, action: AuthActions.AuthActions) {
  switch (action.type) {
    case AuthActions.SIGNUP:
    case AuthActions.SIGNIN:
      return {
        ...state,
        authenticated: true
      };
    case AuthActions.LOGOUT:
      return {
        ...state,
        token: null,
        authenticated: false
      };
    default:
      return state;
  }
}
`

## 318. Adjusting the app module setup

Bundle the reducers throughout the app in the actual app.reducers.ts file, as opposed to adding them to the root app module.

AppReducerMap<> is used to representthe various reducers for your app keyed to their... key

> app.reducers.ts

`import * as fromShoppingList from '../shopping-list/store/shopping-list.reducer';
import * as fromAuth from '../auth/store/auth.reducers';
import {ActionReducerMap} from '@ngrx/store';

export interface AppState {
  shoppingList: fromShoppingList.State,
  auth: fromAuth.State;
}

export const reducers: ActionReducerMap<AppState> = {
  shoppingList: fromShoppingList.shoppingListReducer,
  auth: fromAuth.authReducer
};
`

Then used in the root app module:

`import {reducers} from './store/app.reducers';`

...

`StoreModule.forRoot(reducers)`

## 319. Using Authentication

Directly accessing services from a template can cause issues when using AOT compilation.

Accessing state changes from a template:

Given the following component (just a chunk of it to demonstrate the ngrx changes):

`import * as fromApp from '../../store/app.reducers';
import * as fromAuth from '../../auth/store/auth.reducers';
import {Observable} from 'rxjs/Observable';

@Component({
  selector: 'app-header',
  templateUrl: './header.component.html'
})
export class HeaderComponent implements OnInit {

  authenticatedState: Observable<fromAuth.State>;

  ngOnInit(): void {
    this.authenticatedState = this.store.select('auth');
  }
  constructor(private dataStorageService: DataStorageService,
              private authService: AuthService,
              private store: Store<fromApp.AppState>) {
  }`

We can do the following in the header template:

`<ng-template [ngIf]="!(authenticatedState | async).authenticated">`

Store is the app-wide state, the observable can be a slice of it.

## 320. Dispatch actions

Nothing new

## 321. Getting state access in the Http Interceptor


Use of a [switchmap](https://www.learnrxjs.io/operators/transformation/switchmap.html) - observable returned from within an observable? Stop the auto-wrap into a new observable.

Need to import it apparently:

`import 'rxjs/add/operator/switchMap'`

`@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  constructor(private store: Store<fromApp.AppState>) {}

  intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
    console.log('Intercepted!', req);

    return this.store.select('auth')
      .switchMap((authState: fromAuth.State) => {
        const copiedReq = req.clone({params: req.params.set('auth', authState.token)});
        return next.handle(copiedReq);
      });
  }
}`

## 322. Handling the auth token

We were not ever setting the token upon login as we hadn't handled the SET_TOKEN action in the auth reducer yet.

`case AuthActions.SET_TOKEN:
  return {
    ...state,
    token: action.payload
  };`
  
## 323. Only react to actions once using take(1)

The switchMap code in step 321 is executed each time the auth store changes due to the ongoing subscription that is setup there in the return statement.

The fix is to use .take(1) after the select statement:

`
    return this.store.select('auth')
      .take(1)
      .switchMap((authState: fromAuth.State) => {
        const copiedReq = req.clone({params: req.params.set('auth', authState.token)});
        return next.handle(copiedReq);
      });`
      
This subscribes for the first response only. 

[Race conditions??](https://blog.nrwl.io/rxjs-advanced-techniques-testing-race-conditions-using-rxjs-marbles-53e7e789fba5)

## 324. A closer look at effects

Async tasks / side effects related to dispatching actions, but not resulting in a change of state.

`npm install --save @ngrx/effects`

Then create a new file in the auth dir

>auth.effects.ts

`import {Effect} from '@ngrx/effects';

@Injectable()
export class AuthEffects {

  @Effect()
  authSignup
  
  constructor(private actions$: Actions) { }
}
`

The `@Effect` decorator marks a field as being managed by ngrx effects.

## 325. Auth effects and actions

Need to anotate the class with `@Injectable`
Add the `EffectsModule` to the `app.module` and register them:

>app.module.ts

`imports: [
    EffecstModule.forRoot([AuthEffects])
    ]`

param suffix of $, ie. actions$ means it's an observable.

We then autowire the actions into the effects class.


## 326. Effects - how they work

Effects - side effects that dont affect state. ie. start login... side effect is to attempt to login, and create login success or fail events on response.

Assign a value to your decorated effect, and limit it to a certain action type:

`
@Effect()
authSignup = this.actions$
    .ofType();
`

Need to create new actions to represent attempting/doing a signup request:

> auth.actions.ts

`export cost TRY_SIGNUP = 'TRY_SIGNUP'`

`
export class TrySignup implements Action {
    readonly type = TRY_SIGNUP;
    
    constructor(public payload: {username: string, password: string}) {}
}
`

No need for a reducer as we only care about it in effects.

`
@Effect()
authSignup = this.actions$
    .ofType(AuthActions.TRY_SIGNUP);
`

Anything you chain-on after that method is only called when an event matching that takes place.

Triggering it:

`this.store.dispatch(new TrySignup({username: email, password: password}));`

## 327. Adding auth signup

Same as reducers except we don't change application state directly. Expect to return an Observable.

get payload in an effect (map will wrap in observable):

`
import 'rxjs/add/operator/map'
import 'rxjs/add/operator/switchMap'

@Effect()
  authSignup = this.actions$
    .ofType(AuthActions.TRY_SIGNUP)
    .map((action: AuthActions.TrySignup) => {
      return action.payload;
    })
    .switchMap((authData: {username: string, password: string}) => {
      return fromPromise(firebase.auth().createUserWithEmailAndPassword(authData.username, authData.password));
    })
    .switchMap(() => {
      return fromPromise(firebase.auth().currentUser.getIdToken());
    })
    .mergeMap((token: string) => {
      return [
        {
          type: AuthActions.SIGNUP
        },
        {
          type: AuthActions.SET_TOKEN,
          payload: token
        }
      ]
    });
`

Method to turn promise into observable:

`import { fromPromise } from 'rxjs/observable/fromPromise;`

Also mergeMap - output 2 actions in observables

At the en of effect chain you should dispatch new actions.

Can set `@Effect({dispatch: false})` in the effect decorator if you don't want to do that.

## 328. Adding Auth Signin

mergemap
switchmap

???

Added another effect for signin.

## 329. Navigation as a side effect

Inject router into effects class;

`private router: Router`

then add it in near the end:

`.mergeMap((token: string) => {
    this.router.navigate(['/]');
      return [
        {
          type: AuthActions.SIGNUP
        },
        {
          type: AuthActions.SET_TOKEN,
          payload: token
        }
      ]
    });
`

## 330. Handling logout via ngrx

Dispatched logout action instead of directly calling authservice.logout


## 331. Additional fixes

Inifite looks called with store.select() in guards - potentially other places too. Fix is to do take(1)

## 332. Redirecting Upon Logout

New effect that listens for logout event, and call .do() - allows you to execute code but continuing the 'stream' 

import 'rxjs/add/operators/do'

`.ofType(AuthActions.LOGOUT)
.do(() => {
    this.router.navigate(['/'])
})`

## 333. What's next

Discussion of extent of NGRX use. Form state (ie. f.valid) is localized to a single component (ie. only handling local UI state) - when you have things like this dont bother using ngrx for it.

## 334. Router store package

Changes in route as state changes (ie changing pages) - can be listened to via `@ngrx/router-store`

>app.module.ts 

Add `StoreRouterConnectingModule` the the imports array.

## 335. Store devtools

Discussed how to setup the ngrx store dev tools. Requires redux chrome plugin.

## 336. Lazy loading + dynamic injection

Try using this in the MBO app:

`RouterModule.forRoot(routes, {preloadingStrategy: PreloadAllModules})`

Discussion around how lazily-loaded modules load their store data into the existing overall app state.

In the module for the feature:

`imports: [
    StoreModule.forFeature('featureState', featureReducer)
    ]`


