**Use the ngIf, else, then syntax**

- `<div *ngIf="someConditionVar; then templateTrueCondition, else templateFalceCondition" `
- ngIf has two arguments  `then` and `else`. `then` is template will be show if condition equals true. 'else' is
  template will be show if condition equals false.

```ts
var someConditionVar = true
```

```angular2html

<ng-container *ngIf="someConditionVar;  then templateTrueCondition; else templateFalseCondition"></ng-container>

<ng-template #templateTrueCondition>
  <div> condition is true</div>
</ng-template>

<ng-template #templateFalseCondition>
  <div> condition is false</div>
</ng-template>
```

**ngIf Syntax with <ng-template>**

- *ngIf - structural directive
- [ngIf] - attribute directive
- Use asterisk (*) with `structural directives` and square brackets [] with `attribute directives`.
- `Structural directives` ( think ngFor and ngIf ) involve changing the DOM’s structure. They are essentially adding or
  removing HTML elements. Typically, this is achieved by using `TemplateRef or ViewContainerRef`.
- `Attribute directives` ( think ngClass and ngStyle ) attribute directives change the appearance and behavior of
  elements.

> `Pass multiple input values to the directive`
> Attribute directives`

```angular2html

<div [myCustomDir]="true" [isValid]="false"></div>
```

```ts
@Directive({selector: '[myCustomDir]'})
export class MyCustomDirective {

  @Input() myCustomDir: boolean;

  @Input() isValid: boolean;

}

```

> Structural directives

```angular2html

<div *myDir="'hi!';greet:'good morning'"></div>
```

```ts
@Directive({selector: '[myDir]'})
export class MyDirective {

  @Input() set myDir(text: string) {
    console.log("text:", text);
  }

  @Input() set myDirGreet(greet: string) {
    console.log(greet);
  }

  /*structural directives has access to TemplateRef and ViewContainerRef*/
  constructor(
    private templateRef: TemplateRef<any>,
    private viewContainer: ViewContainerRef) {
  }
}

```

- https://weihungchin.medium.com/asterisk-and-square-brackets-on-angular-directives-when-to-use-them-ecf44e0eb393#:~:text=Square%20brackets%20%5B%5D%20are%20typically,the%20template%20using%20microsyntax%20string
- Notice that for the first @Input bindings, the input name is the same as the directive selector name, `myDir`. For
  subsequent inputs, a prefix of the selector’s name is used, e.g. `myDirGreet`.
- https://angular.io/guide/structural-directives#microsyntax
- In the template, multiple values are passed to the directive in a string-like syntax (known as
  the [microsyntax](https://angular.io/guide/structural-directives#microsyntax)). Each
  value after the first one is separated by a semi-colon and follows by a [key]:[value] pair. With reference to the
  example above, the [key] corresponds to greet and the [value] is the text string 'good morning’ . This allows Angular
  to bind it to our myDirGreet @Input in the .ts file.

> how to use structural directives with ng-container and ng-template

```angular2html

<ng-container *ngIf="true"></ng-container><!--create wrapper as ng-template because it's structural directives-->
<ng-template
  [ngIf]="true"></ng-template> <!--not necessary to wrap ng-template to ng-template because That's what it is ng-template and we just use like attribute directives-->

```

> what's different between `structural` and `attribute` directives

- `Structural` has access to TemplateRef and ViewContainerRef, `Attribute` now always has access to TemplateRef and
  ViewContainerRef
- `Structural` can use only one directive to a tag html because structural directive automatically create wrapper as
  tag `ng-template` over the tag html, so if you use multiple structural directives to a tag html it' mean you create
  multiple wrap `ng-template` over the tag html
- `Atttibute` can use multiple different directive to a tag html

**access ngFor index, odd and even variables**

- inside ***ngFor** https://v2.angular.io/docs/ts/latest/guide/structural-directives.html#!#ngFor
- full example `microsyntax` of angular structural directives with separate scope variables

```angular2html

<div *appCustomMicroSyntaxStructural="let item of items; let i = index; let odd = odd; let even = even">
  Index: {{ i }}, Value: {{ item }}
  <span *ngIf="odd"> (Odd) </span>
  <span *ngIf="even"> (Even) </span>
</div>
```

```ts
import { Directive, Input, TemplateRef, ViewContainerRef } from '@angular/core';

interface MicroSyntaxContext {
  $implicit: any;
  index: number;
  odd: boolean;
  even: boolean;
}

@Directive({
  selector: '[appCustomMicroSyntaxStructural]'
})
export class CustomMicroSyntaxStructuralDirective {
  constructor(
    private templateRef: TemplateRef<MicroSyntaxContext>,
    private viewContainerRef: ViewContainerRef
  ) {
  }

  @Input() set appCustomMicroSyntaxStructuralOf(collection: any[]) {
    this.viewContainerRef.clear();

    for (let i = 0; i < collection.length; i++) {
      const context: MicroSyntaxContext = {
        $implicit: collection[i],
        index: i,
        odd: i % 2 !== 0,
        even: i % 2 === 0
      };


      this.viewContainerRef.createEmbeddedView(this.templateRef, context);
    }
  }
}
```

**advanced ngFor syntax with <ng-template>**

- the same things but different syntax

```angular2html

<ng-container *ngFor="let item of items; trackBy:trackById; let i = index"></ng-container>

<ng-tempalte ngFor [ngForOf]="items" let-item let-i="index"></ng-tempalte>
```

**advanced ngSwitch syntax with ng-template**

- the same things but different syntax

```angular2html

<ng-container [ngSwitch]="item">
  <span *ngSwitchCase="'new'">NEW</span>
  <span *ngSwitchCase="'limited'">LIMITED</span>
</ng-container>

<ng-container [ngSwitch]="item">
  <span>
    <ng-template [ngSwitchCase]="'new'">NEW</ng-template>
    <ng-template [ngSwitchCase]="'limited'">LIMITED</ng-template>
  </span>
</ng-container>

```


