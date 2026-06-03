# RuyiSDK + QEMU 零硬件运行 Hello World 复现记录

## 产出背景

本产出面向没有 RISC-V 开发板的新手用户，记录在完成 WSL/Ubuntu 环境准备后，如何通过 RuyiSDK、`ruyi venv`、`gnu-upstream` 工具链和 `qemu-user-riscv-upstream` 模拟器运行一个最小 Hello World 程序。

该记录用于补充 Windows + WSL 环境搭建后的下一步实践，帮助用户在没有实体硬件设备的情况下，先验证 RISC-V 交叉编译与 QEMU 用户态模拟运行流程。

当前产出已补充实际操作 GIF，展示从环境准备、安装 RuyiSDK、处理缺失依赖、安装工具链、创建并激活虚拟环境、编写程序、交叉编译，到使用 QEMU 成功运行 Hello World 的完整过程。

## 主要内容

- 环境检查
- 创建 Ruyi 虚拟环境
- 安装或使用 `gnu-upstream` 工具链
- 安装或使用 `qemu-user-riscv-upstream` 模拟器
- 编写最小 `hello.c`
- 使用 RISC-V 工具链交叉编译
- 使用 QEMU 模拟运行
- 验证 Hello World 输出结果

## 相关文档

- [RuyiSDK + QEMU 零硬件运行 Hello World 复现记录](./hello-world-qemu.md)

## 说明

实际操作 GIF 已整理在 `assets/` 目录中，并在详细记录中按步骤引用。后续如补充新的截图或录屏材料，也可继续放在该目录下。
