# Observables

## Observables

### Creation

```javascript
import { Component, OnInit } from '@angular/core';
import { Observable, Observer } from 'rxjs';

@Component({
  selector: 'app-home',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.css']
})
export class HomeComponent implements OnInit {
  ngOnInit() {
    const myObservable = Observable.create((observer: Observer) => {
      setTimeout(() => {
        observer.next('first package');
      }, 2000);
      setTimeout(() => {
        observer.next('second package');
      }, 4000);
      setTimeout(() => {
        observer.complete();
      }, 5000);
      // error below won't happen since completed prior
      setTimeout(() => {
        observer.error('this does not work');
      }, 6000);
    });

    myObservable.subscribe(
      (data: string) => console.log(data),
      (error: string) => console.log(error),
      () => console.log('completed')
    );
  }
}
```

### Subject

`Subject`s are `Observer`s and `Observable`s at the same time.  
Good practice to use `Subject` _instead_ of `EventEmitter`

{% tabs %}
{% tab title="users.service.ts" %}
```javascript
import { Injectable } from '@angular/core';
import { Subject } from 'rxjs';

export class UsersService {
  userActivated = new Subject();
}
```
{% endtab %}

{% tab title="user.component.ts" %}
{% code title="user.component.ts" %}
```javascript
import { Component, OnInit } from '@angular/core';
import { ActivatedRoute, Params } from '@angular/router';
import { UsersService } from '../users.service';

@Component({
  selector: 'app-user',
  templateUrl: './user.component.html',
  styleUrls: ['./user.component.css']
})
export class UserComponent implements OnInit {
  id: number;

  constructor(private route: ActivatedRoute, private  usersService: UsersService) { }

  ngOnInit() {
    this.route.params
      .subscribe(
        (params: Params) => {
          this.id = +params['id'];
        }
      );
  }

  onActivate() {
    // "pushes" the new activated user ID
    this.usersService.userActivated.next(this.id);
  }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

