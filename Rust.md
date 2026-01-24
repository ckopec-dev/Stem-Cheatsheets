
# 🦀 The Complete Rust Programming Tutorial

Rust is a systems programming language focused on **memory safety, performance, and concurrency**. It gives you low-level control like C/C++, but with strong compile-time guarantees.

---

# 1. Why Rust?

Rust is designed to prevent:

* ❌ Null pointer dereferencing
* ❌ Data races
* ❌ Use-after-free
* ❌ Buffer overflows

While still providing:

* ✅ Zero-cost abstractions
* ✅ C/C++-level performance
* ✅ Modern tooling
* ✅ Excellent concurrency

Used by: Linux kernel, Firefox, Cloudflare, AWS, embedded systems, game engines, blockchains.

---

# 2. Installing Rust

Install Rust using **rustup**:

```bash
https://rustup.rs
```

Verify:

```bash
rustc --version
cargo --version
```

Rust includes:

* `rustc` → compiler
* `cargo` → package manager + build system
* `rustfmt` → formatter
* `clippy` → linter

---

# 3. Your First Program

Create a project:

```bash
cargo new hello_rust
cd hello_rust
cargo run
```

`src/main.rs`

```rust
fn main() {
    println!("Hello, Rust!");
}
```

Rust uses:

* `fn` → function
* `!` → macro
* `;` → statement end

---

# 4. Variables and Mutability

```rust
let x = 5;        // immutable
let mut y = 10;  // mutable

y += 1;

const PI: f64 = 3.14159;
```

Rust is immutable by default → safer code.

---

# 5. Data Types

## Scalar Types

```rust
let a: i32 = -42;
let b: u64 = 100;
let c: f32 = 3.14;
let d: bool = true;
let e: char = 'R';
```

## Compound Types

```rust
let tup: (i32, f64, char) = (1, 2.5, 'a');
let arr: [i32; 3] = [1, 2, 3];
```

Access:

```rust
println!("{}", tup.0);
println!("{}", arr[1]);
```

---

# 6. Functions

```rust
fn add(a: i32, b: i32) -> i32 {
    a + b   // no semicolon = return
}
```

```rust
let result = add(5, 7);
```

---

# 7. Control Flow

## If

```rust
if x > 5 {
    println!("Big");
} else {
    println!("Small");
}
```

## Loops

```rust
loop { break; }

while x < 10 {
    x += 1;
}

for i in 0..5 {
    println!("{}", i);
}
```

---

# 8. Ownership (The Core of Rust)

Rust’s most important feature.

Rules:

1. Each value has one owner
2. Only one owner at a time
3. Dropped when owner goes out of scope

```rust
let s = String::from("hello");
let t = s;
// s is now invalid ❌
```

To copy:

```rust
let t = s.clone();
```

---

# 9. Borrowing and References

```rust
fn print(s: &String) {
    println!("{}", s);
}

let msg = String::from("hi");
print(&msg);  // borrowed
```

Mutable borrow:

```rust
fn change(s: &mut String) {
    s.push_str(" world");
}

let mut text = String::from("hello");
change(&mut text);
```

Rules:

* Many immutable references OR
* One mutable reference
* Never both

---

# 10. Slices

```rust
let s = String::from("hello");
let part = &s[0..2];
```

Works for arrays too:

```rust
let a = [1,2,3,4];
let slice = &a[1..3];
```

---

# 11. Structs

```rust
struct User {
    name: String,
    age: u8,
}

let user = User {
    name: String::from("Chris"),
    age: 30,
};
```

Methods:

```rust
impl User {
    fn greet(&self) {
        println!("Hi, {}", self.name);
    }
}
```

---

# 12. Enums and Match

```rust
enum Direction {
    North,
    South,
    East,
    West,
}
```

```rust
match dir {
    Direction::North => println!("Up"),
    _ => println!("Other"),
}
```

---

# 13. Option and Result

Rust has no null.

```rust
let x: Option<i32> = Some(5);
```

```rust
match x {
    Some(v) => println!("{}", v),
    None => println!("No value"),
}
```

Result:

```rust
fn divide(a: f64, b: f64) -> Result<f64, String> {
    if b == 0.0 {
        Err("Divide by zero".into())
    } else {
        Ok(a / b)
    }
}
```

Shortcut:

```rust
let val = divide(4.0, 2.0)?;
```

---

# 14. Collections

## Vector

```rust
let mut v = vec![1,2,3];
v.push(4);
```

## HashMap

```rust
use std::collections::HashMap;

let mut scores = HashMap::new();
scores.insert("Alice", 10);
```

---

# 15. Strings

```rust
let mut s = String::from("Hi");
s.push('!');
s.push_str(" Rust");
```

Iterating:

```rust
for c in s.chars() {
    println!("{}", c);
}
```

---

# 16. Error Handling

Recoverable:

```rust
use std::fs::File;

let f = File::open("data.txt")?;
```

Unrecoverable:

```rust
panic!("Crash and burn");
```

---

# 17. Generics

```rust
fn largest<T: PartialOrd>(list: &[T]) -> &T {
    let mut max = &list[0];
    for item in list {
        if item > max { max = item; }
    }
    max
}
```

---

# 18. Traits (Interfaces)

```rust
trait Speak {
    fn speak(&self);
}

impl Speak for Dog {
    fn speak(&self) {
        println!("Woof");
    }
}
```

Trait bounds:

```rust
fn make_speak<T: Speak>(x: T) {
    x.speak();
}
```

---

# 19. Lifetimes

```rust
fn longest<'a>(a: &'a str, b: &'a str) -> &'a str {
    if a.len() > b.len() { a } else { b }
}
```

Ensures references stay valid.

---

# 20. Modules and Crates

```rust
mod math {
    pub fn add(a: i32, b: i32) -> i32 {
        a + b
    }
}
```

```rust
use math::add;
```

Cargo.toml dependency:

```toml
[dependencies]
rand = "0.8"
```

---

# 21. File I/O

```rust
use std::fs;

let content = fs::read_to_string("file.txt")?;
fs::write("out.txt", "Hello")?;
```

---

# 22. Concurrency

## Threads

```rust
use std::thread;

let handle = thread::spawn(|| {
    println!("Hello from thread");
});

handle.join().unwrap();
```

## Channels

```rust
use std::sync::mpsc;

let (tx, rx) = mpsc::channel();

tx.send(42).unwrap();
println!("{}", rx.recv().unwrap());
```

## Mutex + Arc

```rust
use std::sync::{Arc, Mutex};

let counter = Arc::new(Mutex::new(0));
```

---

# 23. Async Rust

```toml
tokio = { version = "1", features = ["full"] }
```

```rust
#[tokio::main]
async fn main() {
    let data = reqwest::get("https://example.com").await?.text().await?;
    println!("{}", data);
}
```

---

# 24. Testing

```rust
#[test]
fn add_test() {
    assert_eq!(2 + 2, 4);
}
```

```bash
cargo test
```

---

# 25. Building a CLI App

```bash
cargo add clap
```

```rust
use clap::Parser;

#[derive(Parser)]
struct Args {
    name: String,
}

fn main() {
    let args = Args::parse();
    println!("Hello {}", args.name);
}
```

---

# 26. Unsafe Rust

```rust
unsafe {
    let p = 0x12345 as *const i32;
}
```

Used for:

* FFI
* OS kernels
* embedded
* performance-critical code

Rust still enforces safety around unsafe blocks.

---

# 27. Project Structure

```
src/
  main.rs
  lib.rs
  bin/
tests/
examples/
```

---

# 28. Performance and Optimization

* Zero-cost abstractions
* LLVM backend
* Manual memory layout
* Inline assembly
* SIMD
* Multithreading

Profiling tools:

* `cargo bench`
* `perf`
* `flamegraph`

---

# 29. Rust Use Cases

* Operating systems
* Embedded firmware
* Game engines
* Web servers
* Blockchains
* WASM
* Machine learning backends

---

# 30. Learning Path

1. Syntax & ownership
2. Traits + generics
3. Collections & error handling
4. Lifetimes
5. Concurrency
6. Async
7. Unsafe + FFI
8. Build real projects

---

# 31. Capstone Project Ideas

* Multithreaded web server
* Redis-like in-memory DB
* Game engine core
* WASM physics engine
* Linux command-line toolkit
* Blockchain prototype

