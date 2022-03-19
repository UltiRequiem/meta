# A modern runtime for JavaScript and TypeScript.

Deno is more complex because lot of ways to ship stuff

## GitHub Workflows

### Continuous Integration

This Action will 👇

- Check Code format

- Lint

- Bundle

- Generate and Upload the Coverage

```yaml
# .github/workflows/ci.yaml

name: CI

on: [push, pull_request]

jobs:
  build:
    name: tests (${{ matrix.os }})
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest, macOS-latest]
      fail-fast: true
    steps:
      - name: Setup Deno
      - uses: actions/checkout@v3
        uses: denoland/setup-deno@v1
        with:
          deno-version: v1.x

      - name: Check Code Format
        if: matrix.os == 'ubuntu-latest'
        run: deno fmt --check

      - name: Lint
        if: matrix.os == 'ubuntu-latest'
        run: deno lint

      - name: Bundle
        run: deno bundle mod.ts mod.bundle.js

      - name: Tests
        run: deno test --coverage=./cov

      - name: Generate Coverage
        if: matrix.os == 'ubuntu-latest'
        run: deno coverage --unstable --lcov ./cov > cov.lcov

      - name: Upload Coverage
        if: matrix.os == 'ubuntu-latest'
        uses: codecov/codecov-action@v2
        with:
          files: cov.lcov
```

## Publicizing

- [Deno Reddit](https://www.reddit.com/r/Deno)

- Discord Showcase Channels:

  - [UltiUniverse](https://dsc.gg/ultirequiem)

  - [Buildergroop](https://discord.gg/builders)

  - [Deno](https://discord.gg/deno)

  - [TypeScript](https://discord.gg/typescript)

  - [Code Clan](https://discord.gg/UBJFwCEBzY)

  - [DevCord](https://discord.gg/devcord)

  - 100Devs

- [Nest.land Discussion](https://github.com/nestdotland/nest.land/discussions)

- Tweet about it

If Supports Node.js all in the [npm](../npm/) section works too!
