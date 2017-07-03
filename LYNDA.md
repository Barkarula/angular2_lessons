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
- [NgIf directive in Angular2](#ngif-directive-in-angular2)
- [NgFor is NgRepeat in Angular2](#ngfor-is-ngrepeat-in-angular2)
- [Custom Attribute Directive](#custom-attribute-directive)
- [Angular2 Pipes](#angular2-pipes)
- [Angular2 Forms](#angular2-forms)
- [Custom Validations](#custom-validations)
- [Angular2 Safe Property Operator](#angular2-safe-property-operator)
- [Services in Angular](#services-in-angular)
- [Enum Declaration in Angular2](#enum-declaration-in-angular2)
- [HTTP Service Module in Angular2](#http-service-module-in-angular2)

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

> NOTE: Attribute Directives `cannot change the structure of the DOM, only change its look/behaviour`

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

# Structural, Attribute Directives in Angular 2

The following are the `built-in` directives in Angular2

- Structural Directives
  - **ngIf** : *ngIf or [ngIf]
  - **ngFor**: *ngFor or [ngFor]
- Attribute Directives
  - **ngClass** : 
  ```js
  [ngClass] = "{'medium-movies': variable===true,'medium-series': variable===false}"
  ```
# NgIf directive in Angular2

>NOTE: When using `Angular's Inbuilt Structural directives` we need to prefix them with an asterisk `*ngif`

## What is the Asterisk doing in Angular 2
The Asterisk is what we call `Synthatic Sugar`. It is the `short hand pattern` for writing that the Platform will convert to the actual pattern.


```html
<div *ngIf="isColored">Pramod</div><!--This is the short hand syntax using the * as syntatic sugar-->
```

```html
<template [ngIf]="isColored"><!--Angular2 knows that template tags shouldn't be bound to the actual DOM-->
  <p>This is the Full version. Template tags are not shown in the actual DOM by Angular2</p>
</template>
```

```js
// MediaComponent
import { Component, Input, Output, EventEmitter } from '@angular/core';

@Component({
    selector:'mw-media',
    templateUrl:'./media.component.html',
    styleUrls:['../app/bootstrap.min.css','./media.component.css']
})
export class MediaComponent{
   
   @Input() mediaItem;
   @Output() delete = new EventEmitter();
   isWatchedOn:boolean = false; //This is connected to *ngIf

   title:String = `Welcome to the Custom Media Component`;

   onDelete = ():void=>{
    this.isWatchedOn = true;
    this.delete.emit(this.mediaItem);
    console.log('The onDelete Button was clicked');
   }//end:onDelete
}//end:class-MediaComponent
```


# NgFor is NgRepeat in Angular2

The Syntatic Sugar syntax for NgFor is as follows

```html
<div>
  <ul>
    <!--Remember that Angular2 is actually replacing the below with a template element-->
    <li *ngFor="let mediaItem of mediaItemList"></li>
  </ul>
</div>
```

# Custom Attribute Directive

In `AngularJS` we would set a directive as an `attribute` by adding the restrict value as `A`
```js
angular.module('myApp').directive('myDirective',function(){
  return{
    restrict:'A' //This means the Directive is used as Attribute directive
  };
});
```

In `Angular2` the way to set a directive as an `attribute` is as follows:

>NOTE: You can still add custom properties even to Attribute Directives, by using the `HostBinding` scope from the `@angular/core` scoped library.

```js
import {Directive, HostBinding} from '@angular/core';

@Directive({
    selector:'[mwFavourite]', //This means that the directive's restrict type is attribute

})
export class FavouriteDirective {
    //Host binding will look for any change to 
    //It takes in a string
    //class refers to the native DOM property
    @HostBinding('class.is-favourite') isFavourite = true; // This means that attribute directive gets a class property set as `is-favourite`
}//end:class-Favourite
```

## Hostbinding and HostListeners

## 1. Hostbinding
Even `custom directives` can listen to their Host elements using the `Host Binding` and `Host Listeners`

>Host binding: favourite.directive.js

```js
import {Directive, HostBinding, Input} from '@angular/core';

@Directive({
    selector:'[mwFavourite]', //This means that the directive's restrict type is attribute

})
export class FavouriteDirective {
    //Host binding will look for any change to 
    //It takes in a string
    //class refers to the native DOM property
    @HostBinding('class.is-favourite') isFavourite = true;

    //set is a keyword in Typescript for a getter method. It is ES6/ES2015, when a property is set
    //NOTE: the setter method property name should match the selector value
    @Input() set mwFavourite(value){
        this.isFavourite = value;
    }//end:setter
}//end:class-Favourite
```

## 2. Hostlistener

The HostListener takes 2 parameters
- The name of the event handler in String format
- `[optional]` An array of arguments that the targetting event will emit

```js
import {Directive, HostBinding, HostListener, Input} from '@angular/core';

@Directive({
    selector:'[mwFavourite]', //This means that the directive's restrict type is attribute

})
export class FavouriteDirective {
    //Host binding will look for any change to 
    //It takes in a string
    //class refers to the native DOM property
    @HostBinding('class.is-favourite') isFavourite = true;
    @HostBinding('class.is-favourite-hovering') isHovering = false;
    
    //REMEMBER - Angular2 works with Native DOM events without the `on` syntax
    @HostListener('mouseenter') onMouseEnter(){
        this.isHovering = true;
    }//end:onMouseEnter

    //Hostlistener for on-mouse-enter and on-mouse-leave events
    @HostListener('mouseleave') onMouseLeave(){
        this.isHovering = false;
    }//end:onMouseLeave

    //set is a keyword in Typescript for a getter method. It is ES6/ES2015, when a property is set
    //NOTE: the setter method property name should match the selector value
    @Input() set mwFavourite(value){
        this.isFavourite = value;
    }//end:setter
}//end:class-Favourite
```

---

# Angular2 Pipes

Angular2 Pipe - A template expression operator that takes in a value and returns a new value representation.

```html
<!--The followoing pipe uses inbuilt date format to change string to date-->
<div>Watched on {{mediaItem.isWatchOn | date: 'shortDate'}}</div>
```

> NOTE: You can also chain multiple pipes. For example

```html
<!--The following pipe uses inbuilt string splicing and then converts the string to UPPERCASE-->
<h2>{{mediaItem.name | slice:0:10 | uppercase}}</h2>
```

# Angular2 Forms

Angular2 Forms are driven by two types:

- Angular2 `FormsModule` Module
- Angular2 `ReactFormsModule` Module
- Constructor Injection type using `FormBuilder` Module

## Forms Module

This type of creating Forms is also called - **Template Driven Forms**

>1. Import the Forms in the app.module.ts
```js
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { HttpModule } from '@angular/http';

//Custom Components and Directives
import { AppComponent } from './app.component';
import {MediaComponent} from './components/media.component';
import {MediaFormComponent} from './components/media.form.component';
import {FavouriteDirective} from './directives/favourite.directive';
import {CategoryListPipe} from './pipes/category-list.pipe';

@NgModule({
  declarations: [
    AppComponent,
    MediaComponent,
    MediaFormComponent,
    FavouriteDirective,
    CategoryListPipe
  ],
  imports: [
    BrowserModule,
    FormsModule, //This is the standard FormsModule
    HttpModule    
  ],
  providers: [],
  bootstrap: [AppComponent]
})

export class AppModule { }//NOTE: this is an empty class
```

>2. Include the Create a FormComponent - `MediaFormComponent`

```js
import {Component} from '@angular/core';

@Component({
    selector:'mw-media-form',
    templateUrl:'./media.form.component.html',
    styleUrls:['./../bootstrap.min.css','./media.component.css']
})
export class MediaFormComponent{
    // medium:String = '';
    onSubmit = (formValue) =>{
        console.log('Submitted ',formValue);
    }//end:onSubmit
}//end:class-MediaFormComponent
```

>3. Include the New Forms Component in the Media - `media.component.html`

```html
<div class="media-view">
    <h2 [textContent]="title"></h2>
    <p *ngIf="isWatchedOn">This all is comming from the Media Component</p>
    <a (click)="onDelete()" class="btn btn-success">Delete</a>
    <br/>
    <!--We want to set the directive to a statement that would be evaluated. We want Angular to know we want to do a binding here-->
    <ul [mwFavourite]="mediaItem.isFavourite">
        <li [textContent]="mediaItem.name"></li>
        <li [textContent]="mediaItem.title"></li>
        <li [textContent]="mediaItem.expertise"></li>
    </ul>
</div>
```

>4. Just style the Media Form component - `media.form.component.html`

```html
<header>
    <h2>Add Media to Watch</h2>
</header>
<!--#mediaItemForm is a variable name-->
<form
    #mediaItemForm="ngForm"
    (ngSubmit)="onSubmit(mediaItemForm.value)">
    <ul>
        <li>
            <label for="medium">Medium</label>
            <!--This is equivalent to ng-model="mediaItemForm.medium" -->
            <select name="medium" id="medium" ngModel>
                <option value="Movies">Movies</option>
                <option value="Series">Series</option>
            </select>
        </li>
        <li>
            <label for="name">Name</label>
            <input type="text" name="name" id="name" ngModel>
        </li>
        <li>
            <label for="category">Category</label>
            <select name="category" id="category" ngModel>
                <option value="Action">Action</option>
                <option value="Science Fiction">Science Fiction</option>
                <option value="Comedy">Comedy</option>
                <option value="Drama">Drama</option>
                <option value="Horror">Horror</option>
                <option value="Romance">Romance</option>
            </select>
        </li>
        <li>
            <label for="year">Year</label>
            <input type="text" name="year" id="year" maxlength="4" ngModel>
        </li>
    </ul>
    <!--This will trigger the ngSubmit event-->
    <button type="submit">Save</button>
</form>
```

## Model-Driven Forms

This type of Forms creations is also called - **Model Driven Forms**
For `Model Driven Forms` You need to use instead the `ReactiveFormsModule` from the `@angular/forms` module:
```js
import { ReactiveFormsModule } from '@angular/forms';
```

>NOTE: You can use both the `ReactiveFormsModule` and the `FormsModule` side-by-side.

Advantages of using the ReactiveFormsModule

- `ReactiveFormsModule` doesnt rely on the `form` tag.
- You can define your on `model`

> app.module.ts

```js
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { ReactiveFormsModule } from '@angular/forms';
import { HttpModule } from '@angular/http';

//Custom Components and Directives
import { AppComponent } from './app.component';
import {MediaComponent} from './components/media.component';
import {MediaFormComponent} from './components/media.form.component';
import {FavouriteDirective} from './directives/favourite.directive';
import {CategoryListPipe} from './pipes/category-list.pipe';

@NgModule({
  declarations: [
    AppComponent,
    MediaComponent,
    MediaFormComponent,
    FavouriteDirective,
    CategoryListPipe
  ],
  imports: [
    BrowserModule,
    ReactiveFormsModule,
    HttpModule    
  ],
  providers: [],
  bootstrap: [AppComponent]
})

export class AppModule { }//NOTE: this is an empty class
```

>media.form.component.ts

```js
import {Component} from '@angular/core';
import {FormGroup, FormControl} from '@angular/forms';

@Component({
    selector:'mw-media-form',
    templateUrl:'./media.form.component.html',
    styleUrls:['./../bootstrap.min.css','./media.component.css']
})
export class MediaFormComponent{
    form:Object; //form ppty

    //This is one of the component's lifecycle method. Similar to angular1.6 .$onInit () method    
    ngOnInit(){
        this.form = new FormGroup({
            medium:new FormControl('Movies'),
            name: new FormControl(),
            category: new FormControl(),
            year: new FormControl()
        });
    }//end:ngOnInit
    
    onSubmit = (formValue) =>{
        console.log('Submitted ',formValue);
    }//end:onSubmit
}//end:class-MediaFormComponent
```

```html
<header>
    <h2>Add Media to Watch</h2>
</header>
<form
    [formGroup]="form"
    (ngSubmit)="onSubmit(form.value)">
    <ul>
        <li>
            <label for="medium">Medium</label>
            <select name="medium" id="medium" formControlName="medium">
                <option value="Movies">Movies</option>
                <option value="Series">Series</option>
            </select>
        </li>
        <li>
            <label for="name">Name</label>
            <input type="text" name="name" id="name" formControlName="name">
        </li>
        <li>
            <label for="category">Category</label>
            <select name="category" id="category" formControlName="category">
                <option value="Action">Action</option>
                <option value="Science Fiction">Science Fiction</option>
                <option value="Comedy">Comedy</option>
                <option value="Drama">Drama</option>
                <option value="Horror">Horror</option>
                <option value="Romance">Romance</option>
            </select>
        </li>
        <li>
            <label for="year">Year</label>
            <input type="text" name="year" id="year" maxlength="4" formControlName="year">
        </li>
    </ul>
    <button type="submit">Save</button>
</form>
```

---

### Custom Validations

Custom Regex Expressions: `[\\w\\-\\s\\/]+`

```js
import {Component} from '@angular/core';
import {FormGroup, FormControl, Validators} from '@angular/forms';

@Component({
    selector:'mw-media-form',
    templateUrl:'./media.form.component.html',
    styleUrls:['./../bootstrap.min.css','./media.component.css']
})
export class MediaFormComponent{
    form:Object; //form ppty

    //This is one of the component's lifecycle method. Similar to angular1.6 .$onInit () method    
    ngOnInit(){
        this.form = new FormGroup({
            medium:new FormControl('Movies'),
            name: new FormControl('',Validators.compose([
                Validators.required,
                Validators.pattern('[\\w\\-\\s\\/]+')
            ])),
            category: new FormControl(),
            year: new FormControl('',this.yearValidator)
        });
    }//end:ngOnInit

    yearValidator(control){
        if(control.value.trim().length === 0){
            return null
        }
        let year = parseInt(control.value);
        let minYear = 1990;
        let maxYear = 2120;
        if(year>= minYear && year<=maxYear){
            return null;
        }else{
            return {'year':true};
        }
    }//end:yearValidation
    
    onSubmit = (formValue) =>{
        console.log('Submitted ',formValue);
    }//end:onSubmit
}//end:class-MediaFormComponent
```
---

## Constructor Injection using the `FormBuilder` Module

We can use the `{FormBuilder}` compoent from `@angular/forms` module, and use it as a service and again achieve the same result as above

```js
import {Component} from '@angular/core';
import {FormBuilder, Validators} from '@angular/forms';

@Component({
    selector:'mw-media-form',
    templateUrl:'./media.form.component.html',
    styleUrls:['./../bootstrap.min.css','./media.component.css']
})
export class MediaFormComponent{
    form:Object; //form ppty

    //Using a constructor for Class instantiation
    constructor(private formBuilder:FormBuilder){        
    }//end:constructor

    //This is one of the component's lifecycle method. Similar to angular1.6 .$onInit () method    
    ngOnInit(){
        this.form = this.formBuilder.group({
            medium:this.formBuilder.control('Movies'),
            name: this.formBuilder.control('',Validators.compose([
                Validators.required,
                Validators.pattern('[\\w\\-\\s\\/]+')
            ])),
            category: this.formBuilder.control(''),
            year: this.formBuilder.control('',this.yearValidator)
        });
    }//end:ngOnInit

    yearValidator(control){
        if(control.value.trim().length === 0){
            return null
        }
        let year = parseInt(control.value);
        let minYear = 1990;
        let maxYear = 2120;
        if(year>= minYear && year<=maxYear){
            return null;
        }else{
            return {'year':true};
        }
    }//end:yearValidation
    
    onSubmit = (formValue) =>{
        console.log('Submitted ',formValue);
    }//end:onSubmit
}//end:class-MediaFormComponent
```
---

# Angular2 Safe Property Operator

Angular's Safe Property Operator is like a safety check but on the HTML side.

```html
<div *ngIf="form.controls.name.error?.pattern" class="error">Name has invalid Characters</div>
```

# Services in Angular2

Built in Services in Angular2. They are

- HTTP Service
- FormBuilder
- Router

> NOTE: Services can be injected in Angular in two places

1. In the Root component using the `providers` array configuration

```js
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { ReactiveFormsModule } from '@angular/forms';
import { HttpModule } from '@angular/http';

//Custom Components and Directives
import { AppComponent } from './app.component';
import {MediaComponent} from './components/media.component';
import {MediaFormComponent} from './components/media.form.component';
import {FavouriteDirective} from './directives/favourite.directive';
import {CategoryListPipe} from './pipes/category-list.pipe';
import {MediaItemService} from './services/media-item.service'; //Importing the MediaItemService module

@NgModule({
  declarations: [
    AppComponent,
    MediaComponent,
    MediaFormComponent,
    FavouriteDirective,
    CategoryListPipe
  ],
  imports: [
    BrowserModule,
    ReactiveFormsModule,
    HttpModule    
  ],
  providers: [
    MediaItemService //This will make it the SingleTon service available to the entire application
  ],
  bootstrap: [AppComponent]
})

export class AppModule { }//NOTE: this is an empty class

```

2. As a constructor in Individual Components

```js
// MediaComponent
import { Component, Input, Output, EventEmitter } from '@angular/core';
//Importing the service in the MediaComponent
import {MediaItemService} from './../services/media-item.service';

@Component({
    selector:'mw-media',
    templateUrl:'./media.component.html',
    styleUrls:['../bootstrap.min.css','./media.component.css']
})
export class MediaComponent{
    mediaItems = [];
    constructor(private mediaItemService:MediaItemService){
    }//end:constructor

    ngOnInit(){
        this.mediaItems = this.mediaItemService.get();
    }
   
   @Input() mediaItem;
   @Output() delete = new EventEmitter();
   isWatchedOn:boolean = false;

   title:String = `Welcome to the Custom Media Component`;

   onDelete = ():void=>{
    this.isWatchedOn = true;
    this.delete.emit(this.mediaItem);
    console.log('The onDelete Button was clicked');
   }//end:onDelete
}//end:class-MediaComponent
```

# Enum Declaration in Angular2

The following code is a way to introduce Enumerations, ENUMs or in AngularJS like `angular.module().constants()`


# HTTP Service Module in Angular2

