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

## color_eyre
