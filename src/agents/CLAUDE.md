[根目录](../../CLAUDE.md) > **src/agents**

# src/agents — Agent 业务逻辑

## 模块职责

6 个 Agent 实现, 每个继承 `BaseAgent` 基类。负责行情数据采集 → AI 分析 → 通知推送的完整流程。

## 入口与启动

- `base.py` — `BaseAgent` 抽象基类, 定义 `run()` 标准流程 (collect → analyze → notify), 含通知去重/静默时段/退避重试
- Agent 在 `server.py` 中实例化并注册到 `AgentScheduler`, 由 APScheduler 定时触发

## Agent 列表

| Agent | 文件名 | 调度 | 模式 | 说明 |
|---|---|---|---|---|
| 盘前分析 | `premarket_outlook.py` | 0 9 * * 1-5 | batch | 综合昨日分析 + 隔夜信息展望 |
| 盘中监测 | `intraday_monitor.py` | */5 9-15 * * 1-5 | single | 实时监控, AI 判断信号 (5 分钟/只) |
| 收盘复盘 | `daily_report.py` | 30 15 * * 1-5 | batch | 大盘概览 + 个股复盘 + 明日关注 |
| 新闻速递 | `news_digest.py` | (能力, 不独立调度) | batch | 新闻抓取/去重/聚合 (已弃用) |
| 技术分析 | `chart_analyst.py` | (能力, 不独立调度) | single | K 线截图 + AI 技术分析 (已弃用) |
| 深度分析 | `tradingagents/` | 手动触发 | single | 多 Agent 投资决策框架, 3-5 分钟 |

## 对外接口

每个 Agent 通过 `BaseAgent` 暴露:
- `collect(context) -> dict` — 采集数据
- `build_prompt(data, context) -> (system_prompt, user_content)` — 构建 prompt
- `run(context) -> AnalysisResult` — 完整执行 (含通知)
- `run_single(context, symbol)` — 逐只股票执行 (single 模式)

## TradingAgents 深度分析

位于 `src/agents/tradingagents/` 子目录, 软依赖 `tradingagents` 库 (git+ssh, 不在 PyPI):

| 文件 | 职责 |
|---|---|
| `agent.py` | `TradingAgentsAgent` 主类 |
| `llm_adapter.py` | 桥接 PanWatch AI Service 到 TradingAgents LLM config |
| `toolkit_adapter.py` | 注入 A 股数据到 TradingAgents data vendor |
| `portfolio_context.py` | 构建持仓元数据上下文 |
| `progress.py` | 进度回调 (LangChain callbacks) |
| `result_mapper.py` | TradingAgents final_state → AnalysisResult |
| `cost_tracker.py` | 月度预算 + 同日缓存 |
| `history_comparison.py` | 历史结果对比 |
| `backfill.py` | 回填历史分析 |
| `financial_data.py` | 财务数据适配 |
| `auto_trigger.py` | 自动触发逻辑 |
| `paper_trading_bridge.py` | 模拟盘信号桥接 |
| `langchain_compat.py` | LangChain 兼容补丁 |

## Prompt 模板

每个 Agent 对应一个 prompt 文件: `prompts/{agent_name}.txt`

## 关键数据流

```
AgentScheduler 触发
  → AgentContextBuilder 构建上下文 (AI 客户端/通知器/持仓/配置)
  → Agent.run()
    → collect() 采集数据 (通过 SignalPackBuilder + Collectors)
    → build_prompt() 构建 prompt
    → ai_client.chat() 调用 LLM
    → notify 发送通知 (含去重/静默/退避)
```

## 测试

- `tests/test_intraday_monitor_json_format.py` — 盘中监测 JSON 校验
- `tests/test_tradingagents_auto_trigger.py` — 自动触发逻辑
- `tests/test_tradingagents_toolkit_isolation.py` — toolkit 隔离

## 变更记录

- 2026-06-17: 初始文档
