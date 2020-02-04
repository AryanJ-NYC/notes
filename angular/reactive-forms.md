# Reactive Forms

```javascript
import {Component, OnInit} from '@angular/core';
import { FormArray, FormControl, FormGroup, Validators } from '@angular/forms';
import { Observable } from 'rxjs';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnInit {
  genders = ['male', 'female'];
  forbiddenUsernames = [ 'Chris', 'Anna' ];
  signupForm: FormGroup;

  ngOnInit() {
    this.signupForm = new FormGroup({
      userData: new FormGroup({
        username: new FormControl(null, [Validators.required, this.forbiddenNames.bind(this)]),
        email: new FormControl(null, [ Validators.required, Validators.email ], this.forbiddenEmails),
      }),
      gender: new FormControl('male'),
      hobbies: new FormArray([]),
    });

    // this.signupForm.valueChanges.subscribe(
    //   value => console.log(value)
    // );

    this.signupForm.statusChanges.subscribe(
      value => console.log(value)
    );
    this.signupForm.setValue({
      userData: {
        username: 'Max',
        email: 'max@test.com',
      },
      gender: 'female',
      hobbies: [],
    });
  }

  onAddHobby() {
    const control = new FormControl(null, Validators.required);
    (<FormArray>this.signupForm.get('hobbies')).push(control);
  }

  onSubmit() {
    console.log(this.signupForm);
  }

  // custom validators
  forbiddenEmails(control: FormControl): Promise<any> | Observable<any> {
    const promise = new Promise<any>((resolve, reject) => {
      setTimeout(() => {
        if (control.value === 'test@test.com') {
          resolve({ emailIsForbidden: true });
        } else {
          resolve(null);
        }
      }, 1500);
    });
    return promise;
  }

  forbiddenNames(control: FormControl): {[s: string]: boolean} {
    if (this.forbiddenUsernames.includes(control.value)) {
      return { nameIsForbidden: true };
    }
    return null;
  }
}
```

