
go back to [Table of Content](./../index.md)

# Getting Started

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [Getting Started](#getting-started)
  - [Installation](#installation)
  - [Updating and Uninstalling](#updating-and-uninstalling)
  - [Creating a Project with Cargo](#creating-a-project-with-cargo)
  - [Building and Running a Cargo Project](#building-and-running-a-cargo-project)
  - [Building for Release](#building-for-release)
  - [Printing Values with **println!** Placeholders](#printing-values-with-println-placeholders)
  - [Project Set Up](#project-set-up)
    - [clippy](#clippy)
    - [cargo-watch](#cargo-watch)
    - [VS Code](#vs-code)
<!-- /code_chunk_output -->

## Installation

Installing `rustup` on Linux or macOS

```properties
curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

You will also need a **linker**, which is a program that Rust uses to join its compiled outputs into one file. It is likely you already have one. If you get linker errors, you should install a C compiler, which will typically include a linker. A C compiler is also useful because some common Rust packages depend on C code and will need a C compiler.
On macOS, you can get a C compiler by running:

```properties
xcode-select --install
```

## Updating and Uninstalling

From your shell, run the following update script:

``` properties
rustup update
```

To uninstall Rust and rustup, run the following uninstall script from your shell:

``` properties
rustup self uninstall
```

Version
To check whether you have Rust installed correctly, open a shell and enter this line:

```properties
rustc --version
```

You should see the **version number**, **commit hash**, and **commit date** for the latest stable version that has been released in the following format:

```log
rustc version-number (commit-hash commit-date)
rustc 1.66.0         (69f9c33d7   2022-12-12)
```

check whether Cargo is installed by entering the following into your terminal:

```properties
cargo --version
```

## Creating a Project with Cargo

Cargo is Rust’s build system and package manager. To create a new project using Cargo:

```properties
cargo new project_name
```

You’ll see that Cargo has generated two files and one directory for us:

- a `Cargo.toml` file
- a `src` directory with a `main.rs` file inside.
  
`Cargo.toml` is in the **TOML (Tom’s Obvious, Minimal Language)** format, which is Cargo’s configuration format. Cargo expects your source files to live inside the `src` directory. The top-level project directory is just for `README` files, license information, configuration files, and anything else not related to your code.
It has also initialized a new `Git` repository along with a `.gitignore` file. `Git` files won’t be generated if you run `cargo new` within an existing `Git` repository;

!!! tip Note
    You can override this behavior by using
    
    ```properties
    cargo new --vcs=git
    ```

## Building and Running a Cargo Project

```properties
cargo build
```

This command creates an executable file in `target/debug/hello_cargo` rather than in your current directory. You can run the executable with this command:

```properties
./target/debug/hello_cargo
```

We just built a project with `cargo build` and ran it with `./target/debug/hello_cargo`, but we can also use `cargo run` to compile the code and then run the resulting executable all in one command:

```properties
cargo run
```

Cargo also provides a command called `cargo check`. This command quickly checks your code to make sure it compiles but doesn’t produce an executable:

```properties
cargo check
```

Why would you not want an executable? Often, `cargo check` is much faster than `cargo build`, because it skips the step of producing an executable. If you’re continually checking your work while writing the code, using `cargo check` will speed up the process! As such, many Rustaceans run `cargo check` periodically as they write their program to make sure it compiles. 

## Building for Release

When your project is finally ready for release, you can use `cargo build --release` to compile it with optimizations.

```properties
cargo build --release
```

This command will create an executable in `target/release` instead of `target/debug`.

## Printing Values with **println!** Placeholders

The `{}` set of curly brackets is a placeholder. You can print more than one value using curly brackets: the first set of curly brackets holds the first value listed after the format string, the second set holds the second value, and so on.

```rust
let x = 5;
let y = 10;

println!("x = {} and y = {}", x, y);
// This code would print x = 5 and y = 10
```

## Project Set Up

### clippy

To install clippy, run 

```properties
rustup component add clippy
```

Then on top of the `main.rs` add:

```rust
#![deny(clippy::all)]
```

### cargo-watch

To install cargo-watch, run: 

```properties
cargo install cargo-watch
```

And then to run cargo-watch and have it running clippy, just run: 

```properties
Cargo-watch -qc -x run -x clippy
```

### VS Code

To force VS Code to format rust files using `rust.analyzer`, add this line to VS Code settings:

```json
"[rust]": {
    "editor.defaultFormatter": "rust-lang.rust-analyzer"
},
```
