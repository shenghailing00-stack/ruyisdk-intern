# RuyiSDK + QEMU 零硬件运行 Hello World 复现记录

## 目标

在没有 RISC-V 实体开发板的情况下，基于已经准备好的 WSL/Ubuntu 环境，通过 RuyiSDK 创建隔离实验环境，使用 RISC-V GNU 工具链交叉编译一个最小 `hello.c` 程序，并通过 QEMU 用户态模拟器运行，验证输出结果。

本文档当前作为复现流程整理稿，部分命令和输出需要在后续实际操作中进一步确认。待确认内容已标注“待实际验证”或“根据实际输出补充”。

## 适用对象

- 已完成 Windows + WSL/Ubuntu 基础环境准备的新手用户
- 暂时没有 RISC-V 开发板，但希望体验 RuyiSDK 工具链使用流程的用户
- 希望先通过 QEMU 跑通最小 RISC-V Hello World 示例的学习者

## 前置条件

- 已安装并可进入 WSL/Ubuntu 环境
- Ubuntu 中已完成基础开发工具准备
- 已安装 RuyiSDK，并可在终端中执行 `ruyi`
- 当前网络环境可访问 RuyiSDK 所需的软件源
- 磁盘空间足够安装工具链、QEMU 与实验文件

> 说明：如果 `ruyi`、工具链或 QEMU 尚未安装，需要根据实际环境先完成安装或同步软件包索引。相关步骤待实际验证后补充精确输出。

## 参考资料

- RuyiSDK 官方文档：根据实际引用链接补充
- RuyiSDK 包管理与虚拟环境相关说明：根据实际引用链接补充
- QEMU 用户态模拟相关资料：根据实际引用链接补充
- 本仓库 Windows + WSL 环境搭建文档：`../windows-wsl-setup/`

## 实验环境

以下信息需要在实际复现时补充：

| 项目 | 信息 |
| --- | --- |
| Windows 版本 | 根据实际环境补充 |
| WSL 版本 | 根据实际环境补充 |
| Ubuntu 版本 | 根据实际环境补充 |
| RuyiSDK 版本 | 根据 `ruyi version` 或实际输出补充 |
| RISC-V 工具链 | `gnu-upstream`，版本待实际验证 |
| QEMU 模拟器 | `qemu-user-riscv-upstream`，版本待实际验证 |

## 操作流程

### 1. 检查 `ruyi`

进入 WSL/Ubuntu 终端后，先确认 `ruyi` 命令是否可用：

```bash
ruyi version
```

如果命令可正常执行，记录版本信息。输出示例待实际验证后补充：

```text
根据实际输出补充
```

如提示找不到命令，需要先返回 RuyiSDK 安装步骤进行检查。

### 2. 创建实验目录

在用户目录下创建一个用于 Hello World 实验的目录：

```bash
mkdir -p ~/ruyi-qemu-hello
cd ~/ruyi-qemu-hello
```

该目录用于存放 Ruyi 虚拟环境、源码文件和编译产物。目录名称可根据实际需要调整。

### 3. 安装或确认 `gnu-upstream` 和 `qemu-user-riscv-upstream`

先查询或确认 RuyiSDK 中是否可使用 `gnu-upstream` 工具链和 `qemu-user-riscv-upstream` 模拟器：

```bash
ruyi list
```

也可以根据 RuyiSDK 实际支持的查询方式检查指定软件包。以下命令形式待实际验证：

```bash
ruyi list gnu-upstream
ruyi list qemu-user-riscv-upstream
```

如果本地尚未安装相关组件，可根据 RuyiSDK 的包管理方式安装。以下命令待实际验证：

```bash
ruyi install gnu-upstream
ruyi install qemu-user-riscv-upstream
```

> 注意：不同版本 RuyiSDK 的包管理命令可能存在差异，此处需要结合实际输出修订。

### 4. 创建 `ruyi venv`

在实验目录中创建 Ruyi 虚拟环境，使工具链和模拟器的使用尽量与系统环境隔离：

```bash
ruyi venv -t gnu-upstream -e qemu-user-riscv-upstream .venv
```

上述命令用于创建名为 `.venv` 的虚拟环境，并关联 `gnu-upstream` 工具链与 `qemu-user-riscv-upstream` 模拟器。具体参数形式待实际验证，如与当前 RuyiSDK 版本不一致，需要根据实际帮助信息调整：

```bash
ruyi venv --help
```

### 5. 激活虚拟环境

创建完成后，激活虚拟环境：

```bash
source .venv/bin/activate
```

激活后可检查当前可用的 RISC-V 编译器和 QEMU 命令：

```bash
which riscv64-unknown-linux-gnu-gcc
which qemu-riscv64
```

命令名称可能因工具链配置而不同，需以实际虚拟环境中的可执行文件为准。

### 6. 编写 `hello.c`

创建一个最小 C 语言 Hello World 程序：

```bash
cat > hello.c <<'EOF'
#include <stdio.h>

int main(void)
{
    printf("Hello, RuyiSDK + QEMU!\n");
    return 0;
}
EOF
```

确认文件内容：

```bash
cat hello.c
```

### 7. 使用 RISC-V 工具链编译

使用 RISC-V GNU 工具链编译程序：

```bash
riscv64-unknown-linux-gnu-gcc hello.c -o hello-riscv
```

如果运行动态链接程序时缺少运行时库路径，可在实际复现中改用静态链接方式进行验证：

```bash
riscv64-unknown-linux-gnu-gcc -static hello.c -o hello-riscv
```

检查生成文件类型：

```bash
file hello-riscv
```

预期应显示该文件为 RISC-V 架构的 ELF 可执行文件。具体输出根据实际结果补充。

### 8. 使用 QEMU 运行

使用 QEMU 用户态模拟器运行 RISC-V 可执行文件：

```bash
qemu-riscv64 ./hello-riscv
```

如果工具链生成的是动态链接程序，可能需要指定 RISC-V sysroot 或动态链接器路径。此部分待实际验证后补充。

### 9. 验证输出

预期输出：

```text
Hello, RuyiSDK + QEMU!
```

如果输出与预期一致，说明已经在没有 RISC-V 硬件的情况下，通过 RuyiSDK 工具链和 QEMU 用户态模拟器跑通了最小 Hello World 示例。

## 运行结果

运行结果待实际复现后补充，包括：

- `ruyi version` 输出
- 工具链和 QEMU 的实际命令名称
- `file hello-riscv` 输出
- `qemu-riscv64 ./hello-riscv` 输出

## 截图/GIF 展示

以下 GIF 后续由实际录屏剪辑补充：

![QEMU 运行 Hello World](./assets/qemu-hello-world.gif)

也可在 `assets/` 目录中补充以下材料：

- 虚拟环境创建过程截图
- 编译命令执行截图
- QEMU 运行结果截图或 GIF

## 常见问题

### 1. `ruyi: command not found`

说明当前 WSL/Ubuntu 环境中无法找到 `ruyi` 命令。需要确认 RuyiSDK 是否已经安装，并检查 PATH 配置是否生效。

### 2. 找不到 RISC-V 编译器

如果 `which riscv64-unknown-linux-gnu-gcc` 没有输出，可能是虚拟环境未激活、工具链未正确安装，或当前工具链提供的命令名称不同。需要根据 `.venv/bin/` 中的实际文件名进行确认。

```bash
ls .venv/bin
```

### 3. QEMU 命令不存在

如果 `qemu-riscv64` 不存在，需要确认 `qemu-user-riscv-upstream` 是否已经安装并被加入当前虚拟环境。

### 4. 运行时提示缺少动态链接器或库

如果 QEMU 运行动态链接程序时报错，可能需要指定 sysroot，或改用静态链接方式编译：

```bash
riscv64-unknown-linux-gnu-gcc -static hello.c -o hello-riscv
qemu-riscv64 ./hello-riscv
```

该问题需要在实际复现时根据具体报错补充最终处理方式。

## 后续整理计划

- 实际复现完整流程，补充精确命令输出
- 确认 `ruyi venv` 参数与当前 RuyiSDK 版本一致
- 补充 `gnu-upstream` 和 `qemu-user-riscv-upstream` 的版本信息
- 补充编译产物 `file` 命令输出
- 录制并剪辑 QEMU 运行 Hello World GIF
- 根据实际问题完善常见问题章节
