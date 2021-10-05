```rust
use std::fmt;
use std::io;
use value_trait::ValueType;

/// Influx parser error
#[derive(Debug)]
pub enum EncoderError {
    /// an invalid filed was encountered
    InvalidField(&'static str),
    /// an invalid timestamp was encountered
    InvalidTimestamp(ValueType),
    /// an invalid value was encountered
    InvalidValue(String, ValueType),
    /// a io error was encountered
    Io(io::Error),
    /// a required field is missing
    MissingField(&'static str),
}

impl PartialEq for EncoderError {
    fn eq(&self, other: &Self) -> bool {
        match (self, other) {
            (EncoderError::InvalidField(v1), EncoderError::InvalidField(v2))
            | (EncoderError::MissingField(v1), EncoderError::MissingField(v2)) => v1 == v2,
            (EncoderError::InvalidTimestamp(v1), EncoderError::InvalidTimestamp(v2)) => v1 == v2,
            (EncoderError::InvalidValue(k1, v1), EncoderError::InvalidValue(k2, v2)) => {
                k1 == k2 && v1 == v2
            }
            (EncoderError::Io(_), EncoderError::Io(_)) => true,

            (_, _) => false,
        }
    }
}

/// Influx parser error
#[derive(Debug, PartialEq)]
pub enum DecoderError {
    /// an `=` in a value was found
    EqInTagValue(usize),
    /// unexpected character
    Unexpected(usize),
    /// a generic error
    Generic(usize, String),
    /// an invalid field
    InvalidFields(usize),
    /// missing value for a tag
    MissingTagValue(usize),
    /// failed to parse float value
    ParseFloatError(usize, lexical::Error),
    /// failed to parse integer value
    ParseIntError(usize, lexical::Error),
    /// trailing characters after the timestamp
    TrailingCharacter(usize),
    /// invalid escape sequence
    InvalidEscape(usize),
    /// unexpected end of message
    UnexpectedEnd(usize),
}

impl fmt::Display for EncoderError {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        write!(f, "{:?}", self)
    }
}

impl fmt::Display for DecoderError {
    fn fmt(&self, f: &mut fmt::Formatter<'_>) -> fmt::Result {
        write!(f, "{:?}", self)
    }
}

impl std::error::Error for EncoderError {}
impl std::error::Error for DecoderError {}

/// Parser result type
pub type EncoderResult<T> = std::result::Result<T, EncoderError>;

/// Parser result type
pub type DecoderResult<T> = std::result::Result<T, DecoderError>;

impl From<io::Error> for EncoderError {
    fn from(e: io::Error) -> Self {
        Self::Io(e)
    }
}
```
