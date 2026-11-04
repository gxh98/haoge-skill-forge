# MCP 骨架模板（mcp-skeleton）

> 熔炉 MCP 产物的起步骨架。拷贝此目录后按 `[必填]` 占位符改造。

## 结构

```
mcp-skeleton/
├── package.json      # 单包定义，bin 指向 dist/server.js
├── tsconfig.json     # TypeScript 配置（Node20+，ESM）
├── src/
│   ├── server.ts     # MCP server 入口：SDK 初始化 + 工具注册
│   └── tools/        # 每个工具一个文件：schema + handler
├── README.md         # 按 mcp-standard.md 第五节结构写
└── LICENSE           # 默认 MIT，除非用户另有要求
```

## server.ts 要点

```typescript
import { McpServer } from "@modelcontextprotocol/sdk/server/mcp.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";
import { z } from "zod";

const server = new McpServer({ name: "[必填]", version: "0.1.0" });

server.tool(
  "[必填] tool_name",           // 动词开头
  "[必填] 何时调用这个工具",      // description 是模型选择的依据
  { /* zod schema */ },
  async (args) => {
    // 错误返回要可行动：为什么失败 + 下一步怎么办
    return { content: [{ type: "text", text: "..." }] };
  }
);

const transport = new StdioServerTransport();
await server.connect(transport);
```

## 知识型 MCP 追加

```typescript
// Resource：常驻最小约束（~300 token）
server.resource("system-prompt", "[name]://system-prompt", async (uri) => ({
  contents: [{ uri: uri.href, text: COMPACT_CONSTRAINTS }]
}));

// Tool：按需加载详细规范（模块化）
server.tool("load_guidelines", "按需加载详细规范",
  { modules: z.array(z.enum(["module-a", "module-b"])) },
  async ({ modules }) => ({ content: [{ type: "text", text: loadModules(modules) }] }));
```

## 密钥纪律

- 从 `process.env.SOME_API_KEY` 读，缺失时明确报错引导配置（不崩溃不静默）
- README 环境变量表写清：变量名 / 用途 / 如何获取 / 最小权限

## 发布检查

- [ ] `npm run build` 通过，`npx .` 本地可起
- [ ] `tools/list` 实测工具清单正确
- [ ] README 含全平台配置示例（Claude Code / Claude Desktop / Cursor / dsh cordis patch / 扣子封装说明）
- [ ] LICENSE 就位
