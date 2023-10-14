# Rust basics

## Naming conventions

1. Variable and function names: `snake_case`
2. Type and trait names: `camelCase` or `PascalCase`
3. Constants: `UPPERCASE`

## Variables and mutability

1. **Variables**: Immutable by default, can be mutable with `mut`

    ```rust
    let x = 5;
    x = 6 // compile error

    let mut y = 11;
    y = 16 // compiles
    ```

2. **Constants**: *always* immutable. Must be set to a expression, not a return of a function or any value computed at runtime, ex. `const WINDOW_HEIGHT: u32 = 720;`
3. **Shadowing**: When re-declaring a variable with the same name, the compiler will use the value of the 2d variable within the particular scope.

    ```rust
    let x = 5;

    let x = x * 5; // shadows the 1st occurance of x, will return 25

    {
        let x = x - 5;
        println!("x={}", x); // prints 'x=20';
    }
    println!("x={}", x); // prints 'x=25'
    ```

## Common data types

1. **Scalars**:
    + **Integers**: Defaults to `i32`
        - Signed integers: `i8`, `i16`, `i32`, `i64`, `i128`
        - Unsigned integers: `u8`, `u16`, `u32`, `u64`, `u128`
        - Pointer-sized signed integer type: `isize`
        - Pointer-sized unsigned integer type: `usize`

        | Length  | Signed  | Unsigned |
        |---------|---------|----------|
        | 8-bit   | `i8`    | `u8`     |
        | 16-bit  | `i16`   | `u16`    |
        | 32-bit  | `i32`   | `u32`    |
        | 64-bit  | `i64`   | `u64`    |
        | 128-bit | `i128`  | `u128`   |
        | arch    | `isize` | `usize`  |

    + **Floating point**: Defaults to `f64`
        - Single-precision floating point: `f32`
        - Double-precision floating point: `f64`
    + **Boolean type**: size 1 byte
        - `bool`, can be `true` or `false`
    + **Character type**: single unicode character enclosed in single quotes. Size is 4 bytes and range is from `U+0000` to `U+D7FF` and from `U+E000` to `U+10FFFF` inclusive.
        - `char`: ex, `'r'`, `'π'`, `'😻'`
2. **Compound types**:
    + **Tuples**: fixed-size collection of values of any data type. Ex., `let tuple: (i32, f64, char) = (42, 3.14, 'A');`
    + **Arrays**: fixed-size collections of same data type. Ex., `let arr: [i32; 3] = [5, 26, 147];`
3. **Ownership types**:
    + **Ownership**: variables own their data and clean it up when they are out of scope
4. **References**: borrowing values without taking ownership
    + **Immutable**: read only data, ex. `&i32`
    + **Mutable**: read and write, ex. `&mut String`. Only one mutable reference per scope
5. **Strings and Slices**:
    + **String**: growable UTF-8 encoded test string, ex. `let my_string = String::from("Hello world");`
    + **Slice**: references to a portion of a data structure, ex `&str`, a string slice
6. **Compound data types**:
    + **C-like structs**: combine values, ex. `struct Cat {name: String, age: u8, neutred: bool,}`
    + **Tuple structs**: ex., `struct Color(u8, u8, u8)`, `struct Kilometers(i32)`
    + **Enums**: list of possibilities for a data type, ex. `enum Color {Red, Green, Blue,}`
7. **Option and Result**:
    + **Option**: `Some` or `None`, hadles optional values or error conditions
    + **Result**: `Ok` or `Err`

## Type casting

1. **Safe casting** `as`
    + **Coercion**:
        - **Numeric cast**:

            ```rust
            let x: i32 = 5;
            let y = x as i64;
            let a = 70 as char;
            ```
 
        - **Pointer casts**: 

            ```rust
            let a = 300 as *const char;
            let b = a as u32;
            ```

2. **Arbitrary casting** `transmute`

    ```rust
    let a = [0u8, 0u8, 0u8, 0u8];
    let b = mem::transmute::<[u8; 4], u32>(a);
    println!("{}", b); // 256
    // or, more concisely:
    let c: u32 = mem::transmute(a);
    println!("{}", c); // 256
    ```

## Operators

1. **Arithmetic operators**:
    + `+`: addition
    + `-`: substraction
    + `*`: multiplication
    + `/`: division
    + `%`: modulus

2. **Comparison operators**:
    + `==`: equal
    + `!=`: not equal
    + `<`: less than
    + `>`: greater than
    + `<=`: less or equal than
    + `>=`: greater or equal than

    ```rust
    let mut res = false;

    // true is 1 and false is 0
    res = true > false; // true

    // z is 122 and Z is 90
    res = 'z' > 'Z'; // true
    ```

3. **Logical operators**:
    + `!`: logical 'not'
    + `|`: logical 'or
    + `&`: logical 'and'
    + `^`: logical 'xor'

## Functions

Defined with the `fn` keyword. The `main` function is the first piece of code run in the executable. Order of functions is not important in Rust, functions called in `main` can be defined below `main`

```rust
fn main() {
    println!("{} + 25 = {}", 5, my_function(5));
}

// `my_function` takes argument an uint32 and returns an uint32
fn my_function(x: u32) -> u32 {
    return x + 25;
}

// when run, prints `5 + 25 = 30`
```

## Methods

Functions defined within the context of a `struct`

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }

    // &mut self to make instance writable
    fn redefine_area(&mut self, new_width: u32) {
        self.width = new_width;
    }

    // associated functions (no self as first parametre)
    fn square(size: u32) -> Rectangle {
        Rectangle {
            width: size,
            height: size,
        }
    }
}
```
