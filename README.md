# Angular Doc Tutorial - Tour of Heroes

### Create a New Workspace and Initial Application

```
ng new my-app-name
```

### Serve the Application

```
ng serve --open
```

### Make Changes to the Application

Use interpolation syntax {{ }} to present the component's property values inside the html template

```
<h1> {{ title }} </h1>
```

### Styles

Application-wide styles go in the src/styles.css file

### Generate a New Component

```
ng generate component component-name (heroes)
```

or ( ng g c heroes)

- The CLI generates the metadata properties in the component.ts file:
  1. selector - the component's CSS element selector
     - matches the name of the HTML element that identifies this component within a parent component's template
  2. templateURL - the location of the component's template file
  3. styleUrls - the location of the component's private CSS styles
- The ngOnInit() is a lifecycle hook. Angular calls ngOnInit() shortly after creating a component. It's a good place to put initialization logic.

### Show the New Component View

To display a component, you must add it to the template of the parent component using its element selector:

```
<app-heroes></app-heroes>
```

### Two-way Data Binding Syntax

```
<input [(ngModel)]="hero.name" placeholder="name" />
```

[(ngModel)] is Angular's two-way data binding syntax. Here it binds the hero.name property to the HTML input textbox so that data can flow in both directions: from the hero.name property to the textbox, and from the textbox back to the hero.name.

To use this, in app.module.ts import FormsModule from @angular/forms, and add FormsModule to the imports array.

### \*ngFor

```
<li *ngFor="let hero of heroes"> {{hero.name}} </li>
```

Angular's repeater directive that repeats the host element for each element in a list.

1. <li> is the host element.
2. heroes holds the mock heroes list form the HeroesComponent.
3. hero holds the current hero object for each iteration through the list.

### Defining Private Styles

You define private styles either inline in the @Component.styles array or as a stylesheet file(s) identified in the @Component.styleUrls array.

```
@Component({
  selector: 'app-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css'] // This stylesheet affects this component only
})
```

### Click Event Binding

```
<li *ngFor="let hero of heroes" (click)="onSelect(hero)">
```

The parenthesis around CLICK tell Angular to listen for the <li> element's click event. When the user clicks in the <li>, Angular executes the onSelect(hero) expression. Define your click method in the component's class.

### Use \*ngIf to Hide Empty Details

```
<div *ngIf="selectedHero"> // Only show this div if there is a selectedHero

  <h2>{{selectedHero.name | uppercase}} Details</h2>
  <div><span>id: </span>{{selectedHero.id}}</div>
  <div>
    <label>name:
      <input [(ngModel)]="selectedHero.name" placeholder="name"/>
    </label>
  </div>

</div>
```

### Class Binding

The Angular class binding makes it easy to add and remove a CSS class conditionally. Just add [class.some-css-class]="some-condition" to the element.

```
[class.selected]="hero === selectedHero" // Added to the <li> element
```

### Master/Detail Components

To make a child component's property available for binding by the external (parent) component, you must use the @Input decorator:

```
@Input hero: Hero; // In the child component
```

Bind the parent component's "selectedHero" property to the child component's "hero" property using property binding:

```
<app-hero-detail [hero]="selectedHero"></app-hero-detail> // In parent component template
```

The child component will receive a hero object through its hero property and display it. This is a one-way data binding from the selectedHero property of the HeroesComponent to the hero property of the target element, which maps to the hero property of the HeroDetailComponent.

## Services

Components shouldn't fetch or save data directly, they should focus on presenting data and delegate data access to a service.

Create a new service:

```
ng generate service service-name
```

### @Injectable Services

The @Injectable decorator marks the class as one that participates in the "dependency injection system." It accepts a metadata object for the service the same way the @Component decorator does.

In the service we will create a method to retrieve data. This can be from a web service, local storage, or a mock data source:

```
import { Hero } from './hero';
import { HEROES } from './mock-heroes'; // Mock data source

getHeroes(): Hero[] {
    return HEROES;
}
```

Import the service and inject it into any component that you want to be able to use the service:

```
import { HeroService } from '../hero.service';

constructor(private heroService: HeroService) { }
```

The parameter simultaneously defines a private heroService property and identifies it as a HeroService injection site. When Angular creates this component, the Dependency Injection system sets the heroService parameter to the singleton instance of the service.

Now we can create a function in the component to retrieve the heroes from the service:

```
getHeroes(): void {
    this.heroes = this.heroService.getHeroes();
}
```

Now call the function in ngOnInit()

### Observables

HeroService.getHeroes() must have an asynchronous signature of some kind. In order to do this, we use the AngularHttpClient.get method to fetch data. HttpClient.get() returns an Observable, which is one of the key classes in the RxJS library. The syntax looks like this:

```
getHeroes(): Observable<Hero[]> {
    return HttpClient.get<Hero[]>()
}
```

The method used to return a Hero[], now it returns an Observable<Hero[]>. To account for this, in your component's method:

```
getHeroes(): void {
    this.heroService.getHeroes()
        .subscribe(heroes => this.heroes = heroes); // Subscribe is similar to .then()
}
```

The new version waits for the Observable to emit the array of heroes—which could happen now or several minutes from now. The subscribe() method passes the emitted array to the callback, which sets the component's heroes property.

### Service-in-Service

In this project, we are creating a message component that displays app messages at the bottom of the screen. We create an injectable, app-wide message service for sending the messages to be displayed, and we will be injecting this service into the hero service to demonstrate a common service-in-service scenario.

In the hero service constructor:

```
constructor(private messageService: MessageService) { }
```

Since we will be binding the messageService property to the Message Component, we must inject it using the public keyword:

```
constructor(public messageService: MessageService) { } // In the MessagesComponent class
```

## Routing

### Add the AppRoutingModule

In Angular, the best practice is to load and configure the router in a separate, top-level module that is dedicated to routing and imported by the root AppModule. By convention, the module class name is AppRoutingModule and it belongs in the app-routing.module.ts in the src/app folder.

```
ng generate module app-routing --flat --module=app
```

--flat puts the file in src/app instead of its own folder
--module=app tells the CLI to register it in the imports array of the AppModule

Configure your routes in app-routing.module.ts:

```
const routes: Routes = [
    { path: 'heroes', component: HeroesComponent }
]
```

Routes tell the Router which view to display when a user clicks a link or pastes a URL. A typical Angular Route has two properties:

- path: a string that matches the URL in the browser address bar.
- component: the component that the router should create when navigating to this route.

This tells the router to match that URL to path: 'heroes' and display the HeroesComponent when the URL is localhost:4200/heroes.

### Add RouterOutlet

Replace the <app-heroes> element with <router-outlet>. This tells the router where to display routed views.

### Add a Navigation Link (routerLink)

```
<a routerLink="/heroes">Heroes</a>
```

### Add a Default Route

```
{ path: '', redirectTo: '/dashboard', pathMatch: 'full'}
```

### Add a Parameterized Route

```
{ path: 'detail/:id', component: HeroDetailComponent }
```

The colon (:) in the path indicates the :id is a placeholder for a specific hero id.

routerLink via the parameterized route:

```
<a *ngFor="let hero of heroes" class="col-1-4"
    routerLink="/detail/{{hero.id}}">
  <div class="module hero">
    <h4>{{hero.name}}</h4>
  </div>
</a>
```

The Angular interpolation binding within the \*ngFor repeater inserts the current iteration's hero.id into each routerLink.

### Routable Component

The HeroDetailComponent needs a way to obtain the hero-to-display. Import:

```
import { ActivatedRoute } from '@angular/router';
import { Location } from '@angular/common';

import { HeroService }  from '../hero.service';
```

Inject the ActivatedRoute, HeroService, and Location services into the constructor:

```
constructor(
  private route: ActivatedRoute,
  private heroService: HeroService,
  private location: Location
) {}
```

- The ActivatedRoute holds information about the route to this instance of the HeroDetailComponent. This component is interested in the route's parameters extracted from the URL. The 'id' parameter is the id of the hero to display.
- The HeroService gets hero data from the remote server and this component will use it to get the hero-to-display.
- The location is an Angular service for interacting with the browser. You can use it to navigate back to the view that navigated here.

Extract the id route parameter:

```
getHero(): void {
    const id = +this.route.snapshot.paramMap.get('id');
    this.heroService.getHero(id)
        .subscribe(hero => this.hero = hero);
}
```

The route.snapshot is a static image of the route information shortly after the component was created.

The paramMap is a dictionary of route parameter values extracted form the URL. The 'id' key returns the id of the hero to fetch.

Route parameters are always strings. The JavaScript (+) operator coverts the string to a number, which is what a hero id should be.

### Location service

```
goBack(): void {
    this.location.back();
}
```

### Enable HTTP Services

HttpClient is Angular's mechanism for communicating with a remote server over HTTP. Make HttpClient available everywhere in the app in two steps. In AppModule:

```
import { HttpClientModule } form '@angular/common/http';
```

Then add HttpClient to the import's array:

```
@NgModule({
  imports: [
    HttpClientModule,
  ],
})
```

### Review in-memory-data.service.ts
