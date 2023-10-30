- interpolation - интерполяция - `{{someVariables}}`
- expressions - выражения
- binding - привязка
- property - свойство
- property binding - привязка свойств
- property binding syntax - синтаксис привязка свойств
- event binding syntax - синтаксис привязка событий
- event - событие
- template - шаблон
- ref- ссылка
- template ref variables - переменные ссылающий на шаблон

**property binding syntax**

- В Angular-e property binding работает через `native directive` который для каждого @input создает свой `js Proxy` и
  приравнивает свойства и значения к `dom element`
- `<input [value]="someVariable">`. It's property binding `[value]="someVariable"`

```ts
@Directive({
  selector: 'for any dom element'
})
export class NativeDirective {

  constructor(
    private renderer2: Renderer2
  ) {
  }

  @Input()
  @GetKeyAndValue()
  setproxy(value: any, property: any) {
    this.renderer2.setProperty(property, value)
  }

  // or 
  ngOnChanges(changes: SimpleChanges): void {
    for (let key in changes) {
      this.renderer2.setProperty(key, changes[key])
    }
  }
}

```

**event binding syntax**

- Нужно указать в component-e `@Output() someEvent:EventEmitter<any> = new EventEmitter<any>()` а в
  template `(someEvent)=onSomeEvent($event)`

```ts
import { EventEmitter } from '@angular/core';

@Component({
  template: 'some-component'
})
export class SomeCompnent {
  @Output() someEvent: EventEmitter<any> = new EventEmitter()
}

export const ParentTempalte = `<some-component (someEvent)="callSomeMethod()"></some-component>`
```

- ЭТо внутренняя кастомная реализация event binding у Angular-a. Внутри Angular/core лежит реализация
- Он реализован с помощью  `native Directive` где сам связывает template c нативными событиями а также создает кастомные
  события
- Event binding

```ts
import { Renderer2 } from '@angular/core';

@Directive({
  selecctor: 'fro any dom element'
})
export class NativeDirective implements AfterViewInit {

  constructor(
    private elementRef: ElementRef,
    private renderer: Renderer2
  ) {
  }


  ngAfterViewInit() {
    this.findElementsWithEventBindings(this.elementRef.nativeElement)
  }

  findElementsWithEventBindings(element: HTMLElement) {
    const elementsWithEventBindings = element.querySelectorAll('[*|*]');

    elementsWithEventBindings.forEach((el: HTMLElement) => {
      const eventAttribute = el.getAttribute('*|*');
      const propertyName = eventAttribute.slice(1, -1); // Remove parentheses
      this.triggerEvent(el, propertyName);
    });
  }

  triggerEvent(element: HTMLElement, eventName: string) {
    // Create a new event
    const event = new Event(eventName);

    // Trigger the event on the element
    this.renderer.dispatchEvent(element, event);
  }


}

```

**Template ref Variables**

- синтаксис - `<input #someRefVariables>` `#someRefVariables`
- на любой tag можем повесить `#someRefVariables` и иметь к этому доступ
- для каждого `tag` в `Angular-e` создается класс нативная директива которая обернута вокруг html tag
- внутри нативной директивы хранятся все свойства которые относятся в `html tag`
- если мы создаем свой component, то он и так уже обернут в директиву которая называется `@Component`
- `@Component` это и есть `@Directive` только Component экстендиться от Directive и имеет одну особенность то что можно
  давать наименования кастомногу тэгу.
- также на один tag можно вешать множество кастомных директив, Но по дефолту уже на любой tag навешан нативная directive
- Можно весь template html представить как набор объектов к которым можем обращаться через `Template ref variables` и
  иметь доступ к всем свойствам этого шаблона которые описаны у Директив и компоненты
- ПО дефолту когда создаем `Template ref variables` он берет либо нативную директиву у `нативных html tag` элементов
  либо берет нашу компоненту `@Component`. А если мы хотим обращаться к другим кастомным директивам то мы должны указать
  в аннотации `@Directive` и у `@Component` свойства `exportAs`. `exportAs` создан специально, чтоб обращаться к
  определенным кастомным директивам. Так как у одно тэга может быть еще множество кастомных директив не считая нативной
  диркетивы, которая создается автоматом под капотом

```html

<div <!--create decorator directive for div divDirective-->
#divrefDirective <!--get ref for divDirective-->
>
<div <!--create decorator directive for div divDirective-->
#divrefDirective2 <!--get ref for divDirective-->
>
<input #inputRefDirective value="alloha">
<div>{{inputRefDirective.value<!--we will see value `alloha` in this place--> }}</div>
</div>
</div>

```


