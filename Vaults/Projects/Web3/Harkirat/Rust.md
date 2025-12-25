![[Pasted image 20251206221115.png]]

# why would i use rust over Nodejs or python 
- Rust don't let you change the type of a variable - type safety
- **1. Rust is much faster** → Compiles to native machine code.
- **2. No garbage collector** → More predictable performance.
- **3. Memory-safe** → No crashes, no null bugs, no data races.
- **4. Great for multithreading** → No GIL like Python, no single-thread limits like JS.
- **5. More stable and safer** → Errors caught at compile time, not at runtime.
- **6. Produces standalone binaries** → No runtime needed (unlike Node.js/Python).
- **7. Suitable for system-level tasks** → OS tools, game engines, blockchain, embedded.
- **8. Zero-cost abstractions** → High-level code with no runtime performance cost
- **9. Reliable for large-scale projects** → Strict compiler prevents many bugs
- **10. Very good performance for backend servers** → Faster than Node.js/Python.
# Initializing rust locally 
Rust projects can be used to do a lot of things
1. Create Backend for a Full stack app
2. Create CLIs (command line interfaces)
3. Create browsers  [Initially Mozilla created the rust back in 2010 to increase the performance of browser , Rust was originally developed by Mozilla as a systems programming language focused on speed, memory safety, and concurrency without a garbage collector.]
4. Great Code Editors
## To initialize a repository 
- Enter [cargo init] in the terminal
- you will get two files one is [main.rs] and second in [cargo.toml]
## To run a rust file
- first [cargo build] to build an executable file  [ .rs to .exe ] 
  then go to target/debug and run .exe file  [ .exe ]
or 
 - you can just directly run the  [cargo run] inside the src folder
# Data Types(Primitive datatypes ) - string , number(interger and floating point) and boolean

## Rust Integers
Rust gives you fixed-size integer types:
### Signed:
`i8, i16, i32, i64, i128, isize`
### Unsigned:
`u8, u16, u32, u64, u128, usize

Most backend & general dev work uses:
- `i32` (default)
- `i64` (for timestamps, DB IDs)
- `usize` (for indexing arrays, slices)
```rust
let a = 10;        // i32 by default
let b: u8 = 255;   // unsigned
let c: i64 = 10000000000;
```

Type conversions are explicit
     there is no implicit conversions in Rust 
	Rust **never** auto-converts types:
	`let x: i32 = 5; let y: u64 = x as u64;`
	This is Rust forcing you to be intentional.

### what if the value overflows????
- will the provided code gets converted into the byte code ?? if we only try to build the code
  ```rust 
   let mut x:i8 = 127;
    for i in 0..1000{
        x = x + 10;  // this is going to overflow for sure as the limit for i8 is                             127 
    }
  ```
- yes if we try to build this code it will get converted to the bytecode .....but how this is wrong
- see when code for compilation the compiler checks for the error like for syntax not the logic , It is tactically checking the code like a viewer so when it runs first line there is no error in it 127 is in the range and when it checks the for loop there is no error in the syntax it is not executing the code so it seems correct to the compiler and it converts the code into the bytecode 
![[Pasted image 20251207000000.png]]
- now when you will run the build it will show the error and get exit 
![[Pasted image 20251206235932.png]]

## floating point numbers in Rust
Rust gives you **two** floating-point types:
### `f32` → 32-bit float (single precision)
### `f64` → 64-bit float (double precision, **default**)
Rust defaults to `f64` because it's faster on modern CPUs and more precise.
```rust 
let x = 3.14;     // f64 by default
let y: f32 = 2.5; // explicitly f32
let z: f64 = -0.01;
```

#### 🔥 Type conversions are explicit
Rust **never** auto-converts types:

```rust
 let x: i32 = 5; let y: u64 = x as u64;
```
This is Rust forcing you to be intentional.

## Booleans
Rust has a single boolean type: :bool
It can only be-
- `true`
- `false`
A `bool` in Rust is **1 byte (8 bits)** in memory — not 1 bit — because CPUs operate on byte-addressable memory.
Cannot auto-convert to integers (like in C)
```rust 
let is_active: bool = true;
if is_active {
    println!("Active!");
}

let a = 10;
let b = 20;
let result = a < b; // true
```
## Strings

