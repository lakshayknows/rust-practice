Rust does **not** have mutable vector _types_.  
It has different **ways a vector can be accessed and mutated**.

Mutability is always attached to:

- a **binding** (variable name), or
    
- a **reference** (borrow),  
    never to the type itself.
    

---

### 1. Mutable Binding (`mut vec: Vec<T>`)

```rust
fn fill_vec(mut vec: Vec<i32>) -> Vec<i32> {
    vec.push(88);
    vec
}
```

- `mut` applies to the **binding**
    
- The function **owns** the vector
    
- Ownership is **moved into** the function
    
- Mutation is **local**
    
- Changes are visible **only through the return value**
    

Mental model:

> “I own this vector, so I may change it.”

Key points:

- Caller loses access unless it cloned
    
- No borrowing involved
    
- Functional / ownership-oriented style
    

---

### 2. Mutable Reference (`vec: &mut Vec<T>`)

```rust
fn fill_vec(vec: &mut Vec<i32>) {
    vec.push(88);
}
```

- `mut` applies to the **reference**
    
- Ownership stays with the **caller**
    
- Function gets **temporary exclusive access**
    
- Mutation affects the caller’s vector directly
    

Mental model:

> “I don’t own this vector, but while I hold it, nobody else may touch it.”

Key points:

- No cloning
    
- No ownership transfer
    
- Enforced exclusivity (`&mut` rule)
    
- Side-effect based
    

---

### 3. Interior Mutability (Mutation Inside the Vector)

```rust
use std::cell::RefCell;

let vec: Vec<RefCell<i32>> = vec![RefCell::new(1)];
```

- Mutability is **inside the elements**
    
- Can mutate through shared references
    
- Rules enforced at **runtime**, not compile time
    

Mental model:

> “I will allow mutation, but I’ll check it at runtime.”

Key points:

- Uses types like `Cell`, `RefCell`, `Mutex`
    
- Enables patterns impossible with `&mut`
    
- Runtime cost and possible panics
    
- Advanced, not default
    

---

### 4. ❌ Hypothetical: `vec: mut Vec<T>` (Not Allowed)

```rust
// This does NOT exist
fn fill_vec(vec: mut Vec<i32>) {}
```

Why this is forbidden:

- `mut` cannot apply to a **type**
    
- Types do not carry mutation permissions
    
- Mutability must be tied to **access**, not existence
    

Core problem Rust avoids:

> “Mutable for whom?”

Allowing this would:

- Blur ownership boundaries
    
- Break aliasing guarantees
    
- Make mutation authority ambiguous
    

Rust’s rule:

> Mutability must always be explicit and scoped.

---

### Summary

- `mut vec: Vec<T>` → mutable **owner**
    
- `vec: &mut Vec<T>` → mutable **borrow**
    
- `Vec<RefCell<T>>` → mutable **interior**
    
- `mut Vec<T>` → **does not exist**
    

Rust separates:

- **who owns**
    
- **who may mutate**
    
- **for how long**
    

That separation is what makes Rust safe _and_ fast.