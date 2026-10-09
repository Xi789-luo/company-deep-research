# 设计系统

**单一事实源：`templates/report-template.html` 里的 `<style>` 块。**
照抄，不要另起炉灶、不要换配色、不要引入 CSS 框架。这份文件解释每个 token 和组件的用途，方便你判断"这段话该用哪个组件"。

## 设计取向

- 读起来像一份**券商研报 / 智库报告**，不像一篇博客。密度高、克制、无装饰性插画。
- 衬线体标题 + 无衬线正文 + 等宽数字。所有数字用 `font-variant-numeric:tabular-nums` 对齐。
- 颜色只用来**编码语义**（强调 / 警示 / 风险 / 正面），不做装饰。
- 浅色 / 深色双主题，跟随系统，也可用 `:root[data-theme]` 强制。
- 正文列宽 `--col:74ch`；需要放表格、堆叠图、条形图的段落用 `<section class="wide">` 解除列宽限制。

## 颜色 token

| token | 用途 |
|---|---|
| `--paper` / `--surface` / `--surface-2` | 页面底 / 卡片底 / 次级底（表头、callout） |
| `--ink` / `--ink-soft` / `--muted` | 标题 / 正文 / 弱化说明 |
| `--rule` / `--rule-soft` | 分隔线（强 / 弱） |
| `--accent` / `--accent-tint` | 强调色。用于 kicker、snum、`.band.mid`、链接、`<em>` 高亮 |
| `--warn` / `--warn-tint` | 警示。`.callout.warnb`、`.chip.mid` |
| `--risk` / `--risk-tint` | 风险。`.risk`、`.chip.no`、`.meter i.on` |
| `--good` / `--good-tint` | 正面。`.chip.ok` |

**规则：`--risk` 只留给"会让人误判"的东西**，不要拿它做普通的负面描述。`--warn` 用于口径存疑、需要读者留神。

## 组件词汇表

| 组件 | 什么时候用 | 关键结构 |
|---|---|---|
| `.thesis` | **结论先行**，全文最重要的一块。4–6 条编号论断 | `<ol><li>`，CSS 自动编号；每条 = 加粗论断 + 证据 + 「需要强调的是…」 |
| `.docmeta` | 页眉下方的元信息条 | `<dl>` 里若干 `<div><dt>标签</dt><dd>值</dd></div>` |
| `.kicker` | 页眉上方的小字，格式「公司深度研究 · 赛道 · 年份」 | `<p class="kicker">` |
| `.standfirst` | 页眉导语 = 一句话核心问题 | `<p class="standfirst">` |
| `.snum` + `h2.sec` | 每个 section 的编号 + 标题 | `<span class="snum">01</span><h2 class="sec">…</h2>` |
| `h3.sub` | 长 section 内的二级分节（带 `.subn` 编号） | `<h3 class="sub"><span class="subn">04.1</span>…</h3>` |
| `.facts` | **4 个左右的关键数字**，网格平铺 | `<div class="facts"><div><div class="fv">值</div><div class="fl">含义</div></div>…</div>` |
| `.tw` + `table` | 任何结构化对比。表头自动等宽大写 | `<div class="tw"><table>`；数字列加 `class="n"`，行首标签加 `class="k"`；`<caption>` 写口径 |
| `.bars` | 同类数值的横向对比（如两位创始人的引用量） | `.bar-row` = 标签 + `.bar-track>.bar-fill` + `.bar-val`；`.bar-fill.alt` 用于第二组 |
| `.stack` | **技术栈分层图**，标出研究对象在哪一层 | `.band`（层）+ `.band.mid`（本研究对象）+ `.membrane`（层间衔接说明） |
| `.risk` | 风险卡 R1..Rn | `.risk-hd` = `.risk-id` + `.risk-t` + `.meter`；`.meter` 里 5 个 `<i>`，点亮的加 `.on` |
| `.callout` | 需要单独框出的提醒 | `.callout` / `.callout.warnb` / `.callout.riskb`，内含 `.ct` 标题 |
| `.chips` / `.chip` | 路径枚举、状态标签 | `.chip.ok` / `.chip.mid` / `.chip.no` |
| `ol.src` | **主要公开信息源**，自动编号 `01.` `02.` | `<ol class="src"><li>标题，日期。<a href>链接</a></li>` |
| `blockquote` + `cite` | 直接引用原话 | `<cite>` 写「出处，日期」 |
| `.num` | 行内数字强调 | `<span class="num">15,733</span>` |

## 排版细节

- `<em>` 是**高亮**不是斜体（有底色），用于需要读者停下来注意的口径词，例如 `<em>参数层</em>`。别滥用，一篇里十来处。
- `<b>` 用于论断的骨架句。**读者只读加粗部分也应该能读懂全文逻辑**——这是检验标准。
- `<h5>` 用于更细的切分，颜色比 `h4` 弱。
- 链接自动带下划线（`border-bottom`），不要手写样式。

## 分享版外壳

工作稿是 fragment（无 `doctype`）。交付时包一层：

```html
<!doctype html>
<html lang="zh-CN">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<meta name="color-scheme" content="light dark">
<title>{{公司名}}</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@400;500;600&family=Noto+Sans+SC:wght@300;400;500;700&family=Noto+Serif+SC:wght@500;600;700&display=swap">
<!-- 这里粘 <style> 块 -->
</head>
<body>
<!-- 这里粘 <nav> 到 </div> 的正文 -->
</body>
</html>
```

字体栈已经写了 `"Noto Serif SC","Songti SC",…` 和 `"Noto Sans SC","PingFang SC",…`，**断网时会自动回落到系统字体，版式不塌**。所以 CDN 只是锦上添花，不是依赖。

## 不要做的事

- 不要引入 Tailwind / Bootstrap / 任何 CDN 上的 CSS 或 JS。
- 不要用 `<img>` 放图表（除非用户自己提供了图）。所有图都是 HTML/CSS。
- 不要为了"好看"加动画、阴影、圆角卡片。这套设计的克制就是它的样子。
- 不要改 `--col` 之外的布局宽度。
