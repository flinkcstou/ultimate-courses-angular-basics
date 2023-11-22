**create nested child routes**

- `Route` has a properties`
    - children
    - title
    - path
    - children
    - resolve
    - data
- `Routes.children` is Route[]

**redirect routes to new paths**

- `Route` has a property `path`, `pathMatch`, `redirectTo`
- Ты должен указывать `redirecTo` вместе с `path and pathMatch` и указываешь это только на тот уровень на котором
  расположен, то есть если это рутовые `Route` то только можешь указывать к другим рутовым `Route`. А Если хочешь во
  вложенном указать вложенное то нужно на каждом уровне это прописывать на какой redirect должен пойти. То есть
  вложенном указываешь куда нужно перенаправлять и если дальше есть вложенное то и в этом вложенном указываешь куда
  нужно перенаправлять
- `pathMatch` имеет два значения `prefix` and `full`

**lazy-loading feature modules**

- loadChildren дает возможность оптимизировать бандл билда и первый main.js будет весить намного меньше а уже потом если
  мы переходим по пути где указан loadChildren то будет скачиваться второй js file где будет исходит от loadChildren.

**dynamic route params and activatedRoute**

- ActivatedRoute всегда создается новый для каждого route а вот Router всегда глобальный
- Что такое dynamic route params и как

**programmatic navigation with Router.navigate()**

Посмотреть как будет работать если создать два routes с разным path но один и тот же компоненту принимает в свойстве
Component 
