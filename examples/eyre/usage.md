```rust
use eyre::{eyre, Report, WrapErr};

fn main() -> Result<(), Report> {
    let e: Report = eyre!("oh no this program is just bad!");

    Err(e).wrap_err("usage example successfully experienced a failure")
}
```
```rust
Finished dev [unoptimized + debuginfo] target(s) in 0.48s
     Running `target/debug/examples/usage`
Error: usage example successfully experienced a failure

Caused by:
    oh no this program is just bad!

Location:
    examples/usage.rs:4:21
```
