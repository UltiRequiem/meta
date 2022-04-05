# [Crates](https://crates.io)

> The Rust community’s crate registry

## Resources

- [My Profile](https://crates.io/users/UltiRequiem)

- [Valid Category Slugs](https://crates.io/category_slugs)

## GitHub Workflows

### Build binaries

```yaml
name: Build binaries

on:
  workflow_dispatch:
  release:
    types: [created]

jobs:
  release:
    name: release ${{ matrix.target }}
    runs-on: ubuntu-latest
    strategy:
      fail-fast: false
      matrix:
        include:
          - target: x86_64-pc-windows-gnu
            archive: zip
          - target: x86_64-unknown-linux-musl
            archive: tar.gz tar.xz
          - target: x86_64-apple-darwin
            archive: zip
    steps:
      - uses: actions/checkout@v3
      - name: Compile and release
        uses: rust-build/rust-build.action@latest
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          RUSTTARGET: ${{ matrix.target }}
          ARCHIVE_TYPES: ${{ matrix.archive }}
          SRC_DIR: "crates/fibora"
```

### CI

```
name: CI

on: [push, pull_request]

jobs:
  check:
    name: Rust project
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Install latest nightly
        uses: actions-rs/toolchain@v1
        with:
          toolchain: nightly
          override: true
          components: clippy, rustfmt

      - name: Run clippy
        uses: actions-rs/cargo@v1
        with:
          command: clippy

      - name: Run fmt
        uses: actions-rs/cargo@v1
        with:
          command: fmt
          args: --all -- --check
```

### Tests

```yaml
name: Coverage

on: [push, pull_request]

jobs:
  coverage:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Install Rust
        run: rustup toolchain install nightly --component llvm-tools-preview
      - name: Install cargo-llvm-cov
        run: curl -LsSf https://github.com/taiki-e/cargo-llvm-cov/releases/latest/download/cargo-llvm-cov-x86_64-unknown-linux-gnu.tar.gz | tar xzf - -C ~/.cargo/bin
      - name: Generate code coverage
        run: cargo llvm-cov --all-features --workspace --lcov --output-path lcov.info
      - name: Upload coverage to Codecov
        uses: codecov/codecov-action@v2
        with:
          files: lcov.info
```

### Asking for a `cli.rs` Domain

```yaml
# cd.yaml

name: Build and Deploy

on: [push]

jobs:
  build-and-deploy:
    concurrency: ci-${{ github.ref }}
    runs-on: ubuntu-latest
    steps:
      - name: Checkout 🛎️
        uses: actions/checkout@v3

      - name: Install and Build 🔧
        run: |
          mkdir output
          cp readme.md output/
          echo ${{ github.event.repository.name }}.cli.rs >> output/CNAME
          echo theme: jekyll-theme-cayman >> output/_config.yml
      - name: Deploy 🚀
        uses: JamesIves/github-pages-deploy-action@v4.2.5
        with:
          branch: gh-pages
          folder: output
```
