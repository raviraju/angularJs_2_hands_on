# angularJs_2_hands_on
Angular JS 2 hands on exploring features
Download repo and execute
1)npm install
2)npm start

AngularJS2 : framework for app development
client side framework : no call to server to retrieve a new visual state as app is used

Getting Started
<!--------------------------------------------/AngularJS2/--------------------------------------------
AngularJS1				AngularJS2
custom-directives		components(lets to create functionality around <custom-tags>)
						No more controllers
						No more scope(to handle communication bw templates and controllers)
Program in diff languages
						ES5, Dart (open src lang from Google) or TypeScript (from Microsoft)
						Use of ES6 features
							classes
							templates
						TypeScript features:
							types (data types of variables)
							annotations (metadata to code)
						ES6 & TypeScript not supported by browsers : converters : webpack, gulpJs
-->
<!--------------------------------------------/Working with our build tool/------------------------------------------
gulpfile.js to configure build tasks
tsconfig.json : typescript compilation 
-->
<!--------------------------------------------/Setting up our template/------------------------------------------
package.json : used by npm to get all dependancies
understanding project structure
-->

Introduction to basics
<!--------------------------------------------/Creating a simple component/------------------------------------------
Every component has 3 parts:
1)Decorator : info about controller
2)View		: template to be shown
3)Controller: javascript which adds functionality to the view

https://angular.io/docs/js/latest/api/
in boot.ts
import {Component} from 'angular2/core';
import {bootstrap} from 'angular2/platform/browser';

@Component({									//decorator to setup info abt component, on creation of component a shadow DOM element for component is created, then loads template into that DOM element
  selector: 'my-app',							//Thus gives us ability to create our own custom html tags and define what tat tag is going to do with Javascript
												//name <my-app>
  template: '<h1>Welcome to my App</h1>'
})												//decorators return a function
-->
<!--------------------------------------------/Using multiple modules/------------------------------------------
boot module, app module

app.component.ts:
export class AppComponent {

}

boot.ts:
import {AppComponent} from './app.component';
-->
<!--------------------------------------------/Understanding template types/------------------------------------------
3 types of template
1)template property having html defined in @Component decorator
	@Component({
		selector: 'my-app',	
		template: '<h1>Welcome to my App</h1>'
	})
2) multi-line template string using backticks``		//but here disadvantage is : use of backticks eliminates all syntax highlightening
	@Component({
		selector: 'my-app',	
		template: `<h1>Welcome to my App</h1>
					<h2>another line</h2>`
	})
3) external file
	@Component({
	  selector: 'my-app',
	  templateUrl: 'partials/app.html' 
	})
 -->
<!--------------------------------------------/Displaying data in our templates/------------------------------------------
use of interpolation : {{}}
 
export class AppComponent {
    name =  'Ravi';

    setnames: string[];
    constructor() {
        this.setnames = ['kiran', 'raju'];
    }
}
  <div class="card search">
    <h1>Artist Directory</h1>
    <span *ngIf="name">name: {{name}}</span>
  </div>
  <ul>
    <li *ngFor="#n of setnames">{{n}}</li>		//here #n is the temp variable
  </ul>
-->
<!--------------------------------------------/Working with events (click)="(onClick($event))"/------------------------------------------
<ul class="artistlist cf">
  <li (click)="onClick($event)" *ngFor="#item of artists">
    <h2>{{item}}</h2>
  </li>
</ul>

onClick(e) {
	console.log(e);
	this.name=e.target.innerHTML;
}

<input #newArtist>
<button (click)="addArtist(newArtist.value)">Add</button>
addArtist(myArtist) {
	this.artists.push(myArtist);
}

<input #newArtist
               (keyup.enter)="addArtist(newArtist.value); newArtist.value=''"
               (blur)="addArtist(newArtist.value); newArtist.value=''"
        >

-->
<!--------------------------------------------/Using properties/------------------------------------------
<span *ngIf="name">name: {{name}}</span>
or
<label *ngIf="name" [innerHTML]="'name: ' + name"></label>
or
<label *ngIf="name" bind-innerHTML="'name: ' + name"></label>

we can also pass reference to HTML element (#artistContainer)
<ul class="artistlist cf">
    <li #artistContainer (click)="onClick(item, artistContainer)" *ngFor="#item of artists">
        <h2>{{item}}</h2>
    </li>
</ul>
onClick(myName, myElement) {
	this.name=myName;
	myElement.style.backgroundColor="#FECE4E";
}
-->
<!--------------------------------------------/Using two-way data binding : [(ngModel)]="name"/------------------------------------------
Respond to event and update property simulatenously
<input #newArtist 
	[value]="name"
	(input)="name=$event.target.value"
   (keyup.enter)="addArtist(newArtist.value); newArtist.value=''"
   (blur)="addArtist(newArtist.value); newArtist.value=''"
>

[value]="name"
(input)="name=$event.target.value"

ngModel : for input fields
<input #newArtist
   [(ngModel)]="name"
   (keyup.enter)="addArtist(newArtist.value); newArtist.value=''"
   (blur)="addArtist(newArtist.value); newArtist.value=''"
>
ngControl : checkbox,radio btn
-->
<!--------------------------------------------/Adding CSS to our component/------------------------------------------
3 ways similar to templates 
1) styles
@Component({
    selector: 'my-app',
    templateUrl: 'partials/app.html',
    styles:['.btn{border-color: #008800; border-radius: 4px;}']
})
2) multi-line styles
@Component({
    selector: 'my-app',
    templateUrl: 'partials/app.html',
    styles:[`.btn{
            border-color: #2e2e2e;
            border-radius: 4px;
            }`]
})
3) styleUrls
@Component({
    selector: 'my-app',
    templateUrl: 'partials/app.html',
    styleUrls: ['css/app.css']
})

-->

Creating a basic module
<!--------------------------------------------/Using more complex data/------------------------------------------
interface Artist {
    name: string;
    shortname: string;
    reknown: string;
    bio: string;
}
var ARTISTS: Artist[] = [
  {
    "name":"Barot Bellingham",
    "shortname":"Barot_Bellingham",
    "reknown":"Royal Academy of Painting and Sculpture",
    "bio":"Barot has just finished his final year at The Royal Academy of Painting and Sculpture, where he excelled in glass etching paintings and portraiture. Hailed as one of the most diverse artists of his generation, Barot is equally as skilled with watercolors as he is with oils, and is just as well-balanced in different subject areas. Barot's collection entitled \"The Un-Collection\" will adorn the walls of Gilbert Hall, depicting his range of skills and sensibilities - all of them, uniquely Barot, yet undeniably different"
  }, {
    "name":"Jonathan G. Ferrar II",
    "shortname":"Jonathan_Ferrar",
    "reknown":"Artist to Watch in 2012",
    "bio":"The Artist to Watch in 2012 by the London Review, Johnathan has already sold one of the highest priced-commissions paid to an art student, ever on record. The piece, entitled Gratitude Resort, a work in oil and mixed media, was sold for $750,000 and Jonathan donated all the proceeds to Art for Peace, an organization that provides college art scholarships for creative children in developing nations"
  }
}
-->
<!--------------------------------------------/Creating a subcomponent/------------------------------------------
to pass info into componenet, we can just set property say : [artist]=item
<ul class="artistlist cf">
  <li class="artist cf" #artistContainer (click)="onClick(item, artistContainer)" *ngFor="#item of artists">
    <artist-item class="content" [artist]=item></artist-item>
  </li>
</ul>
we can access the input parameters in subcomponent using 'input'
@Component ({
  selector: 'artist-item',
  templateUrl: 'partials/artistitem.html',
  inputs: ['artist']									//add any css styles, as the other styles may not be applicable which is component specific
})

import {ArtistItemComponent} from './artist-item.component';
@Component({
    selector: 'my-app',
    templateUrl: 'partials/app.html',
    directives: [ArtistItemComponent],			//to let the decorator know that we have imported module
    styleUrls: ['css/app.css']
})
-->
<!--------------------------------------------/Using multiple subcomponents/-------------------------------------------->
<!--------------------------------------------/Cleaning up components/-------------------------------------------->
<!--------------------------------------------/Filtering content through data pipes/------------------------------------------
Filters are called Pipes
1)simple ones:
<h1>{{artist.name | uppercase}}</h1>
2)

import {Pipe} from 'angular2/core'
@Pipe({
    name: 'find'
})
export class SearchPipe{
    transform(pipeData,[pipeModifier]){
        return pipeData.filter(eachItem =>{
            return eachItem['name'].toLowerCase().includes(pipeModifier.toLowerCase()) ||
                    eachItem['reknown'].toLowerCase().includes(pipeModifier.toLowerCase());
        })
    }
}


import {SearchPipe} from './search.pipe';
@Component({
  selector: 'my-app',
  templateUrl: 'partials/app.html',
  directives: [ArtistItemComponent, ArtistDetailsComponent],
  pipes: [SearchPipe],
  styleUrls: ['css/app.css']
})


<ul *ngIf="query" class="artistlist cf">
  <li (click)="showArtist(item); query='';" class="artist cf" *ngFor="#item of (artists) | find:query">
    <artist-item class="content" [artist]=item></artist-item>
  </li>
</ul>
-->



AngularJS2 Essential Training
<!--------------------------------------------/Basics of TypeScript/------------------------------------------
Angular Src code in TypeScript
TypeScript : Superset of JavaScript , Transpiled into JavaScript
		Adv :ES 2015 Classes, Modules
			 Strong typing for variables, function signatures [strong typing is optional]
			 Still write JavaScript in a ts file
		Writing classes
		Angular decorators
		Parameter type annotations
		
import {Component} from 'angular2/core';
import {FormBuilder} from 'angular2/common';

@Component({									//decorator
    selector: 'media-tracker-app',
    directives: [MediaItemComponent],
    templateUrl: 'app/app.component.html',
    styleUrls: ['app/app.component.css']
})
export class AppComponent {					//export keyword converts the class into a module
	constructor(fromBuilder: FormBuilder){}	// : FormBuilder syntax for strong typing
}
-->

Architecture Overview
<!--------------------------------------------/Component, Bootstrap and DOM/------------------------------------------
AngularJS2 made of components
Start of app : Bootstrap of initial parent component
Component Tree Model like HTML DOM tree model
After bootstapping parent component, check to see if the corresponding HTML view has any nested components,if so, find matches and run those respective component codes

Component in Angular is used to render a portion of HTML and provide a functionality to that portion
															(with help of component class, which helps to define biz logic)
We can also have a component to nest other sub-components
each component is associated with selector html tag
-->
<!--------------------------------------------/Directives and pipes/------------------------------------------
Component is a Directive(provide functionality and can transform DOM) with a Template

2types of directives:
1)Structural : modify layout by altering elements in DOM
2)Attribute	: change behaviour/appearance of existing DOM elements

Directive dont have any templates, intented to be applied to existing/template element in order to change that element in some way

similar to component, directive uses 'selector' to find and apply directive
@Decorator({
	selector: 'myFav'
})
directives can be applied in several ways:
1) specify the selector value as an attribute
<div myFav>
	<img src="fav.png">
</div>
2) add a directive in an assignemnt statement
<div [myFav]="true">
	<img src="fav.png">
</div>

some angular directives:
ngIf (conditionally render elements)
ngFor (looping items to render)
routerLink

Angulare Pipe (|)
another tool to display content : Pipe
angulare pipes
	|uppercase
	|lowercase
	|Date
custom pipes

Pipe : great way to 
	change data in a reusable way,without having to embed tranform logic within component class
	without having to modify data just for display purposes
-->
<!--------------------------------------------/Data binding/------------------------------------------
 binding data to views
 common way : to use interpolation <h1>{{movie.title}}</h1>
 
 directives give flexibilty to add biz logic at client side
 
 local template variable : #var, and use them in sibling or child elements
 Billing Address:<input #billing>
 Shipping Address:<input #shipping>
 <button (click)="shipping.value = billing.value">Use Billing Address</button>
 -->
<!--------------------------------------------/Dependency injection/------------------------------------------
DI : concept of Inversion of Control (IOC)
provide modules needed, instead going and fetching modules

common place of use DI : class constructor of components, directives, pipes and Services
constructor(formBuilder: FormBuilder){}
DI at bootstrap phase:
bootstrap(App,[DataService,FormulaService]);
-->
<!--------------------------------------------/Services and other business logic/------------------------------------------
Refer class and its application logic as a Service

We can just group set of methods into a Service and provide the Service as DI to component

Mainly to write modular,decoupled code, which is easier to maintain and use
-->
<!--------------------------------------------/Data persistence/------------------------------------------
client-side app : intent to read/write some data
requires to pull and persist data
1)In-Memory Storage
2)Browser Storage : Local storage
3)Server side data : leverage Http protocol
	XHR
	JsonP
-->
<!--------------------------------------------/Routing/------------------------------------------
Server routing solution : (via different url request to server)
Client side routing:
	instead of sending unique url request to server,handle them in client
-->

Components
<!--------------------------------------------/Component metadata/------------------------------------------
Decorator : to configure ES2015 classes
		  : Expressiong that evaluates to a function allowing annotation of classes at design time
@Component({}) : to indicate Angular to treat class as a component-->
<!--------------------------------------------/Component selector/-------------------------------------------->
<!--------------------------------------------/Component template/-------------------------------------------->
<!--------------------------------------------/Styling a component: styles, styleUrls : NOTE CSS will be component and its child component specific/-------------------------------------------->
<!--------------------------------------------/Using other components in a component : directives: [MediaItemComponent]/------------------------------------------
@Component({
    selector: 'media-tracker-app',
    directives: [MediaItemComponent],
    templateUrl: 'app/app.component.html',
    styleUrls: ['app/app.component.css']
})
-->
<!--------------------------------------------/Interpolation and the expression context : {{name}} , {{wasWatched()}}/------------------------------------------
Interpolation : way to get data display in a view {{javascript expr that Angular will evaulate}}
			  : mainly used to display data from property we have set on the component class
			  <h2>{{name}}</h2>
			  <div>Watched on 1/13/2016 : {{wasWatched()}}</div>
			  wasWatched(){return true; }
the following are invalid template expressions	//IMPORTANT
1)Assignments
2)Newing up variables
3)Chaining expressions
4)Incrementing/Decrementing
-->
<!--------------------------------------------/Property binding : <h2 [textContent]="name"></h2> or <h2 textContent="{{name}}"></h2>/------------------------------------------
<h2 [textContent]="name"></h2>
<h2 textContent="{{name}}"></h2>
-->
<!--------------------------------------------/Event binding/------------------------------------------
to subscribe to native dom or custom events
<a (click)="onDelete()" class="delete">
        remove
</a>
-->
<!--------------------------------------------/Getting data to the component with input/------------------------------------------
export class AppComponent {
    firstMediaItem = {
        id: 1,
        name: "Firebug",
        medium: "Series",
        category: "Science Fiction",
        year: 2010,
        watchedOn: 1294166565384,
        isFavorite: false
    };
}
<media-item [mediaItemToWatch]="firstMediaItem"></media-item>

::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::
import {Component, Input} from 'angular2/core';
@Input('mediaItemToWatch') mediaItem;

<h2>{{ mediaItem.name }}</h2>
<div>Watched on {{ mediaItem.watchedOn }}</div>
<div>{{ mediaItem.category }}</div>
<div>{{ mediaItem.year }}</div>
-->
<!--------------------------------------------/Subscribing to component events with output/------------------------------------------
import {Component, Input, Output, EventEmitter} from 'angular2/core';
export class MediaItemComponent {
    @Input('mediaItemToWatch') mediaItem;
    @Output('deleted') delete = new EventEmitter();
    
    onDelete(){
        console.log("onDelete");
        this.delete.emit(this.mediaItem);
    }
}

<media-item [mediaItemToWatch]="firstMediaItem"
                (deleted)="onMediaItemDeleted($event)"></media-item>	//$event will have value emitted by event 'deleted'
				
onMediaItemDeleted(mediaItem){
	console.log("onMediaItemDeleted : " + mediaItem);
}
-->

Directives and Pipes
<!--------------------------------------------/Structural directives—ngIf : conditionally render the DOM/------------------------------------------
2types of directives:
1)Structural : modify DOM layout by add/removing DOM elements
2)Attribute : change appearance or behaviour of DOM element they are attached to, do not create/remove DOM elements
ngIf : conditionally render the DOM where the directive is on
Structural directives are applied to normal DOM elements using *ngIf syntax
Angular built-in directives need not be added in  component metadata directives property: [MediaItemComponent], Framework takes care	

<div *ngIf="mediaItem.watchedOn">Watched on {{ mediaItem.watchedOn }}</div>
* is a syntactic sugar for : <template> never makes it to DOM (useful to handle multiple elments instead of introducing a new wrapper dom element 	)
<template [ngIf]="mediaItem.watchedOn">
    <div>Watched on {{ mediaItem.watchedOn }}</div>
</template>
-->
<!--------------------------------------------/Structural directives—ngFor : to repeat markup when looping over some data/------------------------------------------
ngFor : to repeat markup when looping over some data
    <media-item
        *ngFor="#mediaItem of mediaItems"									#mediaItem is local template variable
        (deleted)="onMediaItemDeleted($event)"></media-item>
-->
<!--------------------------------------------/Attribute directives—built in/------------------------------------------
Attribute : change appearance or behaviour of DOM element they are attached to, do not create/remove DOM elements
<media-item>
[ngClass]="{'medium-movies' : mediaItem.medium === 'Movies',
		    'medium-series' : mediaItem.medium === 'Series'}"></media-item>
-->
<!--------------------------------------------/Attribute directives—custom : HostBinding/------------------------------------------
set a class to media-item if its set to favourite or not
import {Directive, HostBinding, Input} from 'angular2/core';

@Directive({
    selector: '[mwFavorite]'
})
export class FavoriteDirective {
    @HostBinding('class.is-favorite') isFavorite = true;
    
    @Input()
    set mwFavorite(value) {
        this.isFavorite = value;
    }
}

import {FavoriteDirective} from './favorite.directive';

@Component({
    selector: 'media-item',
    directives: [FavoriteDirective],
    templateUrl: 'app/media-item.component.html',
    styleUrls: ['app/media-item.component.css']
})

<svg [mwFavorite]="mediaItem.isFavorite"/>
-->
<!--------------------------------------------/Working with events in directives : HostListener/------------------------------------------
import {Directive, HostBinding, HostListener, Input} from 'angular2/core';
@HostBinding('class.is-favorite-hovering') hovering = false;

@HostListener('mouseenter')
onMouseEnter(){
	this.hovering = true;
}

@HostListener('mouseleave')
onMouseLeave() {
	this.hovering = false;
}

-->
<!--------------------------------------------/Angular pipes—built in/------------------------------------------
Pipe : | template expesssion operator that takes in a value and returns new value representation

date pipe
Watched on 1294166565384
<div *ngIf="mediaItem.watchedOn">Watched on {{ mediaItem.watchedOn | date}}</div>
Watched on Jan 5, 2011
<div *ngIf="mediaItem.watchedOn">Watched on {{ mediaItem.watchedOn | date : 'shortDate'}}</div>
Watched on 1/5/2011

slice pipe
Happy Joe: Cheery Road
<h2>{{ mediaItem.name | slice: 0:10 }}</h2>
Happy Joe:

chaning of pipes
Happy Joe:
<h2>{{ mediaItem.name | slice: 0:10 | uppercase }}</h2>
HAPPY JOE:
-->
<!--------------------------------------------/Angular pipes—custom/------------------------------------------
import {Pipe} from 'angular2/core';

@Pipe({
    name: 'categoryList'
})
export class CategoryListPipe {
    transform(mediaItems) {						//first parameter is the value which is piped in
        var categories = [];
        mediaItems.forEach(mediaItem => {
            if (categories.indexOf(mediaItem.category) <= -1) {
                categories.push(mediaItem.category);
            }
        });
        return categories.join(', ');
    }
}
peer: property helps to define if its stateless(by default) or statefull
<heading>
    <div>{{mediaItems | categoryList}}</div>
</heading>
Science Fiction, Comedy, Action, Drama
-->

Forms
<!--------------------------------------------/Angular forms/------------------------------------------
Collect data in web apps, with use of forms

common tasks
1)Collect data
2)Track changes(as we type)
3)Validate data
4)Show Errors

2 approaches to building forms
1)Template driven : majority of form logic in template markup		(Ease of use, Simple)
2)Model driven    : majority of form logic in component class		(Full powered, not dependant on markup)
-->
<!--------------------------------------------/Template-driven forms/------------------------------------------
with builtin ngForm directive shall look out for <form> elements (which has 'form' as its selector value)
but we have to tell angular, wat fields are part of the form, with ngControl
ngControl="medium", shall create new named control behind the scenes
<select name="medium" id="medium" ngControl="medium" required/>

<form #mediaItemForm="ngForm" (ngSubmit)="onSubmit(mediaItemForm.value)"/>

onSubmit(mediaItem){
	console.log(mediaItem);
}
Object {medium: "Movies"}
-->
<!--------------------------------------------/Model-driven forms/------------------------------------------
Control group object : representing form
link with lifecycle events for easy testing (also can patch up with constructor)

import {ControlGroup, Control} from 'angular2/common';
ngOnInit(){
	this.form = new ControlGroup({
		'medium' : new Control('Movies'),
		'name': new Control(''),
		'category': new Control(''),
		'year': new Control('')
	})
}

we got to tell angular tat we have model for the form
<form [ngFormModel]="form" (ngSubmit)="onSubmit(form.value)"/>
-->
<!--------------------------------------------/Validation—built in/------------------------------------------
import {ControlGroup, Control, Validators} from 'angular2/common';
'name': new Control('', Validators.compose([
	Validators.required,
	Validators.pattern('[\\w\\-\\s\\/]+')
])),
On invalid :
<input _ngcontent-lqs-2="" id="name" name="name" ngcontrol="name" required="" type="text" class="ng-dirty ng-invalid ng-touched">
if valid :
<input _ngcontent-lqs-2="" id="name" name="name" ngcontrol="name" required="" type="text" class="ng-dirty ng-touched ng-valid">

<button type="submit" [disabled]="!form.valid">Save</button>
-->
<!--------------------------------------------/Validation—custom : return null if valid, else an object if invalid/------------------------------------------
'year': new Control('', this.yearValidator)
yearValidator(control){
	if(control.value.trim().length === 0) return null;
	var year = parseInt(control.value);
	var minYear = 1900;
	var maxYear = 2100;
	if(year>= minYear && year<= maxYear) return null;
	return {'year' : true};
}
-->
<!--------------------------------------------/Error handling/------------------------------------------
Angular will add errors property to control objects when they are invalid
<input type="text" name="name" id="name" ngControl="name" #name_var="ngForm" required>
Angular shall set control object instance to #name_var local template variable

Angular shall return objects to properties
elvis operator ?. (stop before question mark if its null)
<div *ngIf="name_var.errors?.pattern" class="error">name has invalid characters</div>

yearValidator(control){
	if(control.value.trim().length === 0) return null;
	var year = parseInt(control.value);
	var minYear = 1800;
	var maxYear = 2500;
	if(year>= minYear && year<= maxYear) return null;
	return {'year' : {
						'min' : minYear, 
						'max' : maxYear
					}
			};
}
<input type="text" name="year" id="year" ngControl="year" #year_var="ngForm" maxlength="4">
<div *ngIf="year_var.errors?.year" class="error">invalid year value, should be between {{year_var.errors.year.min}}
	and {{year_var.errors.year.max}}
</div>
-->

Dependency Injection : Modularity : importing task specific services singleton
<!--------------------------------------------/How Angular does dependency injection/------------------------------------------
Done in 2 steps:
1)Service Registration
	Let Angular know list of things that can be injected
	with provider: metadata
2)Retrival of those things by :
	a)Constructor Injection(leveraging typescript type annotations)
	constructor parameters with type info provided
	or
	b)Angular Inject Decorator : @Inject(){}

Angular provides access to Injector, so as to locate the service specifically(usually not needed), as we can make use of Constructor Injection

If service is already existing, the same is used, as a Singleton

register things at component level, those singleton of things are available at component and below the tree
Something registered at bootstrap is available in the entire component tree 
My Service : my own class to encapsulate some logic
-->
<!--------------------------------------------/Services(Singleton) in Angular/------------------------------------------
Services : not Angular specific construct
		   nothing more than a class to encapsulate some logic that we wanna share across places in application
		   provide a architectural way of encapsulating biz logic in a reusable fashion
		   use Mock Services for unit testing of components
		   
		   Inject Services in application
Why use services ?
1)most common use is Data Service : class to handle getting/setting data from data store
	need not create data connection code in each component
2)for business logic

Angular Services : Http, FormBuilder, Router : logic for specific things, which are non-component specific
-->
<!--------------------------------------------/Class constructor injection/------------------------------------------
FormBuilder has a group method to create control group
import {FormBuilder, Control, Validators} from 'angular2/common';
constructor(private formBuilder: FormBuilder){}		//use of private, TypeScript automatically creates a property and set it for us
so instead of
this.form = new ControlGroup({
this.form = this.formBuilder.group({
	'medium' : new Control('Movies'),
	'name': new Control('', Validators.compose([
		Validators.required,
		Validators.pattern('[\\w\\-\\s\\/]+')
	])),
	'category': new Control(''),
	'year': new Control('', this.yearValidator)
})
Such Angular services, need not be explicitly set up them as providers, just import them and use in constructor injection
-->
<!--------------------------------------------/Building a service/------------------------------------------
export class MediaItemService {
    get() {
        return this.mediaItems;
    }
    
    add(mediaItem) {
        this.mediaItems.push(mediaItem);
    }
    
    delete(mediaItem) {
        var index = this.mediaItems.indexOf(mediaItem);
        if (index >= 0) {
            this.mediaItems.splice(index, 1);
        }
    }
    
    mediaItems = [
        {
            id: 1,
            name: "Firebug",
            medium: "Series",
            category: "Science Fiction",
            year: 2010,
            watchedOn: 1294166565384,
            isFavorite: false
        },{
            id: 5,
            name: "Happy Joe: Cheery Road",
            medium: "Movies",
            category: "Action",
            year: 2015,
            watchedOn: 1457166565384,
            isFavorite: false
        }
    ];
}

import {MediaItemService} from './media-item.service';
providers:[MediaItemService],

constructor(private mediaItemService: MediaItemService){}	//constructor injection

if we set the providers in 2 different components, a seperate instance is created for each, each will refer to their own collection data
Can be solved if we provide registration up the component tree
-->
<!--------------------------------------------/Provider registration at Bootstrap/------------------------------------------
if service is registered at Bootstrap component, which is root of component tree, hence service is available throughout
import {MediaItemService} from './media-item.service';

bootstrap(AppComponent,[MediaItemService]);  //second parameter is array of providers
now get rid of providers property down the child components
Hence Angular injects same singleton instance of it to all the children components that ask for it

Value provider : to store some lookup list and provide them at bootstrap
import {provide} from 'angular2/core';

var lookup_lists = { mediums: ['Movies','Series'] };

bootstrap(AppComponent,[
    MediaItemService,
    provide('LOOKUP_LISTS', {useValue : lookup_lists})		//Key: object (useValue or useClass keys)
    ]); 
-->
<!--------------------------------------------/The Inject decorator/------------------------------------------
Constructor injection of class types can be done with typescript
Constructor injection of value types :
import {Component, Inject} from 'angular2/core';

@Inject decorator is used to decorate function parameters
constructor(private formBuilder: FormBuilder,
		private mediaItemService: MediaItemService,
		@Inject('LOOKUP_LISTS') public lookupLists){}		//@Inject passing in string literal that represents value type
													//tells to pass in lookup_lists value object in constructor during constructor injection
<select name="medium" id="medium" ngControl="medium" required>
	<option *ngFor="#medium of lookupLists.mediums" value={{medium}}>{{medium}}</option>
</select>
-->
<!--------------------------------------------/The opaque token : like defintion const symbols/------------------------------------------
Relying on string literals for tokens 'LOOKUP_LISTS' is always a bit of risk : mistyping, harder to maintain, code-editors dont have effective means to refactoring
import {OpaqueToken} from 'angular2/core';

export var lookupLists = {
    mediums: ['Movies', 'Series']
};

export var LOOKUP_LISTS = new OpaqueToken('LookupLists');

import {LOOKUP_LISTS} from './providers';
constructor(private formBuilder: FormBuilder,
		private mediaItemService: MediaItemService,
		@Inject(LOOKUP_LISTS) public lookupLists){}
-->

HTTP
<!--------------------------------------------/The Angular 2 HTTP bundle/------------------------------------------
<script src="node_modules/angular2/bundles/http.dev.js"></script>
<script src="node_modules/rxjs/bundles/Rx.js"></script>

//include HTTP providers in bootstrap
import {HTTP_PROVIDERS} from 'angular2/http';

bootstrap(AppComponent,[
    MediaItemService,
    provide(LOOKUP_LISTS, {useValue : lookupLists}),
    HTTP_PROVIDERS
    ]); 
-->
<!--------------------------------------------/Using a mock back end for HTTP calls/------------------------------------------
import {HTTP_PROVIDERS, XHRBackend} from 'angular2/http';
import {MockXHRBackend} from './mock-xhr-backend';

bootstrap(AppComponent,[
    MediaItemService,
    provide(LOOKUP_LISTS, {useValue : lookupLists}),
    HTTP_PROVIDERS,
    provide(XHRBackend, {useClass: MockXHRBackend})
    ]);
-->	
<!--------------------------------------------/Using HTTP for GET calls/-------------------------------------------->
<!--------------------------------------------/Using UrlSearchParams/-------------------------------------------->
<!--------------------------------------------/Using HTTP for POST,PUT,DELETE calls/------------------------------------------
HTTP class for handling http requests and responses
import {Http, URLSearchParams, Headers} from 'angular2/http';
import {Injectable} from 'angular2/core';
import 'rxjs/add/operator/map';

@Injectable()					//as the class is a Service that we built not a Component or Decorator to make use of provider metadata
export class MediaItemService { //Angular doesnt know how to do constructor injection, so we have to explicitly state it using @Injectable()

    constructor(private http: Http){}
    get(medium) {
        var searchParams = new URLSearchParams();
        searchParams.append('medium', medium);
        return this.http.get('mediaitems', {search: searchParams})
            .map(response => {
                return response.json().mediaItems;
            });
    }
    
    add(mediaItem) {
        var headers = new Headers({ 'Content-Type': 'application/json' });
        return this.http.post('mediaitems', JSON.stringify(mediaItem), { headers: headers })
            .map(response => {});
    }
    
    delete(mediaItem) {
        return this.http.delete(`mediaitems/${mediaItem.id}`)
            .map(response => {});
    }
}
				

ngOnInit() {
	this.getMediaItems(this.medium);
}

onMediaItemDeleted(mediaItem) {
	this.mediaItemService.delete(mediaItem)
		.subscribe(() => {
			this.getMediaItems(this.medium);
		});
}

getMediaItems(medium) {
	this.medium = medium;
	this.mediaItemService.get(medium)
		.subscribe(mediaItems => {				//takes function with upto 3 args : next(successful return of mediaItems),error,completion
			this.mediaItems = mediaItems;
		});
}
-->

Routing
<!--------------------------------------------/The Angular 2 routing bundle/------------------------------------------
to handle client side routing
<script src="node_modules/angular2/bundles/router.dev.js"></script>
<base href="/"> //used by router to handle application root path for routes

import {ROUTER_PROVIDERS} from 'angular2/router';

bootstrap(AppComponent,[
    MediaItemService,
    provide(LOOKUP_LISTS, {useValue : lookupLists}),
    HTTP_PROVIDERS,
    provide(XHRBackend, {useClass: MockXHRBackend}),
    ROUTER_PROVIDERS					//to make Router class available for app for constructor injection
    ]); 
-->
<!--------------------------------------------/Route configuration/------------------------------------------
configure routes at entry point of app : AppComponent.ts
import {RouteConfig,ROUTER_DIRECTIVES} from 'angular2/router';

@RouteConfig([
    {path:'/:medium', component:MediaItemListComponent, name: 'MediaItemsList'},
    {path:'/add', component: MediaItemFormComponent, name: 'AddMediaItem'}
    ])
@Component({
    selector: 'media-tracker-app',	
    //directives: [MediaItemListComponent, MediaItemFormComponent],		//as routing will handle loading those components
    directives:[ROUTER_DIRECTIVES],
    templateUrl: 'app/app.component.html',	
    styleUrls: ['app/app.component.css']
})

app.component.html

    <router-outlet></router-outlet>
    <!--
    <media-item-form></media-item-form>
    <media-item-list></media-item-list>
	
http://localhost:3000/add
<router-outlet _ngcontent-oho-1=""></router-outlet>
<media-item-form _ngcontent-oho-2="" _nghost-oho-3="">
</media-item-form>

http://localhost:3000/movies
<router-outlet _ngcontent-tab-1=""></router-outlet>
<media-item-list _ngcontent-tab-2="" _nghost-tab-3="">
</media-item-list>

-->
<!--------------------------------------------/Router links (from within template): for navigation/------------------------------------------
directive RouterLink : to change behaviour of HTML links to work with router engine
<nav>
    <a [routerLink]="['./List', { medium: '' }]">
        <img src="media/04.png" class="icon" />
    </a>
    <a [routerLink]="['./List', { medium: 'Movies' }]">
        <img src="media/03.png" class="icon" />
    </a>
    <a [routerLink]="['./List', { medium: 'Series' }]">
        <img src="media/02.png" class="icon" />
    </a>
</nav>

    <a [routerLink]="['../AddMediaItem']">
        <img src="media/01.png" class="icon" />
    </a>
-->
<!--------------------------------------------/Router class (from within code): for navigation/-----------------------------------------
this.router.navigate(['../List', {medium: mediaItem.medium}])
--->
