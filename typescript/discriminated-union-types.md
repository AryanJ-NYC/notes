---
description: >-
  Since members of a union might overlap, we need a reliable way to know when
  we're in one case of a union type versus another case.
---

# Discriminated Union Types

{% tabs %}
{% tab title="without-tagged-unions.ts" %}
```typescript
type Dog = {
  name: number;
  barks: true;
  meows: false
}

type Cat = {
  name: string
  barks: false
  meows: true
}

const someFunctionThatUsesCatOrDog = (animal: Cat | Dog) => {
  if (typeof animal.name === 'number') {
    animal.barks // type boolean when you would think it's true
    animal.meows // type boolean when you would think it's false
  }
}

```
{% endtab %}

{% tab title="with-tagged-unions.ts" %}
```typescript
type Dog = {
  type: 'dog'
  name: number;
  barks: true;
  meows: false
}

type Cat = {
  type: 'cat'
  name: string
  barks: false
  meows: true
}

const someFunctionThatUsesCatOrDog = (animal: Cat | Dog) => {
  if (animal.type === 'dog') {
    animal.barks // type is true
    animal.meows // type is false
  }
}
```
{% endtab %}
{% endtabs %}

