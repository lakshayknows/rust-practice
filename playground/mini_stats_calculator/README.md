# Mini Stats Calculator

This project is a simple command-line interface (CLI) tool written in Rust for calculating basic statistics (Mean, Min, Max) from a list of numbers.

**Repository:** [https://github.com/lakshayknows/rust-mini-stats_calculator](https://github.com/lakshayknows/rust-mini-stats_calculator)

## Key Features

-   **Mean Calculation**: Computes the average.
-   **Min/Max Finding**: Finds the smallest and largest numbers.
-   **Interactive Loop**: Runs until the user chooses to quit.

## Code Snippets

Here are some core functions from the implementation:

### Main Loop
```rust
fn main() {
    loop {
        println!("1.Mean");
        println!("2.Min");
        println!("3.Max");
        println!("4.Quit");
        // ... handling input ...
        match choice {
            "1" => {
                let numbers = read_numbers();
                let result = mean(&numbers);
                println!("Mean: {}", result);
            },
            // ... other cases ...
            "4" => {
                println!("Exiting program.");
                break;
            },
            _ => panic!("Invalid Option")
        }
    }
}
```

### Mean Calculation
```rust
fn mean(list: &Vec<i32>) -> f64 {
    if list.is_empty() {
        panic!("Cannot find any vector");
    }

    let mut sum = 0.0;
    let len = list.len() as f64;
    for i in list {
        sum += *i as f64; 
    }
    sum / len
}
```

### Min/Max Logic
```rust
fn max(list: &Vec<i32>) -> i32 {
    if list.is_empty() {
        panic!("Cannot find any vector");
    }
    let mut maximum = list[0];
    for i in list {
        if i > &maximum {
            maximum = *i;
        }        
    }
    maximum
}
```
