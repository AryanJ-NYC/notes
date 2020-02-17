# Cargo

Cargo is Rust's build system and package manager. It handles many tasks like building your code, downloading the libraries your code depends on and building those libraries. The vast majority of Rust projects use cargo.

### Getting Started

```bash
cargo new hello_cargo
cd hello_cargo
```

The `cargo new hello_cargo` command creates a _Cargo.toml_ file and a _src_ directory with a _main.rs_ inside.

{% tabs %}
{% tab title="Cargo.toml" %}
```bash
[package]
name = "hello_cargo"
version = "0.1.0"
authors = ["Your Name <you@example.com>"]
edition = "2018"

[dependencies]
```
{% endtab %}

{% tab title="src/main.rs" %}
```rust
fn main() {
    println!("Hello, world!");
}
```
{% endtab %}
{% endtabs %}

Cargo expects your source files to live inside the _src_ directory. The top-level project directory is just for README files, license information, configuration files, and anything else not related to your code. Using Cargo helps you organize your projects. There’s a place for everything, and everything is in its place.

### Build \(Debug\) and Run

```bash
cargo build
cargo run
```

### Checking your Build

```bash
cargo check
```

* The command above checks your code to make sure it compiles.
* It does not create an executable
* Much faster than `cargo build` because it skips the step of producing an executable

### Building for Release

```bash
cargo build --relase
```



