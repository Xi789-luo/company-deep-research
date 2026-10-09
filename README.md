# company-deep-research · 公司深度研究

一个 Claude Code skill：**给定一个公司名，产出一份可直接分享的单文件 HTML 深度研究报告。**

报告覆盖：结论先行 → 公司脉络 → 组织人才 → 技术路线 → 资本结构 → 竞争格局（上游 / 横向 / 下游「那条缝」）→ 发展走向（路径 / 信号 / 风险卡 / 上限推演）→ 信息源与未核实项。

**核心特点是核实纪律**：每条关键信息都归入「已核实（附一手链接）/ 未核实存疑 / 推算 / 未能获取」，并单独出一节《哪些是事实，哪些还需要打问号》。不继承搜索结果摘要里的评判性结论，冲突的指标口径并列呈现而不是择一。

## 特点

- **不依赖本地终端** —— 纯指令集，没有 Python / npm / 本地字体 / 构建步骤。用到的全部工具是 WebSearch / WebFetch / Read / Write。
- **产出是单文件 HTML** —— CSS 内联，图表用纯 HTML/CSS（无图片、无图表库），断网也能正常显示。
- **跨终端** —— 装在任何一台有 Claude Code 的机器上，行为一致。

## 安装

### 方式一：克隆到个人 skills 目录（推荐，全项目可用）

```bash
git clone https://github.com/Xi789-luo/company-deep-research.git \
  ~/.claude/skills/company-deep-research
```

装好后新开一个 Claude Code 会话即可使用。

### 方式二：只给某个项目用

```bash
cd <你的项目>
mkdir -p .claude/skills
git clone https://github.com/Xi789-luo/company-deep-research.git \
  .claude/skills/company-deep-research
```

### 方式三：作为插件市场安装

一行装好（Claude Code 2.1.275+）：

```
/plugin install company-deep-research --marketplace Xi789-luo/company-deep-research
```

也可以分两步：

```
/plugin marketplace add Xi789-luo/company-deep-research
/plugin install company-deep-research
```

### 更新

```bash
cd ~/.claude/skills/company-deep-research && git pull
```

## 使用

直接说需求即可，skill 会按 description 自动触发：

> 帮我做一份 **Anthropic** 的公司深度研究报告
> 研究一下 **World Labs**，重点看它的竞争位置和风险
> 把 **A 公司和 B 公司** 对比着写一份深度报告

## 仓库结构

```
company-deep-research/
├── SKILL.md                              # 入口：设计约束、输出契约、红线、8 阶段工作流
├── references/
│   ├── workflow.md                       # 8 阶段详细展开（判断要点与常见坑）
│   ├── report-structure.md               # 逐节写作规范
│   ├── analysis-frameworks.md            # 六个分析框架
│   ├── design-system.md                  # CSS token 与组件词汇表
│   ├── sourcing-and-verification.md      # 来源层级、核实纪律、隐私边界
│   └── checklist.md                      # 交付前自检
├── templates/
│   └── report-template.html              # 可直接跑的空壳骨架（含完整设计系统）
└── examples/
    └── excerpt-thesis.md                 # 脱敏的结构化节选示范
```

## 六个分析框架

| 框架 | 对应失败模式 |
|---|---|
| **事实分层** | 把推断写成事实 |
| **不继承搜索摘要的评判** | 把别人的结论当自己的 |
| **口径冲突并列** | 用单一指标下结论 |
| **「它站的那条缝」** | 把竞争写成竞品清单 |
| **风险卡按影响程度排序** | 风险清单变成概率表 |
| **该盯的信号 + 上限推演** | 给出没有可证伪性的预测 |

详见 [`references/analysis-frameworks.md`](references/analysis-frameworks.md)。

## 隐私边界

只采集本人已公开发布的职业信息（姓名、GitHub、雇主、教育、研究方向、公开主页）。**不采集电话号码、微信号，不抓取登录墙后的招聘平台数据。** 详见 [`references/sourcing-and-verification.md`](references/sourcing-and-verification.md)。

## 明确不做

转公众号（PNG 出图 / 封面 / draft 上传）、出 PDF、抓登录墙数据、任何需要装包或跑脚本的动作。

## License

MIT
