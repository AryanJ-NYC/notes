---
description: >-
  Rust’s central feature is ownership. Although the feature is straightforward
  to explain, it has deep implications for the rest of the language.
---

# Ownership

In Rust, memory is managed through a system of ownership with a set of rules the compiler checks at compile time.

## Stack vs. Heap

#### Stack

* all data stored on the stack must have a known, fixed size.

#### Heap

* data with an unknown size at compile time or a size that might change must be stored on the heap
* the heap is less organized
  * when you put data on the heap, you request a certain amount of space
  * the OS finds an empty spot in the heap that is big enough, marks it as being in use and returns a _pointer_ \(the address of that location\)
    * this process is called _allocating on the heap_
  * since the pointer is of a known, fixed size, you can store the pointer on the stack
    * when you want the actual data, you must follow the pointer

#### Writing Data

* pushing to the stack is faster than allocating on the heap because the OS never has to search for a place to store new data; that location is always at the top of the stack
* comparatively, allocating space on the heap requires more work
  * the OS must first find a big enough space to hold the data then perform bookkeeping to prepare for the next allocation

#### Reading Data

* accessing data on the heap is slower than accessing data on the stack because you have to follow a pointer to get there

{% hint style="info" %}
Managing heap data is why ownership exists
{% endhint %}

## Ownership Rules

* each value in Rust has a variable that's called its _owner_
* there can only be one owner at a time
* when the owner goes out of scope, the value will be dropped

## Memory and Allocation

* Rust takes a different approach to garbage collection: memory is automatically returned once the variable that owns it goes out of scope.
* When a variable goes out of scope, Rust calls a function called `drop`

### Ways Variables and Data Interact

#### Move

```rust
let s1 = String::from("hello");
let s2 = s1;
```

It was mentioned that, when a variable goes out of scope, Rust automatically calls the `drop` function and cleans up the heap memory for that variable. However, that can lead to a problem: when `s2` and `s1` go out of scope, they will both try to free the same memory. This is known as a _double free_ error.

Instead of trying to copy the allocated memory that belongs to `s1`, Rust considers `s1` to no longer be valid \(and therefore doesn't need to free anything when `s1` goes out of scope.

```rust
// invalid code; `s1` is borrowed after move
fn main() {
    let s1 = String::from("hello");
    let s2 = s1;

    println!("{}, world!", s1);
}
```

![Representation in memory after s1 has been invalidated](../../.gitbook/assets/image%20%287%29.png)

#### Clone

```rust
let s1 = String::from("hello");
let s2 = s1.clone();

println!("s1 = {}, s2 = {}", s1, s2);
```

## Ownership and Functions

The semantics for passing a value to a function are similar to those for assigning a value to a variable.

```rust
fn main() {
    let s = String::from("hello");  // s comes into scope

    takes_ownership(s);             // s's value moves into the function...
                                    // ... and so is no longer valid here

    let x = 5;                      // x comes into scope

    makes_copy(x);                  // x would move into the function,
                                    // but i32 is Copy, so it’s okay to still
                                    // use x afterward

} // Here, x goes out of scope, then s. But because s's value was moved, nothing
  // special happens.

fn takes_ownership(some_string: String) { // some_string comes into scope
    println!("{}", some_string);
} // Here, some_string goes out of scope and `drop` is called. The backing
  // memory is freed.

fn makes_copy(some_integer: i32) { // some_integer comes into scope
    println!("{}", some_integer);
} // Here, some_integer goes out of scope. Nothing special happens.
```

If we tried to use `s` after the call to `takes_ownership`, Rust would throw a compile-time error.

