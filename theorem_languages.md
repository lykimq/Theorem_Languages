# Theorem Languages

## Static Types vs Dynamic Types

Static types: We must explicitly declare the types of all variables. The data type is known at **compile time** (complile time is when the code is converted into machine code, before the program run). Whereas dynamic types are known at **runtime** (runtime is when the code is executed, after the program run).

Static types catch errors early, where dynamic types can have runtime errors. To secure dynamic code, we need testing and maybe type-checking tools. For static, use the compiler's checks and maybe linters.

Languages that use static types:
- C, C++, Java, C#, Go, Rust, OCaml, Haskell, TypeScript, etc.

Example:
```java
int number = 10 ; // The type of variable is declare explicitly
String name = "John"
```

Languages that use dynamic types:
- Python, Ruby, JavaScript, PHP, Perl, etc.

Example:
```javascript
number = 10; // The type just assigned
add = "John" + 10; // Runtime error
```

## OCaml Languages
OCaml is a functional programming language with strong static typing and type inference.

**Strength**
- Type safety: Strong static type system catches many errors at compile time, making program safer and more robust.
- Performance: it has efficient native code generation, which allows it to compete with C and C++ in terms of speed.
- Expressiveness: The language supports functional programming paradigms, allowing for concise and expressive code.
- Pattern matching: OCaml has powerful pattern matching feature, that simplifies code by allowing easy deconstruction of data types.
- Type inference: developers don't need to annotate types explicitly, as OCaml can infer them, resulting in cleaner code.
- Rich ecosystem: OCaml has a growing ecosystem of libraries and tools, including package management with OPAM.

**Weakness**
- Steep Learning Curve: it can be challenging for beginners, especially for those coming from imperative languages (imperative languages are languages that use statements to change a program's state).
- Limited Libraries: while the ecosystem is growing, it may not be as extensive as languages like Python or JavaScript.
- Tooling and IDE support: Compared to other languages, OCamls has less comprehensive tooling and IDE support.
- Community size: The community is smaller than those of other languages.

**Best Practices**
- Leverage Type Inference: Use OCaml's type inference to write consice code without unecessary type annotations.
- Use Pattern Matching
- Modular Design: Organize code into modules to improve readability and reusability, and maintainability.
- Write Tests: Utilize OCaml's testing frameworks to write unit tests and ensure code reliability.
- Documentation: Document code using comments and OCaml's built-in documentation tools.

## Rust Language

Rust is a systems programming language designed for performance, safety, and concurrency (parallel, switch and asynchronous programming). Rust aims to provide memory safety without sacrificing performance, making it suitable for a wide range of applications, from low-level systems programming to high-level applications.

**Key Features**
- Memory Safety: Rust's ownership model ensures that memory safety without needing a garbage collector. It uses concepts like ownership (means the unique owner of the resource), borrowing (means borrowing the resource without owning it), and lifetimes (means the duration for which the resource is valid) to manage memory.
- Concurrency: The ownership system prevents data races (when two threads access the same memory location concurrently, without synchronization, resulting in unpredictable behavior) at compile time, allowing developers to write concurrent code without fear of common pitfalls.
- Performance: Rust offers performance comparable to C and C++.
- Type System: Rust has a strong, static type system with type inference.
- Pattern matching: Rust supports pattern matching, making it easier to handle complex data structures.
- Tooling: Rust comes with excellent tooling, including Cargo package manager and build system.

**Strength**
- Memory safety without GC: Rust's ownership and borrowing system prevents common memory issues, such as null pointer dereferences, and buffer overflows.
- High Performance: It can achieve performance comparable to C and C++.
- Strong community
- Cross-platform: it can run on various operating systems.
- Excellent Documentation.

**Weakness**
- Steep learning curve: Concepts like ownership, borrowing, and lifetimes can be challenging for beginners.
- Compile times: it can be longer than some other languages.
- Ecosystem Maturity
- Error Messages: it can be verbose or complex, which can make debugging more difficult.

**Ownership**
- Each value in Rust has a single owner (a variable)
- When the owner goes out of scope, the value is dropped.
- Ownership can be transferred to another variable (but not copied).

```rust
fn main () {
    let s1 = String::from("Hello"); // s1 is the owner of the String
    let s2 = s1; // Ownership transferred to s2

    println!("{}", s2); // This will work
    // println!("{}", s1); // This will not work because s1 is no longer in scope
}
```

**Borrowing**
- Borrowing is a way to access and use a value without taking ownership.
- Borrowing is done using references.
- There are two types of references:
    - Immutable references (denoted by `&`)
    - Mutable references (denoted by `&mut`)

```rust
fn main () {
    let s = String::from("Hello");
    // Immutable reference
    let r1 = &s;
    let r2 = &s;
    println!("{} and {}", r1, r2); // This is valid

    // Mutable reference
    let r3 = &mut s; // Mutable reference
    r3.push_str(", world"); // Modify the original value
    println!("{}", r3); // This is valid
}
```

**Lifetimes**
- Lifetimes are the duration for which a reference is valid. They prevent dangling references by ensuring that the data a reference points to is valid for the lifetime of the reference.

```rust
fn main () {
    let result; // (uninitialized)
    {
        let s1 = String::from("Hello");
        let s2 = String::from("World");

        let result = longest(&s1, &s2);// this is the new result and not the one in the main function
        println!("The longest string is {}", result); // This is valid
    }
    // println!("The longest string is {}", result); // This will not work because the result is out of scope
}

fn longest<'a>(s1: &'a str, s2: &'a str) -> &'a str {
    if s1.len() > s2.len(){
        s1
    } else {
        s2
    }
}
```

### Marco
Macros are a powerful feature that allows you to write code that generates other code at compile time. Rust macros come in two main types: declarative macros and procedural macros.

**Declarative Macros**
- It is a type of macro that uses `macro_rules!` to define the macro. They allow to match patterns and generate code based on the patterns. They are like functions that can be called with different arguments.

```rust
macro_rules! say_hello {
    () =>{
        println!("Hello, world!");
    };
}

fn main() {
    // Call the macro
    say_hello!();
}
```

**Procedural Macros**
- They are more advanced and flexible than declarative macros, that take some code as input and produce code as output. Procedural macros require a separate crate to define.

Types of procedural macros:
- Custom derive macros: Create custome behavior for Rust' `#[derive]` attribute.
- Attribute-like macros: They are like attributes but can take arguments and modify the item they are attached to.
- Function-like macros: These look like function calls but works at the syntax level.

Example of a function-like macro:
```rust
macro_rules! create_function {
    ($func_name:ident) => {
        fn $func_name() {
            println!("Function {} called", stringify!($func_name));
        }
    }
}

create_function!(foo);

foo(); // This will call the macro and print "Function foo called"
```

Example of an attribute-like macro:
```rust
// This is the attribute automatically implement the Debug trait for Person struct
#[derive(Debug)]
struct Person {
    name: String,
    age: u8,
}
```

Example of a custom derive macro:
```rust
use my_macro::hello_macro;

#[hello_macro]
pub fn my_function() {
    println!("Hello, world!");
}
```




