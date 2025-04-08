# Grafana 前端 App 目录结构分析

## 主要文件
- `app.ts`: 应用程序的主入口文件，负责初始化和启动应用
- `AppWrapper.tsx`: React应用的根组件，处理应用级别的布局和路由
- `index.ts`: 应用程序的引导文件
- `dev.ts`: 开发环境相关的配置和工具

## 核心目录结构

### core/ - 核心功能实现
- `actions/`: Redux actions定义
- `components/`: 共享的UI组件
- `context/`: React上下文定义
- `hooks/`: 自定义React Hooks
- `internationalization/`: 国际化相关功能
- `navigation/`: 导航相关功能
- `reducers/`: Redux reducers
- `selectors/`: Redux selectors
- `services/`: 核心服务实现
- `utils/`: 工具函数
- `icons/`: 图标组件
- `history/`: 浏览历史管理
- `crash/`: 崩溃报告处理

### features/ - 功能模块
#### 数据可视化相关
- `dashboard/`: 仪表板功能
- `dashboard-scene/`: 新版仪表板实现
- `panel/`: 面板组件
- `visualization/`: 可视化组件
- `canvas/`: 画布功能
- `explore/`: 数据探索功能
- `transformers/`: 数据转换器
- `variables/`: 变量管理

#### 数据源管理
- `datasources/`: 数据源管理
- `connections/`: 连接管理
- `correlations/`: 数据关联功能
- `dataframe-import/`: 数据帧导入

#### 用户和权限管理
- `users/`: 用户管理
- `teams/`: 团队管理
- `serviceaccounts/`: 服务账户
- `api-keys/`: API密钥管理
- `auth-config/`: 认证配置
- `iam/`: 身份和访问管理
- `org/`: 组织管理

#### 告警和监控
- `alerting/`: 告警功能
- `notifications/`: 通知系统
- `logs/`: 日志查看
- `live/`: 实时数据处理

#### 系统管理
- `admin/`: 管理功能
- `plugins/`: 插件管理
- `provisioning/`: 资源配置
- `preferences/`: 用户偏好设置
- `profile/`: 用户档案

#### 其他功能
- `search/`: 搜索功能
- `bookmarks/`: 书签管理
- `playlist/`: 播放列表
- `library-panels/`: 面板库
- `folders/`: 文件夹管理
- `annotations/`: 注释功能
- `commandPalette/`: 命令面板
- `geo/`: 地理信息功能
- `sandbox/`: 沙箱环境

### 其他重要目录
#### api/ - API客户端实现
- 提供与后端服务交互的统一接口层
- `createBaseQuery.ts`: 创建基础API查询的工具函数
- `utils.ts`: API相关的工具函数
- 采用TypeScript实现，确保类型安全
- 支持RESTful和GraphQL接口调用

#### store/ - 状态管理
- 基于Redux的全局状态管理实现
- `configureStore.ts`: Redux store配置，包含中间件和开发工具设置
- `store.ts`: store实例和类型定义
- 集成了Redux Toolkit，简化状态管理
- 支持异步action处理

#### types/ - TypeScript类型定义
- 全局类型定义
- 接口定义
- 枚举类型
- 工具类型
- 确保整个应用的类型安全

#### plugins/ - 插件系统
- `panel/`: 面板插件实现
- `datasource/`: 数据源插件实现
- `sdk.ts`: 插件开发SDK
- 提供插件开发的核心接口和工具
- 支持自定义面板、数据源等扩展

#### routes/ - 路由配置
- `routes.tsx`: 主路由配置文件，定义应用的路由结构
- `RoutesWrapper.tsx`: 路由包装组件，处理认证和权限
- `utils.ts`: 路由相关工具函数
- 基于React Router实现
- 支持懒加载和代码分割

#### angular/ - 旧版Angular组件
- 包含历史遗留的Angular组件
- 提供与React的互操作性
- 渐进式迁移支持
- 向后兼容性保证

#### extensions/ - 扩展功能
- 提供核心功能的扩展点
- 支持自定义功能扩展
- 插件系统的补充
- 提供企业版特性的集成点

#### partials/ - 部分视图组件
- 共享的视图片段
- 可重用的UI组件
- 页面模板
- 布局组件