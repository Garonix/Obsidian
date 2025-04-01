# Grafana 项目目录结构分析

## 根目录主要文件
- `go.mod`/`go.sum`: Go语言依赖管理文件
- `package.json`/`yarn.lock`: Node.js依赖管理文件
- `tsconfig.json`: TypeScript配置文件
- `Dockerfile`: 容器化配置文件
- `Makefile`: 项目构建和管理脚本

## 核心目录结构

### 后端相关目录
#### pkg/ - 核心后端包
- `api/`: API相关定义和实现
- `apiserver/`: API服务器实现
- `server/`: 主服务器实现
- `services/`: 核心服务实现
- `plugins/`: 插件系统相关代码
- `models/`: 数据模型定义
- `middleware/`: HTTP中间件
- `login/`: 认证相关功能
- `storage/`: 存储层实现
- `tsdb/`: 时序数据库相关代码
- `util/`: 通用工具函数

### 前端相关目录
#### public/ - 前端资源
- `app/`: 主应用代码
- `build/`: 构建输出目录
- `sass/`: 样式文件
- `img/`: 图片资源
- `fonts/`: 字体文件
- `lib/`: 前端库文件
- `locales/`: 国际化资源
- `views/`: 视图模板

### 开发和构建相关目录
- `scripts/`: 构建和开发脚本
- `tools/`: 开发工具
- `devenv/`: 开发环境配置
- `e2e/`: 端到端测试
- `packaging/`: 打包相关配置
- `contribute/`: 贡献指南
- `docs/`: 项目文档

### 配置和资源目录
- `conf/`: 配置文件
- `data/`: 数据文件
- `emails/`: 邮件模板
- `grafana-mixin/`: Grafana监控配置

### 其他重要目录
- `.github/`: GitHub配置和工作流
- `.husky/`: Git hooks配置
- `.vscode/`: VS Code配置
- `bin/`: 可执行文件
- `hack/`: 开发辅助脚本
- `kinds/`: 资源类型定义
- `packages/`: 共享包

## 特殊文件说明
- `.drone.yml`: CI/CD配置
- `.golangci.yml`: Go代码检查配置
- `eslint.config.js`: JavaScript/TypeScript代码检查配置
- `stylelint.config.js`: CSS代码检查配置
- `playwright.config.ts`: E2E测试配置
- `jest.config.js`: 单元测试配置

## 开发工具配置
- `.prettierrc.js`: 代码格式化配置
- `.editorconfig`: 编辑器配置
- `.nvmrc`: Node.js版本配置
- `.yarnrc.yml`: Yarn包管理器配置

## 项目文档
- `README.md`: 项目概述
- `CONTRIBUTING.md`: 贡献指南
- `CHANGELOG.md`: 变更日志
- `LICENSE`: 许可证文件
- `SECURITY.md`: 安全政策
- `GOVERNANCE.md`: 项目治理 