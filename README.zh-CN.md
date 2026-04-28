# 💰 Finance Tracker

一个前后端分离的记账与资产管理项目，前端基于 Vue 3 + TypeScript，后端基于 Node.js + Express + MySQL。  
适合用于学习「认证鉴权 + 账单管理 + 统计图表 + 预算与账户管理」的完整业务流程。

---

## 📌 项目亮点

- 🔐 **完整用户体系**：注册、登录、JWT 鉴权、Access Token 自动刷新
- 🧾 **账单管理**：新增账单、分页查询、批量导入、导出 CSV
- 🏦 **账户系统**：多账户（现金/银行卡等）管理，账单自动联动余额
- 📊 **数据可视化**：近 30 天支出趋势、近 6 个月收支对比、分类统计
- 🎯 **预算功能**：按月/按分类设置预算并维护
- 🎨 **体验优化**：深色模式、主题色、响应式三栏布局

---

## 🧱 技术栈

### 前端（`Wallet`）

- ⚡ **Vue 3** + **TypeScript**
- 🧭 **Vue Router**
- 🗂️ **Pinia**
- 🧩 **Element Plus**
- 📈 **ECharts**
- 🌐 **Axios**
- 📄 **PapaParse / XLSX**（导入导出）
- 🛠️ **Vite**

### 后端（`wallet-server`）

- 🚀 **Node.js** + **Express**
- 🗄️ **MySQL (mysql2)**
- 🔑 **jsonwebtoken**
- 🔒 **bcrypt**
- 🌍 **cors**
- ⚙️ **dotenv**

---

## 📁 目录结构

```text
Finance-Tracker/
├── README.md
├── README.zh-CN.md
├── Wallet/                 # Vue 前端
│   ├── src/
│   │   ├── components/
│   │   ├── router/
│   │   ├── stores/
│   │   └── utils/
│   └── package.json
└── wallet-server/          # Express 后端
    ├── index.js
    ├── routes/
    ├── middleware/
    ├── migrations/
    ├── scripts/
    └── package.json
```

---

## ✅ 环境要求

- 🟩 Node.js：建议 `20+`（前端 `package.json` 要求 `^20.19.0 || >=22.12.0`）
- 🐬 MySQL：建议 `8.0+`
- 📦 npm：建议随 Node.js 最新版本

---

## ⚙️ 快速开始

### 1) 安装依赖

```bash
# 前端
cd Wallet
npm install

# 后端
cd ../wallet-server
npm install
```

### 2) 配置数据库连接（后端）

在 `wallet-server` 目录创建 `.env`（如果已存在可直接修改）：

```env
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=your_password
DB_NAME=wallet
PORT=3000
JWT_SECRET=your_access_secret
JWT_REFRESH_SECRET=your_refresh_secret
```

> 提示：`JWT_SECRET` 与 `JWT_REFRESH_SECRET` 建议设置为强随机字符串，避免使用默认值。

### 3) 运行数据库迁移

```bash
cd wallet-server
node scripts/run-migration.js
```

迁移脚本会创建或补充：

- `users`
- `accounts`
- `budgets`
- `transactions` 的 `user_id` / `account_id` 字段（若不存在）

### 4) 启动后端

```bash
cd wallet-server
npm run dev
```

默认服务地址：`http://localhost:3000`

### 5) 启动前端

```bash
cd Wallet
npm run dev
```

默认访问地址：`http://localhost:5173`

---

## 🌐 前端接口配置

前端请求基础地址读取 `VITE_API_BASE`，默认回退到 `http://localhost:3000`。  
可在 `Wallet` 下创建 `.env`：

```env
VITE_API_BASE=http://localhost:3000
```

---

## 🧭 主要页面说明

- 🏠 `/home`：首页三栏布局（资产概览、交易列表、快速操作/统计）
- 📃 `/all-transactions`：交易明细列表
- 🏦 `/accounts`：账户管理
- 🎯 `/budget`：预算设置
- 📊 `/statistics`：统计图页面（趋势图 + 月对比图）
- 👤 `/profile`：用户信息页
- ℹ️ `/about`：关于页面

> 受保护路由需登录后访问，未登录会自动跳转到 `/login`。

---

## 🔌 后端核心 API（概览）

### 认证相关

- `POST /api/auth/register`
- `POST /api/auth/login`
- `POST /api/auth/refresh`
- `POST /api/auth/forgot-password`
- `PUT /api/auth/profile`

### 交易相关

- `GET /api/transactions`（分页）
- `POST /api/transactions`
- `POST /api/transactions/batch`（批量导入）
- `GET /api/transactions/export?format=csv`

### 分类与统计

- `GET /api/categories`
- `GET /api/top-categories`
- `GET /api/today-stats`
- `GET /api/monthly-stats`
- `GET /api/all-categories-stats`
- `GET /api/expense-trend`
- `GET /api/monthly-comparison`

### 账户与预算

- `GET/POST/PUT/DELETE /api/accounts`
- `GET/POST/DELETE /api/budgets`

> 除 `/api/auth/*` 公共接口外，其余接口通常需要 `Authorization: Bearer <accessToken>`。

---

## 🧪 常用开发命令

### 前端（`Wallet`）

```bash
npm run dev         # 启动开发环境
npm run build       # 类型检查 + 生产构建
npm run preview     # 预览构建结果
```

### 后端（`wallet-server`）

```bash
npm run dev         # nodemon 热更新
npm start           # 直接 node 运行
```

---

## 🛠️ 常见问题排查

- ❌ **前端 401 未授权**
  - 检查是否已登录并携带 Access Token
  - 检查 Refresh Token 是否有效
  - 确认后端 `JWT_SECRET/JWT_REFRESH_SECRET` 在重启前后未频繁变更

- ❌ **数据库连接失败**
  - 检查 `.env` 的 `DB_HOST/DB_USER/DB_PASSWORD/DB_NAME`
  - 确认 MySQL 服务已启动且账号有权限

- ❌ **迁移报错 transactions 不存在**
  - 该项目迁移脚本会对 `transactions` 执行加列操作
  - 请先确保基础业务表（如 `transactions`、`categories`）已存在

- ❌ **跨域或请求地址错误**
  - 检查前端 `VITE_API_BASE` 是否与后端端口一致
  - 确认后端已启动在 `3000`（或你配置的端口）

---

## 🚀 后续可扩展方向

- 🔔 账单提醒/周期账单
- 📱 移动端适配与 PWA 离线能力
- 🧠 智能分类建议
- 👥 多人协作账本
- ☁️ Docker 化与 CI 自动部署

