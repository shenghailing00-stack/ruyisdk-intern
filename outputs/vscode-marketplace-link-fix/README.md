# VS Code Marketplace link fix

This directory records a website link fix submitted to the official RuyiSDK website repository.

## Official PR

- Title: fix: update VS Code marketplace link
- Link: https://github.com/ruyisdk/ruyisdk-website/pull/504

## Summary

Fixed the Visual Studio Marketplace link on the RuyiSDK dashboard.

The previous link pointed to the publisher management page:

- https://marketplace.visualstudio.com/manage/publishers/RuyiSDK

That page is only accessible to publisher administrators, so regular users could not open it. The fix replaces it with the public extension detail page:

- https://marketplace.visualstudio.com/items?itemName=RuyiSDK.ruyisdk-vscode-extension

## Scope

- Page: RuyiSDK dashboard
- Files:
  - `src/components/Dashboard/mdx/stats_detail.en.md`
  - `src/components/Dashboard/mdx/stats_detail.zh-Hans.md`
  - `src/components/Dashboard/mdx/stats_detail.de.md`
- Output type: Website link fix / Documentation maintenance
