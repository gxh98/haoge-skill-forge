# MCP 建造标准（mcp-standard）

> 熔炉产出的每个 MCP server 必须符合此标准。来源：MCP 官方协议规范 + 归藏 Generative-UI-MCP 范式。

## 一、两种范式（先选型再动手）

| 范式 | 特征 | 适用 | 样本 |
|---|---|---|---|
| **执行型** | 调外部 API/系统，执行动作 | 需要实时数据、外部操作 | 各类 API 桥接 server |
| **知识型** | 封装专业知识/规范，模型按需检索 | 大量知识、希望省 token | 归藏 Generative-UI-MCP |

知识型标准结构：**Tool（按需加载详细规范）+ Resource（~300 token 常驻最小约束）**。

## 二、技术栈（默认选型）

- TypeScript + `@modelcontextprotocol/sdk`（官方 SDK）
- 单 npm 包，`npx <package>` 即可运行
- 传输：默认 **stdio**（最通用）；需要常驻/多客户端时加 streamable-http
- Node ≥ 20

## 三、工具设计纪律

1. **工具数量克制**：能一个工具加参数解决的，不拆多个（参考 Generative-UI-MCP：一个 `load_ui_guidelines(modules)` 覆盖 5 个模块）
2. **每个工具必须有**：name（动词开头）、description（说清何时调用）、inputSchema（JSON Schema 完整约束）
3. **错误返回要可行动**：不只说"失败"，说"为什么失败+下一步怎么办"
4. **按需加载**：大段知识不进工具 description（那是共享成本），进工具返回体（按需付费）

## 四、安全纪律（红线）

- 密钥/Token 一律走**环境变量**，禁止硬编码、禁止进 CLI 参数
- README 明确列出需要哪些环境变量、什么权限、最小化建议
- 不读取与本功能无关的环境变量和文件
- 网络请求只去声明过的域名

## 五、README 标准结构

1. 一句话定位 + 灵感来源
2. What it does（模块表格）
3. Quick start：**全平台配置示例**——
   - Claude Code：`claude mcp add <name> -- npx <package>`
   - Claude Desktop / Cursor / Windsurf：mcpServers JSON
   - dsh：cordis patch YAML（stdio，密钥写 config.env）
   - 扣子：plugin-creator 封装说明
4. 工具/Resource 参考文档（参数表 + 调用示例 JSON）
5. 环境变量表
6. License（默认 MIT，除非用户另有要求）

## 六、验收专项（叠加在通用清单上）

- JSON-RPC 握手实测：`tools/list` 返回工具清单正确
- `tools/call` 实测：每个工具至少跑通一个真实用例
- 在两个不同客户端实测（如 dsh + Claude Code 或 Cursor）
- 无密钥时行为：明确报错引导配置，不崩溃不静默

## 七、产物落位

- GitHub 仓库 + npm 发布（可选）
- 骨架模板：`assets/mcp-skeleton/`
