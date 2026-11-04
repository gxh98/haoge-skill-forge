# 平台适配手册（platform-adapters）

> 熔炉产物本体平台无关；本手册定义各平台的薄壳适配法。原则：**适配层只放平台差异，不动本体**。

## 平台差异速查表

| 平台 | Skill 落地形态 | MCP 落地形态 | 澄清交互 | 凭据管理 |
|---|---|---|---|---|
| 扣子（Coze） | 技能草稿上传发布 | plugin-creator 封装为扣子插件 | 普通对话 | 技能变量/凭证配置 |
| dsh（DeepSeek Harness） | skills 目录 / dsh 插件 | dsh-mcp-client + cordis patch | 普通对话 | patch 的 config.env（stdio 桥会清环境变量，密钥只能写这里） |
| Claude Code | ~/.claude/skills/<name>/ | `claude mcp add` 或配置 JSON | AskUserQuestion 机制可用 | 环境变量 |
| 通用 MCP 客户端（Cursor/Claude Desktop/Trae 等） | 视客户端 skill 支持而定 | mcpServers 配置 JSON | 普通对话 | 配置 JSON 的 env 字段 |

## 一、扣子适配

### Skill 产物
1. 走 skill-creator 流程：本体目录保存为技能草稿（skill_draft save，root_path 指向产物目录）
2. 首次保存必填：show_name（中文名，遵循昊哥命名纪律）、show_description、icon_url
3. 验证可用后 publish 到"我的技能"
4. 注意：扣子技能目录约定与标准三段式兼容；发布前按扣子规范核对 description 字段映射

### MCP 产物
1. 走 plugin-creator 流程：创建扣子三方插件 Draft，把 MCP server 封装进插件
2. 扣子侧用户通过插件商店/资源库安装
3. 替代路径：若 MCP 有公网 HTTP 端点，扣子也可作为 MCP 客户端直连（扩展>MCP>添加自定义 MCP，支持 Streamable HTTP/SSE，不支持 STDIO）

## 二、dsh 适配

### Skill 产物
1. 简单方式：本体目录放入 dsh skills 路径，模型自动发现
2. 完整方式（多技能打包/带插件页）：参考 dsh-guizang-design 范式——skills/ 收编本体 + cordis.patch.yml + 插件注册工具

### MCP 产物
cordis patch YAML 挂载（两种传输）：

```yaml
# stdio（本地包）
- insert:
    - id: <server-id>
      name: '@deepseek-ai/dsh-mcp-client'
      config:
        serverName: <name>
        transport: stdio
        command: npx
        args: [<package>]
        env:                    # 密钥只能写这里！stdio 桥启动前清除凭据类环境变量
          SOME_API_KEY: xxx

# streamable-http（远程端点）
- insert:
    - id: <server-id>
      name: '@deepseek-ai/dsh-mcp-client'
      config:
        serverName: <name>
        transport: streamable-http
        url: https://<endpoint>
        headers:
          Authorization: Bearer <token>
```

持久化：写入 `$DSH_HOME/cordis.patch.yml`，重启后生效。工具以 `mcp__<serverName>__<tool>` 暴露。

## 三、Claude Code 适配

### Skill 产物
```bash
git clone <repo> ~/.claude/skills/<skill-name>
# 或 npx skills add <repo> --skill <skill-name>
```
本体即标准格式，零改造。澄清交互可用 AskUserQuestion 机制（本体 SKILL.md 的环境适配分支已写明）。

### MCP 产物
```bash
claude mcp add <name> -- npx <package>
```

## 四、通用 MCP 客户端适配

给产物 README 配全平台 JSON 示例：

```json
{
  "mcpServers": {
    "<name>": {
      "command": "npx",
      "args": ["<package>"],
      "env": { "SOME_API_KEY": "your-key" }
    }
  }
}
```

Cursor：`.cursor/mcp.json`；Claude Desktop：`claude_desktop_config.json`；其余客户端类推。

## 五、新平台接入时的动作

1. 查清该平台 skill 发现机制与 MCP 配置机制（官方文档原文）
2. 在本手册追加一节，不动产物本体
3. 在该平台实测一次，结果记入《建造档案.md》
