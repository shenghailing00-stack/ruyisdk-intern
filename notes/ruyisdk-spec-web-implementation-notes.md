
# RuyiSDK Spec 汇总页网页实现全过程笔记

## 1. 任务做了什么

两个阶段：

### 第一阶段：内容整理

先完成网页要展示的内容，包括：

- `spec-overview.md`：页面顶部的导读文案
    
- `spec-cards.json`：每个 Spec 卡片的数据
    

### 第二阶段：网页原型实现

在本地把 `ruyisdk-website` 仓库跑起来，然后基于官网仓库做了一个新的页面原型：

- 路由：`/riscv-specs`
    
- 页面位置：`src/pages/riscv-specs.jsx`
    
- 组件位置：`src/components/RiscvSpecPage/`
    


## 2. 思路


```text
先写内容 → 再跑官网仓库 → 再新增页面 → 再调样式 → 整理输出给 mentor
```


# 3. 第一步：内容部分

## 3.1 建一个单独的内容项目目录

在本地新建了一个任务目录

```text
D:\code\ruyi-spec-page
```

在这个目录里先放任务说明和内容文件

```text
ruyi-spec-page/
├─ task.md
├─ spec-overview.md
└─ spec-cards.json
```

---

## 3.2 用 Codex 先做内容

让 Codex：

- 阅读任务说明
    
- 规划内容结构
    
- 生成导览文案
    
- 整理 spec 卡片数据
    

## 3.3 内容部分后续根据 mentor 反馈做了多轮修改

mentor 提出意见：

- “致读者”区块怎么改
    
- “什么是 Ratified Spec”要不要讲生命周期
    
- “下载说明”可以删掉
    
- “新手先看什么”人群分类要更明显
    
- 某些强调语句太重复，可以删掉
    

# 4. 第二步：去 `ruyisdk-website` 仓库做网页

- 把官网仓库拉到本地
    
- 在官网仓库基础上新增页面
    
- 让本地站点跑起来
    
- 在官网的样式和结构基础上实现这个页面
    

# 5. 第三步：拉取官网仓库

## 5.1 一开始直接 clone 很慢

我最开始用的是：

```powershell
gh repo clone ruyisdk/ruyisdk-website
```

但是速度非常慢，还报过类似错误：

```text
fetch-pack: unexpected disconnect while reading sideband packet
fatal: fetch-pack: invalid index-pack output
```

### 排错

- 网络到 GitHub 不稳定
    
- 下载中断
    
- 仓库对象较多
    
- 没走代理时速度很慢
    
## 5.2 改成浅克隆

为了加快速度，我改用：

```powershell
git clone --depth 1 https://github.com/ruyisdk/ruyisdk-website.git D:\code\ruyisdk-website
```

### `--depth 1` 

表示只拉取最近一层历史，不拉完整提交历史。
只是想本地跑起来、改网页浅克隆即可

## 5.3 最后给 Git 配了代理后才成功

我电脑本身已经有本地代理，端口是：

```text
127.0.0.1:10090
```

于是我给 Git 显式配置了代理：

```powershell
git config --global http.proxy http://127.0.0.1:10090
git config --global https.proxy http://127.0.0.1:10090
```

然后再执行浅克隆，就成功了。

### 反思

Windows 系统里“开了代理”不等于 Git 一定会自动走代理。  
很多时候要手动告诉 Git：

> 以后访问 GitHub 时，请走这个代理。

---

# 6. 第四步：安装 Node.js、npm、pnpm

官网仓库是一个前端项目，要运行它，必须先有：

- Node.js
    
- npm
    
- pnpm
    
## 6.1 安装 Node.js

安装完后要验证：

```powershell
node -v
npm -v
```

如果 `node -v` 有输出，说明 Node 装好了。

## 6.2 遇到 `npm.ps1` 被 PowerShell 拦截

碰到类似报错：

```text
npm : 无法加载文件 C:\Program Files\nodejs\npm.ps1，因为在此系统上禁止运行脚本。
```

### 原因

PowerShell 默认有执行策略限制，不允许某些脚本直接执行。

### 处理方法

后来通过调整 PowerShell 执行策略，或换一种方式调用 npm，最终让 `npm -v` 可以正常运行。
## 6.3 安装 pnpm 时遇到的问题

安装 pnpm 时又遇到几类问题：

### 问题 1：`corepack enable pnpm` 报权限错误

原因通常是：

- 它想写入 `C:\Program Files\nodejs\`
    
- 当前权限不够
    
### 问题 2：`pnpm` 已存在但装坏了

报过类似：

```text
EEXIST: file already exists
```

### 处理思路

核心就是：

- 清掉旧残留
    
- 重新安装 pnpm
    
- 确保 `pnpm -v` 能输出版本号
    
# 7. 第五步：给 npm / pnpm 也配代理

虽然 Git 能 clone 仓库了，但前端依赖安装时，`pnpm ci` 还会去下载很多 npm 包。

所以我又遇到了大量超时报错，比如：

```text
GET https://registry.npmjs.org/... error
The operation was aborted due to timeout
ECONNRESET
```

### 原因

说明：

- Git 走代理了
    
- 但 npm / pnpm 不一定走代理
    
- 所以安装依赖时还是可能超时
    
## 7.1 给 npm 配代理

我用了类似下面的配置：

```powershell
npm config set proxy http://127.0.0.1:10090
npm config set https-proxy http://127.0.0.1:10090
```

## 7.2 依赖安装成功

最后执行：

```powershell
pnpm ci
```

成功把依赖装起来了。

# 8. 第六步：解决 pnpm 的 build scripts 审批问题

安装依赖时，我还遇到了这个报错：

```text
ERR_PNPM_IGNORED_BUILDS
```

### 意思

pnpm 发现某些包在安装时需要运行构建脚本，但出于安全原因先拦住了。

所以我执行了：

```powershell
pnpm ignored-builds
pnpm approve-builds
```

在交互界面里把需要的包批准了。

### 怎么理解？

这一步相当于：

> pnpm 问你：“这些包安装时要执行脚本，你允不允许？”

允许之后，再继续安装就行。

# 9. 第七步：让 Docusaurus 本地站点跑起来

## 9.1 启动命令

依赖装好以后执行：

```powershell
pnpm start
```

## 9.2 又遇到报错：`Docs version "current" has no docs`

报错大意是：

```text
Docs version "current" has no docs! At least one doc should exist at "docs".
```

### 原因

这个官网仓库启用了 Docusaurus 的 docs 功能，但本地 `docs/` 目录是空的。

### 处理方式

我在 `docs/` 下补了一个最小文档：

```text
docs/intro.md
```

让 Docusaurus 至少有一篇文档可以读。

然后再执行：

```powershell
pnpm start
```

就成功了。

## 9.3 最终成功启动

最后终端出现了类似：

```text
Docusaurus website is running at: http://localhost:3000/
```

这说明本地开发服务器已经跑起来了。

# 10.  `localhost`

## 10.1 含义

`localhost` 可以理解成：

> “我自己这台电脑”

比如：

```text
http://localhost:3000/riscv-specs
```

这个页面只有我自己电脑能访问。

## 10.2 他人看不到我的本地页面

`localhost` 指的是每个人自己的电脑。

所以：

- 我电脑上的 `localhost:3000`
    
- 他人电脑上的 `localhost:3000`
不是同一个地方。

# 11. 第八步：把内容文件带进官网仓库

为了让网页页面能直接读取内容，我把之前整理好的内容文件复制进了官网仓库，比如放到：

```text
drafts/riscv-spec-page-content/
```

里面有：

- `spec-overview.md`
    
- `spec-cards.json`
    

这样网页实现时就可以直接读这两个文件。

---

# 12. 第九步：先让 Codex 读仓库结构，再决定页面放哪

没有直接让 Codex “帮我写页面”，先让它分析：

- 这个页面适合放 `docs` 还是 `src/pages`
    
- 现有样式和组件在哪
    
- 最小实现需要改哪些文件
    

结论是：

## 放 `src/pages`

因为它是：

- 导览页
    
- 下载页
    
- 分类卡片页
    
而不是普通文档页。

---

# 13. 第十步：网页实现文件结构

最终首版页面大概涉及这几个文件：

```text
src/pages/riscv-specs.jsx
src/components/RiscvSpecPage/index.jsx
src/components/RiscvSpecPage/styles.module.css
```

### 各自作用

- `src/pages/riscv-specs.jsx`：定义页面路由
    
- `src/components/RiscvSpecPage/index.jsx`：页面主体组件
    
- `styles.module.css`：页面样式
    

# 14. 第十一步：页面做了什么

首版页面实现了这些功能：

- 顶部导读
    
- “致读者”区块
    
- “什么是 Ratified Spec”
    
- “建议按分类阅读”
    
- “新手先看什么”
    
- 6 大类分类展示
    
- 每张 spec 卡片
    
- 下载按钮
    

## 路由

最终页面路由：

```text
/riscv-specs
```

本地访问方式：

```text
http://localhost:3000/riscv-specs
```

---

# 15. 页面优化过程

## 15.1 最小版本

- 页面能打开
    
- 内容能显示
    
- 分类能看
    
- 按钮能点
    

---

## 15.2 第二轮优化

- 分类按钮
    
- 人群导览卡片
    
- 更清楚的页面层级
    


# 16. 第十二步：把任务输出整理进实习仓库

仓库是：

```text
ruyisdk-intern
```

输出目录是：

```text
outputs/riscv-spec-page/
```

---

## 16.1 最终整理进去的内容

我把这些放进了实习仓库：

```text
outputs/riscv-spec-page/
├─ README.md
├─ spec-overview.md
├─ spec-cards.json
├─ implementation-notes.md
├─ screenshots/
└─ web-prototype/
```

### 含义

- `spec-overview.md`：最新版导览文案
    
- `spec-cards.json`：卡片数据
    
- `implementation-notes.md`：网页实现说明
    
- `screenshots/`：页面截图
    
- `web-prototype/`：网页实现涉及的关键代码文件
    

# 17. PowerShell 和 cmd 的一个注意点

## 在 PowerShell 里不能直接照抄 cmd 风格命令

比如我一开始写：

```powershell
copy /Y 源 目标
```

结果报错。

### 原因

在 PowerShell 里，`copy` 实际上对应的是 `Copy-Item`，它不认 `/Y` 这种 cmd 参数。

### 正确写法

应该写成：

```powershell
Copy-Item "源文件" -Destination "目标目录" -Force
```

### 应记住

- **cmd** 风格参数：`/Y`
    
- **PowerShell** 风格参数：`-Force`
    
这两个不能混着用。

# 18. Git 工作流：把文件推进实习仓库的完整过程

## 18.1 查看当前状态

```powershell
git status
```

作用：

- 看哪些文件改了
    
- 哪些文件还没被 Git 跟踪
    
- 当前是否还有没提交的内容
    

## 18.2 把改动加入暂存区

```powershell
git add outputs/riscv-spec-page
```

作用：

- 告诉 Git：我要提交这个目录里的改动
    

## 18.3 提交 commit

```powershell
git commit -m "Update RISC-V spec page output with latest content and web prototype files"
```

作用：

- 把这次改动正式记录下来
    

### `-m` 

`-m` 后面跟的是提交说明，也就是：

> 这次提交做了什么

## 18.4 推送到 GitHub

```powershell
git push origin main
```

作用：

- 把本地已经提交的改动，上传到 GitHub 仓库
    

## 18.5 检查是否完成

```powershell
git status
```

如果看到：

```text
nothing to commit, working tree clean
```

说明：

- 本地改动已经全部提交
    
- 当前工作区是干净的
    

# 19. Git 里遇到过问题

## 19.1 Git 身份没配置，导致 commit 失败

我一开始遇到过：

```text
Author identity unknown
fatal: unable to auto-detect email address
```

### 原因

Git 不知道提交者是谁。

### 解决

配置用户名和邮箱：

```powershell
git config --global user.name "你的GitHub用户名"
git config --global user.email "你的邮箱"
```


## 19.2 `LF will be replaced by CRLF`

这个提示我也见过很多次。

### 含义

这是换行符提示：

- Linux/macOS 常用 `LF`
    
- Windows 常用 `CRLF`
    
这通常不是错误，只是提醒。


# 总结

## 判断是哪一层出了问题

报错通常分几类：

### 环境层

例如：

- Node 没装
    
- npm 被 PowerShell 拦住
    
- pnpm 没装好
    

### 网络层

例如：

- GitHub clone 超时
    
- npm registry 下载超时
    

### 项目层

例如：

- `docs/` 目录为空
    
- Docusaurus 配置需要最小文档
    

### 代码层

例如：

- 页面样式不对
    
- 结构不清楚
    
- 信息重复
    
## 23.3 本地跑起来 ≠ 用户能看到

`localhost` 只属于自己的电脑。  
如果要给别人看：

- 要么发截图
    
- 要么发仓库链接
    
- 要么部署到公网
    

# 常用命令汇总

## GitHub 仓库克隆

```powershell
git clone --depth 1 https://github.com/ruyisdk/ruyisdk-website.git D:\code\ruyisdk-website
```

## 给 Git 配代理

```powershell
git config --global http.proxy http://127.0.0.1:10090
git config --global https.proxy http://127.0.0.1:10090
```

## 检查 Node / npm / pnpm

```powershell
node -v
npm -v
pnpm -v
```

## 安装依赖

```powershell
pnpm ci
```

## 启动本地开发服务器

```powershell
pnpm start
```

## pnpm 审批构建脚本

```powershell
pnpm ignored-builds
pnpm approve-builds
```

## PowerShell 复制文件

```powershell
Copy-Item "源文件" -Destination "目标目录" -Force
```

## Git 工作流

```powershell
git status
git add outputs/riscv-spec-page
git commit -m "Update RISC-V spec page output with latest content and web prototype files"
git push origin main
```

# 收获

第一次走通了一次完整流程：

- 从内容规划开始
    
- 到本地环境搭建
    
- 到官网仓库运行
    
- 到页面实现
    
- 到 Git 提交
    
- 到输出整理给 mentor
    