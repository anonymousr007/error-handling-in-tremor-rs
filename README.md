# Error Handling in Tremor Runtime

### Types of Errors

1. foreign_links

        YamlError(serde_yaml::Error) #[doc = "Error during yaml parsing"];
        JsonError(simd_json::Error);
        JsonAccessError(value_trait::AccessError);
        UrlParserError(url::ParseError);
        Io(std::io::Error);
        FromUtf8Error(std::string::FromUtf8Error);
        Utf8Error(std::str::Utf8Error);
        ParseIntError(std::num::ParseIntError);
        ParseFloatError(std::num::ParseFloatError);
        Sled(sled::Error);
        
        ValueError(tremor_value::Error);
        Base64Error(base64::DecodeError);
        SinkDequeueError(async_sink::SinkDequeueError);
        SinkEnqueueError(async_sink::SinkEnqueueError);
        ElasticError(elastic::Error);
        KafkaError(rdkafka::error::KafkaError);
        
        TryFromIntError(std::num::TryFromIntError);
        AnyhowError(anyhow::Error);
        ChannelReceiveError(std::sync::mpsc::RecvError);
        MsgPackDecoderError(rmp_serde::decode::Error);
        MsgPackEncoderError(rmp_serde::encode::Error);
        GrokError(grok::Error);
        DateTimeParseError(chrono::ParseError);
        SnappyError(snap::Error);
        AddrParseError(std::net::AddrParseError);
        RegexError(regex::Error);
        WsError(async_tungstenite::tungstenite::Error);
        InfluxEncoderError(influx::EncoderError);
        AsyncChannelRecvError(async_channel::RecvError);
        JsonAccessError(value_trait::AccessError);
        CronError(cron::error::Error);
        Postgres(postgres::Error);
        Common(tremor_common::Error);
        Sled(sled::Error);
        DnsError(async_std_resolver::ResolveError);
        GoogleAuthError(gouth::Error);
        ReqwestError(reqwest::Error);
        HttpHeaderError(http::header::InvalidHeaderValue);
        TonicTransportError(tonic::transport::Error);
        TonicStatusError(tonic::Status);
        RustlsError(rustls::TLSError);
        Hex(hex::FromHexError);




## error_chain (deprecated)

- error_chain/tremor-api_src_errors.md
- error_chain/tremor-cli_src_errors.md
- error_chain/tremor-common_src_errors.md
- error_chain/tremor-influx_src_errors.md
- error_chain/tremor-pipeline_src_errors.md
- error_chain/tremor-script_src_errors.md
- error_chain/tremor-value_src_error.md


## anyhow (for applications)

- Macros
  - anyhow - Construct an ad-hoc error from a string or existing non-anyhow error value.
  - bail - Return early with an error.
  - ensure - Return early with an error if a condition is not satisfied.
- Structs
  - Chain `std` - Iterator of a chain of source errors.
  - Error - The Error type, a wrapper around a dynamic error type.
- Traits
  - Context - Provides the context method for Result.
- Type Definitions
  - Result - Result<T, Error>

## eyre

- Macros
  - bail - Return early with an error.
  - ensure - Return early with an error if a condition is not satisfied.
  - eyre - Construct an ad-hoc error from a string.
- Structs
  - Chain -	Iterator of a chain of source errors.
  - DefaultHandler - The default provided error report handler for `eyre::Report`.
  - InstallError - Error indicating that `set_hook` was unable to install the provided ErrorHook
  - Report - The core error reporting type of the library, a wrapper around a dynamic error reporting type.
- Traits
  - ContextCompat - Provides the `context` method for `Option` when porting from `anyhow`
  - EyreHandler - Error Report Handler trait for customizing `eyre::Report`
  - WrapErr - Provides the `wrap_err` method for `Result`.
- Functions
  - set_hook - Install the provided error hook for constructing EyreHandlers when converting Errors to Reports
- Type Definitions
  - Result - type alias for `Result<T, Report>`.
