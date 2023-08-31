# Angular Json Editor

## About this repository
This is a fork of mariohmol's [ang-jsoneditor](https://github.com/mariohmol/ang-jsoneditor)
with support for Angular 11, 12, 13 and 14.
This repository will probably become stale,
when the original will be actively maintained again.

## About the project

Angular wrapper for [jsoneditor](https://github.com/josdejong/jsoneditor)).
A library with that you can view and edit json content interactively.

![Demo Image](projects/docs/images/demo.png)

## Installation

### 1. Install "jsoneditor"

The wrapped library is not packaged in this library.  
You have to install it via
`npm i jsoneditor@9.7`

### 2. Install this wrapper library

To install this library with npm, run one of the command below:

| Compatibility | Command                                 | Stability |
|---------------|-----------------------------------------|-----------|
| Angular 11    | `npm install @maaxgr/ang-jsoneditor@11` | Stable    |
| Angular 12    | `npm install @maaxgr/ang-jsoneditor@12` | Stable    |
| Angular 13    | `npm install @maaxgr/ang-jsoneditor@13` | Stable    |
| Angular 14    | `npm install @maaxgr/ang-jsoneditor@14` | Stable    |

**WARNING:** Although versions are marked as stable,
there can be still bugs because this project isn't heavily integrated in a lot of production projects

## Usage

### Minimal Example

First import Module in module.ts:

```ts
// For Angular 11 + 12 
import { NgJsonEditorModule } from '@maaxgr/ang-jsoneditor'
// Starting Angular 13
import { AngJsoneditorModule } from '@maaxgr/ang-jsoneditor'

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    ....,
    // For Angular 11 + 12 
    NgJsonEditorModule,
    // Starting Angular 13
    AngJsoneditorModule,
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

Then setup your component models as below:
```ts
import {Component} from '@angular/core';
import {JsonEditorOptions} from "@maaxgr/ang-jsoneditor";

@Component({
  selector: 'app-root',
  template: '<json-editor [options]="editorOptions" [data]="initialData" (change)="showJson($event)"></json-editor>' +
    '<div>{{ visibleData | json }}</div>'
})
export class AppComponent {

  public editorOptions: JsonEditorOptions;
  public initialData: any;
  public visibleData: any;

  constructor() {
    this.editorOptions = new JsonEditorOptions()
    this.editorOptions.modes = ['code', 'text', 'tree', 'view'];

    this.initialData = {"products":[{"name":"car","product":[{"name":"honda","model":[{"id":"civic","name":"civic"},{"id":"accord","name":"accord"},{"id":"crv","name":"crv"},{"id":"pilot","name":"pilot"},{"id":"odyssey","name":"odyssey"}]}]}]}
    this.visibleData = this.initialData;
  }

  showJson(d: Event) {
    this.visibleData = d;
  }

}
```

Add Style to style.scss:
```js
@import "~jsoneditor/dist/jsoneditor.min.css";
```

### Access Component

For deeper configuration, add this to component.ts:
```ts
@ViewChild(JsonEditorComponent, { static: false }) editor!: JsonEditorComponent;
```

### Forms

Build it integrated with ReactiveForms:
```ts 
this.form = this.fb.group({
  myinput: [this.data]
});
```
```html
<form [formGroup]="form" (submit)="submit()">
    <json-editor [options]="editorOptions2" formControlName="myinput">
    </json-editor>
</form>
```

### Extra Features

Besides all the
[configuration options](https://github.com/josdejong/jsoneditor/blob/master/docs/api.md)
from the original jsoneditor, Angular Json Editor supports one additional option:

=> `expandAll`: to automatically expand all nodes upon json loaded with the _data_ input.

# Troubleshoot

If you have issue with the height of the component, you can try one of those solutions:

When you import CSS:

```css
@import "~jsoneditor/dist/jsoneditor.min.css";
textarea.jsoneditor-text{min-height:350px;}
```

Or Customizing the CSS:

```css
:host ::ng-deep json-editor,
:host ::ng-deep json-editor .jsoneditor,
:host ::ng-deep json-editor > div,
:host ::ng-deep json-editor jsoneditor-outer {
  height: 500px;
}
```

Or  as a inner style in component:

```html
<json-editor class="col-md-12" #editorExample style="min-height: 300px;" [options]="editorOptionsData" [data]="dataStructure"></json-editor>
```

For code view you can change the height using this example:
```css
.ace_editor.ace-jsoneditor {
  min-height: 500px;
}
```

Use debug mode to see in your console the data and options passed to jsoneditor. Copy this and paste in your issue when reporting bugs.

```html
<json-editor [debug]="true" [options]="editorOptionsData" [data]="dataStructure"></json-editor>
```

## JSONOptions missing params

If you find youself trying to use an custom option that is not mapped here, you can do:

```ts
let editorOptions: JsonEditorOptions = new JsonEditorOptions(); (<any>this.editorOptions).templates = [{menu options objects as in json editor documentation}]
```

See the [issue](https://github.com/mariohmol/ang-jsoneditor/issues/57)

## Internet Explorer

If you want to support IE, please follow this guide:
* https://github.com/mariohmol/ang-jsoneditor/issues/44#issuecomment-508650610

## Multiple editors

To use multiple jsoneditors in your view you cannot use the same editor options.

You should have something like:

```html
<div *ngFor="let prd of data.products" class="w-100-p p-24" >
  <json-editor [options]="makeOptions()" [data]="prd" (change)="showJson($event)"></json-editor>
</div>
```

```ts
makeOptions = () => {
  return new JsonEditorOptions();
}
```

# Demo

Demo component files are included in Git Project under `projects/demo`.  
An explanation how to start the demo is in the [Local Development](projects/docs/local-development.md)-Guide

# Collaborate

See [Local Development](projects/docs/local-development.md)


# License
[MIT](./LICENSE)
