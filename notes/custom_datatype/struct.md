## Rust Structs 

## 🧠 What a Struct _Really_ Is in Rust

A struct is **not** just a bag of fields.

A struct is:

- an **owner of data**
    
- a **boundary of responsibility**
    
- a **unit of meaning**
    

Rust structs exist to answer one question:

> _What data belongs together, and who owns it?_

---

## 🧱 Why Structs Exist (Beyond Syntax)

Before structs:

- data is passed around as raw `Vec<T>`
    
- functions operate on loose values
    
- ownership feels confusing
    

After structs:

- one type owns the data
    
- behavior moves _onto_ the data
    
- ownership becomes obvious
    

Rust uses structs to **force clarity**, not convenience.

---

## 📦 Core Project Abstraction

### Dataset

The core idea of the project evolved from:

```
Vec<i32>
```

to:

```
Dataset { list: Vec<i32> }
```

This single change:

- removed ownership confusion
    
- clarified API boundaries
    
- made method design natural
    

---

## 🧩 Struct Definition Pattern

```rust
struct Dataset {
    list: Vec<i32>
}
```

Key idea:

- the struct **owns** the vector
    
- callers do not manipulate raw data directly
    
- all logic moves into `impl Dataset`
    

---

## 🔁 Methods vs Free Functions

### Before

```rust
fn mean(list: &Vec<i32>) -> f64
```

### After

```rust
impl Dataset {
    fn mean(&self) -> f64
}
```

Rule learned:

> If the first argument is always the same type, it should be a method.

---

## 🧠 `self` vs `&self` vs `&mut self`

### Mental model

- `self` → consumes the struct
    
- `&self` → reads the struct
    
- `&mut self` → modifies the struct
    

90% of methods should use `&self`.

---

## ❌ Common Errors Encountered (and Why They Happened)

### ❌ Iterating with `for i in self.list`

Why it failed:

- `self` was borrowed
    
- iterating by value moves ownership
    
- borrowed data cannot be moved
    

Fix:

- iterate over `&self.list`
    

Lesson:

> Iteration decides ownership, not mutation.

---

### Thinking mutation determines borrowing

Mistake:

> “If I don’t modify anything, I should be able to use `self.list`.”

Reality:

- Rust only cares about **ownership**
    
- iteration by value always consumes
    

Lesson:

> Ownership rules are structural, not semantic.

---

### Using `self` in methods that should not consume data

Mistake:

```rust
fn max(self) -> i32
```

Why it was wrong:

- computing max does not logically destroy data
    
- dataset became unusable after the call
    

Lesson:

> Choose `self` based on meaning, not convenience.

---

## Borrowing Inside Methods (Subtle but Important)

Inside:

```rust
fn max(&self) -> i32
```

All accesses are already borrowed.

Examples:

- `self.list.is_empty()` → implicit borrow
    
- `self.list[0]` → borrow + copy
    
- `for i in &self.list` → explicit borrow (required)
    

Rust auto-borrows **unless ownership ambiguity exists**.

---

## Struct Composition (Struct Inside Struct)

Rust does **not** allow nested struct definitions.

This is invalid:

```
struct A {
    struct B { ... }
}
```

Correct pattern:

- define structs separately
    
- embed one inside the other as a field
    

Example concept:

```
Dataset
 ├── list: Vec<i32>
 └── metadata: MetaData
```

Lesson:

> Rust favors composition over nesting.

---

## When to Add a New Struct

Create a new struct when:

- fields represent a distinct concept
    
- data has a different lifecycle
    
- responsibility is different
    

Example:

- numbers → Dataset
    
- name, timestamps → MetaData
    

---

## Associated Functions vs Methods

### Associated Function

- does **not** take `self`
    
- used for construction or factories
    

Example concept:

```rust
Dataset::from_input()
```

### Method

- operates on an existing instance
    
- takes `&self` or `&mut self`
    

Rule learned:

> If a function creates an instance, it should be an associated function.

---

## Design Mistake During Refactor

Mistake:

- introducing `Dataset`
    
- but still calling old free functions in `main`
    

Why this was wrong:

- two competing abstractions existed
    
- struct was unused
    

Lesson:

> If you add a struct, it must become the public API.

---

## 🧠 Final Clean Design Pattern

- `Dataset` owns data
    
- `Dataset::from_input()` constructs it
    
- `dataset.mean()`, `dataset.min()`, `dataset.max()` analyze it
    
- `main` only orchestrates
    

---

## Ownership Lessons Reinforced

- structs clarify ownership
    
- methods inherit the ownership model
    
- borrowing is enforced at the boundary
    
- Rust never guesses intent
    

---

## One-Liners to Remember

- “Structs define responsibility, not just storage.”
    
- “If fields form a concept, they deserve a struct.”
    
- “Methods default to `&self`.”
    
- “Iteration chooses ownership.”
    
- “Composition beats nesting.”
    
- “If a struct exists, `main` should use it.”
