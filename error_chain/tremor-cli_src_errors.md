```rust
//NOTE: error_chain
#![allow(deprecated)]
#![allow(missing_docs)]
#![allow(clippy::large_enum_variant)]

use crate::util::SourceKind;
use error_chain::error_chain;

impl From<http_types::Error> for Error {
    fn from(e: http_types::Error) -> Self {
        Self::from(format!("{}", e))
    }
}

impl<P> From<std::sync::PoisonError<P>> for Error {
    fn from(e: std::sync::PoisonError<P>) -> Self {
        Self::from(format!("Poison Error: {:?}", e))
    }
}

impl From<tremor_script::errors::CompilerError> for Error {
    fn from(e: tremor_script::errors::CompilerError) -> Self {
        e.error().into()
    }
}

error_chain! {
    links {
        Script(tremor_script::errors::Error, tremor_script::errors::ErrorKind);
        Pipeline(tremor_pipeline::errors::Error, tremor_pipeline::errors::ErrorKind);
        Runtime(tremor_runtime::errors::Error, tremor_runtime::errors::ErrorKind);
    }
    foreign_links {
        YamlError(serde_yaml::Error) #[doc = "Error during yaml parsing"];
        JsonError(simd_json::Error) #[doc = "Error during json parsing"];
        Io(std::io::Error) #[doc = "Error during std::io"];
        FutureTimeoutError(async_std::future::TimeoutError) #[doc = "Error waiting for futures to complete"];
        Globwalk(globwalk::GlobError) #[doc = "Glob walker error"];
        SendError(std::sync::mpsc::SendError<String>);
        AnyhowError(anyhow::Error);
        TestKindError(crate::test::Unknown);
        Url(url::ParseError) #[doc = "Error while parsing a url"];
        Common(tremor_common::Error);
        ParseIntError(std::num::ParseIntError);
    }
    errors {
        TestFailures(stats: crate::test::stats::Stats) {
            description("Some tests failed")
                display("{} out of {} tests failed.", stats.fail, stats.skip + stats.pass)
        }
        FileLoadError(file: String, error: tremor_runtime::errors::Error) {
            description("Failed to load config file")
                display("An error occurred while loading the file `{}`: {}", file, error)
        }
        UnsupportedFileType(file: String, file_type: SourceKind, expected: &'static str) {
            description("Unsupported file type")
                display("The file `{}` has the type `{}`, but tremor expected: {}", file, file_type, expected)
        }
    }
}
```