```rust
/// Result type
pub type Result<T> = std::result::Result<T, Error>;
use std::fmt;

use fmt::Display;
#[derive(Debug)]
/// A Error
pub enum Error {
    /// A map was expected but some other value was found
    ExpectedMap,
    /// A generic serde error
    Serde(String),
    /// A SIMD Json error
    SimdJson(simd_json::Error),
    /// A generic error
    Generic(String),
}

impl From<&str> for Error {
    fn from(s: &str) -> Self {
        Self::Generic(s.to_string())
    }
}

impl Display for Error {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        match self {
            Error::ExpectedMap => write!(f, "Expected a struct, but did not find out"),
            Error::Serde(s) | Error::Generic(s) => f.write_str(s),
            Error::SimdJson(e) => write!(f, "SIMD JSON error: {}", e),
        }
    }
}

impl std::error::Error for Error {}

#[cfg(not(tarpaulin_include))] // this is a simple error
impl serde_ext::de::Error for Error {
    fn custom<T: fmt::Display>(msg: T) -> Self {
        Error::Serde(msg.to_string())
    }
}

#[cfg(not(tarpaulin_include))] // this is a simple error
impl serde_ext::ser::Error for Error {
    fn custom<T: fmt::Display>(msg: T) -> Self {
        Error::Serde(msg.to_string())
    }
}

#[cfg(test)]
mod test {
    use super::Error;

    #[test]
    fn from_str() {
        assert_eq!("snot", format!("{}", Error::from("snot")))
    }
}
```
