---
title: Use Mold Linker in Rust
tags: [cargo, rust, mold, linker]
---

# Use Mold Linker in Rust

Mold linker is a fast and memory-efficient linker. It is a drop-in replacement for GNU ld and lld. It is a good choice for Rust projects. (5x faster than GNU ld and 2x faster than lld)

Add the following to your Cargo config at `YOUR_WORKSPACE/.cargo/config.toml:`

```toml
[target.x86_64-unknown-linux-gnu]
linker = "clang"
rustflags = ["-C", "link-arg=-fuse-ld=/usr/bin/mold"]
```

`mold` does not yet support MacOS, but can use `zld` as an alternative: `brew install michaeleisel/zld/zld`

> `zld` requires full xcode (not cli) to work!!!

```toml
[target.aarch64-apple-darwin]
rustflags = ["-C", "link-arg=-fuse-ld=/opt/homebrew/bin/zld", "-Zshare-generics=y"]
```
