# Struct-Based Mini Stats Calculator

This project demonstrates a more structured approach to the Mini Stats Calculator using Rust `structs`.

**Original Project:** [Mini Stats Calculator](https://github.com/lakshayknows/rust-mini-stats_calculator)

## Key Features

-   **Encapsulation**: Data and methods are bundled in a `Dataset` struct.
-   **Methods**: `mean`, `min`, and `max` are implemented as methods on the struct.
-   **Ownership**: Demonstrates borrowing (`&self`) and ownership transfer.

## Code Snippets

### Struct Definition
```rust
struct Dataset {
    list: Vec<i32> // owns the list
}
```

### Method Implementation
```rust
impl Dataset {
    fn mean(&self) -> f64 {
        if self.list.is_empty(){
            panic!("Empty list")
        }
        let mut sum = 0.0;
        let len = self.list.len() as f64; 
        for i in &self.list{
            sum += *i as f64;    
        } 
        sum/len
    }

    fn min (&self) -> i32 {
        if self.list.is_empty(){
            panic!("Empty list")
        }
        let mut minimum = self.list[0];
        for i in &self.list{
            if *i < minimum{
                minimum = *i;
            }
        } minimum
    }

    fn max(&self) -> i32 {
        if self.list.is_empty(){
            panic!("Empty list")
        }
        let mut maximum = self.list[0];
        for i in &self.list{
            if *i > maximum{
                maximum = *i;
            }
        } maximum
    }
}
```

## Related
- [Rust Learning Log](https://github.com/lakshayknows/rust-learning-log)
