# RuyiSDK Package Manager Documentation Review

## 说明

本清单针对 `ruyisdk/docs` 仓库 `restructure-zh` 分支下现有 `Package-Manager/` 文档进行审查。

审查原则：

- 以 `ruyisdk/ruyi`、`ruyisdk/packages-index` 源码和仓库内容为主要依据。
- 涉及 CLI 行为时，结合本地实际运行结果确认。
- 不根据第三方网页或 AI 推测补充产品功能。
- 文案修改优先保证真实、简洁、易懂。
- 涉及命令时不随意改写；需要修改的命令应先验证实际行为。

## 审查范围

主要检查：

- `Package-Manager/index.md`
- `Package-Manager/installation.mdx`
- `Package-Manager/_binaryPackages.mdx`
- `Package-Manager/_linuxPkg.mdx`
- `Package-Manager/_pythonPip.mdx`
- `Package-Manager/packages.mdx`
- `Package-Manager/intergration.mdx`
- `Package-Manager/misc.mdx`
- `Package-Manager/cases/`

## 验证环境

本地验证环境：

```text
Ruyi 0.48.0
Linux / x86_64
WSL
```

同时对照：

- `ruyisdk/ruyi` 的 `0.48.0` tag
- 当前正式版 `0.52.0` 中对应实现
- `ruyisdk/packages-index` 当前仓库内容

## 已确认问题

> 编号是问题编号
| 编号 | 文档位置（点击打开） | 问题 | 依据 | 建议 |
| --- | --- | --- | --- | --- |
| 1 | [`Package-Manager/installation.mdx`](https://github.com/ruyisdk/docs/blob/restructure-zh/Package-Manager/installation.mdx) | 当前仍把预编译二进制作为第一推荐安装方式，但目前产品方向已调整为优先推荐 PyPI。 | 已确认产品要求；`ruyisdk/ruyi/README.zh.md` 也将 PyPI 标为推荐方式。 | 将 PyPI 调整为第一推荐，并同步安装方式顺序与默认 Tab。 |
| 2 | [`Package-Manager/_pythonPip.mdx`](https://github.com/ruyisdk/docs/blob/restructure-zh/Package-Manager/_pythonPip.mdx) | PyPI 安装说明没有明确 Python 最低版本。 | `ruyisdk/ruyi/pyproject.toml` 在 0.48.0 与 0.52.0 均声明 `requires-python = ">=3.11"`。 | 明确写出 Python >= 3.11。 |
| 3 | [`Package-Manager/misc.mdx`](https://github.com/ruyisdk/docs/blob/restructure-zh/Package-Manager/misc.mdx) | 更新和卸载只覆盖预编译二进制和系统包管理器，没有覆盖 PyPI / pipx。 | 文档内部对照。 | PyPI 成为首选后补充对应升级与卸载说明。 |
| 4 | [`installation.mdx`](https://github.com/ruyisdk/docs/blob/restructure-zh/Package-Manager/installation.mdx) / [`_linuxPkg.mdx`](https://github.com/ruyisdk/docs/blob/restructure-zh/Package-Manager/_linuxPkg.mdx) | 顶层把系统包管理器安装概括为类似 `apt` / `yum`，并称前提是发行版官方收录；实际子页主要是 AUR、Arch Linux CN 与 Gentoo overlay。 | 文档内部对照。 | 调整顶层概括，使其与实际支持来源一致。 |
| 5 | [`Package-Manager/index.md`](https://github.com/ruyisdk/docs/blob/restructure-zh/Package-Manager/index.md) | `ruyi list --name-contains` 与 `ruyi list --verbose --name-contains` 被当作完整命令，但该选项必须带参数。 | 本地运行提示 `expected 1 argument`，退出码 2；`ruyi/ruyipkg/list_cli.py` 中为 `nargs=1`。 | 修改说明或示例，但正式改命令前保持实际验证。 |
| 6 | [`Package-Manager/index.md`](https://github.com/ruyisdk/docs/blob/restructure-zh/Package-Manager/index.md) | `ruyi news read` 被描述为“读取下一条新闻”，实际是不指定 item 时读取全部未读新闻。 | `ruyi/ruyipkg/news_cli.py`：`Defaults to reading all unread items if no item is specified.` | 修改命令说明，不改命令本身。 |
| 7 | [`Package-Manager/index.md`](https://github.com/ruyisdk/docs/blob/restructure-zh/Package-Manager/index.md) | `ruyi self clean` 被描述为“清除数据目录”，容易理解为裸命令即可执行清理。实际必须指定清理目标。 | 本地运行返回 `no data specified for cleaning`，退出码 1；`ruyi/cli/self_cli.py` 行为一致。 | 改为说明该命令需要具体清理选项。 |
| 8 | [`index.md`](https://github.com/ruyisdk/docs/blob/restructure-zh/Package-Manager/index.md) / [`packages.mdx`](https://github.com/ruyisdk/docs/blob/restructure-zh/Package-Manager/packages.mdx) | `ruyi update` 仍主要按单一默认软件源描述，没有体现多软件源行为。 | 本地 `ruyi update --help` 已有 `--repo REPO`；`ruyi repo list` 可列出软件源；`ruyi/ruyipkg/update_cli.py` 无参数时执行 `sync_all()`。 | 补充多软件源说明，并说明 `--repo` 用途。 |
| 9 | [`Package-Manager/packages.mdx`](https://github.com/ruyisdk/docs/blob/restructure-zh/Package-Manager/packages.mdx) | 软件包分类列表缺少 `board-util`。 | 当前 `packages-index/packages/` 中存在 `board-util/`。 | 补充该分类，或避免把易变化的分类列表写成固定完整清单。 |
| 10 | [`Package-Manager/packages.mdx`](https://github.com/ruyisdk/docs/blob/restructure-zh/Package-Manager/packages.mdx) | 多包安装示例中将 `gnu-upstream` 写成 `gnu-upsteam`。 | `packages-index` 中不存在 `gnu-upsteam`；存在 `gnu-upstream`。 | 修正拼写。 |
| 11 | [`Package-Manager/intergration.mdx`](https://github.com/ruyisdk/docs/blob/restructure-zh/Package-Manager/intergration.mdx) | `ruyi list profiles` 示例输出已过时，仍使用旧的简单列表和 `needs flavor(s)`。 | 本地 0.48.0 输出已包含 `arch:` 与 `needs quirks:`；`ruyi/ruyipkg/profile_cli.py` 与实测一致。 | 更新示例输出，避免长期保留过长动态列表。 |
| 12 | [`Package-Manager/intergration.mdx`](https://github.com/ruyisdk/docs/blob/restructure-zh/Package-Manager/intergration.mdx) | 示例中 `myhone-venv` 与 `myhome-venv` 混用。 | 文档静态检查。 | 统一示例名称。 |
| 13 | [`Package-Manager/misc.mdx`](https://github.com/ruyisdk/docs/blob/restructure-zh/Package-Manager/misc.mdx) | telemetry 的 `off` 被简单描述为“关闭遥测数据收集功能”，没有说明首次运行的一次性版本信息上传。 | `ruyisdk/ruyi/README.zh.md` 与 `ruyi/telemetry/provider.py` 均说明首次运行例外。 | 与官方源码和 README 的实际行为保持一致。 |
| 14 | [`Package-Manager/cases/case2.md`](https://github.com/ruyisdk/docs/blob/restructure-zh/Package-Manager/cases/case2.md) | 前文使用 `gnu-milkv-milkv-duo-musl-bin`，后文却称当前工具链为 `gnu-milkv-milkv-duo-bin`。 | `packages-index` 中两个包都存在，分别对应 musl 与 glibc。 | 根据案例实际使用的工具链统一名称。 |
| 15 | [`Package-Manager/cases/case5.md`](https://github.com/ruyisdk/docs/blob/restructure-zh/Package-Manager/cases/case5.md) | 代码块元信息存在明显格式错误：一处 `input` 引号不完整，一处写成 `inpupt`。 | 文档静态检查。 | 修正文档格式。 |
| 16 | [`Package-Manager/cases/case5.md`](https://github.com/ruyisdk/docs/blob/restructure-zh/Package-Manager/cases/case5.md) | zlib 下载、进入目录和解压步骤中的文件路径不一致。 | 按现有命令顺序静态检查可见路径不一致。 | 正式修改前先完整运行该案例，再调整步骤。 |
| 17 | [`Package-Manager/cases/case5.md`](https://github.com/ruyisdk/docs/blob/restructure-zh/Package-Manager/cases/case5.md) | Meson 示例硬编码 `/home/cyan/zlib-ng/venv/meson-cross.ini`。 | 文档静态检查。 | 改成可复用的路径表达，避免绑定某个用户目录。 |

## 本地验证记录

### `ruyi list --name-contains`

实际运行：

```text
ruyi list: error: argument --name-contains: expected 1 argument
exit code: 2
```

说明当前 `index.md` 中不能把它作为无需参数的完整命令示例。

### `ruyi self clean`

实际运行：

```text
fatal error: no data specified for cleaning
info: please check ruyi self clean --help for a list of cleanable data
exit code: 1
```

裸命令不会执行清理。

### 多软件源

实际运行：

```text
$ ruyi update --help
usage: ruyi update [-h] [--repo REPO]

--repo REPO  only sync the repo with this ID
```

同时：

```text
$ ruyi repo list
* ruyisdk (default) priority=0 https://github.com/ruyisdk/packages-index.git
```

与 `ruyi/ruyipkg/update_cli.py`、`repo_cli.py` 的实现一致。

### `ruyi list profiles`

本地输出包含：

```text
generic (arch: riscv64)
baremetal-rv64ilp32 (arch: riscv64, needs quirks: rv64ilp32)
sipeed-lpi4a (arch: riscv64, needs quirks: xthead)
milkv-duo (arch: riscv64, needs quirks: xthead)
```

因此现有文档中的旧输出需要更新。

### `ruyi extract`

实际运行：

```text
info: package ruyisdk-demo-0.20231114.0 has been extracted to ruyisdk-demo-0.20231114.0
```

目录结构：

```text
./ruyisdk-demo-0.20231114.0
./ruyisdk-demo-0.20231114.0/README.md
./ruyisdk-demo-0.20231114.0/rvv-autovec
```

这与当前 `packages.mdx` 中“默认解压到独立子目录”的说明一致。

## 待进一步验证

`case1.md`、`case2.md`、`case6.md`、`case7.md` 中都包含 `ruyi extract` 后继续操作源码的流程。

由于不同软件包的解包布局可能不同，目前只验证了 `ruyisdk-demo`，不能据此判断所有案例都错误。后续正式修改前应分别运行对应案例。

## 重构阶段处理

### `intergration.mdx` 文件名

`intergration` 是 `integration` 的拼写错误。

但当前已有其他页面引用该路径，因此不在本轮直接重命名，留待目录结构调整时统一处理链接。

### 动态输出

部分案例保存了完整设备列表、profile 列表或向导输出。

这类内容会随 `packages-index` 更新而变化。后续重构时应尽量：

- 只保留与案例相关的关键输出。
- 提示用户以当前实际命令输出为准。
- 避免依赖固定编号完成操作。
