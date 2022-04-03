# Deno

> All from [common](../common) applies here.

Deno is a simple, modern and secure runtime for JavaScript and TypeScript that
uses V8 and is built in Rust.

Is cool because 👇

- Has built-in utilities like a dependency inspector/code formatter/linter

- No file, network, or environment access, unless explicitly enabled.

- Supports TypeScript out of the box.

- Ships only a single executable file.

## Musts

- Written in TypeScript

- Named exports

> Even if is only one function

- Code coverage upper than 50%

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
    name: Test (${{ matrix.os }})
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest, macOS-latest]
      fail-fast: true
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Setup Deno
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

### Periodically update dependencies

```yaml
# update-dependencies.yaml

name: update-deno-dependencies

on:
  workflow_dispatch:
  schedule:
    - cron: "0 0 1 * *"

jobs:
  udd:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@3
      - uses: denoland/setup-deno@v1
      - name: Update dependencies
        run: >
          deno run -A https://deno.land/x/udd/main.ts
          $(find . -name "*.ts")
      - name: Create Pull Request
        uses: peter-evans/create-pull-request@v3
        with:
          commit-message: "chore(deps): update deno dependencies"
          title: Update Deno Dependencies
          body: >
            Automated updates by [deno-udd](https://github.com/hayd/deno-udd)
            and [create-pull-request](https://github.com/peter-evans/create-pull-request)
            GitHub action
          branch: update-deno-dependencies
          author: GitHub <noreply@github.com>
          delete-branch: true
```

### Page from Readme with a `js.org` domain

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
          echo ${{ github.event.repository.name }}.js.org >> output/CNAME
          echo theme: jekyll-theme-cayman >> output/_config.yml
      - name: Deploy 🚀
        uses: JamesIves/github-pages-deploy-action@v4.2.5
        with:
          branch: gh-pages
          folder: output
```

> Change the CNAME part if necessary

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

### GitHub Tags

```
typescript, javascript, deno, javascript-lib, npm, npm-package, node,
nodejs, browser, browser-library
```

### Intro

```markdown
# REPO

[![codecov](https://codecov.io/gh/ultirequiem/REPO/branch/main/graph/badge.svg)](https://codecov.io/gh/ultirequiem/REPO)
[![deno doc](https://doc.deno.land/badge.svg)](https://doc.deno.land/https/deno.land/x/REPO/mod.ts)

Description

> Why I did this? If the name is an acronym explain it here.
```

### Usage

```markdown
## Usage

### [Deno 🦕](https://deno.land/x/REPO)

``javascript
import { randomTimeZone } from "https://deno.land/x/REPO/mod.ts";

console.log(`My timezone is ${randomTimeZone()}`);
``

### [Node.js 🦖](https://www.npmjs.com/package/@ultirequiem/REPO)

`javascript import { timeZones } from "@ultirequiem/REPO"; `

### [Browser 🌐](https://developer.mozilla.org/en-US/docs/Glossary/Browser)

You can use any [CDN](https://en.wikipedia.org/wiki/Content_delivery_network) 🔥

Eg. [ESM](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules)
▶ [SkyPack](https://cdn.skypack.dev/@ultirequiem/REPO) _/_
[Script Tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script) ▶
[JSDelivr](https://cdn.jsdelivr.net/npm/@ultirequiem/REPO).

_The API is the same on all this platforms._
```

### Documentation

```
## Documentation

Is hosted on [Deno Doc](https://doc.deno.land/https://deno.land/x/REPO/mod.ts) 🦖
```

### CLI

```markdown
## CLI

### Install

- **Deno 🎃**

`deno install https://deno.land/x/REPO/REPO.ts`

- **Node.js 🐼**

`sh npm install -g @ultirequiem/REPO`

### Usage

`REPO --help`

It includes `list`, `random` and a fuzzy search command!

[A binary for multiple platforms is provided too!](https://github.com/UltiRequiem/REPO/releases/latest)

[Video Showcase](https://youtu.be/8dUX5sPlAX4)
```
