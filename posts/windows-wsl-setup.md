# 面向 Windows 新手的 RuyiSDK + WSL 环境搭建记录：从 WSL 到 ruyi 可用验证

这是一份个人整理记录，主要帮助 Windows 用户通过 WSL 准备 Ubuntu 环境，并衔接后续 RuyiSDK + QEMU 的 Hello World 示例。

## 为什么先整理 WSL 环境

RuyiSDK 的入门实验通常需要一个 Linux 环境。如果手边没有单独的 Linux 系统或 RISC-V 硬件设备，Windows 用户可以先通过 WSL 安装 Ubuntu，再在 Ubuntu 中安装 `ruyi`、工具链和 QEMU 模拟器。

完成这部分后，后续就可以继续使用：

- `ruyi venv` 准备开发环境；
- `gnu-upstream` 工具链编译程序；
- `qemu-upstream` 模拟器运行 RISC-V Hello World 示例。

## PowerShell 和 WSL Ubuntu 终端的区别

刚开始使用 WSL 时，最容易混淆的是命令应该在哪个终端里执行。

| 环境 | 主要用途 | 常见命令示例 |
| --- | --- | --- |
| Windows PowerShell | 管理 Windows 和 WSL，例如安装、查看、启动 Ubuntu | `wsl -l -v`、`wsl --install` |
| WSL Ubuntu 终端 | 在 Linux 环境中安装和使用 RuyiSDK | `sudo apt update`、`ruyi version` |

简单来说，`wsl` 开头的命令一般在 Windows PowerShell 里执行；`sudo apt`、`python3`、`git`、`ruyi` 这类 Linux/RuyiSDK 命令一般在 WSL Ubuntu 终端里执行。

## 1. 检查 WSL 是否可用

以下命令在 Windows PowerShell 中执行。

```powershell
wsl --status
```

如果能看到 WSL 的状态信息，说明 Windows 已经识别到 WSL。还可以查看当前已安装的 Linux 发行版。

```powershell
wsl -l -v
```

预期输出类似：

```text
  NAME      STATE           VERSION
* Ubuntu    Running         2
```

其中 `VERSION` 建议为 `2`，也就是使用 WSL2。

## 2. 安装 WSL 和 Ubuntu

如果还没有安装 WSL，可以在 Windows PowerShell 中执行。建议以管理员身份打开 PowerShell。

```powershell
wsl --install
```

安装完成后，根据提示重启 Windows。重启后，Windows 通常会继续安装 Ubuntu，也可以在开始菜单中找到 Ubuntu。

如果默认安装没有自动安装 Ubuntu，可以先查看可安装的发行版：

```powershell
wsl --list --online
```

然后安装 Ubuntu：

```powershell
wsl --install -d Ubuntu
```

## 3. 启动 Ubuntu 并完成初始化

安装完成后，可以从开始菜单启动 Ubuntu，也可以在 Windows PowerShell 中执行：

```powershell
wsl -d Ubuntu
```

Ubuntu 首次启动时会要求设置 Linux 用户名和密码。这里设置的是 Ubuntu 内部使用的 Linux 用户，不一定要和 Windows 用户名相同。

需要注意：

- 用户名建议使用小写英文字母、数字或连字符；
- 输入密码时，终端不会显示字符，也不会显示星号，这是 Linux 终端的正常行为；
- 这个密码后续执行 `sudo` 命令时会用到，需要记住。

## 4. 确认 Ubuntu 使用 WSL2

以下命令在 Windows PowerShell 中执行。

```powershell
wsl -l -v
```

如果 `Ubuntu` 这一行的 `VERSION` 是 `2`，说明已经在使用 WSL2。

如果显示为 `1`，可以执行：

```powershell
wsl --set-version Ubuntu 2
```

如果希望后续新安装的 Linux 发行版默认使用 WSL2，可以执行：

```powershell
wsl --set-default-version 2
```

## 5. 进入 WSL Ubuntu 终端

后续 RuyiSDK 相关命令建议都在 WSL Ubuntu 终端中执行。

```powershell
wsl -d Ubuntu
```

进入后，在 WSL Ubuntu 终端中检查当前目录：

```bash
pwd
```

如果输出路径以 `/home/` 开头，说明位于 Linux 用户目录。入门阶段建议先回到 Linux 用户目录：

```bash
cd ~
pwd
```

## 6. 更新 Ubuntu 软件源并安装基础依赖

以下命令在 WSL Ubuntu 终端中执行。

```bash
sudo apt update
```

如果需要升级已有软件包，可以执行：

```bash
sudo apt upgrade -y
```

然后安装基础依赖：

```bash
sudo apt install -y ca-certificates curl wget git python3 python3-venv python3-pip build-essential
```

这些软件包用于下载文件、访问 HTTPS 站点、使用 Git、运行 Python 工具以及编译基础程序。

可以用下面的命令简单确认工具是否可用：

```bash
python3 --version
git --version
wget --version
```

## 7. 安装 RuyiSDK 包管理器

这里使用独立 Python 虚拟环境安装 `ruyi`，并把 `ruyi` 命令链接到 `~/.local/bin`，避免直接修改 Ubuntu 系统 Python 环境。

以下命令在 WSL Ubuntu 终端中执行。

```bash
cd ~
python3 -m venv ~/.local/share/ruyi-python
~/.local/share/ruyi-python/bin/python -m pip install --upgrade pip
~/.local/share/ruyi-python/bin/python -m pip install --upgrade ruyi
mkdir -p ~/.local/bin
ln -sf ~/.local/share/ruyi-python/bin/ruyi ~/.local/bin/ruyi
```

确认 `~/.local/bin` 已加入 `PATH`：

```bash
echo "$PATH"
```

如果输出中没有 `/home/你的用户名/.local/bin` 或 `~/.local/bin`，可以把它加入 Shell 配置：

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

确认 `ruyi` 命令位置：

```bash
command -v ruyi
```

预期输出类似：

```text
/home/yourname/.local/bin/ruyi
```

## 8. 验证 ruyi 是否可用

以下命令在 WSL Ubuntu 终端中执行。

```bash
ruyi version
```

如果能正常输出版本信息，并且没有出现 `command not found`，说明 RuyiSDK 包管理器已经可以在 WSL Ubuntu 中使用。

还可以查看帮助信息：

```bash
ruyi --help
```

## 下一步

本文主要完成 Windows + WSL 环境准备和 `ruyi` 可用性验证。完成本文步骤后，可以继续参考 RuyiSDK 官方文档或后续教程，使用 `ruyi venv` 配置 `gnu-upstream` 工具链和 `qemu-upstream` 模拟器，并尝试运行 Hello World 示例。

后续如果没有特别说明，RuyiSDK、工具链、QEMU 和项目编译相关命令都建议在 WSL Ubuntu 终端中执行。

## 常见问题

### `wsl` 命令不存在

如果在 Windows PowerShell 中执行 `wsl --status` 时提示找不到命令，可能是 Windows 版本较旧、WSL 功能尚未启用，或者当前终端不是 Windows PowerShell。

可以先确认正在 Windows PowerShell 中执行命令，并更新 Windows 10 或 Windows 11 到较新版本。之后以管理员身份打开 Windows PowerShell，重新执行：

```powershell
wsl --install
```

### `wsl -l -v` 中没有 Ubuntu

可以在 Windows PowerShell 中执行：

```powershell
wsl --list --online
wsl --install -d Ubuntu
```

安装完成后重新检查：

```powershell
wsl -l -v
```

### Ubuntu 的 VERSION 不是 2

如果 `wsl -l -v` 中 Ubuntu 的 `VERSION` 是 `1`，可以在 Windows PowerShell 中执行：

```powershell
wsl --set-version Ubuntu 2
```

转换完成后再次检查：

```powershell
wsl -l -v
```

### 输入密码时终端没有任何显示

这是 Linux 终端的正常安全行为。输入密码时不会显示字符，也不会显示星号。正常输入密码后按 Enter 即可；如果密码错误，终端会提示重新输入。

### `sudo apt update` 失败

常见原因包括网络连接失败、域名解析失败、软件源暂时不可用。可以先在 WSL Ubuntu 终端中检查网络：

```bash
ping -c 4 archive.ubuntu.com
```

如果网络不通，可以检查 Windows 是否能正常联网，关闭或调整代理、VPN、防火墙后重试，或者根据所在网络环境配置合适的 Ubuntu 软件源镜像。

### `ruyi` 命令不存在

先在 WSL Ubuntu 终端中检查：

```bash
command -v ruyi
echo "$PATH"
```

如果 `command -v ruyi` 没有输出，请重新执行安装步骤。若已经安装但 `PATH` 中没有 `~/.local/bin`，执行：

```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
ruyi version
```
