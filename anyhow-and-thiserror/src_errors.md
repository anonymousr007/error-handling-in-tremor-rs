```rust
use crate::async_sink;
use hdrhistogram::{self, serialization as hdr_s};
use thiserror::Error;
use tremor_influx as influx;

impl Clone for Error {
    fn clone(&self) -> Self {
        Error::ClonedError(format!("{}", self)).into()
    }
}

impl From<sled::transaction::TransactionError<()>> for Error {
    fn from(e: sled::transaction::TransactionError<()>) -> Self {
        Self::from(format!("Sled Transaction Error: {:?}", e))
    }
}

impl From<hdr_s::DeserializeError> for Error {
    fn from(e: hdr_s::DeserializeError) -> Self {
        Self::from(format!("{:?}", e))
    }
}

impl From<Box<dyn std::error::Error>> for Error {
    fn from(e: Box<dyn std::error::Error>) -> Self {
        Self::from(format!("{:?}", e))
    }
}

impl From<Box<dyn std::error::Error + Sync + Send>> for Error {
    fn from(e: Box<dyn std::error::Error + Sync + Send>) -> Self {
        Self::from(format!("{:?}", e))
    }
}

impl From<hdrhistogram::errors::CreationError> for Error {
    fn from(e: hdrhistogram::errors::CreationError) -> Self {
        Self::from(format!("{:?}", e))
    }
}

impl From<hdrhistogram::RecordError> for Error {
    fn from(e: hdrhistogram::RecordError) -> Self {
        Self::from(format!("{:?}", e))
    }
}

impl From<hdrhistogram::serialization::V2SerializeError> for Error {
    fn from(e: hdrhistogram::serialization::V2SerializeError) -> Self {
        Self::from(format!("{:?}", e))
    }
}

impl From<http_types::Error> for Error {
    fn from(e: http_types::Error) -> Self {
        Self::from(format!("{}", e))
    }
}

impl From<glob::PatternError> for Error {
    fn from(e: glob::PatternError) -> Self {
        Self::from(format!("{}", e))
    }
}

impl<T> From<async_channel::SendError<T>> for Error {
    fn from(e: async_channel::SendError<T>) -> Self {
        Self::from(format!("{:?}", e))
    }
}

impl<T> From<async_channel::TrySendError<T>> for Error {
    fn from(e: async_channel::TrySendError<T>) -> Self {
        Self::from(format!("{:?}", e))
    }
}

impl From<tremor_script::errors::CompilerError> for Error {
    fn from(e: tremor_script::errors::CompilerError) -> Self {
        e.error().into()
    }
}

impl<P> From<std::sync::PoisonError<P>> for Error {
    fn from(e: std::sync::PoisonError<P>) -> Self {
        Self::from(format!("Poison Error: {:?}", e))
    }
}

impl<F> From<rental::RentalError<F, Box<Vec<u8>>>> for Error {
    fn from(_e: rental::RentalError<F, Box<Vec<u8>>>) -> Self {
        Self::from("Rental Error".to_string())
    }
}

#[cfg(test)]
impl PartialEq for Error {
    fn eq(&self, _other: &Self) -> bool {
        // This might be Ok since we try to compare Result in tests
        false
    }
}

#[derive(Error, Debug)]
pub enum Error {
        #[error(transparent)]
        ValueError(#[from] tremor_value::Error);
        #[error(transparent)]
        Base64Error((#[from] base64::DecodeError);
        #[error(transparent)]
        YamlError((#[from] serde_yaml::Error) #[doc = "Error during yaml parsing"];
        #[error(transparent)]
        JsonError((#[from] simd_json::Error);
        #[error(transparent)]
        Io((#[from] std::io::Error);
        #[error(transparent)]
        SinkDequeueError((#[from] async_sink::SinkDequeueError);
        #[error(transparent)]
        SinkEnqueueError((#[from] async_sink::SinkEnqueueError);
        #[error(transparent)]
        FromUtf8Error((#[from] std::string::FromUtf8Error);
        #[error(transparent)]
        Utf8Error((#[from] std::str::Utf8Error);
        #[error(transparent)]
        ElasticError((#[from] elastic::Error);
        #[error(transparent)]
        KafkaError((#[from] rdkafka::error::KafkaError);
        #[error(transparent)]
        ParseIntError((#[from] std::num::ParseIntError);
        #[error(transparent)]
        TryFromIntError((#[from] std::num::TryFromIntError);
        #[error(transparent)]
        UrlParserError((#[from] url::ParseError);
        #[error(transparent)]
        ParseFloatError((#[from] std::num::ParseFloatError);
        #[error(transparent)]
        AnyhowError((#[from] anyhow::Error);
        #[error(transparent)]
        ChannelReceiveError((#[from] std::sync::mpsc::RecvError);
        #[error(transparent)]
        MsgPackDecoderError((#[from] rmp_serde::decode::Error);
        #[error(transparent)]
        MsgPackEncoderError((#[from] rmp_serde::encode::Error);
        #[error(transparent)]
        GrokError((#[from] grok::Error);
        #[error(transparent)]
        DateTimeParseError((#[from] chrono::ParseError);
        #[error(transparent)]
        SnappyError((#[from] snap::Error);
        #[error(transparent)]
        AddrParseError((#[from] std::net::AddrParseError);
        #[error(transparent)]
        RegexError((#[from] regex::Error);
        #[error(transparent)]
        WsError((#[from] async_tungstenite::tungstenite::Error);
        #[error(transparent)]
        InfluxEncoderError((#[from] influx::EncoderError);
        #[error(transparent)]
        AsyncChannelRecvError((#[from] async_channel::RecvError);
        #[error(transparent)]
        JsonAccessError((#[from] value_trait::AccessError);
        #[error(transparent)]
        CronError((#[from] cron::error::Error);
        #[error(transparent)]
        Postgres((#[from] postgres::Error);
        #[error(transparent)]
        Common((#[from] tremor_common::Error);
        #[error(transparent)]
        Sled((#[from] sled::Error);
        #[error(transparent)]
        DnsError((#[from] async_std_resolver::ResolveError);
        #[error(transparent)]
        GoogleAuthError((#[from] gouth::Error);
        #[error(transparent)]
        ReqwestError((#[from] reqwest::Error);
        #[error(transparent)]
        HttpHeaderError((#[from] http::header::InvalidHeaderValue);
        #[error(transparent)]
        TonicTransportError((#[from] tonic::transport::Error);
        #[error(transparent)]
        TonicStatusError((#[from] tonic::Status);
        #[error(transparent)]
        RustlsError((#[from] rustls::TLSError);
        #[error(transparent)]
        Hex((#[from] hex::FromHexError);
        #[error("Unknown operator: {}::{}", n, o)]
        UnknownOp(n: String, o: String)
        #[error("The artifact was not found: {}", id)]
        ArtefactNotFound(id: String)
        
        #[error(("The published {} already exists.", key)]
        PublishFailedAlreadyExists(key: String)
        
        #[error("The unpublished {} does not exist.", key)]
        UnpublishFailedDoesNotExist(key: String)
        
        #[error("Cannot unpublish artefact {} which has non-zero instances.", key)]
        UnpublishFailedNonZeroInstances(key: String)
        
        #[error("Cannot unpublish system artefact {}.", key)]
        UnpublishFailedSystemArtefact(key: String)
        #[error("The binding with the id {} already exists.", key)]
        BindFailedAlreadyExists(key: String)
        #[error("Failed to bind non existand {}.", key)]
        BindFailedKeyNotExists(key: String)
       
        // TODO: Old errors, verify if needed
        ClonedError(t: String) {
            description("This is a cloned error we need to get rod of this")
                display("Cloned error: {}", t)
        }

        BadOpConfig(e: String) {
            description("Operator config has a bad syntax")
                display("Operator config has a bad syntax: {}", e)
        }

        UnknownNamespace(n: String) {
            description("Unknown namespace")
                display("Unknown namespace: {}", n)
        }

        InvalidGelfHeader(len: usize, initial: Option<[u8; 2]>) {
            description("Invalid GELF header")
                display("Invalid GELF header len: {}, prefix: {:?}", len, initial)
        }

        InvalidStatsD {
            description("Invalid statsd metric")
                display("Invalid statsd metric")
        }
        InvalidInfluxData(s: String, e: influx::DecoderError) {
            description("Invalid Influx Line Protocol data")
                display("Invalid Influx Line Protocol data: {}\n{}", e, s)
        }
        InvalidBInfluxData(s: String) {
            description("Invalid BInflux Line Protocol data")
                display("Invalid BInflux Line Protocol data: {}", s)
        }
        InvalidSyslogData(s: &'static str) {
            description("Invalid Syslog Protocol data")
                display("Invalid Syslog Protocol data: {}", s)
        }
        BadUtF8InString {
            description("Bad UTF8 in input string")

        }
        InvalidCompression {
            description("Data can't be decompressed")
                display("The data did not contain a known magic header to identify a supported compression")
        }
        NoSocket {
            description("No socket available")
                display("No socket available")
        }
        KvError(s: String) {
            description("KV error")
                display("{}", s)
        }
        TLSError(s: String) {
            description("TLS error")
                display("{}", s)
        }
    }
}

error_chain! {
    links {
        Script(tremor_script::errors::Error, tremor_script::errors::ErrorKind);
        Pipeline(tremor_pipeline::errors::Error, tremor_pipeline::errors::ErrorKind);
    }
    errors {
```
