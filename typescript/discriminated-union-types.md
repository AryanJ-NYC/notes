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
  noise: 'bark'
}

type Cat = {
  name: string
  noise: 'meow'
}

const someFunctionThatUsesCatOrDog = (animal: Cat | Dog) => {
  if (typeof animal.name === 'string') {
      animal.noise // type 'bark' | 'meow' when you would think it's 'bark'
  }
}
```
{% endtab %}

{% tab title="with-tagged-unions.ts" %}
```typescript
type Dog = {
  type: 'dog'
  name: number;
  noise: 'bark'
}

type Cat = {
  type: 'cat'
  name: string
  noise: 'meow'
}

const someFunctionThatUsesCatOrDog = (animal: Cat | Dog) => {
  if (animal.type === 'dog') {
    animal.noise // type is 'bark'
  }
}
```
{% endtab %}
{% endtabs %}

