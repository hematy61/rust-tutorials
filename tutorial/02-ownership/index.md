
[Main Table of Content](./../index.md)

# Ownership

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

- [Ownership](#ownership)
  - [About Ownership](#about-ownership)
  - [The Stack and the Heap](#the-stack-and-the-heap)
  - [Ownership Rules](#ownership-rules)
  - [Variable Scope](#variable-scope)
  - [Strings](#strings)

<!-- /code_chunk_output -->

## About Ownership

Ownership is a set of rules that govern how a Rust program manages memory. All programs have to manage the way they use a computer’s memory while running. Some languages have garbage collection that regularly looks for no-longer-used memory as the program runs; in other languages, the programmer must explicitly allocate and free the memory. 
Rust uses a third approach:<u> memory is managed through a system of ownership with a set of rules that the compiler checks.</u> If any of the rules are violated, the program won’t compile. None of the features of ownership will slow down your program while it’s running.

## The Stack and the Heap

In a systems programming language like Rust, whether a value is on the stack or the heap affects how the language behaves and why you have to make certain decisions. Parts of ownership will be described in relation to the stack and the heap.
Both the stack and the heap are parts of memory available to your code to use at runtime, but they are structured in different ways. 

!!!
    - The **stack** stores values in the order it gets them and removes the values in the opposite order. This is referred to as last in, first out (LIFO). 
    - Adding data is called pushing onto the stack, and removing data is called popping off the stack. All data stored on the stack must have a known, fixed size. 
    - Data with an unknown size at compile time or a size that might change must be stored on the heap instead.

!!!
    - The **heap** is less organized: when you put data on the heap, you request a certain amount of space. The memory allocator finds an empty spot in the heap that is big enough, marks it as being in use, and returns a pointer, which is the address of that location. This process is called **allocating on the heap** and is sometimes abbreviated as just allocating. Because the pointer to the heap is a known, fixed size, you can store the pointer on the stack, but when you want the actual data, you must follow the pointer.
    - Pushing to the stack is faster than allocating on the heap because the allocator never has to search for a place to store new data; that location is always at the top of the stack. Comparatively, allocating space on the heap requires more work because the allocator must first find a big enough space to hold the data and then perform bookkeeping to prepare for the next allocation.
    - Accessing data in the heap is slower than accessing data on the stack because you have to follow a pointer to get there. 
    - When your code calls a function, the values passed into the function (including, potentially, pointers to data on the heap) and the function’s local variables get pushed onto the stack. When the function is over, those values get popped off the stack.

Keeping track of what parts of code are using what data on the heap, minimizing the amount of duplicate data on the heap, and cleaning up unused data on the heap so you don’t run out of space are all problems that ownership addresses. Once you understand ownership, you won’t need to think about the stack and the heap very often, but knowing that the main purpose of ownership is to manage heap data can help explain why it works the way it does.

## Ownership Rules

First, let’s take a look at the ownership rules. Keep these rules in mind as we work through the examples that illustrate them:

- Each value in Rust has an owner.
- There can only be one owner at a time.
- When the owner goes out of scope, the value will be dropped.

## Variable Scope

As a first example of ownership, we’ll look at the **scope** of some variables. 

!!!
    A **scope** is the range within a program for which an item is valid. 
    
Take the following variable:

```rust
let s = "hello";
```

The variable `s` refers to a string literal, where the value of the string is hardcoded into the text of our program. The variable is valid from the point at which it’s declared until the end of the current scope.


```rust
{                      // s is not valid here, it’s not yet declared
    let s = "hello";   // s is valid from this point forward
    // do stuff with s
}                      // this scope is now over, and s is no longer valid
```

## Strings

The types covered previously are of a known size, can be stored on the stack and popped off the stack when their scope is over, and can be quickly and trivially copied to make a new, independent instance if another part of code needs to use the same value in a different scope. But we want to look at data that is stored on the heap and explore how Rust knows when to clean up that data, and the `String` type is a great example.

