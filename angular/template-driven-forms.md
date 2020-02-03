# Template-Driven Forms

{% tabs %}
{% tab title="form.component.html" %}
`ngModel` can be **unbound**, **one-way bound** or **two-way bound**:

* **unbound** if we want access to data onSubmit \(makes it a control\)
* **one-way bound** if we want to provide data to the template via TypeScript code
* **two-way bound** if we want to capture each keystroke

We can use data in groups by providing `reference_name=reference_type`. Some examples include lines 5, 20, 26

```javascript
<div class="container">
  <div class="row">
    <div class="col-xs-12 col-sm-10 col-md-8 col-sm-offset-1 col-md-offset-2">
      <form (ngSubmit)="onSubmit()" #form="ngForm">
        <div id="user-data" ngModelGroup="userData" #userData="ngModelGroup">
          <div class="form-group">
            <label for="username">Username</label>
            <input type="text" id="username" class="form-control" name="username" ngModel required>
          </div>
          <button class="btn btn-default" type="button" (click)="suggestUserName()">Suggest an Username</button>
          <div class="form-group">
            <label for="email">Mail</label>
            <input type="email" id="email" class="form-control" name="email" ngModel required email #email="ngModel">
            <span *ngIf="!email.valid && email.touched" class="help-block">Please enter a valid email!</span>
          </div>
        </div>
        <span *ngIf="!userData.valid && userData.touched" class="help-block">Data is invalid!</span>
        <div class="form-group">
          <label for="secret">Secret Questions</label>
          <select id="secret" class="form-control" name="secret" [ngModel]="defaultQuestion">
            <option value="pet">Your first Pet?</option>
            <option value="teacher">Your first teacher?</option>
          </select>
        </div>
        <div class="form-group">
          <textarea name="questionAnswer" rows="3" [(ngModel)]="answer"></textarea>
        </div>
        <p>Your reply: {{ answer }}</p>
        <div class="radio" *ngFor="let gender of genders">
          <label>
            <input type="radio" name="gender" [value]="gender" required ngModel>
            {{ gender }}
          </label>
        </div>
        <button
          class="btn btn-primary"
          type="submit"
          [disabled]="!form.valid">Submit</button>
      </form>
    </div>
  </div>
  <hr>
  <div class="row" *ngIf="submitted">
    <h3>Your Data</h3>
    <p>Username: {{user.username}}</p>
    <p>Email: {{user.email}}</p>
    <p>Secret Question: Your first {{user.secretQuestion}}</p>
    <p>Answer: {{user.answer}}</p>
    <p>Gender: {{user.gender}}</p>
  </div>
</div>
```
{% endtab %}

{% tab title="form.component.ts" %}
```typescript
import { Component, ViewChild } from '@angular/core';
import { NgForm } from '@angular/forms';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  @ViewChild('form') signupForm: NgForm;
  answer: string;
  defaultQuestion = 'pet';
  genders = ['male', 'female'];
  user = {
    username: '',
    email: '',
    secretQuestion: '',
    answer: '',
    gender: '',
  };
  submitted = false;

  suggestUserName() {
    const suggestedName = 'Superuser';
    this.signupForm.form.patchValue({
      userData: {
        username: suggestedName,
      },
    });
  }

  onSubmit() {
    this.submitted = true;
    this.user.username = this.signupForm.value.userData.username;
    this.user.email = this.signupForm.value.userData.email;
    this.user.secretQuestion = this.signupForm.value.secret;
    this.user.answer = this.signupForm.value.questionAnswer;
    this.user.gender = this.signupForm.value.gender;

    this.signupForm.reset();
  }
}
```
{% endtab %}
{% endtabs %}



