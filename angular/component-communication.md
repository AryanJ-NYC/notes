# Component Communication

## Event Emission

{% tabs %}
{% tab title="cockpit.component.ts" %}
Use the `@Output()` decorator to designate a variable as an output for that component.

Assign a variable as a `new EventEmitter` to allow it to emit itself.

```typescript
export class CockpitComponent implements OnInit {
  @Output() blueprintCreated = new EventEmitter<{blueprintName: string, blueprintContent: string}>();
  @Output() serverCreated = new EventEmitter<{serverName: string, serverContent: string}>();

  onAddServer() {
    this.serverCreated.emit({ serverName: this.newServerName, serverContent: this.newServerContent });
  }

  onAddBlueprint() {
    this.blueprintCreated.emit({ blueprintName: this.newServerName, blueprintContent: this.newServerContent });
  }
}
```
{% endtab %}

{% tab title="app-cockpit.html.html" %}
Assume `onBlueprintAdded()` and `onServerAdded()` are functions in the component that uses `app-cockpit`.

Then used as:

```markup
<app-cockpit
  (blueprintCreated)="onBlueprintAdded($event)"
  (serverCreated)="onServerAdded($event)"
></app-cockpit>
```
{% endtab %}
{% endtabs %}

## Input Binding

Use `@Input` decorator to designate the variable as an input variable. `@Input` takes an alias as a parameter.

```typescript
@Input('srvElement') element: {type: string, name: string, content: string};
```

Then in the component where we use the component above, use `[srvElemeent]` to bind to that property.

