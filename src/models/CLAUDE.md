[根目录](../../CLAUDE.md) > **src/models**

# src/models — 市场定义

## 模块职责

市场代码、交易时段、股票数据结构定义。

## 关键文件

- `market.py` — `MarketCode` (CN/HK/US 枚举), `MarketDef` (时区/交易时段/代码正则), `StockData`, `IndexData`, 预定义 `MARKETS` 字典

## 市场定义

| 市场 | 代码 | 时区 | 交易时段 | 代码正则 |
|---|---|---|---|---|
| A 股 | CN | Asia/Shanghai | 09:30-11:30, 13:00-15:00 | `^[036]\d{5}$` |
| 港股 | HK | Asia/Hong_Kong | 09:30-12:00, 13:00-16:00 | `^\d{5}$` |
| 美股 | US | America/New_York | 09:30-16:00 | `^[A-Z]{1,5}$` |

`is_trading_time()` 方法考虑周末 (weekday >= 5 不交易)。

## 变更记录

- 2026-06-17: 初始文档
