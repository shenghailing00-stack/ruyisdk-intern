# VS Code case1 code block rendering fix

This directory records a documentation rendering fix submitted to the official RuyiSDK website repository.

## Official PR

- Title: docs: fix code block rendering in VS Code case1
- Link: https://github.com/ruyisdk/ruyisdk-website/pull/495
- Status: Submitted, checks passed, pending review/merge

## Summary

Fixed a Markdown rendering issue in the English i18n documentation page:

- `i18n/en/docusaurus-plugin-content-docs/current/VSCode-Plugins/cases/case1.md`

The bash command lines under steps 5 and 6 were not properly nested in the ordered list, which caused incorrect rendering on the English documentation page. The fix indented the bash command lines so the code blocks render correctly and step 7 continues normally.
