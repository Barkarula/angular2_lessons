# Angular 2 Essential Training

## Important Links
- [Angular CLI Website](https://cli.angular.io/)


## Table of Contents

- [Introduction](#introduction)
- [Architecture Overview](#architecture-overview)
- [Data Binding](#data-binding)
- [Decorators in Angular2](#decorators-in-angular2)
- [Various Platforms in Angular2](#various-platforms-in-angular2)
- [About Browser Platform](#about-browser-platform)
- [Angular Template Syntax](#angular-template-syntax)

# Introduction

The following are the 6 fundamental building blocks of **Angular 2**

## Directives
## Components
## Services
## HTTP
## Routing
## Pipes

# Architecture Overview

Angular runs on **Components**, The starting point of an Angular app is the **Bootstrap** component.

> NOTE: In Angular 2, A Component is actually *A Directive with a Template*

> There are 2 types of Directives
- **Structural**  - Structural Directives modify layouts, by altering elements in the DOM
- **Attribute** - Attribute Directives change the behaviour/appearance of an existing DOM element

> A **Pipe** takes end data, like a string or an array, and runs some logic to transform it to a new output.

---

# Data binding

Angular 2 uses - `one-way data binding`. Along with data binding, there are many element to the template syntax:

- `Template Expressions and statements`
- `Value binding`: A Binding syntax for property, attribute, class & style binding
- `Event binding`
- `Template Expressions Operators`

# NG Model, The new `#` Symbol in Angular 2

Just like `ng-model` in *AngularJS*, You can also create local template variables, created in markup using the HASH symbol `#` to get a reference of the element.

```html
<div>
    Billing Address: <input value="Enter a value here" #billing/><!--the # symbol here serves as ng-model-->
    Shipping Address: <input value="Enter a valure here" #shipping-address/>
    <button (click)="billing.value == shipping.value"></button><!--Getting the value of the ng-model using the .value-->
</div>
```

# Decorators in Angular2

## What are Decorators in Angular2 ?
A `Decorator` is an Expression that evaluates to a function allowing annotation of classes at design time.

- `@NgModule` - It means that the class code is intended to be an `Angular Module` 
- `FUN FACT` - Remember the Word - `DIP Bootstrap` - declarations, imports, providers, bootstrap

```js
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core'; //@NgModule belongs to @angular/core
import { FormsModule } from '@angular/forms';
import { HttpModule } from '@angular/http';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [ //This is used to make our custom code - Components, Directives, Pipes available for our module.
    AppComponent // These modules are internal to AppModule, and DONT come from external modules.
  ],
  imports: [
    BrowserModule, //This is used to bring other Angular modules, that our module - AppModule would need.
    FormsModule,
    HttpModule
  ],
  providers: [],
  bootstrap: [AppComponent] //NOTE: This means that the AppComponents is the ROOT DOM Component
})
export class AppModule { } //NOTE: This is an empty class
```

# Various Platforms in Angular2

`Angular 2` is designed to work on multiple platforms. Some of them are: 

- **Browser Platform** - This is for Web Applications
- **Web worker Platform** - Web workers for creating multiple threads in a browser
- **Server Platform** - Angular 2 supports server side rendering
- **Mobile Platform** - Ionic 2/3 supports Angular 2 code on Mobile devices.

# About Browser Platform

Angular 2 exports a `platformBrowserDynamic` function for targetting the browser from the `platformBrowserDynamic` scoped package.

```js
platformBrowserDynamic().bootstrapModule(AppModule);
```

This is equivalent to the React's `ReactDom.render()` function where you are injecting the React Module into the index.html

```js
ReactDom.render(<Header/>, document.getElementById('headerComponent'));
```

>`main.ts` file

```js
import { enableProdMode } from '@angular/core';
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic'; //This is required for platformBrowserDynamic scope variable

import { AppModule } from './app/app.module';
import { environment } from './environments/environment';

if (environment.production) {
  enableProdMode();
}

platformBrowserDynamic().bootstrapModule(AppModule);
```

> NOTE: When it comes to styling a component, Angular 2 uses `scoped styling` to style your components itself.

---

# Angular Template Syntax

Angular 2 has the following features when it comes to templating syntax:
- Interpolation
- Binding
- Expressions
- Conditional Templating
- Template Variables
- Template expression operators

---

## Interpolation

In `Angular 2`, just like `AngularJS`, we use the `{{}}` for String interpolation. however, unlike `AngularJS` the following are **not supported in `Angular2`**

- Assignments
- Newing up of Variables
- Chaining Expressions
- Incrementing/Decrementing

---

## Property Bindings / ng-bind in Angular JS

Another way of using `ng-bind` from `AngularJS` in `Angular2` is by using the `property-binding syntax`

> property-binding - []
- `[textContent]="name"` is similar to `ng-bind="name"`
- `textContent={{name}}` is also similar to `ng-bind="name"`

```html
<div class="media-view">
    <h2 [textContent]="title"></h2><!--This is the property-binding-->
    <p>This is a custom angular code generated content</p>
</div>
```


## Input @angular/core {Input} - Similar to the Isolated Scope property Bindings

This is similar to Isolated Scope's property bindings, The `@angular/core Input` decorator, is used to bind the property of the component to it's field properties

```js
// MediaComponent
import { Component, Input } from '@angular/core'; //Import the Input from @angular/core

@Component({
    selector:'mw-media',
    templateUrl:'./media.component.html',
    styleUrls:['../app/bootstrap.min.css','./media.component.css']
})
export class MediaComponent{
   
   @Input() mediaItem; //You can even pass the alias as @Input('mediaItemAs')
    
   title:String = `Welcome to the Custom Media Component`;

   onDelete = ():void=>{
    console.log('The onDelete Button was clicked');
   }//end:onDelete
}//end:class-MediaComponent
```

To Use the above definition, we again use the Property Binding syntax in our App Component:

>app.component.html

```html
<h1 [textContent]="title"></h1>
<mw-media [mediaItem]="simpleObject"></mw-media><!--Or it can be the alias property - mediaItemAs-->
```

---

## Output @angular/core {Output, EventEmitter} - Similar to $scope.$emit

Angular 2 has now an `@Output()` Decorator which when combined with the `new EventEmitter()` instance, gives a way to emit events UP the DOM hierarchy.

This is similar to `$scope.$emit()` in `AngularJS`:

>media-item.component.ts

```js
// MediaComponent
import { Component, Input, Output, EventEmitter } from '@angular/core'; //@Output() + EventEmitter() = $scope.$emit

@Component({
    selector:'mw-media',
    templateUrl:'./media.component.html',
    styleUrls:['../app/bootstrap.min.css','./media.component.css']
})
export class MediaComponent{
   
   @Input() mediaItem;
   @Output() delete = new EventEmitter(); // Or we can alias the property name by @Output('anotherName')

   title:String = `Welcome to the Custom Media Component`;

   onDelete = ():void=>{
    this.delete.emit(this.mediaItem); //delete field has .emit due to the EventEmitter instance.
    console.log('The onDelete Button was clicked');
   }//end:onDelete
}//end:class-MediaComponent
```

>app.component.html

```html
<h1>
  {{title}}  
</h1>
<mw-media [mediaItem]="simpleObject" 
(delete)="mediaComponentDeleteHandler($event)"></mw-media>
```

>app.component.ts
```js
import { Component } from '@angular/core';
// import {platformBrowserDynamic} from @angular/platform-browser-dynamic; //This is deprecated

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./bootstrap.min.css','./app.component.css']
})
export class AppComponent {
  title:String = 'Hello Angular2 - Welcome to Angular 2!';

  simpleObject:any = {
    name:'Pramod',
    title:'Developer',
    expertise:'AngularJS'
  };//end:simpleObject

  mediaComponentDeleteHandler(mediaItem){ //This is the event handler subscribed to media component's delete event
    console.log('Media Component Emitted the following: ', mediaItem);
  }//end:mediaComponentDeleteHandler

}//end:class-AppComponent
```

---

