---
description: >-
  The Option type is used in many places because it encodes the very common
  scenario in which a value could be something or it could be nothing.
---

# The Option Enum



Rust does not have nulls, but it does have an enum that can encode the concept of a value being present or absent. This enum is `Option<T>`, and it is [defined by the standard library](https://doc.rust-lang.org/std/option/enum.Option.html) as follows:

```rust
enum Option<T> {
    Some(T),
    None,
}
```

Not having to worry about incorrectly assuming a not-null value helps you to be more confident in your code. In order to have a value that can possibly be null, you must explicitly opt in by making the type of that value `Option<T>`. Then, when you use that value, you are required to explicitly handle the case when the value is null. Everywhere that a value has a type that isn’t an `Option<T>`, you _can_ safely assume that the value isn’t null. This was a deliberate design decision for Rust to limit null’s pervasiveness and increase the safety of Rust code.

```rust
fn print_x_y(x: i32, y: Option<i32>) {
    match y {
        Some(v) => println!("{} + {} = {}", x, v, x + v),
        None => println!("y has None value"),
    }
}

fn main() {
    let x = 5;
    let mut y = Some(5);
    print_x_y(x, y); // 5 + 5 = 10

    y = None;
    print_x_y(x, y); // y has None value
}
```

