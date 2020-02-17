# Hello, World!

{% tabs %}
{% tab title="main.rs" %}
```rust
fn main() {
  println!("Hello, world!");
}
```
{% endtab %}

{% tab title="Compilation" %}
```bash
rustc main.rs
./main
```
{% endtab %}
{% endtabs %}

## Anatomy of a Rust Program

* The `main` function is special; it is always the first code that runs in every executable Rust program.
* `println!` calls a Rust macro
  * If it called a function instead, it would be entered as `println` \(without the `!`\)
* After compilation, Rust outputs a binary executable.
* As opposed to dynamic languages \(Ruby, Python, JS\), compiling and running are separate steps
  * this means you can compile the language and give to someone else \(without having Rust installed\)

