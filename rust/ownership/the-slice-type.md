# The Slice Type

A _string slice_ is a reference to part of a `String`, and it looks like this:

```rust
let s = String::from("hello world");

let hello = &s[0..5];
let world = &s[6..11];
```

![String slice referring to part of a String](../../.gitbook/assets/image%20%288%29.png)

