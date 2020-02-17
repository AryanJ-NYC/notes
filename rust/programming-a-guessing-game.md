# Programming a Guessing Game

## Adding a Dependency

```bash
cargo install cargo-edit
cargo add rand
```

The [`cargo-edit` crate](https://github.com/killercup/cargo-edit) allows users to manage `cargo` dependencies from the CLI.

## Guessing Game

```rust
use rand::Rng;
use std::cmp::Ordering;
use std::io;

fn main() {
    let secret_random_number = rand::thread_rng().gen_range(1, 101);
    println!("Guess a number between 1-100!");
    println!("Please input your guess.");

    loop {
        let mut guess = String::new();
        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");
        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => {
                println!("Please enter a number between 1-100");
                continue;
            }
        };

        println!("You guessed: {}", guess);

        match guess.cmp(&secret_random_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
    }
}

```

## Bringing libraries into scope

To obtain user input and then print the result as output, we need to bring the `io` \(input/output\) library into scope. The `io` library comes from the standard library \(which is known as `std`\):

```rust
use std::io;
```

## Mutability

In Rust, variables are immutable by default. The following example shows how to use `mut` before a variable name to make a variable mutable:

```rust
let foo = 5; // immutable
let mut bar = 5; // mutable
```

## Passing by Reference

The `&` indicates that this argument is a _reference_, which gives you a way to let multiple parts of your code access one piece of data without needing to copy that data into memory multiple times. References are a complex feature, and one of Rust’s major advantages is how safe and easy it is to use references.

## io::Result

`read_line()` returns an [`io::Result`](https://doc.rust-lang.org/std/io/type.Result.html). The Result types are enumerations often referred to as enums. An enumeration is a type that can have a fixed set of values.

An instance of `io::Result` has an [`expect` method](https://doc.rust-lang.org/std/result/enum.Result.html#method.expect) that you can call. If this instance of `io::Result` is an `Err` value, `expect` will cause the program to crash and display the message that you passed as an argument to `expect`. If the `read_line` method returns an `Err`, it would likely be the result of an error coming from the underlying operating system. If this instance of `io::Result` is an `Ok` value, `expect` will take the return value that `Ok` is holding and return just that value to you so you can use it. In this case, that value is the number of bytes in what the user entered into standard input.

## Rand

We’re adding two lines in the middle. The `rand::thread_rng` function will give us the particular random number generator that we’re going to use: one that is local to the current thread of execution and seeded by the operating system. 

We call the `gen_range` method on the random number generator. This method is defined by the `Rng` trait that we brought into scope with the `use rand::Rng` statement. The `gen_range` method takes two numbers as arguments and generates a random number between them. It’s inclusive on the lower bound but exclusive on the upper bound, so we need to specify `1` and `101` to request a number between 1 and 100.

{% hint style="info" %}
`cargo doc --open` creates docs specific to the crates we're using. Check sidebar on left!
{% endhint %}

## Match

We use a [`match`](https://doc.rust-lang.org/book/ch06-02-match.html) expression to decide what to do next based on which variant of `Ordering` was returned from the call to `cmp` with the values in `guess` and `secret_number`.

A `match` expression is made up of _arms_. An arm consists of a _pattern_ and the code that should be run if the value given to the beginning of the `match` expression fits that arm’s pattern. Rust takes the value given to `match` and looks through each arm’s pattern in turn. The `match` construct and patterns are powerful features in Rust that let you express a variety of situations your code might encounter and make sure that you handle them all.

Let’s walk through an example of what would happen with the `match` expression used here. Say that the user has guessed 50 and the randomly generated secret number this time is 38. When the code compares 50 to 38, the `cmp` method will return `Ordering::Greater`, because 50 is greater than 38. The `match` expression gets the `Ordering::Greater` value and starts checking each arm’s pattern. It looks at the first arm’s pattern, `Ordering::Less`, and sees that the value `Ordering::Greater` does not match `Ordering::Less`, so it ignores the code in that arm and moves to the next arm. The next arm’s pattern, `Ordering::Greater`, _does_ match `Ordering::Greater`! The associated code in that arm will execute and print `Too big!` to the screen. The `match` expression ends because it has no need to look at the last arm in this scenario.  


