# Project Management with Packages, Crates and Modules

## Packages and Crates

* when we run `cargo new my-project`, we have a package that only contains `src/main.rs`. Therefore, we only have a binary crate named `my-project`
* If a package contains `src/main.rs` _and_ `src/lib.rs`, the package contains a library crate with the same name as the package and `src/lib.rs` is its crate root

## Modules

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
        fn seat_at_table() {}
    }

    mod serving {
        fn take_order() {}
        fn serve_order() {}
        fn take_payment() {}
    }
}

mod back_of_house {
    pub struct Breakfast {
        pub toast: String,
        seasonal_fruit: String, // private
    }

    impl Breakfast {
        pub fn summer(toast: &str) -> Breakfast {
            Breakfast {
                toast: String::from(toast),
                seasonal_fruit: String::from("peaches"),
            }
        }
    }

    fn fix_incorrect_order() {
        cook_order();
        super::front_of_house::serving::serve_order(); // super like .. in filesystems
    }

    fn cook_order() {}
}

pub fn eat_at_restaurant() {
    // absolute path
    crate::front_of_house::hosting::add_to_waitlist();
    // relative path
    front_of_house::hosting::add_to_waitlist();

    // order breakfast in the summer with Rye toast
    let mut meal = back_of_house::Breakfast::summer("Rye");
    meal.toast = String::from("Wheat");
    println!("I'd like {} toast please", meal.toast);

    // next line does not compile due to privacy
    meal.seasonal_fruit = String::from("Blueberries");
}

```

* `hosting` and `serving` are _siblings_
* `front_of_house` is the _parent_ of `hosting` \(and `serving`\)
* `hosting` and `serving` are _children_ of `front_of_house`
* Both `hosting` and `add_to_waitlist` must be made public to be used by `eat_at_restaurant`
* if we make a `struct` public, its fields remain private \(`Breakfast`, for example\)
* however, if we make an `enum` public, all its variants are public

## Bringing Paths into Scope with \`use\`

### Providing New Names with \`as\`

```rust
use std::fmt::Result;
use std::io::Result as IoResult;

fn function1() -> Result {
    // --snip--
}

fn function2() -> IoResult<()> {
    // --snip--
}
```

### Re-exporting Names with \`pub use\`

```rust
mod front_of_house {
    pub mod hosting {
        pub fn add_to_waitlist() {}
    }
}

pub use crate::front_of_house::hosting;
```

* by using `pub use`, external code can now call the `add_to_waitlist` function using `hosting::add_to_waitlist`.

### Using External Packages

```rust
use rand::Rng;
fn main() {
    let secret_number = rand::thread_rng().gen_range(1, 101);
}
```

#### Using Nested Paths

{% tabs %}
{% tab title="long.rs" %}
```rust
use std::io;
use std::cmp::Ordering;
```
{% endtab %}

{% tab title="short.rs" %}
```rust
use std::{cmp::Ordering, io};
```
{% endtab %}

{% tab title="glob.rs" %}
```
use std::collections::*;
```
{% endtab %}
{% endtabs %}

## Separating Modules into Different Files

{% tabs %}
{% tab title="src/lib.rs" %}
```rust
mod front_of_house;
```
{% endtab %}

{% tab title="src/front\_of\_house.rs" %}
```rust
pub mod hosting;
pub mod serving;
```
{% endtab %}

{% tab title="src/front\_of\_house/hosting.rs" %}
```rust
  pub fn add_to_waitlist() {}
  fn seat_at_table() {}
```
{% endtab %}

{% tab title="src/front\_of\_house/serving.rs" %}
```rust
  fn take_order() {}
  pub fn serve_order() {}
  fn take_payment() {}
```
{% endtab %}
{% endtabs %}

* using a semicolon after `mod front_of_house` in `src/lib.rs`, rather than a block, tells Rust to load contents of the module from another file with the same name as the module
* similarly in `front_of_house.rs`, we can make modules public inside corresponding files names in `front_of_house` directory

