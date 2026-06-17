[根目录](../../../CLAUDE.md) > [frontend](../) > **packages/api**

# packages/api — 前端 API 客户端

## 模块职责

前端与后端通信的 API 客户端层, 包含 TypeScript 类型定义和请求封装。

## 入口

- `index.ts` — re-export 所有模块
- `client.ts` — `fetchAPI<T>()` 核心函数, 自动注入 JWT token, 超时控制 (20s), 错误处理

## 模块列表

| 文件 | 职责 |
|---|---|
| `types.ts` | 共享 TS 类型 (AIModel, AIService, NotifyChannel, DataSource) |
| `app.ts` | 应用级 API (版本号) |
| `auth.ts` | 登录认证 |
| `stocks.ts` | 自选股 CRUD + Agent 触发 |
| `dashboard.ts` | 首页概览 (指数/持仓/行情/扫描) |
| `insight.ts` | 深度分析 |
| `paper-trading.ts` | 模拟盘 (账户/持仓/交易/指标/通知设置) |
| `chat.ts` | AI 对话 |
| `discovery.ts` | 选股发现 |
| `recommendations.ts` | 策略推荐信号 |
| `tradingagents.ts` | TradingAgents 深度分析特有 API |

## 关键模式

- 所有 API 返回 `Promise<T>`, `fetchAPI` 自动 unwrap `{code, data, message}` 取出 `data`
- JWT token 存 `localStorage`, `isAuthenticated()` 做过期检查
- 401 响应自动 `logout()` + 跳转 `/login`

## 变更记录

- 2026-06-17: 初始文档
