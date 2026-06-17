[根目录](../../CLAUDE.md) > **src/web**

# src/web — FastAPI REST API 层

## 模块职责

FastAPI web 应用, 提供 REST API + JWT 认证 + SQLite (SQLAlchemy ORM) + 结构化日志。`server.py` 启动后挂载此模块。

## 入口与启动

- `app.py` — FastAPI app 实例, 注册 21+ 个 router, 挂载 `ResponseWrapperMiddleware` + CORS
- 所有 API 响应被 `ResponseWrapperMiddleware` 包装为 `{code, success, data, message}` 格式

## API 路由

所有路由前缀 `/api`, 除 `/api/auth` 和 `/api/market` 外均需 JWT 认证:

| 路由文件 | 前缀 | 功能 |
|---|---|---|
| `auth.py` | `/api/auth` | 登录认证 (JWT) |
| `stocks.py` | `/api/stocks` | 自选股 CRUD |
| `quotes.py` | `/api/quotes` | 实时行情 |
| `klines.py` | `/api/klines` | K 线数据 |
| `insights.py` | `/api/insights` | 深度分析 |
| `agents.py` | `/api/agents` | Agent 配置/启停/触发 |
| `accounts.py` | `/api/accounts` | 交易账户管理 |
| `settings.py` | `/api/settings` | 系统设置 |
| `providers.py` | `/api/providers` | 数据源 provider 配置 |
| `channels.py` | `/api/channels` | 通知渠道 |
| `datasources.py` | `/api/datasources` | 数据源管理 |
| `dashboard.py` | `/api/dashboard` | 首页概览数据 |
| `paper_trading.py` | `/api/paper-trading` | 模拟盘 API |
| `price_alerts.py` | `/api/price-alerts` | 价格提醒 |
| `discovery.py` | `/api/discovery` | 选股发现 |
| `recommendations.py` | `/api/recommendations` | 推荐信号 |
| `news.py` | `/api/news` | 新闻推送 |
| `history.py` | `/api/history` | 历史分析记录 |
| `logs.py` | `/api/logs` | 系统日志 |
| `chat.py` | `/api/chat` | AI 对话 |
| `context.py` | `/api/context` | Agent 上下文 |
| `market.py` | `/api/market` | 市场指数(公开) |
| `suggestions.py` | `/api/suggestions` | 股票建议 |
| `templates.py` | `/api/templates` | Prompt 模板 |
| `feedback.py` | `/api/feedback` | 建议反馈 |

## 关键依赖与配置

- `database.py` — SQLAlchemy engine + SessionLocal, SQLite WAL 模式
- `models.py` — 35+ ORM 模型 (AIService, AIModel, Account, Stock, Position, AgentConfig, PaperTradingAccount, StrategySignalRun, ChatConversation 等)
- `response.py` — `ResponseWrapperMiddleware` (ASGI 级, 避免 streaming hang)
- `migrations.py` — 版本化 schema 迁移
- `log_handler.py` — DB 日志 handler
- `stock_list.py` — 股票列表加载

## 数据模型要点

- `accounts` / `positions`: 多账户多股票持仓, 支持交易风格 (short/swing/long)
- `agent_configs`: 可配置调度表达式、AI 模型、通知渠道
- `paper_trading_account`: 模拟盘单例, 含市场配资比例 (market_allocations)
- `price_alert_rules`: 条件组+冷却+每日上限的多级价格提醒
- `strategy_signal_runs`: 策略信号与候选榜, 驱动模拟盘开仓
- `chat_conversations` / `chat_messages`: AI 对话会话

## 测试

- 通过 API 端点直接测试
- 无独立单元测试文件

## 相关文件清单

- `/src/web/app.py`
- `/src/web/database.py`
- `/src/web/models.py`
- `/src/web/response.py`
- `/src/web/migrations.py`
- `/src/web/log_handler.py`
- `/src/web/stock_list.py`
- `/src/web/api/*.py` (26 文件)

## 变更记录

- 2026-06-17: 初始文档
