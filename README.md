# 2048 - 离线游戏

一个纯前端、零依赖的 2048 网页游戏，单文件即可部署。基于同名 Android 离线游戏合集移植，保留三种对战模式。

## 在线体验

- 默认地址：<https://wang2000star.github.io/2048-wfz/>
- 自定义域名：<https://2048.wfz521.com/>

## 功能

- **✋ 手动模式**：手机滑动 / 电脑方向键 / WASD / 鼠标拖动
- **🎲 随机模式**：电脑随机选择有效方向自动对弈
- **🧠 智能模式**：Expectimax + Snake Heuristic AI，从 C++ 版（[`ai.cpp`](https://github.com/wang2000star/2048-wfz/blob/main/index.html) 内联）移植
- **分数与最高分**：最高分用 `localStorage` 持久化，下次打开自动恢复
- **移动优先**：viewport-fit=cover、`safe-area-inset`、`touch-action: none` 防误触
- **视觉**：深色主题，色块/字号比例与 Android 版完全一致

## 一键部署

整站只有一个文件 [`index.html`](./index.html)，没有任何外部依赖、没有构建步骤。

### GitHub Pages

1. Fork 或新建仓库，把 `index.html` 推到 `main` 分支根目录
2. 仓库 Settings → Pages → Source: `Deploy from a branch` → `main` / `/(root)`
3. 等 30 秒，访问 `https://<用户名>.github.io/<仓库名>/`

### 自定义域名

1. 在域名解析商添加 CNAME 记录：`<子域名>` → `<用户名>.github.io`
2. 仓库 Settings → Pages → Custom domain 填入子域名，Save
3. 勾选 Enforce HTTPS（GitHub 自动签 Let's Encrypt 证书，几分钟内可用）

### 其它平台

- Cloudflare Pages / Netlify / Vercel：直接连仓库或拖拽 `index.html`，秒级上线
- 任意静态托管：上传 `index.html` 即可

## 技术细节

- 整个游戏约 23KB，单 HTML 文件包含全部 CSS + JS
- AI 搜索深度 `max(3, distinct_tiles - 2)`，上限 4（C++ 原版上限更高，Web 上做性能折中）
- 启发式权重与 C++ 原版完全一致：
  - `EMPTY_WEIGHT = 270`、`MERGES_WEIGHT = 700`
  - `MONOTONICITY_WEIGHT = 47, POWER = 4`
  - `SUM_WEIGHT = 11, POWER = 3.5`
  - `LOST_PENALTY = 200000`
- 转置缓存（`Map`）剪枝重复子局面，单次决策 < 200ms（中等棋盘）

## 致谢

- 2048 原作 © Gabriele Cirulli
- AI 算法基于 [nneonneo/2048-ai](https://github.com/nneonneo/2048-ai) 的 expectimax 实现
