
go back to [Table of Content](./../index.md)

# Common Programing Concepts

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [Common Programing Concepts](#common-programing-concepts)
  - [Variables](#variables)
    - [Let](#let)
    - [Constants](#constants)
    - [Shadowing](#shadowing)
  - [Data types](#data-types)
    - [Scaler Types](#scaler-types)

<!-- /code_chunk_output -->

## Variables

!!! note
    By default variables are immutable

Variables can always be type annotated.

```rust
let logical: bool = true;  // annotating boolean type

let a_float: f64 = 1.0;  // Regular annotation
let an_integer   = 5i32; // Suffix annotation
```

### Let

We use the `let` statement to create a variable. Here’s an example:

```rust
let apples = 5;
```

Although variables are **immutable** by default, you can make them **mutable** by adding `mut` in front of the variable name.

```rust
let mut apples = 5;
```

### Constants

Like immutable variables, constants are values that are bound to a name and are not allowed to change.
First, you aren’t allowed to use `mut` with constants. Constants aren’t just immutable by default—<u>they’re always immutable</u>. You declare constants using the `const` keyword instead of the `let` keyword, and <u>the type of the value must be annotated</u>.

```rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

Rust’s naming convention for constants is to use **all uppercase with underscores between words (SCREAMING_SNAKE_CASE)**.

### Shadowing

you can declare a new variable with the same name as a previous variable. Rustaceans say that the first variable is shadowed by the second, which means that the second variable is what the compiler will see when you use the name of the variable. In effect, the second variable overshadows the first, taking any uses of the variable name to itself until either it itself is shadowed or the scope ends. 
<u>We can shadow a variable by using the same variable’s name and repeating the use of the `let` keyword</u> as follows:

```rust
fn main() {
    let x = 5;
    
    // creates a new variable x by repeating let x =, taking the 
    // original value and adding 1 so the value of x is then 6.
    let x = x + 1;

    {
        // Then, within an inner scope created with the curly 
        // brackets, the third let statement also shadows x and
        // creates a new variable, multiplying the previous value 
        // by 2 to give x a value of 12. When that scope is over, 
        // the inner shadowing ends and x returns to being 6
        let x = x * 2;
        println!("The value of x in the inner scope is: {x}");
    }

    println!("The value of x is: {x}");
}

// The value of x in the inner scope is: 12
// The value of x is: 6

```

Shadowing is different from marking a variable as mut.
we’ll get a compile-time error if we accidentally try to reassign to this variable without using the `let` keyword. By using `let`, we can perform a few transformations on a value but have the variable be immutable after those transformations have been completed.

```rust
fn main() {
    let spaces = "     ";
    {
        // shadowing to another type
        let spaces = spaces.len();
    }

    let mut another_variable = "rust";

    // reassignment of the same type is ok
    another_variable = "rust language";

    // reassignment of another type and compile complains:
    // expected `&str`, found `usize`
    another_variable = another_variable.len();
}
```

Another difference between `mut` and shadowing is that because we’re effectively creating a new variable when we use the `let` keyword again, we can change the type of the value but reuse the same name. For example, say our program asks a user to show how many spaces they want between some text by inputting space characters, and then we want to store that input as a number:

```rust
let spaces: &str = "   ";
let spaces: usize = spaces.len(); 
```

Shadowing thus spares us from having to come up with different names, such as `spaces_str` and `spaces_num`; instead, we can reuse the simpler spaces name.

## Data types

### Scaler Types

A scalar type represents a single value. Rust has four primary **scalar types: integers, floating-point numbers, Booleans, and characters.**

- **signed integers**: `i8`, `i16`, `i32`, `i64`, `i128` and `isize` (pointer size)
- **unsigned integers**: `u8`, `u16`, `u32`, `u64`, `u128` and `usize` (pointer size)
- **floating point**: `f32`, `f64`
- **`char` Unicode scalar values** like 'a', 'α' and '∞' (4 bytes each)
- **boolean** either `true` or `false`
- **unit type** `()`, whose only possible value is an empty tuple: `()`
