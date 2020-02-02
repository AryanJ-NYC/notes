# Services

## Creation

Note, if we want to inject a service _into_ our service, we must decorate it with `@Injectable()`.

{% tabs %}
{% tab title="accounts.service" %}
```typescript
import {Injectable} from '@angular/core';
import {LoggingService} from './logging.service';

// decorator added to the service we want to inject with other service (aka the injectee)
@Injectable()
// OR
@Injectable({ providedIn: 'root' }) // this auto provides it in root
export class AccountsService {
  accounts = [
    {
      name: 'Master Account',
      status: 'active'
    },
    {
      name: 'Testaccount',
      status: 'inactive'
    },
    {
      name: 'Hidden Account',
      status: 'unknown'
    }
  ];

  // if we inject a service into something, that something must have metadata attached to it
  constructor(private loggingService: LoggingService) {}

  addAccount(name: string, status: string) {
    this.accounts.push({ name, status });
    this.loggingService.logStatusChange(status);
  }

  updateStatus(id: number, status: string) {
    this.accounts[id].status = status;
    this.loggingService.logStatusChange(status);
  }
}
```
{% endtab %}

{% tab title="app.module" %}
```typescript
// provide it in component or module we want to use it in
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';

import { AppComponent } from './app.component';
import { AccountComponent } from './account/account.component';
import { NewAccountComponent } from './new-account/new-account.component';
import {AccountsService} from './accounts.service';
import {LoggingService} from './logging.service';

@NgModule({
  declarations: [
    AppComponent,
    AccountComponent,
    NewAccountComponent
  ],
  imports: [
    BrowserModule,
  ],
  providers: [AccountsService, LoggingService],
  bootstrap: [AppComponent]
})
export class AppModule { }
```
{% endtab %}

{% tab title="app.component" %}
```typescript
// use it!
import {Component, OnInit} from '@angular/core';
import { AccountsService } from './accounts.service';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css'],
})
export class AppComponent implements OnInit {
  accounts: {name: string, status: string}[] = [];

  constructor(private accountsService: AccountsService) {}

  ngOnInit() {
    this.accounts = this.accountsService.accounts;
  }
}
```
{% endtab %}

{% tab title="new-account.component" %}
```typescript
// use it 2!!
import { Component } from '@angular/core';
import {AccountsService} from '../accounts.service';

@Component({
  selector: 'app-new-account',
  templateUrl: './new-account.component.html',
  styleUrls: ['./new-account.component.css'],
})
export class NewAccountComponent {
  constructor(private accountsService: AccountsService) {}

  onCreateAccount(accountName: string, accountStatus: string) {
    this.accountsService.addAccount(accountName, accountStatus);
  }
}
```
{% endtab %}
{% endtabs %}

