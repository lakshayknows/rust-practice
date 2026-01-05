### Returning `Vec<T>` from Functions (Why Rust Is Happy)

---

### The Core Idea

Rust can return a `Vec<T>` from a function even though its length is not known at compile time because **the size of the `Vec<T>` itself _is_ known at compile time**.

What is returned is **not the elements**, but a **fixed-size handle** to them.

---

### What a `Vec<T>` Really Is

A `Vec<T>` is a struct with three fields:

- Pointer → points to heap memory where elements live
    
- Length → number of elements currently stored
    
- Capacity → total allocated space on the heap
    

This layout is **fixed**.

On a 64-bit system:

- Pointer: 8 bytes
    
- Length: 8 bytes
    
- Capacity: 8 bytes
    

Total size: **24 bytes (always)**

The number of elements does **not** affect the size of the `Vec<T>` value itself.

---

### Stack vs Heap (Mental Model)

- **Stack**
    
    - Needs sizes known at compile time
        
    - Stores fixed-size values
        
- **Heap**
    
    - Allows dynamic size
        
    - Managed at runtime
        

`Vec<T>`:

- The struct lives on the **stack**
    
- The elements live on the **heap**
    

---

### Why Returning a `Vec<T>` Works

When you write:

```rust
fn make_vec() -> Vec<i32> {
    vec![1, 2, 3, 4, 5]
}
```

Rust:

- Knows the function returns a **fixed-size struct**
    
- Moves the `Vec<T>` (24 bytes) to the caller
    
- Does **not** copy the elements
    
- Leaves heap memory untouched
    

Only ownership changes.

---

### Contrast with Arrays

#### Array

```rust
fn make_array() -> [i32; 5]
```

- Entire array is stored inline
    
- Size is part of the type
    
- Returning means copying **all elements**
    
- Size grows with element count
    

Arrays are **the data itself**.

---

#### Vector

```rust
fn make_vec() -> Vec<i32>
```

- Only a handle is returned
    
- Heap data stays in place
    
- Cheap, predictable move
    
- Size independent of element count
    

Vectors are **a pointer to the data**.

---

### Ownership Transfer

When a function returns a `Vec<T>`:

- Ownership moves to the caller
    
- No deep copy happens
    
- Heap memory remains valid
    
- Rust enforces safety at compile time
    

---

### One-Line Rule

**Rust can return `Vec<T>` because its metadata has a fixed size, even though its data does not.**

---

### Related Concepts (Mental Links)

- `String` behaves the same way as `Vec<u8>`
    
- Slices (`&[T]`) cannot be returned without lifetimes
    
- This design enables zero-cost abstractions
