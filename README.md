> Only Wails v3

# ToQuery/wails3-build-action@v3

GitHub action to build Wails.io: the action will install GoLang and NodeJS and run a build.
This will be used on a [Wails.io](https://v3alpha.wails.io/) v3 project.

By default, the action will build and upload the results to GitHub; on a tagged build, it will also upload to the
release.

# Default build

```yaml
- uses: ToQuery/wails3-build-action@v3
  with:
    build-name: wailsApp
    build-platform: linux/amd64
```

## Build with No uploading

```yaml
- uses: ToQuery/wails3-build-action@v3
  with:
    build-name: wailsApp
    build-platform: linux/amd64
    package: false
```

## GitHub Action Options

| Name                    | Default              | Description                                        |
|-------------------------|----------------------|----------------------------------------------------|
| `go-version`            | `1.25`               | Version of Go to use                               |
| `wails3-version`        | `latest`             | Wails version to use                               |
| `build-name`            | none, required input | The name of the binary                             |
| `build-obfuscate`       | `false`              | Obfuscate the build using Garble                   |
| `build-platform`        | `darwin/universal`   | Platform to build for                              |
| `build-cache`           | `true`               | Cache the build                                    |
| `package`               | `true`               | Upload workflow artifacts & publish release on tag |

### Node Config

| Name           | Default | Description     |
|----------------|---------|-----------------|
| `node-build`   | `true`  | Node.js version |
| `node-version` | `20.x`  | Node.js version |

### pnpm Config

| Name           | Default | Description     |
|----------------|---------|-----------------|
| `pnpm-build`   | `true`  | Node.js version |
| `pnpm-version` | `10`    | pnpm version    |

### Deno Config

| Name                     | Default   | Description                        |
|--------------------------|-----------|------------------------------------|
| `deno-build`             | 'false'   | Deno compile command to run        |
| `deno-version`           | `v1.20.x` | Deno version to use                |
| `deno-working-directory` | `.`       | Working directory for Deno command |

## Example Build

```yaml
name: Wails build

on: [ push, pull_request ]

jobs:
  build:
    strategy:
      fail-fast: false
      matrix:
        build: [
          { name: wailsTest, platform: linux/amd64, os: ubuntu-latest },
          { name: wailsTest, platform: windows/amd64, os: windows-latest },
          { name: wailsTest, platform: darwin/universal, os: macos-latest }
        ]
    runs-on: ${{ matrix.build.os }}
    steps:
      - uses: actions/checkout@v4
        with:
          submodules: recursive
      - uses: ToQuery/wails3-build-action@v3
        with:
          build-name: ${{ matrix.build.name }}
          build-platform: ${{ matrix.build.platform }}
          build-obfuscate: true
```
