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

* The CLI generates the metadata properties in the component.ts file:
    1. selector - the component's CSS element selector
        * matches the name of the HTML element that identifies this component within a parent component's template
    2. templateURL - the location of the component's template file
    3. styleUrls - the location of the component's private CSS styles
* The ngOnInit() is a lifecycle hook. Angular calls ngOnInit() shortly after creating a component. It's a good place to put initialization logic.

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

### *ngFor

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

### Use *ngIf to Hide Empty Details

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



