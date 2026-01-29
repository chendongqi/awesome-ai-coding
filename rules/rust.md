# Rust Development Rules

## Ownership & Borrowing (CRITICAL)

ALWAYS understand ownership:

```rust
// WRONG: Moving value
fn take_ownership(s: String) {
    println!("{}", s);
}

let s = String::from("hello");
take_ownership(s);
println!("{}", s);  // ERROR: s moved!

// CORRECT: Borrowing
fn borrow_string(s: &String) {
    println!("{}", s);
}

let s = String::from("hello");
borrow_string(&s);
println!("{}", s);  // OK: s still owned

// CORRECT: Mutable borrow
fn modify_string(s: &mut String) {
    s.push_str(" world");
}

let mut s = String::from("hello");
modify_string(&mut s);
```

## Error Handling (MANDATORY)

ALWAYS use Result<T, E> for recoverable errors:

```rust
// WRONG: Panic on error
fn parse_number(s: &str) -> i32 {
    s.parse().unwrap()  // Panics on error!
}

// CORRECT: Result type
fn parse_number(s: &str) -> Result<i32, ParseIntError> {
    s.parse()
}

// CORRECT: Error propagation
fn process_data(input: &str) -> Result<i32, Box<dyn Error>> {
    let num = input.parse()?;  // ? operator propagates error
    Ok(num * 2)
}

// CORRECT: Custom error types
use thiserror::Error;

#[derive(Error, Debug)]
enum MyError {
    #[error("Invalid input: {0}")]
    InvalidInput(String),
    #[error("IO error: {0}")]
    Io(#[from] std::io::Error),
}
```

## Memory Safety (CRITICAL)

NEVER create data races or memory leaks:

```rust
// WRONG: Data race (won't compile, but example)
let mut data = vec![1, 2, 3];
let handle = thread::spawn(|| {
    data.push(4);  // ERROR: cannot borrow mutably
});

// CORRECT: Arc + Mutex for shared mutable state
use std::sync::{Arc, Mutex};
use std::thread;

let data = Arc::new(Mutex::new(vec![1, 2, 3]));
let data_clone = Arc::clone(&data);

let handle = thread::spawn(move || {
    let mut data = data_clone.lock().unwrap();
    data.push(4);
});

handle.join().unwrap();
```

## Pattern Matching (MANDATORY)

ALWAYS use exhaustive pattern matching:

```rust
// WRONG: Partial match
match value {
    Some(x) => println!("{}", x),
    // Missing None case!
}

// CORRECT: Exhaustive match
match value {
    Some(x) => println!("{}", x),
    None => println!("No value"),
}

// CORRECT: if let for single case
if let Some(x) = value {
    println!("{}", x);
}
```

## Traits (CRITICAL)

ALWAYS use traits for abstraction:

```rust
// CORRECT: Trait definition
trait Drawable {
    fn draw(&self);
}

// CORRECT: Trait implementation
struct Circle {
    radius: f64,
}

impl Drawable for Circle {
    fn draw(&self) {
        println!("Drawing circle with radius {}", self.radius);
    }
}

// CORRECT: Trait bounds
fn draw_shape<T: Drawable>(shape: &T) {
    shape.draw();
}
```

## Async Programming (MANDATORY)

ALWAYS use async/await properly:

```rust
// CORRECT: Async function
use tokio;

async fn fetch_data(url: &str) -> Result<String, reqwest::Error> {
    let response = reqwest::get(url).await?;
    response.text().await
}

// CORRECT: Spawning async tasks
#[tokio::main]
async fn main() {
    let handle = tokio::spawn(async {
        fetch_data("https://example.com").await
    });
    
    let result = handle.await.unwrap();
}
```

## Code Quality Checklist

Before marking work complete:
- [ ] Ownership and borrowing rules followed
- [ ] Result<T, E> used for error handling
- [ ] No unwrap() in library code (use ?)
- [ ] Exhaustive pattern matching
- [ ] Traits used for abstraction
- [ ] No data races or memory leaks
- [ ] Async/await used correctly
- [ ] Clippy warnings resolved
- [ ] rustfmt applied
- [ ] Documentation comments on public APIs
