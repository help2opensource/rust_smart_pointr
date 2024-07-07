### Rust Smart Pointers Cheatsheet

#### `Box<T>`

**Purpose**: Allocate data on the heap.

  ```rust
  let boxed = Box::new(5);
  println!("boxed: {}", boxed);
  
  let x = *boxed;
  println!("x: {}", x);
 ```

#### `Rc<T>`

**Purpose**: Multiple ownership in single-threaded contexts with reference counting.
```rust
use std::rc::Rc;

let rc = Rc::new(5);
let rc1 = Rc::clone(&rc);
let rc2 = Rc::clone(&rc);

println!("rc: {}", rc);
println!("Reference count: {}", Rc::strong_count(&rc));
```

### `Arc<T>`

**Purpose**: Multiple ownership in multi-threaded contexts with atomic reference counting.

```rust
use std::sync::Arc;
use std::thread;

let arc = Arc::new(5);
let arc1 = Arc::clone(&arc);

let handle = thread::spawn(move || {
    println!("arc1: {}", arc1);
});

handle.join().unwrap();
println!("arc: {}", arc);
```

