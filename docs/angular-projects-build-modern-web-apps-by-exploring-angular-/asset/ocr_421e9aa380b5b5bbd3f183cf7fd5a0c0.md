```
constructor(private http: HttpClient) {
  this.userUrl = `${environment.apiUrl}/users/${environment.username}`;
}

getUser(): Observable<User> {
  return this.http.get<User>(this.userUrl);
}
}
```

5. Create a new method to fetch repositories of the current GitHub user:

```
getRepos(): Observable<Repository[]> {
  return this.http.get<Repository[]>(this.userUrl +
  '/repos');
}
```

Now that we have created the prerequisites for fetching the user repositories from the GitHub API, we can start building the component that will display those repositories:

1. Execute the following command to create a new Angular component using the Angular CLI:

```
ng generate component repositories
```

2. Open the repositories.component.ts file and add the following import statements:

```
import { Observable } from 'rxjs';
import { map } from 'rxjs/operators';
import { GithubService } from '../github.service';
import { Repository } from '../repository';
```

3. Inject GithubService into the constructor of the RepositoriesComponent class and create a component property to get the result of the getRepos method:

```
export class RepositoriesComponent implements OnInit {

  repos$: Observable<Repository[]> | undefined;

  constructor(private githubService: GithubService) {}
```