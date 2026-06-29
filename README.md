# 实验室管理平台

实验室管理平台是一个面向高校实验室的项目制协作系统，围绕“项目”串联账号权限、耗材管理、文件资料、会议通知和 AI 助手。项目采用 Monorepo 结构，后端使用 Fastify + PostgreSQL，前端使用 React + Vite，整体遵循“微内核 + 插件化”架构。

## 当前状态

- 已可本地启动并演示完整主流程
- 已支持三级角色：`student`、`professor`、`lab_admin`
- 已完成前端模块化重构，不再是一页式长工作台
- 项目插件已接入，跨模块的 `project_id` 关联仍在持续完善

## 核心能力

- 账号登录、找回密码、角色分级、账号管理
- 项目管理、任务、成员、进度纪要
- 耗材目录、领用申请、审批、入库、库存流水
- 文件资料、版本记录、外链资料兼容
- 会议预约、通知、公告
- AI 对话、FAQ、知识库管理

## 快速启动

### 1. 准备环境

- Node.js 20+
- pnpm 9+
- Docker Desktop

建议复制环境变量模板：

```powershell
Copy-Item .env.example .env
```

### 2. 启动开发环境

```powershell
docker compose up --build -d
docker compose ps
```

默认地址：

- Web: `http://localhost:5173`
- API: `http://localhost:3000`
- Health: `http://localhost:3000/health`

### 3. 演示账号

```text
实验室管理员：admin / Admin@123456
教授：professor / Professor@123456
学生：student001 / Student@123456
```

## 常用命令

```powershell
corepack pnpm run ci
corepack pnpm --filter @lab/web typecheck
docker compose up --build -d
docker compose logs -f api
docker compose exec api pnpm --filter @lab/api db:migrate
```

## 仓库结构

```text
lab-management-platform/
├── apps/
│   ├── api/                Fastify API 宿主
│   └── web/                React + Vite 前端
├── docs/                   项目文档
├── infra/                  Nginx / PostgreSQL 基础设施配置
├── packages/
│   ├── contracts/          OpenAPI 与共享契约
│   └── core/               微内核：认证、权限、事件、审计
├── plugins/                业务插件
├── docker-compose.yml      开发环境
├── docker-compose.prod.yml 生产环境
└── CLAUDE.md               AI 协同开发上下文
```

## 文档入口

- [docs/README.md](./docs/README.md): 文档总索引
- [docs/DEVELOPMENT.md](./docs/DEVELOPMENT.md): 开发与代码组织
- [docs/API.md](./docs/API.md): 常用 API 说明
- [docs/DATABASE.md](./docs/DATABASE.md): 数据库 schema 与关键表
- [docs/PERMISSION_MATRIX.md](./docs/PERMISSION_MATRIX.md): 角色与权限范围
- [docs/PROJECT_PROGRESS.md](./docs/PROJECT_PROGRESS.md): 当前进度与待办
- [docs/DEPLOYMENT.md](./docs/DEPLOYMENT.md): 生产部署说明
- [docs/AI_MODULE.md](./docs/AI_MODULE.md): AI 模块接入与配置
- [docs/plugin-template.md](./docs/plugin-template.md): 新增插件模板

## 开发约定

- 业务规则写在 `plugins/*`，不要写进 `packages/core`
- OpenAPI 变更以 `packages/contracts/openapi/core-api.yaml` 为准
- 提交前运行 `corepack pnpm run ci`
- 不提交 `.env`、证书、真实密钥或个人机器绝对路径
