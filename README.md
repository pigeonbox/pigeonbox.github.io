# pigeonbox.github.io

PigeonBox 文档站的 **org 用户站**（<https://pigeonbox.github.io/>）。

## 这个仓里有什么

**只有 CI，没有内容。**

文档站的正文真相源在 hub 仓 [`pigeonbox/pigeonbox`](https://github.com/pigeonbox/pigeonbox) 的
`docs-site/` 目录（VitePress 1.x）。本仓的 `deploy.yml` 做的事是：

1. 检出 hub 仓（`repository: pigeonbox/pigeonbox`）
2. `npm ci && npm run docs:build` 构建文档站
3. 把 `docs-site/.vitepress/dist` 作为 Pages 产物发布

这样内容只有一份、不会出现"用户站里的文档和 hub 里的文档对不上"。

## 内容改了怎么上线

- **自动**：hub 仓的 `docs-site-publish` workflow 监听 `docs-site/**` 与 `docs/**` 变更，
  用 org secret `PB_BOT_TOKEN` 触发本仓的 `repository_dispatch`（事件类型 `docs-site-changed`），
  本仓重建并发布。
- **兜底**：本仓另有每日定时重建（`cron: '17 4 * * *'`），跨仓触发万一漏了最迟一天补齐。
- **手动**：本仓 workflow 支持 `workflow_dispatch`。

## 为什么要单独一个仓

GitHub Pages 的 org 根地址 `pigeonbox.github.io` 只能由**名为 `pigeonbox.github.io` 的用户站仓**
提供。`pigeonbox/charts` 仓的 Pages 挂在 `https://pigeonbox.github.io/charts/`（子路径），
两者互不影响。

## 注意

- 站内文档的增删改**不要在本仓做**，去 hub 仓 `docs-site/`。
- `deploy.yml` 的构建步骤就是门禁：VitePress 构建失败（死链、include 路径失效）则不发布，
  线上仍是上一版。
- 全部 action 按 commit SHA 钉版（依赖升级由 Dependabot 跟进 `github-actions` 生态）。

## License

Apache-2.0，详见 hub 仓 LICENSE。
