
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
      - [Integer Types](#integer-types)
      - [Floating-Point Types](#floating-point-types)
      - [Integer Overflow](#integer-overflow)
      - [Numeric Operations](#numeric-operations)
      - [Boolean Type](#boolean-type)
      - [Character Type](#character-type)
      - [Compound Types](#compound-types)
        - [Tuple Type](#tuple-type)
          - [Defining Tuples](#defining-tuples)
          - [Accessing Tuple Elements](#accessing-tuple-elements)
        - [Array Type](#array-type)
          - [Defining Arrays](#defining-arrays)
          - [Accessing Array Elements](#accessing-array-elements)
          - [Invalid Array Element Access](#invalid-array-element-access)
  - [Functions](#functions)
    - [Parameters](#parameters)
    - [Statements and Expressions](#statements-and-expressions)
    - [Return Values](#return-values)

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

Keep in mind that Rust is a **statically typed language**, which means that it must know the types of all variables at compile time. We’ll look at two data type subsets: 

- Scalar types
- compound types

In cases when many types are possible, such as when we converted a `String` to a numeric type using `parse`, we must add a type annotation, like this:

```rust
let guess: u32 = "42".parse().expect("Not a number!");
```

If we don’t add the `: u32` type annotation, Rust will display `no_type_annotations` error.

### Scaler Types

<u>A scalar type represents a **single** value.</u> Rust has four primary **scalar types: integers, floating-point numbers, Booleans, and characters.**

- **integers**
  - **signed integers**: `i8`, `i16`, `i32`, `i64`, `i128` and `isize` (pointer size)
  - **unsigned integers**: `u8`, `u16`, `u32`, `u64`, `u128` and `usize` (pointer size)
- **floating point**: `f32`, `f64`
- **`char` Unicode scalar values** like 'a', 'α' and '∞' (4 bytes each)
- **boolean** either `true` or `false`

#### Integer Types

An integer is a number without a fractional component.

 Length  | Signed  | Unsigned 
-------- | ------- | --------- 
8-bit | i8 | u8
16-bit | i16 | u16
32-bit | i32 | u32
64-bit | i64 | u64
128-bit | i128 | u128
arch | isize | usize

Each variant can be either signed or unsigned and has an explicit size. Signed and unsigned refer to whether it’s possible for the number to be negative. It’s like writing numbers on paper: when the sign matters, a number is shown with a plus sign or a minus sign; however, when it’s safe to assume the number is positive, it’s shown with no sign. Signed numbers are stored using **two’s complement** representation.

Each signed variant can store numbers from $-(2^{n - 1})$ to $2^{n - 1} - 1$ inclusive, where $n$ is the number of bits that variant uses. So an `i8` can store numbers from $-(27)$ to $27 - 1$, which equals $-128$ to $127$. Unsigned variants can store numbers from $0$ to $2^{n - 1}$, so a `u8` can store numbers from $0$ to $2^{8 - 1}$, which equals $0$ to $255$.

Additionally, the `isize` and `usize` types depend on the architecture of the computer your program is running on, which is denoted in the table as “arch”: 64 bits if you’re on a 64-bit architecture and 32 bits if you’re on a 32-bit architecture.

Number | literals
------ | -------- 
Decimal | 98_222
Hex | 0xff
Octal| 0o77
Binary | 0b1111_0000
Byte (u8 only) | b'A'

You can write integer literals which only has one type in any of the forms shown in above Table. 
Note that number literals that can be multiple numeric types allow a type suffix, such as `57u8`, to designate the type. 
Number literals can also use _ as a visual separator to make the number easier to read, such as 1_000, which will have the same value as if you had specified 1000.

- integer types default to `i32`. 
- The primary situation in which you’d use `isize` or `usize` is when indexing some sort of collection.

#### Floating-Point Types

Rust also has two primitive types for floating-point numbers, which are numbers with decimal points. Rust’s floating-point types are `f32` and `f64`.

!!!
    - The default type is `f64` because on modern CPUs, it’s roughly the same speed as f32 but is capable of more precision. 
    - All floating-point types are signed.

```rust
fn main() {
    let x = 2.0; // f64

    let y: f32 = 3.0; // f32
}
```

#### Integer Overflow

Let’s say you have a variable of type u8 that can hold values between 0 and 255. If you try to change the variable to a value outside that range, such as 256, integer overflow will occur, which can result in one of two behaviors. 

1. When you’re compiling in **debug mode**, Rust includes checks for integer overflow that cause your program to panic at runtime if this behavior occurs. Rust uses the term **panicking** when a program exits with an error.
2. When you’re compiling in **release mode** with the `--release` flag, Rust does not include checks for integer overflow that cause panics. Instead, if overflow occurs, Rust performs *two’s complement wrapping*. In short, values greater than the maximum value the type can hold “wrap around” to the minimum of the values the type can hold. In the case of a `u8`, the value 256 becomes 0, the value 257 becomes 1, and so on. The program won’t panic, but the variable will have a value that probably isn’t what you were expecting it to have. Relying on integer overflow’s wrapping behavior is considered an error.

#### Numeric Operations

Rust supports the basic mathematical operations you’d expect for all the number types: 

- addition
- subtraction
- multiplication
- division
- remainder. 

!!! note
    Integer division truncates toward zero to the nearest integer. 

```rust
fn main() {
    // addition
    let sum = 5 + 10;

    // subtraction
    let difference = 95.5 - 4.3;

    // multiplication
    let product = 4 * 30;

    // division
    let quotient = 56.7 / 32.2;
    let truncated = -5 / 3; // Results in -1

    // remainder
    let remainder = 43 % 5;
}
```

#### Boolean Type

A Boolean type in Rust has two possible values: `true` and `false`. Booleans are <u>one byte in size</u>. The Boolean type in Rust is specified using `bool`. For example:

```rust
let t = true;
let f: bool = false; // with explicit type annotation
```

#### Character Type

Rust’s `char` type is the language’s most primitive alphabetic type. Here are some examples of declaring `char` values:

```rust
let c = 'z';
let z: char = 'ℤ'; // with explicit type annotation
let heart_eyed_cat = '😻';
```

!!!
    - Note that we specify `char` literals with single quotes, as opposed to `string` literals, which use double quotes.
    - Rust’s `char` type is <u>four bytes in size</u> and represents a **Unicode Scalar Value**, which means it can represent a lot more than just ASCII. Accented letters; Chinese, Japanese, and Korean characters; emoji; and zero-width spaces are all valid `char` values in Rust.

#### Compound Types

Compound types can group multiple values into one type. Rust has two primitive compound types: 

- tuples
- arrays

##### Tuple Type

Tuples have a fixed length: once declared, they cannot grow or shrink in size.

###### Defining Tuples

We create a tuple by writing a comma-separated list of values inside parentheses. Each position in the tuple has a type, and the types of the different values in the tuple don’t have to be the same.

```rust
let tup = (500, 6.4, 1); // the default type gets inferred as (i32, f64, u8)
let tup: (u16, f64, u8) = (500, 6.4, 1); // with type annotations

```    

###### Accessing Tuple Elements

To <u>get the individual values</u> out of a tuple, we can use pattern matching to destructure a tuple value, like this:

```rust
let tup = (500, 6.4, 1);
let (x, y, z) = tup;
println!("The value of y is: {y}"); // The value of y is: 6.4
```

This is called **destructuring** because it breaks the single tuple into three parts. 
We can also access a tuple element directly by using a period `(.)` followed by the index of the value we want to access. The first index in a tuple is 0. For example:

```rust
    let x: (i32, f64, u8) = (500, 6.4, 1);
    let five_hundred = x.0;
    let six_point_four = x.1;
    let one = x.2;
```

!!!
    The tuple without any values has a special name, **unit**. This value and its corresponding type are both written `()` and represent an empty value or an empty return type. Expressions implicitly return the unit value if they don’t return any other value.

##### Array Type

Unlike a tuple, every element of an array must have the same type. Unlike arrays in some other languages, arrays in Rust have a fixed length.

!!!
    Arrays are useful 
    - when you want your data allocated on the stack rather than the heap
    - or when you want to ensure you always have a fixed number of elements.

An array isn’t as flexible as the vector type, though. A vector is a similar collection type provided by the standard library that is allowed to grow or shrink in size. 

###### Defining Arrays

We write the values in an array as a comma-separated list inside square brackets:

```rust
let a = [1, 2, 3, 4, 5];
```

You write an array’s type using square brackets with the type of each element, a semicolon, and then the number of elements in the array, like so:

```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
```

Here, `i32` is the type of each element. After the semicolon, the number 5 indicates the array contains five elements.

You can also initialize an array to contain the same value for each element by specifying the initial value, followed by a semicolon, and then the length of the array in square brackets, as shown here:

```rust
let a = [3; 5]; // same as writing let a = [3, 3, 3, 3, 3];
```

###### Accessing Array Elements

An array is a single chunk of memory of a known, fixed size that can be allocated on the stack. You can access elements of an array using indexing, like this:

```rust
    let a = [1, 2, 3, 4, 5];

    let first = a[0];
    let second = a[1];
```

###### Invalid Array Element Access

When you attempt to access an element using indexing, Rust will check that the index you’ve specified is less than the array length. If the index is greater than or equal to the length, Rust will panic (*runtime error*). This check has to happen at runtime, especially in this case, because the compiler can’t possibly know what value a user will enter when they run the code later.

!!!
    This is an example of **Rust’s memory safety principles** in action. In many low-level languages, this kind of check is not done, and when you provide an incorrect index, invalid memory can be accessed. Rust protects you against this kind of error by immediately exiting instead of allowing the memory access and continuing. 

## Functions

The `fn` keyword, allows you to declare new functions. Rust code uses snake_case as the conventional style for function and variable names.

```rust
fn main() {
    println!("Hello, world!");
    another_function();
}

fn another_function() {
    println!("Another function.");
}
```

Note that we defined `another_function` after the `main` function in the source code; we could have defined it before as well. Rust doesn’t care where you define your functions, only that they’re defined somewhere in a scope that can be seen by the caller.
The lines execute in the order in which they appear in the `main` function. First the “Hello, world!” message prints, and then `another_function` is called and its message is printed.

### Parameters

parameters, are special variables that are part of a function’s signature. In function signatures, you must declare the type of each parameter. 

```rust
fn main() {
  print_labeled_measurement(5, 'h');
}

fn print_labeled_measurement(value: i32, unit_label: char) {
  println!("The measurement is: {value}{unit_label}"); // The measurement is: 5h
}
```

### Statements and Expressions

- **Statements** are instructions that perform some action and do not return a value.
- **Expressions** evaluate to a resultant value.

```rust
fn main() {
    let y = 6;
}
```

Creating a variable and assigning a value to it with the let keyword is a statement. `let y = 6;` is a statement. Function definitions are also statements like `main` function. **Remember, statements do not return values.**

Expressions evaluate to a value like a math operation, such as 5 + 6, which is an expression that evaluates to the value 11. Expressions can be part of statements. For example, 

- The `6` in the statement `let y = 6;` is an expression that evaluates to the value 6. 
- Calling a function is an expression. 
- Calling a macro is an expression. 
- A new scope block created with curly brackets is an expression

An example of a new scope block created with curly brackets:

```rust
fn main() {
  let y = {
    let x = 3;
    x + 1
  };

  println!("The value of y is: {y}");
}
```

this expression,

```rust
  {
    let x = 3;
    x + 1
  }
```

is a block that, in this case, evaluates to 4. That value gets bound to `y` as part of the `let` statement. Note that the `x + 1` line doesn’t have a semicolon at the end, which is unlike most of the lines you’ve seen so far. Expressions do not include ending semicolons. If you add a semicolon to the end of an expression, you turn it into a statement, and it will then not return a value.

### Return Values

Functions can return values to the code that calls them. We don’t name return values, but we must declare their type after an arrow `(->)`. In Rust, the return value of the function is synonymous with the value of the final expression in the block of the body of a function. You can return early from a function by using the `return` keyword and specifying a value, but most functions return the last expression implicitly. Here’s an example of a function that returns a value:

```rust
fn five() -> i32 {
  5 // no semicolon here mean return 5 
}

fn main() {
  let x = five();
  println!("The value of x is: {x}");
}
```

There are no function calls, macros, or even `let` statements in the `five` function—just the number 5 by itself. That’s a perfectly valid function in Rust. Note that the function’s return type is specified too, as `-> i32`.

Another example:

```rust
fn main() {
  let x = plus_one(5);
  println!("The value of x is: {x}"); // The value of x is: 6
}

fn plus_one(x: i32) -> i32 {
  x + 1
}
```

if we place a semicolon at the end of the line containing x + 1, changing it from an expression to a statement, we’ll get a `mismatched types` error. The definition of the function `plus_one` says that it will return an `i32`, but statements don’t evaluate to a value, which is expressed by `()`, the unit type. 
