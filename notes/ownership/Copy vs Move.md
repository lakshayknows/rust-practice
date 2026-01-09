## Copy vs Move in Rust

Rust draws a hard line between **copying** and **moving** values. That line lives between the **stack** and the **heap**.

---

### Copy (Stack Values)

Scalar values with a **fixed size known at compile time** are stored on the stack.
Copying them is cheap, so Rust does it automatically.

Examples:

* Integers (`i32`, `u64`)
* Floating-point numbers
* Booleans
* Characters
* Tuples (only if all elements are `Copy`)

```rust
let x = 21;
let y = x;
```

What happens:

* `x` is copied into `y`
* Both `x` and `y` are valid
* They do **not** share memory
* Reusing `x` is perfectly fine

This works because these types implement the `Copy` trait.

---

### Move (Heap Values)

Values with **dynamic size** live on the heap.
Rust does **not** copy heap data by default.

Example:

```rust
let s1 = String::from("hello");
let s2 = s1;
```

What actually happens:

* Ownership of the heap data (`"hello"`) is **moved** from `s1` to `s2`
* `s1` becomes invalid
* `s2` is now the sole owner

Why?
Rust enforces this rule:

> There can only be **one owner** of a value at a time.

This prevents **double frees** and memory bugs at compile time.

Trying to use `s1` after the move will cause a compiler error.

---

### Deep Copy with `clone()`

If you *explicitly* want a full copy of heap data, use `.clone()`.

```rust
let s1 = String::from("hello");
let s2 = s1.clone();
```

What happens now:

* The heap data `"hello"` is **duplicated**
* Two separate heap allocations are created
* `s1` and `s2` each own their own memory
* Both variables remain valid

Important:

* `clone()` is **explicit** because it is more expensive
* Rust never deep-copies heap data implicitly

---

### Mental Model (Very Important)

* **Copy** → Duplicate bits on the stack (cheap, automatic)
* **Move** → Transfer ownership (default for heap data)
* **Clone** → Duplicate heap data (explicit, costly)
