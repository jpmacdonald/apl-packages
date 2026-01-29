# apl-packages

Package registry for APL.

Contains TOML templates that describe how to discover and download packages. The `apl-pkg` indexer processes these to generate a binary index.

## Structure

```
packages/
  ri/
    ripgrep.toml
  fd/
    fd.toml
  ...
```

Packages are sharded by first two letters of the name.

## Example package

```toml
[package]
name = "ripgrep"
description = "Fast grep"
homepage = "https://github.com/BurntSushi/ripgrep"
license = "MIT"

[discovery]
source = "github"
owner = "BurntSushi"
repo = "ripgrep"
tag_pattern = "{{version}}"

[assets.select]
arm64-macos = { contains = "aarch64-apple-darwin" }
x86_64-macos = { contains = "x86_64-apple-darwin" }

[install]
bin = ["rg"]
```

## Adding a package

1. Create `packages/<first-two-letters>/<name>.toml`
2. Run `cargo run -p apl-pkg -- check` to validate
3. Submit PR

## CI

On push to main, the indexer rebuilds the index, signs it with Ed25519, and uploads to R2.

## License

MIT
