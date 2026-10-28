

```typescript
@NgModule({
  imports: [
    CommonModule,
    RouterModule.forChild([
      { path: '', component: AdminComponent }
    ]),
    PoiModule,
    ChartsModule
  ],
  declarations: [
    AdminComponent
  ],
})
export class AdminModule {}
```

3. 打开 angular.json 文件并在 build 部分的 options 属性中添加以下属性：

```json
"options": {
  "outputPath": "dist/apps/tour",
  "index": "apps/tour/src/index.html",
  "main": "apps/tour/src/main.ts",
  "polyfills": "apps/tour/src/polyfills.ts",
  "tsConfig": "apps/tour/tsconfig.app.json",
  "assets": ["apps/tour/src/favicon.ico",
    "apps/tour/src/assets"],
  "styles": [
    "./node_modules/@angular/material/prebuilt-themes/deeppurple-amber.css",
    "apps/tour/src/styles.css"
  ],
  "scripts": [],
  "allowedCommonJsDependencies": ["chart.js"]
}
```

The chart.js npm package is a **CommonJS** module. The Angular CLI displays a warning when using a CommonJS module because it can prevent it from optimizing your application, resulting in large bundle sizes. Setting the allowedCommonJsDependencies option will not display the warning while building the application.

4. 打开 admin.component.ts 文件并添加以下 import 语句：

```
import { Component, OnDestroy, OnInit } from '@angular/core';
import { Store } from '@ngrx/store';
import { PoiActions, PoiEntity, PoiSelectors } from '@packt/poi';
import { Subscription } from 'rxjs';
```

5. 修改 AdminComponent 类，使其与应用程序 store 交互以获取 POI 数据：

```
export class AdminComponent implements OnInit,
  OnDestroy {

  private subscription: Subscription | undefined;

  constructor(private store: Store) { }

  ngOnInit(): void {
    this.subscription = this.store.select(
      PoiSelectors.getAllPoi).subscribe();
    this.store.dispatch(PoiActions.init());
  }

  ngOnDestroy() {
    this.subscription?.unsubscribe();
  }
}
```

在前面的代码中，我们使用 subscription 属性手动订阅 `getAllPoi` 选择器，而不是使用 async pipe。在这种情况下，我们还必须在组件的 `ngOnDestroy` 生命周期钩子中手动取消订阅，使用 unsubscribe 方法。如果我们未能这样做，我们可能会在应用程序中引入内存泄漏。

现在我们已经设置了与 store 的交互，我们可以从本地存储中获取统计数据并创建饼图：

+   1. 执行以下 Nx CLI 命令在 admin 库中创建一个服务：

```
```bash
nx generate service admin --project=admin
```
```

+   2. 打开 `admin.service.ts` 文件并添加以下 import 语句：

```
```typescript
import { PoiEntity } from '@packt/poi';
```
```

+   3. 创建一个方法来从浏览器本地存储中获取所有保存的流量统计数据：

```
```typescript
getStatistics(pois: PoiEntity[]): number[] {
  return pois.map(poi => {
    const stat = localStorage.getItem('tour-' +
      poi.id) ?? 0;
    return +stat;
  });
}
```
```

在前面的方法中，我们根据其 id 属性获取每个 POI 的流量。我们通过添加 `+` 前缀将 `stat` 属性转换为数字。

+   4. 打开 `admin.component.ts` 文件并添加以下 import 语句：

```
```typescript
import { Label } from 'ng2-charts';
import { AdminService } from './admin.service';
```
```

+   5. 声明用于在饼图上显示的标签和实际数据的组件属性，并在 `AdminComponent` 类的构造函数中注入 `AdminService`：

```
```typescript
data: number[] = [];
labels: Label[] = [];
```
```

```
constructor(private store: Store, private
  adminService: AdminService) { }
```

+   6. 创建一个组件方法来设置图表的标签和数据：

```
private buildChart(pois: PoiEntity[]) {
  this.labels = pois.map(poi => poi.name);
  this.data = this.adminService.getStatistics(pois);
}
```

图表标签是 POI 的标题，数据来自 `adminService` 变量的 `getStatistics` 方法。

+   7. 在 `getAllPoi` 选择器的 `subscribe` 方法内部调用 `buildChart` 方法：

```
ngOnInit(): void {
  this.subscription = this.store.select(
    PoiSelectors.getAllPoi).subscribe(pois => {
      this.buildChart(pois);
    });
  this.store.dispatch(PoiActions.init());
}
```

+   8. 最后，打开 `admin.component.html` 文件并用以下 HTML 模板替换其内容：

```
<div class="chart" *ngIf="data.length">
  <canvas height="100" baseChart
    [data]="data"
    [labels]="labels"
    chartType="pie">
  </canvas>
</div>
```

在前面的模板中，我们使用 `baseChart` 指令将 `canvas` 元素转换为图表。图表使用 `chartType` 属性设置为饼图，并使用相应的组件属性中的 `data` 和 `labels` 属性。

如果我们现在使用 Nx Console 运行应用程序，访问地图上的一些 POI，并切换到 http://localhost:4200/admin URL，我们应该会看到以下输出：

![](img/efea53d01337e7b223389b38dcdb0160_286_0.png)

Figure 8.21 – POI statistics

管理员现在可以完全了解每个 POI 在访问方面的表现。我们的管理员门户现已完成。访客门户可以与 store 交互并在浏览器的本地存储中为每个 POI 保存访问统计数据。然后管理员门户可以获取该数据并使用 ng2-charts 库在饼图上显示它。

# Summary

在本项目中，我们为在地图上访问 POI 并为每个 POI 显示访问统计数据构建了一个企业门户应用程序。首先，我们看到了如何使用 Nx Dev Tools 为新的 Nx monorepo 应用程序搭建脚手架。然后，我们为应用程序创建了两个不同的门户，一个访客门户和一个管理员门户。我们学习了如何在访客门户中使用 NgRx 库来维护和管理应用程序的状态。最后，我们看到了如何在管理员门户中使用图表库来为每个 POI 显示统计数据。

在下一章中，我们将使用 Angular CLI 使用 Angular CDK 构建一个 UI 组件库。

# Practice questions

让我们看几个练习题：

+   1. 哪个 npm 包可以创建 Nx monorepo 应用程序？
+   2. Angular CLI 和 Nx CLI 有什么区别？
+   3. 如何在 monorepo 的库中启用 NgRx？
+   4. 如何从 store 中选择数据？
+   5. 如何在 NgRx 中与 HTTP 交互？
+   6. 我们在哪里修改 NgRx store 的状态？
+   7. 根状态和特性状态有什么区别？
+   8. 我们可以在 Angular 应用程序中使用哪个 npm 包来使用 Google Maps？
+   9. 如何手动订阅 NgRx 选择器？
+   10. 我们使用哪个组件在 Google Maps 上显示附加信息？

# Further reading

以下是一些用于在本书章节中学到的内容之上的链接：

+   - Nx Dev Tools: https://nx.dev/
+   - Nx CLI: https://nx.dev/latest/angular/getting-started/cli-overview
+   - NgRx: https://ngrx.io/
+   - NgRx store guide: https://ngrx.io/guide/store
+   - Angular Material card component: https://material.angular.io/components/card/overview
+   - Angular Google Maps: https://github.com/angular/components/tree/master/src/google-maps
+   - ng2-charts: https://valor-software.com/ng2-charts/
+   - create-nx-workspace: https://www.npmjs.com/package/create-nx-workspace

# 9
# Building
a Component
UI Library Using
Angular CLI and
Angular CDK

Angular 应用程序由组织成模块的 Angular 组件组成。当组件需要在模块之间共享相似的外观或行为时，我们将它们的功能提取到可重用组件中并分组到共享模块中。可重用组件可能从具有许多控件的复杂 **user interface**（UI）结构（例如表单）到单个原生 **HyperText Markup Language (HTML)** 元素（例如按钮）不等。

组件 UI 库是一组可重用组件，可以在特定应用程序域之外使用。采用 monorepo 架构构建的大型企业应用程序可以在其所有应用程序中使用这些组件。组织外部的项目也可以将相同的组件库作为外部依赖项使用。

