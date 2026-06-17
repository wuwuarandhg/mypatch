[根目录](../../CLAUDE.md) > **src/core**

# src/core — 核心工具库

## 模块职责

AI 客户端、通知系统、调度引擎、模拟盘、策略引擎、数据源编排、入场候选、上下文管理等公共基础设施。

## AI 客户端

- `ai_client.py` — 基于 `openai.AsyncOpenAI` 的 LLM 调用封装, 支持多模态图片输入, 跟踪 token 用量

## 通知系统

| 文件 | 职责 |
|---|---|
| `notifier.py` | Apprise 多通道通知 (Telegram/钉钉/Bark/DingTalk/Webhook), 带代理/重试/退避 |
| `notify_dedupe.py` | 通知去重 (基于 `NotifyThrottle` 表, SHA1 内容 hash) |
| `notify_policy.py` | 通知策略 (静默时段、去重 TTL 配置覆盖) |

## 调度系统

- `scheduler.py` — `AgentScheduler` 基于 APScheduler AsyncIOScheduler, 支持 cron/interval, batch/single 模式
- `schedule_parser.py` — 调度表达式解析 (5 段 cron / `interval:3m` 格式)
- `price_alert_scheduler.py` — 价格提醒定时扫描
- `paper_trading_scheduler.py` — 模拟盘定时扫描 (每分钟, 交易时段)
- `context_scheduler.py` — Agent 上下文维护
- `context_builder.py` — `ContextBuilder` 构建 `AgentContext`

## 模拟盘系统

| 文件 | 职责 |
|---|---|
| `paper_trading_engine.py` | 核心引擎: 按信号建仓/平仓, 多市场资金配比, 虚拟账户追踪 |
| `paper_trading_notifier.py` | 模拟盘通知 (开/平仓, 日报, 盘前摘要) |
| `paper_trading_scheduler.py` | 定时扫描 (每分钟检查持仓) |

## 策略引擎

| 文件 | 职责 |
|---|---|
| `strategy_engine.py` | 信号生成、后验评估、权重调优 |
| `strategy_catalog.py` | 策略目录 (可版本化/可启停/可调权重) |
| `signals/` | `SignalPack` / `SignalPackBuilder` — 给 Agent 的结构化输入包 |

## 数据源编排

- `providers/orchestrator.py` — `Orchestrator`: 按优先级遍历 provider, 主备容灾, TTL 缓存, 健康度指标
- `providers/base.py` — Provider 抽象基类
- `providers/cache.py` — TTL 缓存实现

## 其他核心模块

| 文件 | 职责 |
|---|---|
| `entry_candidates.py` | 入场候选榜刷新 (按天去重) |
| `suggestion_pool.py` | 股票建议池 |
| `analysis_history.py` | 分析历史读写 |
| `analysis_link.py` | 深度分析外部链接 |
| `context_store.py` | Agent 上下文持久化 |
| `context_builder.py` | AgentContext 构建 |
| `kline_context.py` | K 线上下文生成 |
| `news_ranker.py` | 新闻排序 |
| `stock_link.py` | 股票平台链接 (雪球/东方财富等) |
| `json_safe.py` | JSON 安全序列化 |
| `json_store.py` | JSON 文件持久化 |
| `timezone.py` | 时区工具 |
| `log_context.py` | 结构化日志上下文 |
| `agent_runs.py` | Agent 执行记录 |
| `agent_catalog.py` | Agent 种子配置定义 |
| `update_checker.py` | 版本更新检查 |
| `prediction_outcome.py` | 预测后验评估 |
| `intraday_event_gate.py` | 盘中事件门控 |
| `data_collector.py` | 数据采集基类 |

## 测试

- `tests/test_notify_policy.py` — 通知策略
- `tests/test_json_safe.py` — JSON 安全序列化
- `tests/test_stock_link.py` — 股票链接
- `tests/test_timezone.py` — 时区工具

## 变更记录

- 2026-06-17: 初始文档
