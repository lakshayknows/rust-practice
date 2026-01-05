
---
### Empty Vector

```rust
let mut v: Vec<i32> = Vec::new();
```

- Creates an empty vector
    
- Type annotation needed if not inferred
    
- Grows dynamically at runtime
    
---
### Using `vec!` Macro (Literal Initialization)

```rust
let v = vec![1, 2, 3];
```

- Most common and readable
    
- Size inferred automatically
    
- Allocates and initializes in one step
    

---

### Repeating Elements with `vec!`

```rust
let v = vec![3; 3]; // [3, 3, 3]
```

- Syntax: `vec![value; count]`
    
- Similar to array repeat syntax
    
- Count decided at runtime
    

---

### Using `Vec::from()`

```rust
let v = Vec::from([1, 2, 3]);
let v = Vec::from("hello"); // Vec<u8>
```

- Converts from arrays, slices, and other compatible types
    
- Based on the `From` trait
    
- Useful when transforming existing data
    

---

### Mental Model

- `Vec::new()` → empty container
    
- `vec!` → literal construction
    
- `Vec::from()` → conversion
    

---

### Key Insight

**`vec!` is syntax sugar.  
`Vec::from` is trait-powered conversion.**

---