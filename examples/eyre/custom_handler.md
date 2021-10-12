```rust
use backtrace::Backtrace;
use eyre::EyreHandler;
use std::error::Error;
use std::{fmt, iter};

fn main() -> eyre::Result<()> {
    // Install our custom eyre report hook for constructing our custom Handlers
    install().unwrap();

    // construct a report with, hopefully, our custom handler!
    let mut report = eyre::eyre!("hello from custom error town!");

    // manually set the custom msg for this report after it has been constructed
    if let Some(handler) = report.handler_mut().downcast_mut::<Handler>() {
        handler.custom_msg = Some("you're the best users, you know that right???");
    }

    // print that shit!!
    Err(report)
}

// define a handler that captures backtraces unless told not to
fn install() -> Result<(), impl Error> {
    let capture_backtrace = std::env::var("RUST_BACKWARDS_TRACE")
        .map(|val| val != "0")
        .unwrap_or(true);

    let hook = Hook { capture_backtrace };

    eyre::set_hook(Box::new(move |e| Box::new(hook.make_handler(e))))
}

struct Hook {
    capture_backtrace: bool,
}

impl Hook {
    fn make_handler(&self, _error: &(dyn Error + 'static)) -> Handler {
        let backtrace = if self.capture_backtrace {
            Some(Backtrace::new())
        } else {
            None
        };

        Handler {
            backtrace,
            custom_msg: None,
        }
    }
}

struct Handler {
    // custom configured backtrace capture
    backtrace: Option<Backtrace>,
    // customizable message payload associated with reports
    custom_msg: Option<&'static str>,
}

impl EyreHandler for Handler {
    fn debug(&self, error: &(dyn Error + 'static), f: &mut fmt::Formatter<'_>) -> fmt::Result {
        if f.alternate() {
            return fmt::Debug::fmt(error, f);
        }

        let errors = iter::successors(Some(error), |error| (*error).source());

        for (ind, error) in errors.enumerate() {
            write!(f, "\n{:>4}: {}", ind, error)?;
        }

        if let Some(backtrace) = self.backtrace.as_ref() {
            writeln!(f, "\n\nBacktrace:\n{:?}", backtrace)?;
        }

        if let Some(msg) = self.custom_msg.as_ref() {
            writeln!(f, "\n\n{}", msg)?;
        }

        Ok(())
    }
}
```

```rust
Finished dev [unoptimized + debuginfo] target(s) in 1m 08s
     Running `target/debug/examples/custom_handler`
Error: 
   0: hello from custom error town!

Backtrace:
   0: custom_handler::Hook::make_handler
             at examples/custom_handler.rs:40:18
   1: custom_handler::install::{{closure}}
             at examples/custom_handler.rs:30:47
   2: eyre::capture_handler
             at src/lib.rs:551:23
   3: eyre::error::<impl eyre::Report>::from_adhoc
             at src/error.rs:110:28
   4: eyre::private::new_adhoc
             at src/lib.rs:1133:9
   5: custom_handler::main
             at examples/custom_handler.rs:11:22
   6: core::ops::function::FnOnce::call_once
             at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/core/src/ops/function.rs:227:5
   7: std::sys_common::backtrace::__rust_begin_short_backtrace
             at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/std/src/sys_common/backtrace.rs:125:18
   8: std::rt::lang_start::{{closure}}
             at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/std/src/rt.rs:63:18
   9: core::ops::function::impls::<impl core::ops::function::FnOnce<A> for &F>::call_once
             at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/core/src/ops/function.rs:259:13
      std::panicking::try::do_call
             at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/std/src/panicking.rs:401:40
      std::panicking::try
             at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/std/src/panicking.rs:365:19
      std::panic::catch_unwind
             at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/std/src/panic.rs:434:14
      std::rt::lang_start_internal::{{closure}}
             at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/std/src/rt.rs:45:48
      std::panicking::try::do_call
             at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/std/src/panicking.rs:401:40
      std::panicking::try
             at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/std/src/panicking.rs:365:19
      std::panic::catch_unwind
             at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/std/src/panic.rs:434:14
      std::rt::lang_start_internal
             at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/std/src/rt.rs:45:20
  10: std::rt::lang_start
             at /rustc/c8dfcfe046a7680554bf4eb612bad840e7631c4b/library/std/src/rt.rs:62:5
  11: main
  12: __libc_start_main
  13: _start



you're the best users, you know that right???
```
