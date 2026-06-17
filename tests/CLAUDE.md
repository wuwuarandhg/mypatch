[根目录](../CLAUDE.md) > **tests**

# tests — 后端单测

## 模块职责

23 个 pytest 单测文件, 覆盖核心模块和 Agent 逻辑。

## 测试文件清单

| 文件 | 覆盖模块 |
|---|---|
| `conftest.py` | 全局 fixtures: 默认屏蔽通知, mock stock_link, mock 账户/信号数据 |
| `test_notify_policy.py` | 通知策略 (静默时段、去重 TTL) |
| `test_json_safe.py` | JSON 安全序列化 |
| `test_stock_link.py` | 股票平台链接 |
| `test_timezone.py` | 时区工具 |
| `test_quote_orchestrator.py` | 行情 provider 编排 |
| `test_kline_orchestrator.py` | K 线 provider 编排 |
| `test_intraday_monitor_json_format.py` | 盘中监测 JSON 格式校验 |
| `test_paper_trading_notify.py` | 模拟盘通知 |
| `test_paper_trading_allocation.py` | 模拟盘资金配比 |
| `test_cn_symbol_mapping.py` | CN 股票代码映射 |
| `test_run_progress_stale.py` | Agent 运行状态处理 |
| `test_tradingagents_toolkit_isolation.py` | TradingAgents toolkit 隔离 |
| `test_tradingagents_auto_trigger.py` | TradingAgents 自动触发 |
| +9 更多文件 |

## 配置

- `pyproject.toml` 定义 testpaths / python_files / python_functions
- `conftest.py` `pytest_itemcollected` hook: 用中文 docstring 替换节点名

## 运行方式

```bash
python -m pytest tests/ -v                # 默认不发通知
python -m pytest tests/ -v --notify       # 真实发送通知
python -m pytest tests/test_xxx.py -v     # 单个文件
```

## CI 集成

- `.github/workflows/release.yml`: Docker build 前 `python -m pytest tests/ -x -q`, 失败中断发布
- `scripts/pre-push`: push 前自动跑测试

## 变更记录

- 2026-06-17: 初始文档
