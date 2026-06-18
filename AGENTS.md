# AGENTS.md

## 项目

SMBX World 社区主页（门户页）项目。基于 Astro 6.x 的静态站点，中文（zh-CN），复古马力欧主题设计。

## 命令

- `bun run dev` — 启动开发服务器（监听 0.0.0.0）
- `bun run build` — 生产构建到 `dist/`
- `bun run preview` — 预览生产构建
- `bun run deploy` — 服务器部署（运行 `deploy.sh`：git pull → bun install → bun run build → rsync 到 `/data/wwwroot/www.smbx.world/`）

**包管理器：Bun。** 不要使用 npm/yarn/pnpm。

## 结构

```
src/
  pages/          # 路由文件 (*.astro) — 每个文件对应一个页面
  layouts/        # BaseLayout.astro（单一布局，包裹所有页面）
  components/     # BlockBox.astro（基于表格的彩色盒子，主要布局原语）
  styles/         # global.css（单一文件，约 1400 行，包含 normalize.css）
public/
  images/         # 静态资源（affiliates/, assets/, screenshots/, uploads/）
```

## 约定

- **未配置 lint、typecheck 或测试命令。** TypeScript 仅使用 Astro 严格模式。
- 所有内容均为中文。任何面向用户的文本请保持中文。
- 布局有意使用 `<table>` 元素（复古设计），主要结构不使用现代 CSS grid/flex。
- `BlockBox` 组件接受 `color` 属性：`blue | green | orange | white`。每种颜色有不同的精灵样式。
- `BaseLayout` 接受可选的 `theme` 属性：`default | desert | ice`。主题会切换背景/管道/页脚图片。
- 包含数据的页面（如 episodes、links）在 frontmatter 部分定义内容数组。
- 图片灯箱效果在 `BaseLayout.astro` 中内联实现（原生 JS，无库依赖）。
- `BlockBox` 有客户端高度调整脚本，使内容对齐到 32px 网格。
- 组件属性必须使用小写属性名（如 cellpadding 而非 cellPadding）。
- 外部链接必须设置为新窗口打开，使用 `target="_blank" rel="noopener noreferrer"`。
- CSS 必须使用非压缩格式，标准格式化和分节注释。
- 站点配置使用 `https://www.smbx.world` 作为基础 URL。
- BlockBox 组件内容段落间距为 0.8em，行高为 1.7，最后一段无底部边距。
- BlockBox 组件默认 padding-bottom: 12px，但 `.block-links` BlockBox 和 `.archive-body` 橙色 BlockBox 使用 padding-bottom: 0。
- 管道导航使用 div flex 布局。
- 管道下移效果仅在 hover 文字链接时触发，通过 CSS `:has()` 选择器实现。
- `.featured-image` 使用 flex 布局，align-items: center 和 justify-content: center 来居中图片。

## 硬性约束

- 不要移除管道背景图片（pipe.png 及其主题变体）
- 所有图片文件必须组织在 `public/images/` 下的子目录中

## 部署

`deploy.sh` 在服务器上运行（非本地）。它拉取 `main` 分支，构建，然后将 `dist/` rsync 到网站根目录。脚本假设 Bun 位于 `$HOME/.bun/bin`。