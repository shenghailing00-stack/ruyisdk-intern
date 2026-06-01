# Windows 用户基于 WSL 搭建 RuyiSDK 开发环境

本文是我在 RuyiSDK 实习期间整理的一份 Windows + WSL 环境搭建说明，主要用于帮助 Windows 新手用户准备后续 RuyiSDK 入门实验所需的 Ubuntu 环境。

本文面向 Windows 新手用户，说明如何通过 WSL 准备 Ubuntu Linux 环境，并安装 RuyiSDK 包管理器 `ruyi`。完成本文步骤后，您可以回到主文档继续完成 `ruyi venv`、`gnu-upstream` 工具链、`qemu-upstream` 模拟器和 Hello World 示例。

## 教程目标

完成本文后，您将获得一个可用于 RuyiSDK 入门实验的 Ubuntu Linux 环境：

- Windows 中已启用 WSL；
- 已安装并启动 Ubuntu；
- Ubuntu 使用 WSL2 运行；
- Ubuntu 中已安装基础依赖；
- Ubuntu 中可以执行 `ruyi version`。

## 适用对象

本文适合以下用户：

- 正在使用 Windows 10 或 Windows 11；
- 不熟悉 Linux，但希望按照 RuyiSDK 文档完成入门实验；
- 没有单独安装 Linux 系统，希望通过 WSL 使用 Ubuntu；
- 准备继续学习 RuyiSDK、`ruyi venv`、RISC-V 交叉编译工具链和 QEMU 模拟运行。

如果您已经在 Linux x86_64 环境中工作，可以直接阅读主文档中的环境搭建章节。

## 整体流程

建议按照以下顺序完成：

1. 在 Windows PowerShell 中检查 WSL 是否可用。
2. 安装 WSL 和 Ubuntu。
3. 检查 Ubuntu 是否运行在 WSL2。
4. 进入 WSL Ubuntu 终端。
5. 更新 Ubuntu 软件源并安装基础依赖。
6. 安装 RuyiSDK 包管理器。
7. 验证 `ruyi` 是否可用。
8. 回到主文档继续完成后续 RuyiSDK 入门流程。

## PowerShell 与 WSL Ubuntu 终端的区别

Windows PowerShell 和 WSL Ubuntu 终端是两个不同的命令行环境。

| 环境 | 主要用途 | 常见命令示例 |
| --- | --- | --- |
| Windows PowerShell | 管理 Windows 和 WSL，例如安装、查看、启动 Ubuntu | `wsl -l -v`、`wsl --install` |
| WSL Ubuntu 终端 | 在 Linux 环境中安装和使用 RuyiSDK | `sudo apt update`、`ruyi version` |

本文会在每个步骤前标明命令应在哪个环境中执行。请特别注意：后续 RuyiSDK 相关命令主要在 WSL Ubuntu 终端中执行，而不是在 Windows PowerShell 中执行。

## 检查是否已经安装 WSL

以下命令在 Windows PowerShell 中执行。

```powershell
wsl --status
```

如何判断成功：

- 如果能看到 WSL 的状态信息，说明 Windows 已经识别到 WSL。
- 如果提示 `wsl` 命令不存在，请先确认 Windows 版本是否支持 WSL，并参考本文“常见问题排查”。

还可以查看当前已安装的 Linux 发行版。

以下命令在 Windows PowerShell 中执行。

```powershell
wsl -l -v
```

预期输出示例：

```text
  NAME      STATE           VERSION
* Ubuntu    Running         2
```

其中：

- `NAME` 表示已安装的 Linux 发行版名称，例如 `Ubuntu`；
- `STATE` 表示当前状态，`Running` 表示正在运行，`Stopped` 表示已停止；
- `VERSION` 表示 WSL 版本，建议为 `2`。

## 安装 WSL

如果前面检查发现尚未安装 WSL，可以安装默认的 WSL 和 Ubuntu。

以下命令在 Windows PowerShell 中执行。建议以管理员身份打开 Windows PowerShell。

```powershell
wsl --install
```

安装完成后，根据提示重启 Windows。

如何判断成功：

- 重启后，Windows 会继续安装 Ubuntu，或可以在开始菜单中找到 Ubuntu；
- 再次执行 `wsl --status` 时，不再提示 `wsl` 命令不存在。

如果默认安装没有自动安装 Ubuntu，可以查看可安装的发行版列表。

以下命令在 Windows PowerShell 中执行。

```powershell
wsl --list --online
```

然后安装 Ubuntu。

以下命令在 Windows PowerShell 中执行。

```powershell
wsl --install -d Ubuntu
```

## 安装或启动 Ubuntu

安装完成后，可以从开始菜单启动 Ubuntu，也可以通过命令启动。

以下命令在 Windows PowerShell 中执行。

```powershell
wsl -d Ubuntu
```

Ubuntu 首次启动时，会要求设置 Linux 用户名和密码。这里设置的是 Ubuntu 内部使用的 Linux 用户，不一定要与 Windows 用户名相同。

注意事项：

- 用户名建议使用小写英文字母、数字或连字符；
- 输入密码时，终端不会显示任何字符，也不会显示星号，这是 Linux 终端的正常行为；
- 请记住该密码，后续执行 `sudo` 命令时会用到。

如何判断成功：

- 设置完成后，终端提示符通常会变成类似 `用户名@计算机名:~$` 的形式；
- 此时已经进入 WSL Ubuntu 终端。

## 检查 WSL 版本

RuyiSDK 入门实验建议使用 WSL2。相比 WSL1，WSL2 使用真实 Linux 内核，系统调用兼容性更好，更接近普通 Linux 环境，适合后续安装工具链、运行开发工具和模拟器。

以下命令在 Windows PowerShell 中执行。

```powershell
wsl -l -v
```

预期输出示例：

```text
  NAME      STATE           VERSION
* Ubuntu    Running         2
```

如何判断成功：

- `Ubuntu` 这一行的 `VERSION` 为 `2`，说明 Ubuntu 正在使用 WSL2。

如果 `VERSION` 为 `1`，可以转换为 WSL2。

以下命令在 Windows PowerShell 中执行。

```powershell
wsl --set-version Ubuntu 2
```

如果希望后续新安装的 Linux 发行版默认使用 WSL2，可以执行：

以下命令在 Windows PowerShell 中执行。

```powershell
wsl --set-default-version 2
```

## 进入 WSL Ubuntu 终端

后续 RuyiSDK 相关命令建议都在 WSL Ubuntu 终端中执行。

以下命令在 Windows PowerShell 中执行。

```powershell
wsl -d Ubuntu
```

进入后，可以检查当前目录。

以下命令在 WSL Ubuntu 终端中执行。

```bash
pwd
```

预期输出示例：

```text
/home/yourname
```

如何判断成功：

- 如果输出路径以 `/home/` 开头，说明您位于 Linux 用户目录；
- 如果看到的是类似 `/mnt/c/Users/...` 的路径，说明当前位于 Windows 磁盘挂载目录。入门阶段建议先回到 Linux 用户目录。

以下命令在 WSL Ubuntu 终端中执行。

```bash
cd ~
pwd
```

## 更新 Ubuntu 软件源

安装工具前，建议先更新 Ubuntu 软件源索引。

以下命令在 WSL Ubuntu 终端中执行。

```bash
sudo apt update
```

如果系统要求输入密码，请输入前面设置的 Linux 用户密码。输入时终端不显示字符，这是正常现象。

如何判断成功：

- 命令结束后没有出现 `Failed`、`Could not resolve`、`Unable to fetch` 等明显错误；
- 最后一行可能显示可以升级的软件包数量，这是正常现象。

如果需要升级已有软件包，可以执行：

以下命令在 WSL Ubuntu 终端中执行。

```bash
sudo apt upgrade -y
```

## 安装基础依赖

以下命令在 WSL Ubuntu 终端中执行。

```bash
sudo apt install -y ca-certificates curl wget git python3 python3-venv python3-pip build-essential
```

这些软件包用于下载文件、访问 HTTPS 站点、使用 Git、运行 Python 工具以及编译基础程序。

如何判断成功：

- 命令正常结束，没有 `E: Unable to locate package` 等错误；
- 可以执行以下命令查看基础工具是否可用。

以下命令在 WSL Ubuntu 终端中执行。

```bash
python3 --version
git --version
wget --version
```

预期输出说明：

- `python3 --version` 能输出 Python 版本；
- `git --version` 能输出 Git 版本；
- `wget --version` 能输出 Wget 版本。

## 安装 RuyiSDK 包管理器

本节使用独立 Python 虚拟环境安装 `ruyi`，并把 `ruyi` 命令链接到 `~/.local/bin`。这样不会直接修改 Ubuntu 系统 Python 环境。

以下命令在 WSL Ubuntu 终端中执行。

```bash
cd ~
python3 -m venv ~/.local/share/ruyi-python
~/.local/share/ruyi-python/bin/python -m pip install --upgrade pip
~/.local/share/ruyi-python/bin/python -m pip install --upgrade ruyi
mkdir -p ~/.local/bin
ln -sf ~/.local/share/ruyi-python/bin/ruyi ~/.local/bin/ruyi
```

确认 `~/.local/bin` 已加入 `PATH`。

以下命令在 WSL Ubuntu 终端中执行。

```bash
echo "$PATH"
```

如果输出中没有 `/home/你的用户名/.local/bin` 或 `~/.local/bin`，可以把它加入 Shell 配置。

以下命令在 WSL Ubuntu 终端中执行。

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

如何判断成功：

- 安装命令正常结束；
- 执行 `command -v ruyi` 可以找到 `ruyi` 命令。

以下命令在 WSL Ubuntu 终端中执行。

```bash
command -v ruyi
```

预期输出示例：

```text
/home/yourname/.local/bin/ruyi
```

## 验证 ruyi 是否可用

以下命令在 WSL Ubuntu 终端中执行。

```bash
ruyi version
```

如何判断成功：

- 能正常输出 `ruyi` 的版本信息和版权信息；
- 没有出现 `command not found`。

还可以查看帮助信息。

以下命令在 WSL Ubuntu 终端中执行。

```bash
ruyi --help
```

如果以上命令可以正常输出，说明 RuyiSDK 包管理器已经可以在 WSL Ubuntu 中使用。

## 下一步：回到主文档继续完成入门示例

完成本文后，请回到主文档继续阅读“环境搭建与安装”和“首个项目实践”：

- 使用 `ruyi venv` 准备开发环境；
- 安装或选择 `gnu-upstream` 工具链；
- 安装或选择 `qemu-upstream` 模拟器；
- 编写、编译并运行 Hello World 示例。

请注意：除非主文档特别说明，后续 RuyiSDK、工具链、QEMU 和项目编译相关命令都应在 WSL Ubuntu 终端中执行。

## 常见问题排查

### `wsl` 命令不存在

现象：在 Windows PowerShell 中执行 `wsl --status`，提示找不到命令。

可能原因：

- Windows 版本较旧，不支持当前 WSL 安装方式；
- WSL 功能尚未启用；
- 当前终端不是 Windows PowerShell。

处理建议：

1. 确认正在 Windows PowerShell 中执行命令。
2. 更新 Windows 10 或 Windows 11 到较新版本。
3. 以管理员身份打开 Windows PowerShell 后重新执行：

以下命令在 Windows PowerShell 中执行。

```powershell
wsl --install
```

### `wsl -l -v` 中没有 Ubuntu

现象：能执行 `wsl -l -v`，但列表中没有 `Ubuntu`。

处理建议：

以下命令在 Windows PowerShell 中执行。

```powershell
wsl --list --online
wsl --install -d Ubuntu
```

安装完成后重新执行：

以下命令在 Windows PowerShell 中执行。

```powershell
wsl -l -v
```

如果看到 `Ubuntu`，说明安装成功。

### Ubuntu 的 VERSION 不是 2

现象：`wsl -l -v` 中 Ubuntu 的 `VERSION` 为 `1`。

处理建议：

以下命令在 Windows PowerShell 中执行。

```powershell
wsl --set-version Ubuntu 2
```

转换完成后再次检查。

以下命令在 Windows PowerShell 中执行。

```powershell
wsl -l -v
```

如果 Ubuntu 的 `VERSION` 为 `2`，说明已经切换到 WSL2。

### Ubuntu 首次启动要求输入用户名和密码

这是正常现象。Ubuntu 需要创建一个 Linux 用户，用于日常开发和执行 `sudo` 命令。

建议：

- 用户名使用小写英文字母或数字；
- 密码需要记住，后续执行 `sudo apt update`、`sudo apt install` 时会用到；
- 该密码通常不是 Windows 登录密码，除非您设置成相同内容。

### 输入密码时终端没有任何显示

这是 Linux 终端的正常安全行为。输入密码时不会显示字符，也不会显示星号。

处理方法：

1. 正常输入密码。
2. 按 Enter。
3. 如果密码错误，终端会提示重新输入。

### `sudo apt update` 失败

常见现象包括网络连接失败、域名解析失败、软件源暂时不可用。

可以先检查网络。

以下命令在 WSL Ubuntu 终端中执行。

```bash
ping -c 4 archive.ubuntu.com
```

如果网络不通，可以尝试：

- 检查 Windows 是否可以正常联网；
- 关闭或调整代理、VPN、防火墙后重试；
- 稍后重新执行 `sudo apt update`；
- 根据所在网络环境配置合适的 Ubuntu 软件源镜像。

以下命令在 WSL Ubuntu 终端中执行。

```bash
sudo apt update
```

### `ruyi` 命令不存在

现象：执行 `ruyi version` 时提示 `command not found`。

可能原因：

- 尚未安装 `ruyi`；
- `~/.local/bin` 没有加入 `PATH`；
- 当前终端不是 WSL Ubuntu 终端。

处理建议：

以下命令在 WSL Ubuntu 终端中执行。

```bash
command -v ruyi
echo "$PATH"
```

如果 `command -v ruyi` 没有输出，请重新执行安装步骤。若已经安装但 `PATH` 中没有 `~/.local/bin`，执行：

以下命令在 WSL Ubuntu 终端中执行。

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
ruyi version
```

### 不确定命令应该在 PowerShell 还是 Ubuntu 终端执行

可以按以下原则判断：

- `wsl` 开头的命令通常在 Windows PowerShell 中执行；
- `sudo apt`、`python3`、`git`、`ruyi` 等 Linux/RuyiSDK 命令通常在 WSL Ubuntu 终端中执行；
- 后续主文档中的 RuyiSDK 入门命令，默认在 WSL Ubuntu 终端中执行。

如果看到提示符类似：

```text
PS C:\Users\yourname>
```

说明当前是 Windows PowerShell。

如果看到提示符类似：

```text
yourname@computer:~$
```

说明当前是 WSL Ubuntu 终端。
