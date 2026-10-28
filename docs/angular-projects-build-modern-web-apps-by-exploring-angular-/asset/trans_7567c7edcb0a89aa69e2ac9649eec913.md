
```typescript
ngOnInit(): void {
    }


}
```

`cards` 属性将在稍后从 Angular 应用中使用我们想要显示的卡片数据进行设置。

6. 打开 `card-list.component.html` 文件，将其内容替换为以下 HTML 模板：

```html
<div>
  <div class="card m-4" *ngFor="let card of cards">
    <header class="card-header">
      <p class="card-header-title">{{card.title}}</p>
    </header>
    <div class="card-content">
      <div class="content">{{card.description}}</div>
    </div>
    <footer class="card-footer">
      <a [href]="card.link" class=
        "card-footer-item">View on Wikipedia</a>
    </footer>
  </div>
</div>
```

前面的模板使用了 Bulma 库的卡片组件，并遍历 `cards` 组件属性来显示每一张卡片，使用的是 `*ngFor` 指令。

7. 打开 `card-list.component.css` 文件，添加以下 CSS 样式：

```css
:host > div {
  display: grid;
  grid-auto-flow: column;
  overflow: auto;
}

.card {
  width: 200px;
}
```

在前面的样式中，我们使用 `:host` 选择器来定位组件宿主元素中的 `div` 元素，并应用 **CSS Grid** 样式将所有卡片显示在一行中。

8. 打开 `ui-controls.module.ts` 文件，将 CommonModule 添加到 `@NgModule` 装饰器的 imports 数组中，如下所示：

```typescript
import { NgModule } from '@angular/core';
import { UiControlsComponent } from './ui-controls.component';
import { CardListComponent } from './card-list/card-list.component';
import { CommonModule } from '@angular/common';

@NgModule({
  declarations: [
    UiControlsComponent,
    CardListComponent
  ],
  imports: [CommonModule],
  exports: [
    UiControlsComponent,
    CardListComponent
  ]
})
export class UiControlsModule { }
```

在卡片列表组件模板中使用的 *ngFor 指令需要 CommonModule 类。

我们的组件现在已经准备好接收数据并以卡片列表的形式显示它。让我们看看如何从 Angular 应用中使用它，如下所示：

1. 首先，执行以下命令来构建组件 UI 库：

```
ng build ui-controls
```

Angular CLI 将开始构建该库，一旦你在终端中看到以下输出，构建就已完成：

![](img/efea53d01337e7b223389b38dcdb0160_298_0.png)

图 9.1 – 库构建输出

2. 打开 `app.module.ts` 文件，将 `UiControlsModule` 类添加到 `@NgModule` 装饰器的 `imports` 数组中，如下所示：

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { UiControlsModule } from 'ui-controls';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    UiControlsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

> **重要说明**
> 我们从 ui-controls 命名空间（即库名）导入 UiControlsModule，而不是从工作区中的完整绝对路径导入。

3. 打开 app.component.ts 文件，声明一个 Card[] 类型的组件属性，如下所示：

```typescript
import { Component } from '@angular/core';
import { Card } from 'ui-controls';
import { assassins } from './assassins';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'my-components';
  cards: Card[] = assassins;
}
```

我们使用 assassins.ts 文件中的演示数据来初始化 cards 组件属性，你可以在入门部分的 GitHub 仓库中找到该文件。

4. 打开 app.component.html 文件，将其内容替换为以下 HTML 模板：

```html
<div class="container is-fluid">
  <h1 class="title">Assassins Creed Series</h1>
  <lib-card-list [cards]="cards"></lib-card-list>
</div>
```

5. 要预览应用，运行 `ng serve` 并在浏览器中打开 `http://localhost:4200`。你应该会看到类似以下的内容：

### 刺客信条系列

![](img/efea53d01337e7b223389b38dcdb0160_300_0.png)

图 9.2 – 卡片列表组件

卡片列表组件显示消费应用通过 `cards` 输入属性传递的数据。在下一节中，我们将进一步改进我们的组件，使我们的卡片能够更改它们在列表中的位置。

### 添加拖放功能

卡片列表组件的一个功能是，我们将能够通过在列表内拖放卡片来更改卡片的位置。卡片列表的顺序应使用输出属性绑定发送回消费应用。

Angular CDK 包含一个拖放模块，我们可以用于此目的。为此，请按照以下步骤操作：

1. 打开 `ui-controls.module.ts` 文件，从 `@angular/cdk/drag-drop` 命名空间导入 `DragDropModule`，如下所示：

```typescript
import { DragDropModule } from '@angular/cdk/drag-drop';
```

2. 将 `DragDropModule` 类添加到 `@NgModule` 装饰器的 `imports` 数组中，如下所示：

```typescript
@NgModule({
  declarations: [
    UiControlsComponent,
    CardListComponent
  ],
  imports: [
    CommonModule,
    **DragDropModule**
  ],
  exports: [
    UiControlsComponent,
    CardListComponent
  ]
})
```

3. 打开 `card-list.component.html` 文件并修改模板，如下所示：

```html
<div cdkDropListOrientation="horizontal" cdkDropList
(cdkDropListDropped)="sortCards($event)">
  <div cdkDrag class="card m-4" *ngFor="let card of
    cards">
    <header class="card-header">
      <p class="card-header-title">{{card.title}}</p>
    </header>
    <div class="card-content">
      <div class="content">{{card.description}}</div>
    </div>
    <footer class="card-footer">
      <a [href]="card.link" class=
        "card-footer-item">View on Wikipedia</a>
    </footer>
  </div>
</div>
```

首先，我们为每个卡片元素添加 `cdkDrag` 指令，以便能够通过拖动来移动它。然后，我们为容器元素添加 `cdkDropList` 指令，将其标记为放置列表。Angular CDK 中的放置列表表示其内容可以使用拖放操作重新排序。我们将拖放方向设置为 `horizontal`，因为我们的卡片列表渲染在单行中，并且我们还将 `sortCards` 组件方法绑定到放置列表的 `cdkDropListDropped` 事件。

4. 打开 card-list.component.ts 文件并添加以下导入语句：

```typescript
import { Component, Input, Output, OnInit, EventEmitter } from '@angular/core';
import { Card } from '../card';
import { CdkDragDrop, moveItemInArray } from '@angular/cdk/drag-drop';
```

5. 使用 @Output 装饰器创建输出属性，并在 sortCards 组件方法中使用它将重新排序后的列表发送给组件的消费者，如下所示：

```typescript
export class CardListComponent implements OnInit {
  @Input() cards: Card[] = [];
  @Output() cardChange = new EventEmitter<Card[]>();

  constructor() { }

  ngOnInit(): void {
  }

  sortCards(event: CdkDragDrop<string[]>): void {
    moveItemInArray(this.cards, event.previousIndex, event.currentIndex);
    this.cardChange.emit(this.cards);
  }
}
```

在前面的代码片段中，我们使用 DragDropModule 的内置方法 moveItemInArray 来更改 cards 属性的顺序。我们将包含移动卡片的先前索引和当前索引的 event 参数传递给 moveItemInArray 方法。我们还使用 cardChange 属性的 emit 方法将更改传播回 Angular 应用。

卡片列表组件现在获得了拖放超级能力。让我们试一试，如下所示：

1. 打开 app.component.html 文件，为 lib-card-list 组件的 cardChange 事件添加事件绑定，如下所示：

```html
<div class="container is-fluid">
<h1 class="title">Assassins Creed Series</h1>
<lib-card-list [cards]="cards"
(cardChange)="onCardChange($event)">
</lib-card-list>
</div>
```

2. 打开 app.component.ts 文件并创建一个 onCardChange 方法来记录新的卡片列表，如下所示：

```typescript
onCardChange(cards: Card[]) {
  console.log(cards);
}
```

3. 运行以下命令来构建库：

```
ng build ui-controls
```

4. 执行 Angular CLI 的 serve 命令来启动你的应用，如下所示：

```
ng serve
```

5. 尝试从列表中拖放一些卡片，并注意浏览器控制台窗口中的输出。

我们 UI 库的第一个组件现在已具备使其成为拖放列表的所有功能。它可以以 Bulma 卡片格式显示从我们的 Angular 应用传递的列表。它还可以使用 Angular CDK 拖放模块更改列表中每个项目的顺序，并将更改传播回我们的应用。

在下一节中，我们将创建库的第二个组件，用于将数据复制到剪贴板。

