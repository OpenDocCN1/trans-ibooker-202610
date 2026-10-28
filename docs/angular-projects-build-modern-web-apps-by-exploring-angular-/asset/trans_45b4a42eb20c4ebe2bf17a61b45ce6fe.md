

如果我们运行 `ng serve` 来预览应用，应该会在页面左侧看到个人信息面板：

![](img/efea53d01337e7b223389b38dcdb0160_224_0.png)

图 7.3 – 个人信息

我们作品集应用的第一个功能现已完成。它显示了 GitHub 个人资料中的个人信息，附带简短简介和一些社交网络链接。在下一节中，我们将构建应用的仓库功能。

## 列出用户仓库

GitHub 用户资料包含用户拥有的仓库列表，称为**源仓库**（**sources**），以及用户贡献过的仓库，称为**分叉仓库**（**forks**）。我们应用的仓库功能将仅显示源仓库。

仓库和组织功能将具有类似的用户界面。因此，我们需要创建一个可用于这两个功能的组件：

- 1. 执行以下 Angular CLI 命令创建一个新组件：

```bash
ng generate component panel
```

- 2. 打开 `panel.component.ts` 文件，使用 `@Input` 装饰器定义两个输入属性：

```typescript
import { Component, Input, OnInit } from '@angular/core';

@Component({
  selector: 'app-panel',
  templateUrl: './panel.component.html',
  styleUrls: ['./panel.component.scss']
})
export class PanelComponent implements OnInit {

  @Input() caption: string = '';
  @Input() icon: string = '';

  constructor() { }

  ngOnInit(): void {
  }

}
```

- 3. 打开 `panel.component.html` 文件，将其内容替换为以下 HTML 模板：

```html
<div class="card mb-4">
  <div class="card-header">
    <i class="bi bi-{{icon}} me-1"></i>
```

```typescript
constructor(private http: HttpClient) {
  this.userUrl = `${environment.apiUrl}/users/${environment.username}`;
}

getUser(): Observable<User> {
  return this.http.get<User>(this.userUrl);
}
}
```

5. 创建一个新方法，用于获取当前 GitHub 用户的仓库：

```typescript
getRepos(): Observable<Repository[]> {
  return this.http.get<Repository[]>(this.userUrl +
  '/repos');
}
```

现在我们已经完成了从 GitHub API 获取用户仓库的准备工作，可以开始构建显示这些仓库的组件了：

1. 执行以下命令，使用 Angular CLI 创建一个新的 Angular 组件：

```bash
ng generate component repositories
```

2. 打开 `repositories.component.ts` 文件，添加以下导入语句：

```typescript
import { Observable } from 'rxjs';
import { map } from 'rxjs/operators';
import { GithubService } from '../github.service';
import { Repository } from '../repository';
```

3. 将 `GithubService` 注入 `RepositoriesComponent` 类的构造函数中，并创建一个组件属性来获取 `getRepos` 方法的结果：

```typescript
export class RepositoriesComponent implements OnInit {

  repos$: Observable<Repository[]> | undefined;

  constructor(private githubService: GithubService) {}
```

```typescript
ngOnInit(): void {
  this.repos$ = this.githubService.getRepos().pipe(
    map(repos => repos.filter(repo => !repo.fork))
  );
}
}
```

我们使用 `pipe` RxJS 操作符将 `getRepos` 方法返回的可观察对象与 `map` 操作符组合，以过滤掉分叉仓库，仅保留源仓库。过滤操作通过数组的标准 `filter` 方法完成。

4. 打开 `repositories.component.html` 文件，将其内容替换为以下 HTML 模板：

```html
<app-panel caption="Repositories" icon="archive">
  <div class="row row-cols-1 row-cols-md-3 g-4">
    <div class="col p-2" *ngFor="let repo of repos$ | async">
      <div class="card h-100">
        <div class="card-body">
          <h5 class="card-title">
            <a [href]="repo.html_url">{{repo.name}}</a>
          </h5>
          <p class="card-text">{{repo.description}}</p>
        </div>
      </div>
    </div>
  </div>
</app-panel>
```

在上述模板中，我们将组件的主要内容包装在 `app-panel` 组件内，并设置标题和图标属性。

7. 运行 `ng serve` 预览应用，你应该会在个人信息功能旁边看到新的面板：

![](img/efea53d01337e7b223389b38dcdb0160_230_0.png)

图 7.4 – 仓库

我们应用的第二个功能已完成。它显示了 GitHub 个人资料中的公共仓库列表。我们的应用现在还有一个面板组件，我们可以在下一节中使用它来构建应用的组织功能。

## 可视化组织成员资格

GitHub 用户可以是某个 GitHub 组织的成员。我们的应用将显示用户组织列表，并附带每个组织的一些附加信息。让我们开始构建组织列表：

- 1. 创建一个接口来定义组织的属性：

```bash
ng generate interface organization
```

- 2. 打开 `organization.ts` 文件，添加以下属性：

```typescript
export interface Organization {
  login: string;
  description: string;
  avatar_url: string;
}
```

- 3. 打开 `github.service.ts` 文件，导入 `Organization` 接口：

```typescript
import { Organization } from './organization';
```

- 4. 创建一个新方法，用于获取当前 GitHub 用户的组织：

```typescript
getOrganizations(): Observable<Organization[]> {
  return this.http.get<Organization[]>(this.userUrl +
    '/orgs');
}
```

- 5. 执行以下命令，为我们的功能创建一个 Angular 组件：

```bash
ng generate component organizations
```

- 6. 打开 `organizations.component.ts` 文件，添加以下导入语句：

```typescript
import { Observable } from 'rxjs';
import { GithubService } from '../github.service';
import { Organization } from '../organization';
```

- 7. 将 `GithubService` 注入 `OrganizationsComponent` 类的构造函数中，并将其 `getOrganizations` 方法的结果设置为一个可观察的组件属性：

```typescript
export class OrganizationsComponent implements OnInit {

  orgs$: Observable<Organization[]> | undefined;

  constructor(private githubService: GithubService) { }

  ngOnInit(): void {
    this.orgs$ =
      this.githubService.getOrganizations();
  }

}
```

8. 打开 `organizations.component.html` 文件，将其内容替换为以下 HTML 模板：

```html
<app-panel caption="Organizations" icon="diagram-3">
  <div class="list-group">
    <a href="https://www.github.com/{{org.login}}"
      class="list-group-item list-group-item-action"
      *ngFor="let org of orgs$ | async">
      <div class="row">
        <img [src]="org.avatar_url">
        <div class="col-sm-9">
          <div class="d-flex w-100 justify-content-between">
            <h5 class="mb-1">{{org.login}}</h5>
          </div>
          <p class="mb-1">{{org.description}}</p>
        </div>
      </div>
    </a>
  </div>
</app-panel>
```

在上述 HTML 模板中，我们将组件的主要内容放在 `app-panel` 组件内，并传入适当的标题和图标。我们显示每个组织的名称和描述。每个组织都被包装在一个指向该组织 GitHub 页面的锚点元素中。

9. 打开 `organizations.component.scss` 文件，为组织徽标添加以下 CSS 样式：

```css
img {
  width: 60px;
  height: 40px;
}
```

10. 打开 `app.component.html` 文件，在第二个带有 `col-12 col-sm-12` 类选择器的元素中添加 `OrganizationsComponent` 的选择器：

```html
<div class="col-sm-9">
  <div class="row">
    <div class="col-12 col-sm-12">
      <app-repositories></app-repositories>
    </div>
  </div>
  <div class="row">
    <div class="col-12 col-sm-12">
      <app-organizations></app-organizations>
    </div>
  </div>
</div>
```

11. 运行 `ng serve` 启动应用，你应该会在仓库功能下方看到组织列表：

![](img/efea53d01337e7b223389b38dcdb0160_233_0.png)

图 7.5 – 组织

