# 昊哥-技能熔炉（skill-forge）

> 建技能的技能。给熔炉三样东西之一：**一本书、一个想法、或别人的作品**——它按顶尖工艺标准产出 Skill 或 MCP 产物，且产物在扣子、dsh（DeepSeek Harness）、Claude Code 及通用 MCP 客户端都能跑。

## 为什么有它

AI 技能的质量不靠运气，靠工艺。熔炉把"建技能"这件事本身变成技能：

- **方法论源头**：Anthropic Agent Skills 官方规范 + MCP 官方协议规范
- **工艺标杆**：解剖归藏（op7418）头部仓库蒸馏（guizang-ppt-skill 29k★ / Generative-UI-MCP / dsh-guizang-design）
- **核心心法**：约束越严产物越稳 · 流程代替运气 · 防御性写作 · 验收前置

## 三种建造场景

| 场景 | 输入 | 流程手册 |
|---|---|---|
| A 书转技能 | 一本书/一套课程的**原文** | `references/forge-from-book.md` |
| B 口述想法 | 一个想法，口述迭代 | `references/forge-from-idea.md` |
| C 改造他人作品 | 别人的 Skill/MCP 仓库 | `references/forge-from-existing.md`（许可证+安全双闸门） |

## 两类产物

- **Skill**：方法论、思维框架、流程纪律类能力（标准三段式：SKILL.md + references/ + assets/）
- **MCP**：需外部数据/动作的执行类能力，或大量知识按需检索的知识型能力（TS 单包 + stdio）

产物本体平台无关；扣子 / dsh / Claude Code / 通用 MCP 客户端各一层薄壳适配（见 `references/platform-adapters.md`）。

## 目录结构

```
skill-forge/
├── SKILL.md                      # 熔炉主文件：六条铁律 + 三要素澄清 + 场景路由
├── references/                   # 七份工艺手册（按需加载）
│   ├── forge-from-book.md        # 场景A：书转技能
│   ├── forge-from-idea.md        # 场景B：口述想法迭代
│   ├── forge-from-existing.md    # 场景C：改造他人作品
│   ├── skill-standard.md         # Skill 建造标准
│   ├── mcp-standard.md           # MCP 建造标准
│   ├── platform-adapters.md      # 四平台适配手册
│   └── acceptance-checklist.md   # P0/P1/P2/P3 验收清单
└── assets/
    ├── skill-skeleton/           # Skill 骨架模板（直接拷贝改造）
    └── mcp-skeleton/             # MCP 骨架模板（结构+要点）
```

## 使用方式

在任意 Agent 环境对模型说：**"我要建个技能"** / **"把这本书变成技能"** / **"把这个 skill 改成我的"**——熔炉接管后续流程。

## 状态

v0.1.0 骨架版。首个实战：梁宁《产品思维30讲》→ Skill（书转场景验证）。

## License

MIT
