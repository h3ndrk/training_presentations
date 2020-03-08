# Structured Objects
```rust
pub struct Issue {
    pub id: u32,
    pub priority: IssuePriority,
    pub title: String,
    pub description: String,
    pub sub_tasks: Vec<String>
}

impl Issue {
    pub fn set_priority(&mut self, new_priority: IssuePriority) {
        self.priority = new_priority.to_owned();
    }
}

impl Default for Issue {
    fn default() -> Issue {
        Issue {
            id: 1,
            priority: IssuePriority::Bug,
            title: "".to_string(),
            description: "".to_string(),
            sub_tasks: vec![]
        }
    }
}
```

# Enumerations
> Rust’s enums are most similar to **algebraic data types** in functional languages, such as F#, OCaml, and Haskell.

> The most similar equivalent in C++ is the std::variant.

## Example: IssuePriority
```rust
enum IssuePriority {
    Enhancement,
    Bug,
    TODO,
    Milestone(u8)
}

fn main() {
    let mut milestone = Issue::default();
    milestone.set_priority(IssuePriority::Milestone(42));
}
```

## Option enum
### The Billion-dollar mistake
> I call it my billion-dollar mistake. At that time, I was designing the first comprehensive type system for references in an object-oriented language. My goal was to ensure that all use of references should be absolutely safe, with checking performed automatically by the compiler. But I couldn’t resist the temptation to put in a null reference, simply because it was so easy to implement. This has led to innumerable errors, vulnerabilities, and system crashes, which have probably caused a billion dollars of pain and damage in the last forty years.

### Rust does not have **null**
```rust
enum Option<T> {
    Some(T),
    None,
}
```
> Generally, this helps catch one of the most common issues with null: assuming that something isn’t null when it actually is.

* Improves confidence in the code

## Other enums: Result
### Explicit error handling
```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```

## Pattern matching
### Match Control Flow Operator
```rust
fn print_issue_priority(priority: IssuePriority) {
    match priority {
        IssuePriority::Enhancement => println!("Enhancement!"),
        IssuePriority::Bug => println!("Bug!"),
        IssuePriority::Milestone(number: u8) => println!("Milstone #{}!", number),
        _ => println!("TODO!"),
    }
}
```
TODO: https://blog.rust-lang.org/inside-rust/2020/03/04/recent-future-pattern-matching-improvements.html

### Functions
```rust
fn compute_length(&input: &String) -> usize {
    input.len() // Input is of the type 'String'
}
fn compute_length(input: &String) -> usize {
    input.len() // Input is of the type '&String'
}
```