
> 仅支持 Wails v3

# ToQuery/wails3-build-action@v3
GitHub action 用于构建 Wails.io：该 action 将安装 GoLang 和 NodeJS 并运行构建。
这将用于 [Wails.io](https://v3alpha.wails.io/) v3 项目。

默认情况下，action 将构建并上传结果到 GitHub；在标签构建时，它还将上传到发布版本。

# 默认构建
```yaml
- uses: ToQuery/wails3-build-action@v3
  with:
    build-name: wailsApp
    build-platform: linux/amd64
```

## 不上传的构建

```yaml
- uses: ToQuery/wails3-build-action@v3
  with:
    build-name: wailsApp
    build-platform: linux/amd64
    package: false
```

## GitHub Action 选项

| 名称                                 | 默认值               | 描述                                                        |
|--------------------------------------|----------------------|-------------------------------------------------------------|
| `build-name`                         | 无，必需输入         | 二进制文件名称                                              |
| `build-obfuscate`                    | `false`              | 使用 Garble 混淆构建                                        |
| `build-platform`                     | `darwin/universal`   | 构建目标平台                                                |
| `build-cache`                        | `true`               | 缓存构建                                                    |
| `package`                            | `true`               | 上传工作流构件并在标签时发布                                |
| `wails3-version`                     | `latest`             | 使用的 Wails 版本                                           |
| `go-version`                         | `1.25`               | 使用的 Go 版本                                              |
| `node-version`                       | `20.x`               | Node.js 版本                                                |
| `pnpm-version`                       | `10`                 | pnpm 版本                                                   |
| `deno-build`                         | ''                   | 要运行的 Deno 编译命令                                      |
| `deno-working-directory`             | `.`                  | Deno 命令的工作目录                                         |
| `deno-version`                       | `v1.20.x`            | 使用的 Deno 版本                                            |
| `app-working-directory`              | `.`                  | Wails 应用的工作目录                                        |

## 构建示例

```yaml
name: Wails build

on: [push, pull_request]

jobs:
  build:
    strategy:
      fail-fast: false
      matrix:
        build: [
          {name: wailsTest, platform: linux/amd64, os: ubuntu-latest},
          {name: wailsTest, platform: windows/amd64, os: windows-latest},
          {name: wailsTest, platform: darwin/universal, os: macos-latest}
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
