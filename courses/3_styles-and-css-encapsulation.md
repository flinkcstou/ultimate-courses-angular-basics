**Style a host Element**

- style a host element
    - used by `:host { }` in scss component for tag

**View Encapsulation: Shadow DOM and Emulated**

- `shadow dom` works like native shadow dom
- `emulated` it's custom logic by angular framework

**Set inline styles with style bindings**

- inline styles - `<div style="border:none" >`
- bindings styles - <div [styles.border]="'none'" >`

**Complex inline Styles with NgStyle**

-complex inline styles with NgStyle = `<div [ngStyle]="{border:'none'}"" >`

**Conclusion**

- `inline styles` accepts only string separated by a semicolon `key:value; key1:value1`

```ts
var inlineStyles = -`border:none; width:100px`;

var template = `<div [style]="inlineStyles" >`

```

- `binding styles` accepts only value;

```ts

var bindingStyles = "none";

var template = `<div [style.border]="bindigStyles" >`

```

- `complex inline styles with ngStyle` accepts object
- `complex inline styles with ngStyle` if accepts object key is property of style and value is property value of style

```ts

var complexInlineStyles = {
  border: 'none',
  'background-color': 'blue'
};

var template = `<div [ngStyle]="complexInlineStyles" >`
```

**Set classes with class bindings**

- bindings styles - `<div  [class.promo]="true">`
- bindings styles accepts value as boolean if value equals true then class will be established if not then will not be
  show in template
- You can create mulitple bindings styles in one tag html. Not only one value of class

**Complex classes with ngClass**

- complex classes = `<div [ngClass]="{promo:true, container:false}" >`

**Conclusion**

- `binding class` - accepts only value as a boolean or any other type and it will be converted to boolean

```ts
var bindingClass = true;

var template = `<div [class.promo]="bindingClass"> `

```

- `complex classes with ngClass` accepts string, array of string and object
- `complex classes with ngClass` if accepts object then value will be boolean or any type then value will be converted
  to
  boolean. Key is name of class
-

```ts

var complexClassesNgClass = {'promo': true, 'not-promo': false, 'new-promo': 'some-string'}

var tempalte = `<div [ngClass]="complexClassesNgClass" >`

```
