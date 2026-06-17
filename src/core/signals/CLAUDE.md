[根目录](../../../CLAUDE.md) > [src/core](../) > **signals**

# src/core/signals — 策略信号生成包

## 模块职责

为 Agent 提供结构化输入 (`SignalPack`)。采集行情/技术/资金/新闻/事件/持仓, 构建统一数据包供 AI 分析。

## 关键文件

- `signal_pack.py` — `SignalPack` (冻结 dataclass) + `SignalPackBuilder` (带内存缓存, 避免重复网络调用)
- `structured_output.py` — AI 结构化 JSON 解析工具

## 数据流

```
SignalPackBuilder
  → 行情 (QuoteProvider)
  → K 线摘要 (KlineCollector)
  → 新闻 (NewsCollector)
  → 资金流 (CapitalFlowCollector)
  → 事件 (EventsCollector)
  → 持仓 (DB Position)
  → SignalPack {quote, technical, news, capital_flow, events, position, missing}
```

## 变更记录

- 2026-06-17: 初始文档
