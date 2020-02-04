# Directives

## Built-in Direc tives

### ngIf

Asterisk \(\*\) indicates it's a structural directive \(adds or removes from DOM\).

```markup
<p *ngIf="isServerCreated">
  Server was created, server name is {{ serverName }}
</p>
```

### ngFor

```markup
<li
  class="list-group-item"
  *ngFor="let oddNumber of oddNumbers"
  [ngClass]="{ odd: oddNumber % 2 !== 0}"
>
  {{ oddNumber }}
</li>
```

### ngStyle / ngClass

{% tabs %}
{% tab title="server.component.html" %}
```markup
<h1
  [ngClass]="{ online: serverStatus === 'online' }"
  [ngStyle]="{ backgroundColor: getColor() }"
>
  Server {{ serverId }} is {{ serverStatus }}.
</h1>
```
{% endtab %}

{% tab title="server.component.ts" %}
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-server',
  templateUrl: './server.component.html',
})
export class ServerComponent {
  serverStatus = 'offline';

  constructor() {
    this.serverStatus = Math.random() > 0.5 ? 'online' : 'offline';
  }

  getColor() {
    return this.serverStatus === 'online' ? 'green' : 'red';
  }
}
```
{% endtab %}
{% endtabs %}

## Attribute Directives

{% tabs %}
{% tab title="better-highlight.directive.ts" %}
```typescript
import { Directive, ElementRef, HostBinding, HostListener, Input, OnInit, Renderer2 } from '@angular/core';

@Directive({
  selector: '[appBetterHighlight]'
})
export class BetterHighlightDirective implements OnInit {
  @Input() defaultColor = 'transparent'; // bound just like element input
  @Input() highlightColor = 'blue'; // bound just like element input
  @HostBinding('style.backgroundColor') backgroundColor: string;

  constructor(private elRef: ElementRef, private renderer: Renderer2) { }

  ngOnInit() {
    this.backgroundColor = this.defaultColor;
  }

  @HostListener('mouseenter') mouseOver(eventData: Event) {
    this.renderer.setStyle(this.elRef.nativeElement, 'background-color', this.highlightColor);
    // this.backgroundColor = this.highlightColor;
  }

  @HostListener('mouseleave') mouseLeave(eventData: Event) {
    this.renderer.setStyle(this.elRef.nativeElement, 'background-color', this.defaultColor);
  }
}
```
{% endtab %}

{% tab title="some.component.html" %}
```markup
<!-- to use it, pass it as an attribue to an element -->
<li appBetterHighlight class="list-group-item" defaultColor="blue" highlightColor="red">
  {{ evenNumber }}
</li>
```
{% endtab %}
{% endtabs %}

## Structural Directives

{% tabs %}
{% tab title="unless.directive.ts" %}
```typescript
import { Directive, Input, TemplateRef, ViewContainerRef } from '@angular/core';

@Directive({
  selector: '[appUnless]'
})
export class UnlessDirective {
  // input name must match name of directive
  @Input('appUnless') set unless(condition: boolean) {
    if (!condition) {
      this.viewContainerRef.createEmbeddedView(this.templateRef);
    } else {
      this.viewContainerRef.clear();
    }
  }
  constructor(private templateRef: TemplateRef<any>, private viewContainerRef: ViewContainerRef) { }
}
```
{% endtab %}

{% tab title="odd.component.html" %}
```markup
<div *appUnless="!onlyOdd">
  <li>
    {{ oddNumber }}
  </li>
</div>
```
{% endtab %}
{% endtabs %}

