## 🎯 Goal of the Project

Build a **CLI-based statistics calculator** in Rust that:

- Reads a list of numbers from user input
    
- Computes **mean**, **min**, **max**
    
- Uses only **core Rust concepts** (no iterators magic at first)
    
- Teaches ownership, borrowing, parsing, and error handling _by friction_
    

This project was not about “getting it done fast”, but about **learning how Rust thinks**.

---

## 🧠 Core Concepts Learned (High-Level)

- Rust does **not guess intent**
    
- Text input is **always `String` first**
    
- Numbers must be **parsed explicitly**
    
- Containers must be **created and filled intentionally**
    
- Errors are **values**, not side-effects
    
- Logic bugs are different from compiler errors
    

---

## 🧱 Project Architecture (Final)

### Data Flow (Very Important)

```
stdin
→ String
→ Vec<&str>
→ Vec<i32>
→ &Vec<i32>
→ computation (mean / min / max)
```

Rust forces you to **explicitly walk through each step**.

---

## 📌 Functions and Their Responsibilities

### `main`

- Displays menu
    
- Reads user choice
    
- Delegates work
    
- Prints results
    
- Does **not** do parsing or math
    

Main lesson:

> `main` coordinates, it does not compute.

---

### `read_numbers() -> Vec<i32>`

**Hardest but most important function**

Responsibilities:

- Read a line of input
    
- Split by delimiter (`,` in this case)
    
- Parse each piece into `i32`
    
- Collect results into a vector
    

Key ideas:

- `read_line` writes into a `String`
    
- `split` gives `&str`, not numbers
    
- `parse()` returns `Result`, not a value
    
- `Vec<i32>` must be created **outside the loop**
    

---

### `mean(&Vec<i32>) -> f64`

Responsibilities:

- Compute arithmetic mean
    
- Handle empty list explicitly
    

Key ideas:

- Integer division lies
    
- Floating-point intent must be explicit
    
- Sum first, divide once
    
- Casting at the end does **not** fix earlier integer math
    

---

### `min(&Vec<i32>) -> i32` and `max(&Vec<i32>) -> i32`

Responsibilities:

- Linear scan
    
- Track best value so far
    

Key ideas:

- Initial value must come from data (`list[0]`)
    
- Never invent values (like `0` or `i+1`)
    
- Loop variables from `&Vec<T>` are **references**
    
- Dereference when assigning
    

---

## ❌ Errors Made (And Why They Happened)

### ❌ Trying to read numbers directly into `Vec<i32>`

```rust
let mut list: Vec<i32> = io::stdin().read_line(&mut list);
```

Why this failed:

- `read_line` works only with `String`
    
- It returns `Result<usize>`, not parsed data
    
- Rust does not do implicit parsing
    

Lesson:

> Input is always text first.

---

### ❌ Believing `.parse()` can create a vector

```rust
list.trim().parse()
```

Why this failed:

- `parse()` parses **one value**
    
- It cannot parse multiple numbers
    
- Parsing and splitting are separate responsibilities
    

Lesson:

> Parsing is per-element, not per-line.

---

### ❌ Thinking the problem was “list not known at compile time”

This was a **misdiagnosis**.

Reality:

- Rust is fine with runtime-sized vectors
    
- The real problem was **data not being stored anywhere**
    

Lesson:

> If you don’t store it, it doesn’t exist.

---

### ❌ Creating vectors inside loops

```rust
for i in pre_list {
    let mut vector = Vec::new();
    vector.push(i);
}
```

Why this failed:

- Vector was recreated every iteration
    
- It was dropped immediately
    
- Nothing survived the loop
    

Lesson:

> Containers live outside loops. Loops only fill them.

---

### ❌ Trying to cast strings to integers

```rust
i as i32
```

Why this failed:

- `as` is for numeric casts only
    
- Strings must be **parsed**, not cast
    

Lesson:

> `as` ≠ `parse`

---

### ❌ Ignoring `Result` from `parse()`

```rust
let i = i.parse();
vector.push(i);
```

Why this failed:

- `parse()` returns `Result<i32, _>`
    
- Vector expected `i32`
    
- Rust forced an explicit failure policy
    

Lesson:

> If something can fail, Rust makes you deal with it.

---

### ❌ Doing division inside the loop for mean

```rust
avg += i / len;
```

Why this failed:

- Integer division truncates
    
- Precision was lost early
    
- Casting later didn’t fix it
    

Lesson:

> Divide once, after summation.

---

## ✅ Key Fixes That Unlocked Progress

### ✔ Explicit parsing with failure handling

```rust
let num: i32 = item.parse().expect("Invalid number");
```

Meaning:

- Acknowledge parsing can fail
    
- Choose to crash loudly
    
- Compiler is satisfied
    

---

### ✔ Correct accumulator design

```rust
let mut sum = 0.0;
for i in list {
    sum += *i as f64;
}
sum / len
```

Meaning:

- Choose numeric domain early
    
- Avoid integer math entirely for mean
    

---

### ✔ Correct ownership & borrowing

- Pass `&Vec<i32>` to computation
    
- Do not consume data unnecessarily
    
- Dereference only when assigning
    

---

## 🧠 Mental Models That Matter (Write These Down)

- **Rust does nothing implicitly**
    
- **Data flows forward through variables**
    
- **Loops don’t build data unless you tell them to**
    
- **Casting is not parsing**
    
- **Errors are values**
    
- **Containers must have a clear owner**
    
- **If logic is wrong, types won’t save you**
    

---

## 🔑 One-Sentence Rules (High Signal)

- “Strings are parsed, not cast.”
    
- “Create containers once, fill them in loops.”
    
- “If you don’t store it, it doesn’t exist.”
    
- “Division happens once, after summation.”
    
- “`parse()` returns a promise, not a value.”
    
- “Rust won’t guess your intent.”
    

---

## 🚀 Natural Next Steps (When Revisiting Later)

- Replace `&Vec<T>` with `&[T]`
    
- Replace `panic!` with `Result`
    
- Implement `median`
    
- Rewrite using iterators
    
- Add graceful input retry
    
- Add unit tests
    

---

## 🧠 Final Reflection

This project was not about statistics.  
It was about **learning how Rust forces clarity**.

You didn’t just learn syntax.  
You learned **how to think in Rust**.

That’s the hard part.