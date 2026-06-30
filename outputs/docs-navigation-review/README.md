# RuyiSDK docs navigation review and supplement checklist

## 说明

本清单基于已整理的 `navigation.md` 导航大纲，对照 `ruyisdk-docs` 当前文档结构，记录规划路径是否已有对应内容，并从文档颗粒度和可补充操作材料的角度整理后续适合拆成小 PR 的工作。


## 参考文件

- 导航大纲：`xijing21/ruyisdk-work/docs/navigation.md`
- 对照仓库：`ruyisdk/docs`
- 对照分支：`restructure-zh`
- 产出仓库：`shenghailing00-stack/ruyisdk-intern`

## 对照目标

- 提取 `navigation.md` 中规划的章节与路径。
- 对照 `ruyisdk-docs` 当前实际文件，标记已存在、缺失、路径不一致、可能已有相近内容。
- 优先识别可补充的操作步骤、截图、GIF、验证命令、命令输出、FAQ、注意事项和文档规范问题。
- 将后续工作拆成可以单独提交的小 PR，避免一次性重构目录。

## 检查维度

- 路径状态：规划路径是否在当前官方仓库中按同名路径存在。
- 内容相近度：是否已有同主题内容散落在当前目录中。
- 颗粒度：是否偏粗、过短、重复或过长。
- 操作完整度：是否有前置条件、步骤、验证命令、预期输出、失败处理。
- 媒体材料：是否适合补截图、GIF、终端输出或图示。
- 规范问题：相对路径、图片引用、代码块语言标注、表格格式、中英文空格、术语统一。

## 当前结构概览

`navigation.md` 规划的目标路径主要是 `intro/`、`start/`、`scen/`、`prod/`、`boards/`、`res/`、`faq/`、`comm/`、`legal/`。

`ruyisdk-docs` 当前主要结构为：

- `intro.md`
- `Package-Manager/`
- `IDE/`
- `VSCode-Plugins/`
- `Other/`
- `k230d/`
- `legal/`

因此多数条目只是路径体系不一致或已有相近内容但未按大纲拆分。

## 文档导航对照表

| navigation.md 规划路径 | 当前 docs 状态 | 当前内容概述 | 颗粒度判断 | 建议操作：保持 / 补充 / 合并 / 拆分 | 可补充内容 | 可拆分 PR |
| --- | --- | --- | --- | --- | --- | --- |
| `index.md` | 路径不一致 | 当前有 `intro.md` 作为入口介绍。 | 入口偏概览，缺少面向新用户的路径选择。 | 补充 | 增加“我该从哪里开始”的快速分流、安装后验证命令。 | 首页入口文案与快速分流补充。 |
| `intro/` | 路径不一致 | 介绍类内容集中在 `intro.md`、`Other/platform-support.md`、`legal/privacyPolicy.md`。 | 主题完整但集中度较高。 | 拆分 | 保留 mentor 大纲，不急于移动正文；先列出每篇应承载的边界。 | intro 章节内容边界说明。 |
| `intro/overview.md` | 可能已有相近内容 | `intro.md` 有项目介绍、功能、设备、架构图。 | 偏粗，适合压缩成产品总览。 | 补充 | 增加适用对象、核心能力、读者下一步链接。 | 产品总览补充。 |
| `intro/prioduct.md` | 缺失 | 未发现同名文件；路径名疑似 `product.md` 拼写问题。 | 尚无独立产品详述。 | 补充 | 确认文件名后补产品组成、包管理器、IDE、插件、组件之间关系。 | 确认拼写并补产品详述草稿。 |
| `intro/ecosystem.md` | 可能已有相近内容 | `intro.md` 有 Community 简述，`Other/partner-guide.md` 有生态接入。 | 生态面向用户与伙伴的边界可更清晰。 | 补充 | 社区入口、支持矩阵、资源接入、反馈渠道。 | 生态与社区入口补充。 |
| `intro/platforms.md` | 路径不一致 | `Other/platform-support.md` 有处理器架构、操作系统与发行版支持。 | 内容较完整，但位置不同。 | 保持 | 增加“安装前检查清单”和推荐环境。 | 平台支持页增加检查清单。 |
| `intro/privacy.md` | 路径不一致 | `legal/privacyPolicy.md` 已有隐私政策与遥测控制。 | 法律正文较完整，操作控制可更突出。 | 补充 | 增加一段快速关闭遥测、清理遥测数据的命令索引。 | 隐私与遥测操作索引。 |
| `intro/repos.md` | 缺失 | 未发现核心仓库总览。 | 缺少仓库地图。 | 补充 | 包管理器、packages-index、VS Code 插件、Eclipse 插件、support-matrix 等仓库职责。 | 核心仓库列表小页。 |
| `start/qemu.md` | 可能已有相近内容 | `Package-Manager/intergration.mdx`、`case6.md`、`VSCode-Plugins/index.md`、`IDE/cases/sipeed-lpi4a-ide-hello-world.md` 都涉及 QEMU。 | 内容分散，快速入门路径不够短。 | 补充 | 零硬件 Hello World：安装、创建 venv、编译、`ruyi-qemu` 运行、预期输出。 | 快速入门 QEMU 小教程。 |
| `start/hw.md` | 可能已有相近内容 | `Package-Manager/cases/case3.md`、`case4.md`、`k230d/intro.mdx` 有刷写和启动。 | 操作材料较多，但分散且偏案例。 | 补充 | 新手开发板流程：选板、刷写、启动、登录、验证网络、常见失败。 | 开发板初体验补充。 |
| `scen/index.md` | 缺失 | 当前没有典型场景总览页。 | 缺少“场景到文档”的入口。 | 补充 | 以任务为中心列出镜像、烧录、交叉编译、sysroot、调试。 | 典型场景导览页。 |
| `scen/mirror.md` | 可能已有相近内容 | `Package-Manager/packages.mdx` 有切换 ISCAS 镜像和 `ruyi update`。 | 操作步骤存在，但入口不显眼。 | 补充 | 镜像源配置、验证、回滚、网络失败 FAQ。 | 软件源与镜像加速页。 |
| `scen/flash.md` | 可能已有相近内容 | `Package-Manager/cases/case3.md`、`case4.md`、`k230d/intro.mdx` 覆盖 dd、fastboot、K230D。 | 案例较完整，通用指南可抽象。 | 拆分 | 烧录前检查、设备识别、风险提示、成功验证、失败恢复。 | 通用镜像烧录检查清单。 |
| `scen/cross-compile.md` | 可能已有相近内容 | `case1.md`、`case2.md`、`case5.md`、`case7.md` 覆盖 CoreMark、Milk-V Duo、CMake/Meson。 | 示例多但学习路径不够线性。 | 补充 | 从安装工具链到验证二进制架构、运行验证的最短路径。 | 交叉编译最小路径页。 |
| `scen/sysroot.md` | 可能已有相近内容 | `Package-Manager/intergration.mdx` 解释 venv、sysroot、profile。 | 概念与操作混在一篇，偏长。 | 拆分 | sysroot 来源、`--sysroot-from`、目录结构、CMake/Meson 使用方式。 | sysroot 操作页。 |
| `scen/debug.md` | 可能已有相近内容 | `IDE/cases/milkv-duo-ide.md` 和 `VSCode-Plugins/cases/case2.md` 有 gdbserver 调试。 | Eclipse 案例很长，VS Code 案例偏短。 | 拆分 | gdbserver 准备、端口检查、GDB 连接、断点验证、常见报错。 | 远程调试通用步骤。 |
| `scen/edu.md` | 缺失 | 未发现教学实验总览。 | 缺少课程/实验入口。 | 补充 | 课堂实验建议、可复用示例、实验环境要求。 | 教学实验资源索引。 |
| `scen/multi-dev.md` | 缺失 | 未发现多设备管理说明。 | 缺少多板卡/多镜像流程。 | 补充 | 设备命名、日志记录、镜像版本表、串口/IP 管理。 | 多设备管理注意事项。 |
| `scen/demo/duo-flash.md` | 路径不一致 | `Package-Manager/cases/case3.md` 是 Milk-V Duo dd 刷写。 | 内容较完整。 | 保持 | 增加开头适用范围、成功截图或串口输出。 | Milk-V Duo 烧录页补验证输出。 |
| `scen/demo/duo-compile.md` | 路径不一致 | `Package-Manager/cases/case2.md`、`case7.md` 是 Milk-V Duo 编译示例。 | 两篇有重叠，可区分 CoreMark 与 examples。 | 合并 | 增加“选择哪个案例”的说明，减少重复安装步骤。 | Milk-V Duo 编译案例去重。 |
| `scen/demo/lp4a-debug.md` | 可能已有相近内容 | LicheePi 4A 有编译案例和 IDE Hello World，远程调试更集中在 Milk-V Duo。 | 调试链路不完整。 | 补充 | LicheePi 4A gdbserver/SSH/端口/断点验证。 | LicheePi 4A 调试案例。 |
| `prod/pkg/install.md` | 路径不一致 | `Package-Manager/installation.mdx` 与 `_binaryPackages.mdx`、`_linuxPkg.mdx`、`_pythonPip.mdx` 覆盖安装。 | 已拆得较细，但入口页较短。 | 保持 | 增加安装方式选择表、`ruyi --version` 预期输出。 | 安装页增加选择表和验证。 |
| `prod/pkg/cli.md` | 路径不一致 | `Package-Manager/index.md` 有命令表，`packages.mdx`、`misc.mdx` 有详解。 | 命令表较宽，部分命令说明分散。 | 拆分 | 命令按 update/list/install/venv/device/self 分组，补输出示例。 | CLI 命令参考拆分。 |
| `prod/pkg/venv.md` | 路径不一致 | `Package-Manager/intergration.mdx` 有 venv、工具链、sysroot、QEMU。 | 227 行，主题偏多。 | 拆分 | venv 生命周期、激活/退出、目录结构、CMake/Meson 集成。 | venv 与 sysroot 分页。 |
| `prod/eclipse/install.md` | 路径不一致 | `IDE/index.md` 有下载、启动、在线/离线安装插件。 | 概览页承载安装细节，基本可用。 | 保持 | 补版本要求、安装成功验证、失败截图。 | Eclipse 安装验证补充。 |
| `prod/eclipse/project.md` | 可能已有相近内容 | `IDE/cases/sipeed-lpi4a-ide-hello-world.md`、`milkv-duo-ide.md` 有创建/导入项目。 | Milk-V Duo 案例过长，项目创建步骤可单独抽出。 | 拆分 | 新建项目、导入项目、绑定 venv、构建验证。 | Eclipse 创建项目小页。 |
| `prod/eclipse/debug.md` | 可能已有相近内容 | `IDE/cases/milkv-duo-ide.md` 有运行与调试 GIF。 | 单篇 353 行，调试部分适合拆分。 | 拆分 | Debug Configuration、gdbserver、Skip download、常见错误。 | Eclipse 调试流程拆分。 |
| `prod/vscode/install.md` | 路径不一致 | `VSCode-Plugins/installation.mdx` 已有前置要求、获取插件、首次使用、设置项。 | 32 行，偏短。 | 补充 | VSIX 安装截图、Marketplace 链接、安装后检测。 | VS Code 插件安装补充。 |
| `prod/vscode/usage.md` | 路径不一致 | `VSCode-Plugins/index.md`、`packages.mdx`、`venv.mdx`、`build.mdx`、`extract.mdx`、`board-docs.mdx` 分散描述功能。 | 有些页偏短，`build.mdx` 偏长。 | 拆分 | 常用任务路径、图示、命令面板入口、输出日志解释。 | VS Code 插件使用导览。 |
| `prod/comp/overview.md` | 可能已有相近内容 | `Other/GNU-type.md` 有工具链和 QEMU 表。 | 35 行，偏短。 | 补充 | 组件关系图、选择建议、和包名对应关系。 | 基础组件概览。 |
| `prod/comp/gcc.md` | 可能已有相近内容 | `Other/GNU-type.md`、`Package-Manager/packages.mdx` 有 GNU 工具链。 | 信息散落。 | 补充 | GNU 工具链类型、目标三元组、适用开发板、验证命令。 | GCC 工具链说明。 |
| `prod/comp/llvm.md` | 可能已有相近内容 | `Other/GNU-type.md`、`Package-Manager/cases/case6.md` 有 LLVM 和 QEMU。 | 示例有，组件说明少。 | 补充 | LLVM 安装、sysroot 搭配、编译和 QEMU 验证。 | LLVM 工具链说明。 |
| `prod/comp/qemu.md` | 可能已有相近内容 | `Other/GNU-type.md`、`intergration.mdx`、`case6.md` 涉及 QEMU。 | 使用说明分散。 | 补充 | user/system QEMU 区别、`ruyi-qemu` 来源、常见限制。 | QEMU 模拟器说明。 |
| `prod/comp/roadmap.md` | 缺失 | 未发现路线图页面。 | 缺少维护计划入口。 | 补充 | 可链接到公开 Roadmap 或暂列“待补充”。 | 路线图占位说明。 |
| `boards/selection.md` | 可能已有相近内容 | `intro.md` 有支持设备表，未形成选型指南。 | 偏列表，缺选择建议。 | 补充 | 按目标场景推荐板卡、最低配置、是否适合新手。 | 开发板选型指南。 |
| `boards/matrix.md` | 可能已有相近内容 | `intro.md` 支持设备表，`intergration.mdx` 链接 support-matrix。 | 当前文档内矩阵不足。 | 补充 | 支持矩阵入口、字段解释、如何读矩阵。 | 支持矩阵说明页。 |
| `boards/flashing.md` | 可能已有相近内容 | `case3.md`、`case4.md`、`k230d/intro.mdx` 有刷写教程。 | 案例多，通用前置缺。 | 拆分 | dd/fastboot/TF 卡对比、备份、设备节点确认。 | 通用刷写指南。 |
| `boards/examples.md` | 可能已有相近内容 | `Package-Manager/cases/`、`IDE/cases/`、`VSCode-Plugins/cases/` 都有示例。 | 示例分散。 | 补充 | 示例索引表：板卡、工具、目标、预计耗时。 | 开发板示例索引。 |
| `boards/specific/duo-flash.md` | 路径不一致 | `Package-Manager/cases/case3.md`。 | 内容可用。 | 保持 | 增加串口启动验证、常见卡点。 | Duo 刷写验证补充。 |
| `boards/specific/duo-compile.md` | 路径不一致 | `case2.md`、`case7.md`。 | 两篇相关但目标不同。 | 合并 | 区分 CoreMark 与 vendor examples。 | Duo 编译案例导航。 |
| `boards/specific/lp4a-debug.md` | 可能已有相近内容 | `IDE/cases/sipeed-lpi4a-ide-hello-world.md` 有 LicheePi 4A 构建和 QEMU，缺远程调试闭环。 | 需要补齐调试链。 | 补充 | gdbserver、SSH、GDB 连接、断点截图/GIF。 | LPi4A 调试小案例。 |
| `res/bare-metal.md` | 缺失 | 未发现裸机开发资源页。 | 缺少学习资源。 | 补充 | 裸机工具链、示例工程、链接到 Milk-V Duo 裸机资料。 | 裸机学习资源。 |
| `res/linux-app.md` | 可能已有相近内容 | CoreMark、Hello World、CMake/Meson 示例可作为 Linux 应用入门。 | 示例多，学习路线缺。 | 补充 | 应用开发最小路径、部署到板卡运行。 | Linux 应用学习路线。 |
| `res/kernel.md` | 缺失 | 未发现内核实验页。 | 缺少实验资源。 | 补充 | 内核构建、镜像、启动参数、调试入口。 | 内核实验资源索引。 |
| `res/courses.md` | 缺失 | 未发现课程资源页。 | 缺少课程索引。 | 补充 | 公开课、实验材料、适用人群。 | 教学资源索引。 |
| `faq/index.md` | 缺失 | FAQ 内容散落在安装、镜像、调试和隐私页面。 | 缺少集中入口。 | 补充 | 按安装、网络、镜像、编译、调试、插件分类。 | FAQ 总览页。 |
| `faq/qa-install.md` | 可能已有相近内容 | `installation.mdx`、`_binaryPackages.mdx`、`_pythonPip.mdx` 有安装注意。 | 操作说明有，问答形式少。 | 补充 | PATH、权限、pipx、镜像源、版本检查。 | 安装 FAQ。 |
| `faq/qa-debug.md` | 可能已有相近内容 | `milkv-duo-ide.md`、`VSCode-Plugins/cases/case2.md` 有调试注意。 | 调试问题散落在案例里。 | 补充 | gdbserver 不存在、端口不通、架构不匹配、下载失败。 | 编译与调试 FAQ。 |
| `comm/overview.md` | 可能已有相近内容 | `intro.md` 有 Community，`Other/partner-guide.md` 有生态接入。 | 社区入口偏弱。 | 补充 | 论坛、GitHub、Issue、Discussions、贡献路径。 | 社区入口页。 |
| `comm/repos.md` | 缺失 | 未发现 Maintainer 和仓库职责页。 | 缺少维护信息。 | 补充 | 核心仓库、职责、反馈渠道、Maintainer 信息来源。 | 仓库与 Maintainer 索引。 |
| `comm/contributing.md` | 可能已有相近内容 | `Other/partner-guide.md` 面向生态资源接入，不等同普通贡献指南。 | 面向对象不同。 | 补充 | 文档贡献、代码贡献、Issue 模板、PR 检查项。 | 文档贡献指南。 |
| `legal/licenses.md` | 路径不一致 | 仓库根目录有 `LICENSE`。 | 法律信息存在但不是 docs 页面。 | 补充 | 许可证说明页链接到 LICENSE。 | 开源许可说明页。 |
| `legal/trademarks.md` | 缺失 | 未发现商标政策。 | 缺失。 | 补充 | 商标使用范围、品牌名称写法、Logo 使用说明。 | 商标政策页。 |
| `legal/privacy.md` | 路径不一致 | `legal/privacyPolicy.md` 已有隐私政策。 | 内容完整，文件名不同。 | 保持 | 可补短入口或别名，避免重复正文。 | 隐私政策入口对齐。 |

## 规范与质量问题线索

- `navigation.md` 中 `intro/prioduct.md` 疑似拼写错误，建议确认是否应为 `intro/product.md`。
- 术语写法需要统一：`VSCode` / `VS Code`，`Milkv` / `MilkV` / `Milk-V`，`Licheepi` / `LicheePi`，`gdbsever` / `gdbserver`。
- 中英文空格可统一，例如“使用Duo”“不支持sftp”“执行ruyi”等位置适合补空格。
- 图片 alt 文本可增强，`VSCode-Plugins/` 下多处使用 `alt text` 或纯数字图片名，后续可以改成描述性 alt。
- 代码块语言和元信息需要检查，`Package-Manager/cases/case5.md` 中存在 `input` 引号不完整和 `inpupt` 拼写问题。
- 部分终端输出代码块没有语言标注，可统一为 `text`，命令块统一为 `bash`。
- 表格可读性可以优化，`Package-Manager/index.md` 的命令表较宽，`Other/GNU-type.md` 中仓库和文章链接相邻，适合拆列或补说明。
- 绝对 `/docs/...` 链接在后续导航调整时需要统一检查，避免路径迁移后断链。

## 适合拆分的小 PR 建议
1. CLI 命令参考整理：先不移动目录，只把命令按功能分组，补常用输出示例。
2. venv/sysroot 说明拆分：从 `intergration.mdx` 中提炼 venv 生命周期、sysroot 来源、CMake/Meson 使用。
3. 开发板刷写通用检查清单：补 dd/fastboot 前置检查、设备节点确认、成功验证和失败处理。
4. Eclipse Milk-V Duo 长案例拆分：保留原案例，先抽出调试步骤和常见问题，降低单篇长度。
5. VS Code 插件使用导览：补命令面板入口、各视图截图说明、构建日志和失败排查。
6. FAQ 起步页：先收集安装、网络、镜像、编译、调试、插件的高频问题，作为后续补正文的索引。
7. 文档规范修正小 PR：统一术语、中英文空格、代码块语言、图片 alt、明显拼写问题。

## 后续计划

- 整理包管理器、Eclipse 插件、VS Code 插件的操作型材料，给每个功能补验证命令和预期输出。
- 可补 FAQ 与社区贡献入口，将散落在案例中的注意事项沉淀为可检索的问题页。
- 导航结构稳定后，再按大纲逐步迁移或建立跳转，避免一次性大规模目录调整。
