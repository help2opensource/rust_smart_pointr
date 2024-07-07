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

#### `Arc<T>`

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

#### `RefCell<T>`
**Purpose**: Interior mutability in single-threaded contexts with runtime borrow checking.

```rust
use std::cell::RefCell;

let ref_cell = RefCell::new(5);

// Mutable borrow
*ref_cell.borrow_mut() += 1;
println!("ref_cell: {}", ref_cell.borrow());

```
#### `Cell<T>`

**Purpose**: Interior mutability for Copy types in single-threaded contexts.
```rust
use std::cell::Cell;

let cell = Cell::new(5);
cell.set(10);
println!("cell: {}", cell.get());
```

#### `Mutex<T>`
**Purpose**: Interior mutability with thread safety.
```rust
use std::sync::{Arc, Mutex};
use std::thread;

let mutex = Arc::new(Mutex::new(5));

{
    let mut num = mutex.lock().unwrap();
    *num += 1;
}

let handles: Vec<_> = (0..10).map(|_| {
    let mutex = Arc::clone(&mutex);
    thread::spawn(move || {
        let mut num = mutex.lock().unwrap();
        *num += 1;
    })
}).collect();

for handle in handles {
    handle.join().unwrap();
}

println!("mutex: {}", *mutex.lock().unwrap());
```

#### `RwLock<T>`
**Purpose**: Multiple readers or exclusive writer with thread safety.
```rust
use std::sync::{Arc, RwLock};
use std::thread;

let rwlock = Arc::new(RwLock::new(5));

{
    let r1 = rwlock.read().unwrap();
    let r2 = rwlock.read().unwrap();
    println!("r1: {}, r2: {}", *r1, *r2);
}

{
    let mut w = rwlock.write().unwrap();
    *w += 1;
    println!("w: {}", *w);
}

let handles: Vec<_> = (0..10).map(|_| {
    let rwlock = Arc::clone(&rwlock);
    thread::spawn(move || {
        let mut w = rwlock.write().unwrap();
        *w += 1;
    })
}).collect();

for handle in handles {
    handle.join().unwrap();
}

println!("rwlock: {}", *rwlock.read().unwrap());
```

### Summary

- **`Box<T>`**: Use for heap allocation.
- **`Rc<T>`**: Use for multiple ownership in single-threaded contexts.
- **`Arc<T>`**: Use for multiple ownership in multi-threaded contexts.
- **`RefCell<T>`**: Use for single-threaded interior mutability with runtime borrow checking.
- **`Cell<T>`**: Use for single-threaded interior mutability for `Copy` types.
- **`Mutex<T>`**: Use for thread-safe interior mutability.
- **`RwLock<T>`**: Use for thread-safe multiple readers or single writer.



