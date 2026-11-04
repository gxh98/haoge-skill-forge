---
name: skill-forge
description: 昊哥-技能熔炉。建造 Agent 技能（Skill）与 MCP 插件的原技能。当用户说"建个技能""做个skill""帮我做个插件/MCP""把这本书变成技能""把这个想法做成工具""改造/魔改这个skill或MCP""把这个技能改成我的"时使用。覆盖三种建造场景（书转技能、口述想法迭代、改造他人作品），产物平台无关，可适配扣子、dsh（DeepSeek Harness）、Claude Code 及通用 MCP 客户端。当用户提到 "build a skill" "create MCP server" "forge a skill" "turn this book into a skill" 时同样适用。
---

# 昊哥-技能熔炉（skill-forge）

> 建技能的技能。给熔炉三样东西之一：一本书、一个想法、或别人的作品——它按顶尖工艺标准产出 Skill 或 MCP 产物，且产物在任何主流 Agent 平台都能跑。

## 总原则（六条铁律，每次建造必守）

1. **约束越严，产物越稳**：不给产物留无限自由度。主题、布局、流程全部收进预设选项；AI 需要的不是自由，是明确边界。
2. **流程代替运气**：禁止"一句需求直接出产物"。每个产物必须走 澄清 → 大纲（中间产物落盘）→ 建造 → 验收 四段，每段有校准点。
3. **防御性写作**：先把"模型最常犯的错"列出来，再写显式检查步骤。产物里每个注意事项都必须回答"这条在防什么错"。
4. **验收前置**：建造前先定义"什么算对"。不接受"看起来对"——必须在真实环境调用成功、规范逐条通过、干净环境盲测触发。
5. **本体平台无关**：产物本体只含平台无关内容（SKILL.md / references / assets / MCP server 包），平台差异全部收进薄壳适配层。禁止把单一平台的私有机制写进本体。
6. **一源一档**：每次建造产出《建造档案.md》（需求原文、大纲、验收结果、版本号），落在产物目录，便于迭代追溯。

## Step 0 · 三要素澄清（每次建造前必做，不澄清不动手）

向用户确认三个要素，一次最多问 3 个问题；信息缺口不影响开工时可做合理假设并显式说明：

| # | 要素 | 选项 | 为什么要问 |
|---|------|------|-----------|
| 1 | **建造场景** | A 书转技能 / B 口述想法 / C 改造他人作品 | 决定走哪份流程手册（references/forge-from-*.md） |
| 2 | **产物类型** | Skill / MCP / 不确定 | 不确定时按"产物判断矩阵"给建议（见下） |
| 3 | **目标平台** | 扣子 / dsh / Claude Code / 通用MCP客户端 / 全平台（默认） | 决定套哪层壳（references/platform-adapters.md） |

**产物判断矩阵**（用户不确定 Skill 还是 MCP 时）：

| 输入物的本质 | 建议产物 | 理由 |
|---|---|---|
| 方法论、思维框架、写作/设计规范、流程纪律 | **Skill** | 知识方法类靠提示词+参考文件生效，无需外部调用 |
| 需要实时数据、外部系统动作、结构化 API 交互 | **MCP** | 执行类能力必须落到工具调用 |
| 大量专业知识但希望模型按需检索省 token | **MCP（知识型）** | 参考归藏 Generative-UI-MCP：Tool 按需加载 + Resource 常驻最小约束 |
| 混合型（方法+数据） | **Skill 为主 + MCP 为辅** | 分别封装，Skill 里声明何时调 MCP |

## Step 1 · 场景路由

确认场景后，**先读对应手册再动手**：

| 场景 | 手册 | 核心动作 |
|---|---|---|
| A 书转技能 | `references/forge-from-book.md` | 读原文（禁二手总结）→ 蒸馏能力点 → 建造 |
| B 口述想法 | `references/forge-from-idea.md` | 7问澄清访谈 → 大纲确认 → 建造 → 迭代 |
| C 改造他人作品 | `references/forge-from-existing.md` | **许可证审查 → 安全审计** → 解剖 → 改写 |

**红线**：场景 C 未完成许可证审查和安全审计前，禁止进入改写环节。

## Step 2 · 建造

按产物类型读建造标准：
- Skill 产物：`references/skill-standard.md`（骨架模板在 `assets/skill-skeleton/`）
- MCP 产物：`references/mcp-standard.md`（骨架模板在 `assets/mcp-skeleton/`）

## Step 3 · 验收（不过闸不交付）

读 `references/acceptance-checklist.md`，按 P0/P1/P2/P3 分级逐项过闸。**P0 全绿是最低交付线**。关键验收项：
- 真实环境调用成功（目标平台实际跑一次）
- 规范逐条核对（Skill 对 Anthropic 规范 / MCP 对官方协议）
- 干净环境盲测：仅凭 description，模型能否正确触发

## Step 4 · 平台适配（套壳）

读 `references/platform-adapters.md`。三个必配壳：
1. **扣子**：skill-creator 上传发布（Skill）/ plugin-creator 封装（MCP）
2. **dsh**：skills 目录直放 / dsh 插件打包（参考 dsh-guizang-design 范式）
3. **Claude Code 及通用 MCP 客户端**：~/.claude/skills 标准格式 / npx stdio 配置 JSON

## 环境适配说明

- **扣子 Agent 环境**：澄清用普通对话逐条确认；产物上传走 skill-creator / plugin-creator 技能流程
- **dsh 环境**：澄清用普通对话；壳适配参考 cordis.patch.yml 机制
- **Claude Code**：澄清可用 AskUserQuestion 机制
- 不要假设某平台的专有交互机制在其他平台可用；不确定时退回普通对话确认

## 版本与迭代

- 产物语义化版本：v0.1.0 起步，每次实质修改进位
- 产物 SKILL.md 头部保留来源识别段（熔炉出品 + 版本 + 日期），不写入产物生成内容
- 迭代时先更新《建造档案.md》再改本体
