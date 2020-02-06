# TypeScript

## Arrays

### Rest Elements

```typescript
// A list of strings with at least 1 element
let friends: [string, ...string[]] = [
  'Sara',
  'Tali',
  'Chloe',
];

// a heterogenous list
let list: [number, boolean, ...string[]] = [
  1,
  false,
  'a',
  'b',
  'c',
]
```

## Functions

### Rest Parameters

{% tabs %}
{% tab title="unsafe.ts" %}
```typescript
// arguments is not typed
function sumVariadic(): number {
  return Array
    .from(arguments)
    .reduce((total, n) => total + n, 0)
}
```
{% endtab %}

{% tab title="safe.ts" %}
```typescript
function sumVariadicSafe(...numbers: number[]) {
  return numbers.reduce((total, n) => total + n, 0)
}
```
{% endtab %}
{% endtabs %}

