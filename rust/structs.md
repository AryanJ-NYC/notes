---
description: >-
  A struct, or structure, is a custom data type that lets you name and package
  together multiple related values that make up a meaningful group.
---

# Structs

A struct is defined and instantiated as follows:

```rust
struct Rectangle {
    height: u32,
    width: u32,
}

fn main() {
    let rectangle = Rectangle {
        width: 30,
        height: 50,
    };
}
```

## Method Syntax

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

fn main() {
    let rect1 = Rectangle { width: 30, height: 50 };

    println!(
        "The area of the rectangle is {} square pixels.",
        rect1.area()
    );
}
```

## Associated Functions

Associated functions are what some programming languages call _static functions_.

```rust
impl Rectangle {
    fn area(&self) -> u32 {
        self.height * self.width
    }
    fn square(size: u32) -> Rectangle {
        Rectangle {
            width: size,
            height: size,
        }
    }
}

fn main() {
    let square = Rectangle::square(30);

    println!(
        "The area of the rectangle is {} square pixels.",
        square.area()
    );
}

```

