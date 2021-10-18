<h1 align=center>Tremor in anyhow and thiserror</h1>

## Focused on some keywords:

1. chain_err
2. map_err
3. expect
4. errorkind

## Important

- `thiserror` is used for multiple-errors in errors.rs files.
- Delete `error-chain` in `Cargo.toml` file and add `thiserror` + `anyhow`.
  - tremor-pipeline/Cargo.toml
  - tremor-script/Cargo.toml
  - tremor-cli/Cargo.toml
  - /Cargo.toml
- `chain_err` found in 2 files
  - src/codec/binflux.rs
  - tremor-script/src/lexer.rs
- 
