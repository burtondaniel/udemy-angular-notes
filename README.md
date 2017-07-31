# udemy-angular-notes
https://www.udemy.com/the-complete-guide-to-angular-2
Notes from an angular 2 course on Udemy.

## 11. A Basic Project Setup using Bootstrap for Styling
You can add libraries (css and js) using NPM.

npm install --save bootstrap


Which will install the library in the node_modules folder in your project, then add the css to the .angular-cli.json file:


## 13. How an Angular App gets Loaded and Started
index.html is served by the server - this is defined in the angular-cli file
There is an <app-root> element  in there by default. An app component can bind to it as follows:


Selector identifies the dom element to be replaced with the content of the component.

Ng serve creates script bundle and chucks them into the index.html file that cause angular to be bootstrapped.
Main.ts is the file which contains the JS / TS that is executed first.
Example:

It loads the app.module file, which in turn declares components to load, imports, etc.

Hence:



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
While ‘ng serve’ is running

ng generate component <component-name>
ng g c <component-name>

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
	```{{ data }}```
Property binding:
	```[property] = “data”```

Typescript can react to user events via Event Binding 
Two way binding: bind typescript vars to user update of data
  ```[(ngModel)] = “data”```
## 23. String Interpolation
Define vars in your component class:
```
export class ServerComponent {
	serverId = 10;
	serverStatus: string = ‘offline’;
}
```
Output it in the template:

`<p>Server with ID {{ serverId }} is {{ serverStatus }}</p>`

Any condition (other than multiline / block expressions) which can be resolved to a string is acceptable inside the curly braces.
Ie: `{{ ‘server’ }}`
