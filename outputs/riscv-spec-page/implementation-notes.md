\# RISC-V Spec 汇总页网页实现说明



\## 当前状态

已完成内容部分整理，并基于 `ruyisdk-website` 仓库完成了本地网页原型首版实现。



\## 已完成内容

\- 完成 `spec-overview.md` 页面导读文案整理

\- 完成 `spec-cards.json` 结构化卡片数据整理

\- 在本地 `ruyisdk-website` 中实现了 `/riscv-specs` 页面原型

\- 实现导读区、人群导览卡片、分类分区、Spec 卡片和下载按钮的首版展示



\## 页面实现说明

\- 当前页面路由：`/riscv-specs`

\- 页面实现位置：`src/pages` + `src/components`

\- 当前为本地原型，已在本地 Docusaurus 开发环境中跑通

\- 尚未提交到官方 `ruyisdk-website` 远端仓库



\## 当前说明

\- `spec-cards.json` 中时间字段使用的是 `mirrorDate`

\- `mirrorDate` 表示镜像目录日期，不代表规范官方发布日期

## PR 状态

当前首版实现已经提交到 RuyiSDK 官方 website 仓库：

- https://github.com/ruyisdk/ruyisdk-website/pull/460

后续如有修改，将根据该 PR 下的 review 意见和 maintainer 反馈继续更新。



