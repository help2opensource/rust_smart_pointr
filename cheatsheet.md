### Rust Smart Pointers Cheatsheet

#### `Box<T>`

**Purpose**: Allocate data on the heap.

- **Usage**:
  ```rust
  let boxed = Box::new(5);
  println!("boxed: {}", boxed);
