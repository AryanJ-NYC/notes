# Routing

## Links

* `routerLinkActive` - class to give link when active
* `routerLinkOptions` - accepts an object of options `routerLink` - link to go to
  * if `/` in front, link is absolute
  * if nothing in front \(just `servers`, for instance\), path is relative to current path

```javascript
<ul class="nav nav-tabs">
  <li role="presentation" routerLinkActive="active" [routerLinkActiveOptions]="{exact: true}">
    <a href="#" routerLink="/">Home</a>
  </li>
  <li role="presentation" routerLinkActive="active"><a href="#" routerLink="/servers">Servers</a></li>
  <li role="presentation" routerLinkActive="active"><a href="#" routerLink="/users">Users</a></li>
</ul>
```

### Navigating Programmatically

```javascript
import { Component } from '@angular/core';
import {Router} from '@angular/router';

@Component({
  selector: 'app-home',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.css']
})
export class HomeComponent {

  constructor(private router: Router) { }

  onLoadServers() {
    // insert complex calculation here
    this.router.navigate(['servers']);
  }
}
```

### Route Parameters

```javascript
ngOnInit() {
  this.user = {
    id: this.route.snapshot.params.id, // use snapshot for initialization
    name: 'Fred',
  };

  // subscribe to changes in route parameters
  this.route.params.subscribe((params: Params) => {
    this.user.id = params.id;
  });
}
```

## Route Queries and Fragments

### Passing

Can pass params via link or programmatically:

{% tabs %}
{% tab title="html" %}
```markup
<a
  [routerLink]="['/servers', 5, 'edit']"
  [queryParams]="{ allowEdit: '1' }"
  fragment="loading"
  href="#"
  class="list-group-item"
  *ngFor="let server of servers">
  {{ server.name }}
</a>
```
{% endtab %}

{% tab title="component" %}
```typescript
  onLoadServer(id: number) {
    // complex calculation
    this.router.navigate(['servers', id, 'edit'], {
      queryParams: { allowEdit: '1' },
      fragment: 'loading',
      queryParamsHandling: 'preserve',
    });
  }
```
{% endtab %}
{% endtabs %}

* `queryParamsHandling` extra property set to `preserve` any existing query parameters we have

### Retrieving

```typescript
ngOnInit() {
  console.log(
    this.route.snapshot.queryParams,
    this.route.snapshot.fragment
  ); // ☝️ to access one time
  this.route.queryParams.subscribe();
  this.route.fragment.subscribe();
}
```

## Guards

### Authentication Guards

{% tabs %}
{% tab title="AuthGuard.service.ts" %}
```typescript
import {ActivatedRouteSnapshot, CanActivate, CanActivateChild, Router, RouterStateSnapshot} from '@angular/router';
import {Observable} from 'rxjs';
import {Injectable} from '@angular/core';
import {AuthService} from './auth.service';

@Injectable()
export class AuthGuardService implements CanActivate, CanActivateChild {
  constructor(private authService: AuthService, private router: Router) {}

  canActivate(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): Observable<boolean> | Promise<boolean> | boolean  {
    return this.authService.isAuthenticated().then((authenticated: boolean) => {
      if (authenticated) {
        return true;
      } else {
        this.router.navigate(['/']);
      }
    });
  }

  canActivateChild(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): Observable<boolean> | Promise<boolean> | boolean {
    return this.canActivate(route, state);
  }
}
```
{% endtab %}

{% tab title="app-routing.module.ts" %}
```typescript
const appRoutes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'users', component: UsersComponent, children: [
      { path: ':id', component: UserComponent },
    ] },
  { path: 'servers', canActivateChild: [ AuthGuardService ], component: ServersComponent, children: [
      { path: ':id', component: ServerComponent },
      { path: ':id/edit', component: EditServerComponent },
    ] },
  { path: '404', component: PageNotFoundComponent },
  { path: '**', redirectTo: '/404' }
];
```
{% endtab %}
{% endtabs %}

### Deactivation Guards

{% tabs %}
{% tab title="can-deactivate-guard.service.ts" %}
```typescript
import {Observable} from 'rxjs';
import {ActivatedRouteSnapshot, CanDeactivate, RouterStateSnapshot} from '@angular/router';

export interface CanComponentDeactivate {
  canDeactivate: () => Observable<boolean> | Promise<boolean> | boolean;
}

export class CanDeactivateGuardService implements CanDeactivate<CanComponentDeactivate> {
  canDeactivate(
    component: CanComponentDeactivate,
    currentRoute: ActivatedRouteSnapshot,
    currentState: RouterStateSnapshot,
    nextState?: RouterStateSnapshot
  ): Observable<boolean> | Promise<boolean> | boolean {
    return component.canDeactivate();
  }
}
```
{% endtab %}

{% tab title="app-routing.module.ts" %}
```typescript
const appRoutes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'users', component: UsersComponent, children: [
      { path: ':id', component: UserComponent },
    ] },
  { path: 'servers', canActivateChild: [ AuthGuardService ], component: ServersComponent, children: [
      { path: ':id', component: ServerComponent },
      { path: ':id/edit', component: EditServerComponent, canDeactivate: [CanDeactivateGuardService] }, // passed here (don't forget to declare in module)
    ] },
  { path: '404', component: PageNotFoundComponent },
  { path: '**', redirectTo: '/404' }
];
```
{% endtab %}

{% tab title="edit-server.component.ts" %}
```typescript
import { Component, OnInit } from '@angular/core';
import {ActivatedRoute, Params, Router} from '@angular/router';

import { ServersService } from '../servers.service';
import {CanComponentDeactivate} from './can-deactivate-guard.service';
import {Observable} from 'rxjs';

@Component({
  selector: 'app-edit-server',
  templateUrl: './edit-server.component.html',
  styleUrls: ['./edit-server.component.css']
})
export class EditServerComponent implements OnInit, CanComponentDeactivate {
  allowEdit = false;
  changesSaved = false;
  server: {id: number, name: string, status: string};
  serverName = '';
  serverStatus = '';

  constructor(
    private route: ActivatedRoute,
    private serversService: ServersService,
    private router: Router,
  ) { }

  ngOnInit() {
    this.route.queryParams.subscribe((qParams: Params) => {
      this.allowEdit = qParams.allowEdit === 'true';
    });

    this.server = this.serversService.getServer(1);
    this.serverName = this.server.name;
    this.serverStatus = this.server.status;
  }

  // implements the canDeactivate() interface here
  canDeactivate(): Observable<boolean> | Promise<boolean> | boolean {
    if (!this.allowEdit) {
      return true;
    }
    if ((this.serverName !== this.server.name || this.serverStatus !== this.server.status) && !this.changesSaved) {
      return confirm('Do you want to discard changes?');
    }
    return true;
  }

  onUpdateServer() {
    this.serversService.updateServer(this.server.id, {name: this.serverName, status: this.serverStatus});
    this.changesSaved = true;
    this.router.navigate(['../'], { relativeTo: this.route });
  }
}
```
{% endtab %}
{% endtabs %}

### Route Resolvers

{% tabs %}
{% tab title="server-resolver.service.ts" %}
```typescript
import { ActivatedRouteSnapshot, Resolve, RouterStateSnapshot } from '@angular/router';
import {Observable} from 'rxjs';
import {ServersService} from '../servers.service';
import {Injectable} from '@angular/core';

interface Server {
  id: number;
  name: string;
  status: string;
}

@Injectable()
export class ServerResolverService implements Resolve<Server> {
  constructor(private serversService: ServersService) {}

  resolve(route: ActivatedRouteSnapshot, state: RouterStateSnapshot): Observable<Server> | Promise<Server> | Server {
    return this.serversService.getServer(Number(route.params.id));
  }
}
```
{% endtab %}

{% tab title="server.component.ts" %}
```typescript
import { Component, OnInit } from '@angular/core';
import {ActivatedRoute, Data, Router} from '@angular/router';

@Component({
  selector: 'app-server',
  templateUrl: './server.component.html',
  styleUrls: ['./server.component.css']
})
export class ServerComponent implements OnInit {
  server: {id: number, name: string, status: string};

  constructor(
    private route: ActivatedRoute,
    private router: Router,
  ) { }

  ngOnInit() {
    this.route.data.subscribe((data: Data) => this.server = data.server);
  }

  onEdit() {
    this.router.navigate(['edit'], { relativeTo: this.route, queryParamsHandling: 'preserve' });
  }
}
```
{% endtab %}
{% endtabs %}

