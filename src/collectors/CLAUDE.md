[根目录](../../CLAUDE.md) > **src/collectors**

# src/collectors — 无状态数据采集器

## 模块职责

封装第三方数据源调用, 保持无状态, 返回类型化数据结构。

## Collector 列表

| 文件 | 数据源 | 说明 |
|---|---|---|
| `akshare_collector.py` | AKShare | 行情、基本面、龙虎榜等 |
| `kline_collector.py` | Provider Orchestrator | K 线数据采集 |
| `news_collector.py` | 雪球/东方财富/财联社 | 新闻抓取与去重 |
| `capital_flow_collector.py` | 东方财富 | 资金流向 |
| `discovery_collector.py` | 东方财富 | 选股发现 |
| `events_collector.py` | 东方财富 | 事件驱动数据 |
| `screenshot_collector.py` | Playwright | K 线截图 (给 chart_analyst 用) |

## 关键依赖

- `akshare`, `efinance` — CN 市场数据
- `Playwright` — 截图 (Docker 中自动安装到 DATA_DIR)

## 变更记录

- 2026-06-17: 初始文档
