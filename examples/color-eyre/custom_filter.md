```rust
use color_eyre::{eyre::Report, eyre::WrapErr, Section};
use tracing::{info, instrument};

#[instrument]
fn main() -> Result<(), Report> {
    std::env::set_var("RUST_BACKTRACE", "1");
    #[cfg(feature = "capture-spantrace")]
    install_tracing();

    color_eyre::config::HookBuilder::default()
        .add_frame_filter(Box::new(|frames| {
            let filters = &["custom_filter::main"];

            frames.retain(|frame| {
                !filters.iter().any(|f| {
                    let name = if let Some(name) = frame.name.as_ref() {
                        name.as_str()
                    } else {
                        return true;
                    };

                    name.starts_with(f)
                })
            });
        }))
        .install()
        .unwrap();

    Ok(read_config()?)
}

#[cfg(feature = "capture-spantrace")]
fn install_tracing() {
    use tracing_error::ErrorLayer;
    use tracing_subscriber::prelude::*;
    use tracing_subscriber::{fmt, EnvFilter};

    let fmt_layer = fmt::layer().with_target(false);
    let filter_layer = EnvFilter::try_from_default_env()
        .or_else(|_| EnvFilter::try_new("info"))
        .unwrap();

    tracing_subscriber::registry()
        .with(filter_layer)
        .with(fmt_layer)
        .with(ErrorLayer::default())
        .init();
}

#[instrument]
fn read_file(path: &str) -> Result<(), Report> {
    info!("Reading file");
    Ok(std::fs::read_to_string(path).map(drop)?)
}

#[instrument]
fn read_config() -> Result<(), Report> {
    read_file("fake_file")
        .wrap_err("Unable to read config")
        .suggestion("try using a file that exists next time")
}
```
```rust
Finished dev [unoptimized + debuginfo] target(s) in 46.80s
     Running `target/debug/examples/custom_filter`
Oct 12 19:52:50.882  INFO read_config:read_file{path="fake_file"}: Reading file
Error: 
   0: Unable to read config
   1: No such file or directory (os error 2)

Location:
   /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/core/src/result.rs:1897

  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ SPANTRACE ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

   0: custom_filter::read_file with path="fake_file"
      at examples/custom_filter.rs:50
   1: custom_filter::read_config
      at examples/custom_filter.rs:56

  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ BACKTRACE ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
                                ⋮ 5 frames hidden ⋮                               
   6: <core::result::Result<T,F> as core::ops::try_trait::FromResidual<core::result::Result<core::convert::Infallible,E>>>::from_residual::h3f154fa1ed8a9fd5
      at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/core/src/result.rs:1897
   7: custom_filter::read_file::h4bb2f2a71326c7e3
      at /workspaces/error-handling-in-tremor-rs/color-eyre/examples/custom_filter.rs:53
   8: custom_filter::read_config::h0cabb33810e92a71
      at /workspaces/error-handling-in-tremor-rs/color-eyre/examples/custom_filter.rs:58
                                 ⋮ 1 frame hidden ⋮                               
  10: core::ops::function::FnOnce::call_once::h38ea8ee56d5345d0
      at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/core/src/ops/function.rs:227
                                ⋮ 15 frames hidden ⋮                              

Suggestion: try using a file that exists next time

Run with COLORBT_SHOW_HIDDEN=1 environment variable to disable frame filtering.
Run with RUST_BACKTRACE=full to include source snippets.
```

# If we run `COLORBT_SHOW_HIDDEN=1`

```rust
Finished dev [unoptimized + debuginfo] target(s) in 0.03s
     Running `target/debug/examples/custom_filter`
Oct 12 20:10:28.491  INFO read_config:read_file{path="fake_file"}: Reading file
Error: 
   0: Unable to read config
   1: No such file or directory (os error 2)

Location:
   /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/core/src/result.rs:1897

  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ SPANTRACE ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

   0: custom_filter::read_file with path="fake_file"
      at examples/custom_filter.rs:50
   1: custom_filter::read_config
      at examples/custom_filter.rs:56

  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ BACKTRACE ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
   1: color_eyre::config::EyreHook::default::hcbf54f8e3fac5d75
      at /workspaces/error-handling-in-tremor-rs/color-eyre/src/config.rs:1016
   2: color_eyre::config::EyreHook::into_eyre_hook::{{closure}}::h98b3013553ed08e7
      at /workspaces/error-handling-in-tremor-rs/color-eyre/src/config.rs:1070
   3: eyre::capture_handler::h4c4877caeae36905
      at /usr/local/cargo/registry/src/github.com-1ecc6299db9ec823/eyre-0.6.5/src/lib.rs:551
   4: eyre::error::<impl eyre::Report>::from_std::hd699b2628f69daf7
      at /usr/local/cargo/registry/src/github.com-1ecc6299db9ec823/eyre-0.6.5/src/error.rs:90
   5: eyre::error::<impl core::convert::From<E> for eyre::Report>::from::h08b3cd1cad3fba13
      at /usr/local/cargo/registry/src/github.com-1ecc6299db9ec823/eyre-0.6.5/src/error.rs:464
   6: <core::result::Result<T,F> as core::ops::try_trait::FromResidual<core::result::Result<core::convert::Infallible,E>>>::from_residual::h3f154fa1ed8a9fd5
      at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/core/src/result.rs:1897
   7: custom_filter::read_file::h4bb2f2a71326c7e3
      at /workspaces/error-handling-in-tremor-rs/color-eyre/examples/custom_filter.rs:53
   8: custom_filter::read_config::h0cabb33810e92a71
      at /workspaces/error-handling-in-tremor-rs/color-eyre/examples/custom_filter.rs:58
   9: custom_filter::main::hd5fbae2d1d55ade4
      at /workspaces/error-handling-in-tremor-rs/color-eyre/examples/custom_filter.rs:29
  10: core::ops::function::FnOnce::call_once::h38ea8ee56d5345d0
      at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/core/src/ops/function.rs:227
  11: std::sys_common::backtrace::__rust_begin_short_backtrace::hb7f82228a189c729
      at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/std/src/sys_common/backtrace.rs:125
  12: std::rt::lang_start::{{closure}}::he3b6cd86cb0eba7e
      at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/std/src/rt.rs:63
  13: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once::ha9408abe89f69dc4
      at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/core/src/ops/function.rs:259
  14: std::panicking::try::do_call::h5b0cc9e9102acb65
      at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/std/src/panicking.rs:401
  15: std::panicking::try::hddc7f8229138b3ba
      at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/std/src/panicking.rs:365
  16: std::panic::catch_unwind::hfa401ff8bab2986e
      at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/std/src/panic.rs:434
  17: std::rt::lang_start_internal::{{closure}}::h8163422320d11405
      at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/std/src/rt.rs:45
  18: std::panicking::try::do_call::hc742cc7bb4f0fb20
      at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/std/src/panicking.rs:401
  19: std::panicking::try::ha37d8d2dd1acf7d3
      at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/std/src/panicking.rs:365
  20: std::panic::catch_unwind::h8a5381d5ecf437bc
      at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/std/src/panic.rs:434
  21: std::rt::lang_start_internal::h7e2cee8c90d4a4d3
      at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/std/src/rt.rs:45
  22: std::rt::lang_start::he7f17c828fcc812f
      at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/std/src/rt.rs:62
  23: main<unknown>
      at <unknown source file>:<unknown line>
  24: __libc_start_main<unknown>
      at <unknown source file>:<unknown line>
  25: _start<unknown>
      at <unknown source file>:<unknown line>

Suggestion: try using a file that exists next time

Run with COLORBT_SHOW_HIDDEN=1 environment variable to disable frame filtering.
Run with RUST_BACKTRACE=full to include source snippets.
```

# If we run `RUST_BACKTRACE=1`

```rust
Finished dev [unoptimized + debuginfo] target(s) in 0.04s
     Running `target/debug/examples/custom_filter`
Oct 12 20:12:17.224  INFO read_config:read_file{path="fake_file"}: Reading file
Error: 
   0: Unable to read config
   1: No such file or directory (os error 2)

Location:
   /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/core/src/result.rs:1897

  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ SPANTRACE ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

   0: custom_filter::read_file with path="fake_file"
      at examples/custom_filter.rs:50
   1: custom_filter::read_config
      at examples/custom_filter.rs:56

  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ BACKTRACE ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
                                ⋮ 5 frames hidden ⋮                               
   6: <core::result::Result<T,F> as core::ops::try_trait::FromResidual<core::result::Result<core::convert::Infallible,E>>>::from_residual::h3f154fa1ed8a9fd5
      at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/core/src/result.rs:1897
   7: custom_filter::read_file::h4bb2f2a71326c7e3
      at /workspaces/error-handling-in-tremor-rs/color-eyre/examples/custom_filter.rs:53
   8: custom_filter::read_config::h0cabb33810e92a71
      at /workspaces/error-handling-in-tremor-rs/color-eyre/examples/custom_filter.rs:58
                                 ⋮ 1 frame hidden ⋮                               
  10: core::ops::function::FnOnce::call_once::h38ea8ee56d5345d0
      at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/core/src/ops/function.rs:227
                                ⋮ 15 frames hidden ⋮                              

Suggestion: try using a file that exists next time

Run with COLORBT_SHOW_HIDDEN=1 environment variable to disable frame filtering.
Run with RUST_BACKTRACE=full to include source snippets.
```
