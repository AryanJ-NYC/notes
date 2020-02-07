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

### Typing This

```typescript
function fancyDate(this: Date) {
  return `${this.getDate()}/${this.getMonth()}/${this.getFullYear()}`;
}
```

### Overloading

An overloaded function is a function with multiple signatures. Imagine a function, `reserve` that can book one-way and round trips:

```typescript
type Reserve = {
  (from: Date, to: Date, destination: string): Reservation
  (from: Date, destination: string)
}

const reserve: Reserve = (
  from: Date,
  toOrDestination: Date | string,
  destination?: string
) => {
  // we'll need to 'prove' to TypeScript the types of each parameter
  if (toOrDestination instanceof Date && destination !== undefined) {
    // book a one-way trip
  } else if (typeof toOrDestination === 'string') {
    // book a round trip
  }
}
```

