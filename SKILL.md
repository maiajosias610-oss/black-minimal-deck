---
name: black-minimal-deck
description: 生成「黑色极简风」单文件 HTML 演示文稿（纯黑底 #000000 + 荧光柠绿 #cdf030，Inter + Noto Sans SC，固定 16:9 舞台，30+ 复用组件），并可一键导出高保真 PPTX。Use when the user asks for a black minimal / dark keynote-style HTML deck, a VC/business-plan slide deck in the black+lime style, or converting such an HTML deck to PPTX. Triggers: 黑色极简 PPT、黑底荧光绿演示、极简风幻灯片、black minimal deck、dark minimal slides.
---

# Black Minimal Deck（黑色极简演示设计系统）

一个可直接交付的单文件 HTML 演示系统：纯黑底 + 荧光柠绿强调色，固定 16:9 舞台，
无需构建工具，双击即可在浏览器打开演示，也可一键导出为高保真 PPTX。

## 何时使用

- 用户要「黑色极简风 / 黑底荧光绿」的演示文稿、路演 BP、产品发布页
- 用户已有内容大纲（md / 文字），需要快速成稿为一套幻灯片
- 用户要把本风格的 HTML 幻灯片导出成 PPTX

## 快速开始

1. 复制 `templates/deck.html` 作为起点（内含设计令牌页 + 组件画廊 + 示例页，可直接当参考稿）。
2. 按下方「版式配方」替换各 `<section class="slide">` 的内容。
3. 浏览器打开验证：`文件路径/deck.html#slide-N` 可直达第 N 页。
4. 需要交付 PPTX 时运行 `python scripts/render_pptx.py <deck.html> <输出.pptx>`。

## 设计令牌（唯一颜色来源，禁止散落硬编码）

| Token | 值 | 用途 |
|---|---|---|
| `--bg` | `#000000` | 页面背景 |
| `--text` | `#FFFFFF` | 主文字 |
| `--accent` | `#cdf030` | 强调色：序号/高亮/实心块/图表 |
| `--gray` | `#bbbbbb` | 次级文字、说明 |
| `--line` | `rgba(187,187,187,0.25)` | 描边、分隔线 |
| `--pad-x` / `--pad-y` | `120px` / `100px` | 页面安全边距 |
| `--stage-w` / `--stage-h` | `1920px` / `1080px` | 固定 16:9 舞台 |

换主题 = 只改 `:root` 里的变量，不改任何组件。

## 硬性设计约束（违反即返工）

1. **颜色一律走 CSS 变量**；确需透明度用 `rgba(205,240,48,x)`（accent 的透明变体）。
2. **禁止 emoji**、禁止紫粉渐变、禁止 AI 模板感（花哨阴影、彩色玻璃拟态）。
3. 字体：西文 `Inter`，中文 `Noto Sans SC`；仅此两族（离线自动回退系统字体）。
4. 固定 16:9 舞台（1920×1080，JS 自动缩放居中），内容不出血、不滚动。
5. 每页信息量克制：一个主标题 + 一组组件；留白是设计的一部分。
6. 每页底部放 `页码`（右上 `page-num-fixed`）与可选 `label`（章节英文标记）。

## 字体层级

| 类 | 规格 | 用途 |
|---|---|---|
| `.display` | 900 / 110px / 大写 | 封面巨字 |
| `.h1` | 800 / 64px | 页标题 |
| `.h2` | 700 / 44px | 次级大标题 |
| `.h3` | 400 / 22px / 灰 | 引导句 |
| `.module-title` | 600 / 20px | 卡片内标题 |
| `.body` | 400 / 17px / 灰 / 行高1.64 | 正文 |
| `.label` | 500 / 12px / 大写 | 页眉角标 |
| `.quote` | 500 / 22px / 斜体 | 引用 |
| `.chapter-num` | 900 / 260px / accent | 章节巨号 |
| `.kpi-num` / `.bnum` | 900 / 44px / 64px / accent | 指标大数 |

## 组件目录（全部在 templates/deck.html 内有示例）

**卡片类**
- `.proof`：空心灰描边卡片，可点击展开（`data-proof` + `.proof-detail`）——并列要点/证据
- `.proof-core`：实心荧光绿块黑字——每页至多一个，放最关键的结论
- `.diff3 .diff-card`：三栏差异化卡片（`.diff-no` 荧光绿编号）
- `.case`：案例卡；`.team-card`：团队卡（`.team-avatar` 圆形头像位）
- `.kpi`（`.kpi-num/.kpi-unit/.kpi-label`）：KPI 卡，`.kpi-grid` 默认四列
- `.big3`：三个超大数字；`.gate`：四宫格提问卡

**流程 / 结构类**
- `.funnel-row` + `.funnel-step` + `.funnel-arrow`：递进漏斗（第 1/3/5/7 格自动递进绿底，末格高亮）
- `.synergy` + `.synergy-step` + `.synergy-arrow`：横向链路；`.shared .chip`：共享能力胶囊
- `.io-band`：底部闭环图形条（`.io-pill` 实心起点 / `.io-node` 描边节点 / `.io-chip` 标签 / `.io-arrow` 圆点箭头 SVG）
- `.compare-table .compare-row`：对比行（`.active` = 荧光绿实心高亮）
- `.module-list .module-item`：下划线要点列表

**数据类**
- `.data-table`：细线数据表（th 灰小字 + 行 hover）
- `.finance-bars` + `.bar` + `.bar-fill`：纯 CSS 柱状图（高度内联 style 控制）

**页面骨架类**
- `.toc-grid` + `.toc-row`（`.toc-num` 荧光绿序号）：目录
- `.chapter-num`：章节过渡页巨号
- `.exec-bar`：左荧光绿竖线的原则/提示条
- `.bottom-bar` / `.brand-foot` / `.page-num-fixed`：页脚三件套
- `.anim` + `style="--d:.15s"`：进场动画（依次延迟 0.1~0.6s）

**架构图（SVG）**
- 参考第 7 页：节点 = 圆角描边 rect（核心节点 accent 描边或实心），连线 = 直角折线
  `stroke-linejoin="round"`，箭头一律用**圆点 marker**（主流程 `#cdf030`，回传/反馈 `#777` + `stroke-dasharray`）
- 虚线模块分组框：`stroke-dasharray="6 4"` 彩色描边（每模块一色，低饱和）

## 版式配方（常见页型）

1. **封面**：`label` 角标 + `.display` 巨字 + 中文 `.cover-title` + `.cover-sub` 一句话
2. **目录**：`.toc-grid` 两列 8 行内
3. **章节过渡**：右上 `.chapter-num` + 左侧 `.h1` + `.h3`
4. **要点页**：左 `.module-list` / `.proof` 列表，右 `.proof-core` 或 `.kpi-grid`
5. **数据页**：`.kpi-grid` / `.big3` 打头，`.data-table` 或 `.finance-bars` 承接
6. **流程页**：`.funnel-row` 或 `.synergy` 链路 + 底部 `.io-band` 闭环
7. **结尾页**：`.display` 一句话定位 + `.quote` + 联系方式（`.body`）

## 交互与翻页（模板已内置，勿删）

- 键盘 ←→↑↓ / 空格 / PgUp PgDn；滚轮；触屏滑动；PREV/NEXT 按钮
- `#slide-N` URL 深链直达
- `.anim` 进场动画依赖 `.slide.active`，新增页面时给元素加 `.anim` 与 `--d` 延迟

## 导出 PPTX（scripts/render_pptx.py）

- 原理：Playwright 无头 Chromium 按 3840×2160 逐页截图 → python-pptx 全出血拼成 16:9 页
- 依赖：`pip install playwright python-pptx` + `playwright install chromium`
- 特性：文字以高清图像保留，零变形、零丢字；代价是不可再编辑文本
- 用法：`python scripts/render_pptx.py deck.html 输出.pptx [--pages 1-11]`

## 脱敏与共享

`templates/deck.html` 已完成脱敏：无真实公司 / 人名 / 数据 / 地标照片 / 比赛品牌，
内容全部为「示例科技 / 项目 Alpha / 成员 A」类中性占位，可放心公开分发与二改。
