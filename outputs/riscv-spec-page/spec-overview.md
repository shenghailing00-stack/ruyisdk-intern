# RISC-V Ratified Specs 导览

这份导览面向刚开始接触 RISC-V 的开发者、RuyiSDK 用户，以及需要快速找到规范下载入口的人。RuyiSDK 镜像目录收录了一批已经批准的 RISC-V 规范文件，适合用于学习、实现、移植、适配和工程查阅。

本页是帮助定位和理解规范用途的导览页，不替代规范原文；涉及实现细节时，请以 PDF 原文为准。

## 什么是 Ratified Spec

Ratified Spec 可以理解为 RISC-V International 已经正式批准的规范。相比草案或讨论中的文档，Ratified Spec 更适合作为工程实现和长期维护的依据：芯片、操作系统、固件、工具链、调试器和应用生态可以围绕这些规范建立更稳定的兼容关系。

不过，Ratified 并不表示每个人都需要从头读完所有文档。RISC-V 规范覆盖的范围很广，从基础指令集到平台固件、调试追踪、服务器 SoC、ABI 和向量 intrinsic 都有涉及。按分类阅读，可以先抓住自己当前任务需要的部分，避免一开始就被大量术语淹没。

## 建议按分类阅读

RISC-V 官方 reference 页将规范放在几个大的方向下，这些方向对应不同的工程问题：

- Core Architecture：理解 RISC-V 处理器和软件运行模型的基础。
- Profiles：理解某类平台应该具备哪些 ISA 能力组合，便于生态兼容。
- Hardware：面向硬件平台、SoC、IOMMU、QoS、服务器系统等实现问题。
- Debug, Trace, and RAS：面向调试、执行追踪、错误记录和可靠性相关能力。
- Platform Software：面向固件、启动、SBI、UEFI、平台管理等系统软件接口。
- Application Enablement：面向 ABI、应用移植、semihosting、向量 intrinsic 等应用和工具链支持。

用这个框架阅读时，可以先知道一份规范属于哪类问题，再决定是否深入细节。

## 新手先看什么

如果你刚接触 RISC-V，建议先从 Core Architecture 开始，优先理解 Unprivileged Architecture 和 Privileged Architecture。前者说明普通程序能看到的指令和执行模型，后者说明操作系统、异常、中断和特权级如何工作。

如果你在使用 RuyiSDK 做系统软件、发行版或固件相关工作，可以接着看 Platform Software，尤其是 SBI、BRS、UEFI Protocol 和 RPMI。这些规范更贴近日常系统启动、内核交互和平台管理。

如果你关心软件生态兼容，可以阅读 Profiles。Profiles 不是介绍某一条指令，而是说明一类平台应该满足哪些能力组合，适合用来判断软件是否能在目标 RISC-V 平台上稳定运行。

如果你做硬件、SoC 或平台验证，可以阅读 Hardware 分类中的 IOMMU、PLIC、Server SoC、QoS 等规范。这些文档通常更偏硬件结构、平台接口和系统集成。

如果你做调试器、追踪工具、可靠性分析或芯片 bring-up，可以阅读 Debug, Trace, and RAS。这里的文档会帮助你理解调试接口、追踪数据、trace connector、错误记录等能力。

如果你做编译器、运行库、应用移植或向量库，可以阅读 Application Enablement。ABI 和 Vector Intrinsic 对软件生态尤其重要，它们决定了函数调用、二进制接口和向量代码如何被工具链支持。

## 下载说明

本导览中的下载链接指向 RuyiSDK 镜像目录：

https://fast-mirror.isrc.ac.cn/ruyisdk/humans/docs/riscv-ratified-specs/

后续页面展示可以直接使用 `spec-cards.json` 中的分类、标题、摘要和下载链接生成卡片。
