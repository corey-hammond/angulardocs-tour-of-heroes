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
