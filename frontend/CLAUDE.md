[根目录](../CLAUDE.md) > **frontend**

# frontend — React + TypeScript 前端

## 模块职责

基于 Vite + React 18 + TypeScript 的单页应用, shadcn/ui 组件 + Tailwind CSS 样式。

## 入口与启动

- `main.tsx` — ReactDOM.createRoot, 挂载 BrowserRouter + ToastProvider
- `App.tsx` — 路由配置 + 导航 (桌面端顶部/移动端底部) + 登录守卫

## 项目结构

```
frontend/
├── src/
│   ├── main.tsx              # 入口
│   ├── App.tsx               # 路由 + 导航
│   ├── index.css             # Tailwind 入口
│   ├── pages/                # 页面组件
│   │   ├── Dashboard.tsx     # 首页
│   │   ├── Stocks.tsx        # 持仓页
│   │   ├── Opportunities.tsx # 机会页
│   │   ├── Agents.tsx        # Agent 配置页
│   │   ├── Settings.tsx      # 设置页
│   │   ├── DataSources.tsx   # 数据源管理
│   │   ├── History.tsx       # 历史分析
│   │   ├── AnalysisDetail.tsx # 深度分析详情
│   │   ├── PriceAlerts.tsx   # 价格提醒
│   │   ├── PaperTrading.tsx  # 模拟盘
│   │   ├── Login.tsx         # 登录页
│   │   └── Stocks.tsx        # 股票列表
│   ├── components/           # 应用级组件
│   │   └── ChatWidget.tsx    # AI 对话小窗
│   ├── hooks/                # 自定义 hooks
│   │   └── use-theme.ts      # 暗亮主题切换
│   └── lib/                  # 工具函数
│       ├── kline-scorer.ts   # K 线评分
│       ├── logger-map.ts     # 日志映射
│       └── utils.ts          # 通用工具
├── packages/                 # pnpm workspaces
│   ├── api/                  # API 客户端 + 类型
│   ├── base-ui/              # shadcn/ui 基础组件
│   └── biz-ui/               # 业务组件
├── vite.config.ts            # Vite 配置 (port 5183, proxy /api → :8000)
├── tailwind.config.js        # Tailwind 主题
└── tsconfig.json             # TypeScript 配置
```

## 页面路由

| 路径 | 页面 | 说明 |
|---|---|---|
| `/` | Dashboard | 首页概览 |
| `/portfolio` | Stocks | 持仓管理 |
| `/opportunities` | Opportunities | 机会发现 |
| `/alerts` | PriceAlerts | 价格提醒 |
| `/paper-trading` | PaperTrading | 模拟盘 |
| `/agents` | Agents | Agent 配置 |
| `/history` | History | 分析历史 |
| `/datasources` | DataSources | 数据源配置 |
| `/settings` | Settings | 系统设置 |
| `/login` | Login | 登录 |
| `/analysis/:symbol/:date` | AnalysisDetail | 深度分析详情 |

## API 客户端

- `packages/api/src/client.ts` — `fetchAPI<T>()` 封装: JWT token 注入, 自动 unwrap `{code, data, message}`, 超时控制, 401 自动登出
- 各模块 API 函数按功能拆分: `dashboard.ts`, `stocks.ts`, `paper-trading.ts`, `insight.ts` 等

## 组件体系

- `@panwatch/base-ui` — shadcn/ui 原子组件 (button, dialog, input, select, tabs, calendar 等)
- `@panwatch/biz-ui` — 业务组件: `InteractiveKline` (K线图), `KlineModal`, `DeepAnalysisModal`, `Onboarding`, `StockInsightModal`, `PriceAlertFormDialog` 等

## 变更记录

- 2026-06-17: 初始文档
