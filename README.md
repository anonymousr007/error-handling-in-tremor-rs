# Error Handling in Tremor Runtime

## error_chain (deprecated)

- error_chain/tremor-api_src_errors.md
- error_chain/tremor-cli_src_errors.md
- error_chain/tremor-common_src_errors.md
- error_chain/tremor-influx_src_errors.md
- error_chain/tremor-pipeline_src_errors.md
- error_chain/tremor-script_src_errors.md
- error_chain/tremor-value_src_error.md

## anyhow (for applications)

* a trait object based error type for easy idiomatic error handling in Rust applications.
* 

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

## thiserror (for libraries)

## eyre

- Macros
  - bail - Return early with an error.
  - ensure - Return early with an error if a condition is not satisfied.
  - eyre - Construct an ad-hoc error from a string.
- Structs
  - Chain -	Iterator of a chain of source errors.
  - DefaultHandler - The default provided error report handler for eyre::Report.
  - InstallError - Error indicating that set_hook was unable to install the provided ErrorHook
  - Report - The core error reporting type of the library, a wrapper around a dynamic error reporting type.
- Traits
  - ContextCompat - Provides the context method for Option when porting from anyhow
  - EyreHandler - Error Report Handler trait for customizing eyre::Report
  - WrapErr - Provides the wrap_err method for Result.
- Functions
  - set_hook - Install the provided error hook for constructing EyreHandlers when converting Errors to Reports
- Type Definitions
  - Result - type alias for Result<T, Report>

## color_eyre
