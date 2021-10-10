```
error[E0432]: unresolved import `crate::async_sink`
  --> src/errors.rs:20:5
   |
20 | use crate::async_sink;
   |     ^^^^^^^^^^^^^^^^^ no `async_sink` in the root

error[E0432]: unresolved import `error_chain`
  --> src/errors.rs:21:5
   |
21 | use error_chain::error_chain;
   |     ^^^^^^^^^^^ maybe a missing crate `error_chain`?

error[E0432]: unresolved import `hdrhistogram`
  --> src/errors.rs:22:20
   |
22 | use hdrhistogram::{self, serialization as hdr_s};
   |                    ^^^^ no `hdrhistogram` in the root

error[E0432]: unresolved import `tremor_influx`
  --> src/errors.rs:24:5
   |
24 | use tremor_influx as influx;
   |     ^^^^^^^^^^^^^^^^^^^^^^^ no `tremor_influx` in the root

error: cannot determine resolution for the macro `error_chain`
   --> src/errors.rs:124:1
    |
124 | error_chain! {
    | ^^^^^^^^^^^
    |
    = note: import resolution is stuck, try simplifying macro imports

error[E0433]: failed to resolve: use of undeclared crate or module `sled`
  --> src/errors.rs:32:11
   |
32 | impl From<sled::transaction::TransactionError<()>> for Error {
   |           ^^^^ use of undeclared crate or module `sled`

error[E0433]: failed to resolve: use of undeclared crate or module `sled`
  --> src/errors.rs:33:16
   |
33 |     fn from(e: sled::transaction::TransactionError<()>) -> Self {
   |                ^^^^ use of undeclared crate or module `sled`

error[E0433]: failed to resolve: use of undeclared crate or module `hdr_s`
  --> src/errors.rs:38:11
   |
38 | impl From<hdr_s::DeserializeError> for Error {
   |           ^^^^^ use of undeclared crate or module `hdr_s`

error[E0433]: failed to resolve: use of undeclared crate or module `hdr_s`
  --> src/errors.rs:39:16
   |
39 |     fn from(e: hdr_s::DeserializeError) -> Self {
   |                ^^^^^ use of undeclared crate or module `hdr_s`

error[E0433]: failed to resolve: use of undeclared crate or module `http_types`
  --> src/errors.rs:74:11
   |
74 | impl From<http_types::Error> for Error {
   |           ^^^^^^^^^^ use of undeclared crate or module `http_types`

error[E0433]: failed to resolve: use of undeclared crate or module `http_types`
  --> src/errors.rs:75:16
   |
75 |     fn from(e: http_types::Error) -> Self {
   |                ^^^^^^^^^^ use of undeclared crate or module `http_types`

error[E0433]: failed to resolve: use of undeclared crate or module `glob`
  --> src/errors.rs:80:11
   |
80 | impl From<glob::PatternError> for Error {
   |           ^^^^ use of undeclared crate or module `glob`

error[E0433]: failed to resolve: use of undeclared crate or module `glob`
  --> src/errors.rs:81:16
   |
81 |     fn from(e: glob::PatternError) -> Self {
   |                ^^^^ use of undeclared crate or module `glob`

error[E0433]: failed to resolve: use of undeclared crate or module `async_channel`
  --> src/errors.rs:86:14
   |
86 | impl<T> From<async_channel::SendError<T>> for Error {
   |              ^^^^^^^^^^^^^ use of undeclared crate or module `async_channel`

error[E0433]: failed to resolve: use of undeclared crate or module `async_channel`
  --> src/errors.rs:87:16
   |
87 |     fn from(e: async_channel::SendError<T>) -> Self {
   |                ^^^^^^^^^^^^^ use of undeclared crate or module `async_channel`

error[E0433]: failed to resolve: use of undeclared crate or module `async_channel`
  --> src/errors.rs:92:14
   |
92 | impl<T> From<async_channel::TrySendError<T>> for Error {
   |              ^^^^^^^^^^^^^ use of undeclared crate or module `async_channel`

error[E0433]: failed to resolve: use of undeclared crate or module `async_channel`
  --> src/errors.rs:93:16
   |
93 |     fn from(e: async_channel::TrySendError<T>) -> Self {
   |                ^^^^^^^^^^^^^ use of undeclared crate or module `async_channel`

error[E0433]: failed to resolve: use of undeclared crate or module `tremor_script`
  --> src/errors.rs:98:11
   |
98 | impl From<tremor_script::errors::CompilerError> for Error {
   |           ^^^^^^^^^^^^^ use of undeclared crate or module `tremor_script`

error[E0433]: failed to resolve: use of undeclared crate or module `tremor_script`
  --> src/errors.rs:99:16
   |
99 |     fn from(e: tremor_script::errors::CompilerError) -> Self {
   |                ^^^^^^^^^^^^^ use of undeclared crate or module `tremor_script`

error[E0433]: failed to resolve: use of undeclared crate or module `rental`
   --> src/errors.rs:110:14
    |
110 | impl<F> From<rental::RentalError<F, Box<Vec<u8>>>> for Error {
    |              ^^^^^^ use of undeclared crate or module `rental`

error[E0433]: failed to resolve: use of undeclared crate or module `rental`
   --> src/errors.rs:111:17
    |
111 |     fn from(_e: rental::RentalError<F, Box<Vec<u8>>>) -> Self {
    |                 ^^^^^^ use of undeclared crate or module `rental`

error[E0412]: cannot find type `Error` in this scope
  --> src/errors.rs:26:16
   |
26 | impl Clone for Error {
   |                ^^^^^ not found in this scope
   |
help: consider importing one of these items
   |
20 | use std::error::Error;
   |
20 | use std::fmt::Error;
   |
20 | use std::io::Error;
   |

error[E0433]: failed to resolve: use of undeclared type `ErrorKind`
  --> src/errors.rs:28:9
   |
28 |         ErrorKind::ClonedError(format!("{}", self)).into()
   |         ^^^^^^^^^ not found in this scope
   |
help: consider importing this enum
   |
20 | use std::io::ErrorKind;
   |

error[E0412]: cannot find type `Error` in this scope
  --> src/errors.rs:32:56
   |
32 | impl From<sled::transaction::TransactionError<()>> for Error {
   |                                                        ^^^^^ not found in this scope
   |
help: consider importing one of these items
   |
20 | use std::error::Error;
   |
20 | use std::fmt::Error;
   |
20 | use std::io::Error;
   |

error[E0412]: cannot find type `Error` in this scope
  --> src/errors.rs:38:40
   |
38 | impl From<hdr_s::DeserializeError> for Error {
   |                                        ^^^^^ not found in this scope
   |
help: consider importing one of these items
   |
20 | use std::error::Error;
   |
20 | use std::fmt::Error;
   |
20 | use std::io::Error;
   |

error[E0412]: cannot find type `Error` in this scope
  --> src/errors.rs:44:43
   |
44 | impl From<Box<dyn std::error::Error>> for Error {
   |                                           ^^^^^ not found in this scope
   |
help: consider importing one of these items
   |
20 | use std::error::Error;
   |
20 | use std::fmt::Error;
   |
20 | use std::io::Error;
   |

error[E0412]: cannot find type `Error` in this scope
  --> src/errors.rs:50:57
   |
50 | impl From<Box<dyn std::error::Error + Sync + Send>> for Error {
   |                                                         ^^^^^ not found in this scope
   |
help: consider importing one of these items
   |
20 | use std::error::Error;
   |
20 | use std::fmt::Error;
   |
20 | use std::io::Error;
   |

error[E0412]: cannot find type `Error` in this scope
  --> src/errors.rs:56:52
   |
56 | impl From<hdrhistogram::errors::CreationError> for Error {
   |                                                    ^^^^^ not found in this scope
   |
help: consider importing one of these items
   |
20 | use std::error::Error;
   |
20 | use std::fmt::Error;
   |
20 | use std::io::Error;
   |

error[E0412]: cannot find type `Error` in this scope
  --> src/errors.rs:62:42
   |
62 | impl From<hdrhistogram::RecordError> for Error {
   |                                          ^^^^^ not found in this scope
   |
help: consider importing one of these items
   |
20 | use std::error::Error;
   |
20 | use std::fmt::Error;
   |
20 | use std::io::Error;
   |

error[E0412]: cannot find type `Error` in this scope
  --> src/errors.rs:68:62
   |
68 | impl From<hdrhistogram::serialization::V2SerializeError> for Error {
   |                                                              ^^^^^ not found in this scope
   |
help: consider importing one of these items
   |
20 | use std::error::Error;
   |
20 | use std::fmt::Error;
   |
20 | use std::io::Error;
   |

error[E0412]: cannot find type `Error` in this scope
  --> src/errors.rs:74:34
   |
74 | impl From<http_types::Error> for Error {
   |                                  ^^^^^ not found in this scope
   |
help: consider importing one of these items
   |
20 | use std::error::Error;
   |
20 | use std::fmt::Error;
   |
20 | use std::io::Error;
   |

error[E0412]: cannot find type `Error` in this scope
  --> src/errors.rs:80:35
   |
80 | impl From<glob::PatternError> for Error {
   |                                   ^^^^^ not found in this scope
   |
help: consider importing one of these items
   |
20 | use std::error::Error;
   |
20 | use std::fmt::Error;
   |
20 | use std::io::Error;
   |

error[E0412]: cannot find type `Error` in this scope
  --> src/errors.rs:86:47
   |
86 | impl<T> From<async_channel::SendError<T>> for Error {
   |                                               ^^^^^ not found in this scope
   |
help: consider importing one of these items
   |
20 | use std::error::Error;
   |
20 | use std::fmt::Error;
   |
20 | use std::io::Error;
   |

error[E0412]: cannot find type `Error` in this scope
  --> src/errors.rs:92:50
   |
92 | impl<T> From<async_channel::TrySendError<T>> for Error {
   |                                                  ^^^^^ not found in this scope
   |
help: consider importing one of these items
   |
20 | use std::error::Error;
   |
20 | use std::fmt::Error;
   |
20 | use std::io::Error;
   |

error[E0412]: cannot find type `Error` in this scope
  --> src/errors.rs:98:53
   |
98 | impl From<tremor_script::errors::CompilerError> for Error {
   |                                                     ^^^^^ not found in this scope
   |
help: consider importing one of these items
   |
20 | use std::error::Error;
   |
20 | use std::fmt::Error;
   |
20 | use std::io::Error;
   |

error[E0412]: cannot find type `Error` in this scope
   --> src/errors.rs:104:45
    |
104 | impl<P> From<std::sync::PoisonError<P>> for Error {
    |                                             ^^^^^ not found in this scope
    |
help: consider importing one of these items
    |
20  | use std::error::Error;
    |
20  | use std::fmt::Error;
    |
20  | use std::io::Error;
    |

error[E0412]: cannot find type `Error` in this scope
   --> src/errors.rs:110:56
    |
110 | impl<F> From<rental::RentalError<F, Box<Vec<u8>>>> for Error {
    |                                                        ^^^^^ not found in this scope
    |
help: consider importing one of these items
    |
20  | use std::error::Error;
    |
20  | use std::fmt::Error;
    |
20  | use std::io::Error;
    |

error[E0601]: `main` function not found in crate `errors`
   --> src/errors.rs:16:1
    |
16  | / #![allow(deprecated)]
17  | | #![allow(missing_docs)]
18  | | #![allow(clippy::large_enum_variant)]
19  | |
...   |
272 | |     }
273 | | }
    | |_^ consider adding a `main` function to `src/errors.rs`

error: aborting due to 38 previous errors

Some errors have detailed explanations: E0412, E0432, E0433, E0601.
For more information about an error, try `rustc --explain E0412`.
```