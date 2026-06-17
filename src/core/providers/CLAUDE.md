[根目录](../../../CLAUDE.md) > [src/core](../) > **providers**

# src/core/providers — 数据源主备调度

## 模块职责

行情 / K 线 / 资金流 / 事件数据的多 provider 主备调度。每个类型按 `DataSource.priority` 顺序遍历, 第一个成功返回, 并写入 TTL 缓存。

## 架构

```
上层请求 (Agent/API)
  → ProviderRequest 传入类型+市场+股票
  → Orchestrator 查询 DataSource 表获取已启用 provider 列表
  → 按 priority 遍历 provider.fetch(req)
  → 第一个 success + 非空 → 返回 + 缓存
  → 全部失败 → 返回 error
```

## Provider 实现

| 目录 | 文件 | 说明 |
|---|---|---|
| `quote/` | `tencent.py`, `yfinance.py` | 实时行情 |
| `kline/` | `tencent.py`, `tushare.py`, `yfinance.py` | K 线数据 |
| `capital_flow/` | `eastmoney.py` | 资金流向 |
| `discovery/` | `eastmoney.py` | 选股发现 |
| `events/` | `eastmoney.py` | 事件驱动 |

## 关键配置

- `base.py` — `Provider`, `ProviderRequest`, `ProviderResponse`, `QuoteProvider` 类型定义
- `cache.py` — `TTLCache` (quote 默认 5s TTL, 跨调用方共享)
- `orchestrator.py` — 主调度器 + 滚动健康度指标 (最近 100 次)

## 测试

- `tests/test_quote_orchestrator.py` — 行情编排
- `tests/test_kline_orchestrator.py` — K 线编排

## 变更记录

- 2026-06-17: 初始文档
