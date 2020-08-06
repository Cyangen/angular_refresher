```html
<!-- new-account.component.html -->
<div class="row">
  <div class="col-xs-12 col-md-8 col-md-offset-2">
    <div class="form-group">
      <label>Account Name</label>
      <input
        type="text"
        class="form-control"
        #accountName><!-- here the local reference -->
    </div>
    <div class="form-group">
      <select class="form-control" #status><!-- here the local reference -->
        <option value="active">Active</option>
        <option value="inactive">Inactive</option>
        <option value="hidden">Hidden</option>
      </select>
    </div>
    <button
      class="btn btn-primary"
      (click)="onCreateAccount(accountName.value, status.value)">
      Add Account
    </button>
  </div>
</div>
```



```typescript
// new-account.component.ts
@Component({
  selector: 'app-new-account',
  templateUrl: './new-account.component.html',
  styleUrls: ['./new-account.component.css']
})
export class NewAccountComponent {

  constructor() {}

  onCreateAccount(accountName: string, accountStatus: string) {
    ...
    ...
  }
}
```
