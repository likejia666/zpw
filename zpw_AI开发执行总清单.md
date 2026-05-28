# ZPW 项目 AI 开发执行总清单

> 目标：把 zpw 项目拆到 **AI 不需要再猜，就能直接开工** 的程度。
>
> 使用范围：本文件用于记录项目开发范围、阶段、模块、任务、依赖、状态、验收标准、风险与回填结果。
>
> 当前仓库现状：仓库内目前以设计文档为主，尚无完整 Laravel / uniapp / migration / 测试 / Workerman 实现代码。
>
> 本文件定位：
> - 不是产品需求文档
> - 不是泛泛开发计划
> - 不是汇报用 PPT 摘要
> - 是 AI / 开发者可直接执行的施工总清单

---

# 0. 当前现状

## 0.1 已具备
- 项目概述
- 技术架构
- 数据库设计
- 页面设计与交互
- 功能流程图
- API 接口规范
- 安全与性能说明
- 部署说明

## 0.2 当前不具备
- Laravel 工程代码
- uniapp 前端代码
- migration
- seeders
- factories
- Workerman 启动实现
- 测试代码
- 可运行部署脚本

## 0.3 当前开发目标
- 从 0 建立可运行工程骨架
- 按文档逐步落地主业务链路
- 最终具备测试、部署、上线能力

---

# 1. 全局规则

## 1.1 状态枚举
- `todo`：未开始
- `ready`：依赖满足，可直接开工
- `doing`：开发中
- `blocked`：被阻塞
- `review`：代码完成，待复核
- `testing`：测试中
- `done`：完成
- `cancelled`：取消

## 1.2 优先级
- `P0`：主链路阻塞项，必须优先完成
- `P1`：核心增强项
- `P2`：体验 / 运营增强
- `P3`：优化项

## 1.3 任务编号规则
格式：`模块-子域-序号`

示例：
- `BOOT-APP-001`
- `AUTH-API-REG-001`
- `JOB-DB-001`
- `CHAT-WS-001`
- `ADMIN-AUDIT-001`

## 1.4 任务要求
每条任务应尽量满足：
- 只有一个明确目标
- 有明确输入
- 有明确输出
- 有明确依赖
- 有明确验收标准
- AI 一次能完成或明显推进

## 1.5 禁止事项
- 禁止一个任务混多个无关目标
- 禁止擅自修改状态机、接口结构、错误码口径
- 禁止未测试就标记 `done`
- 禁止扩 scope
- 禁止跳过依赖硬做

## 1.6 完成回填要求
任务完成后至少回填：
- 状态更新
- 实际改动文件
- 未完成项
- 风险点
- 是否有文档偏差
- 后续建议任务

---

# 2. 开发阶段总图

- 阶段 A：项目骨架与基础设施
- 阶段 B：用户与认证
- 阶段 C：简历与求职者闭环
- 阶段 D：企业认证与雇主闭环
- 阶段 E：职位与报名主链路
- 阶段 F：聊天、通知、互动
- 阶段 G：后台治理与运营
- 阶段 H：测试、部署、上线维护

---

# 3. 模块总览

| 模块 | 状态 | 优先级 | 依赖 |
|---|---|---:|---|
| 项目骨架与基础设施 | ready | P0 | 无 |
| 用户与认证 | blocked | P0 | 项目骨架与基础设施 |
| 简历系统 | blocked | P0 | 用户与认证 |
| 企业认证 | blocked | P0 | 用户与认证、文件系统 |
| 职位系统 | blocked | P0 | 企业认证 |
| 报名系统 | blocked | P0 | 职位系统、简历系统 |
| 聊天与 WebSocket | blocked | P1 | 用户与认证 |
| 评论 / 举报 / 黑名单 | blocked | P1 | 用户与认证、职位系统 |
| 收藏 / 通知 / 浏览历史 | blocked | P2 | 用户与认证 |
| 工种 / 帮助中心 / 人才库 | blocked | P2 | 用户与认证、核心业务 |
| 后台管理 | blocked | P0 | 项目骨架、权限体系 |
| 安全 / 测试 / 部署 | blocked | P0 | 随主链路推进 |

---

# 第1章：项目骨架与基础设施

> 目标：把 zpw 从“只有设计文档”推进到“可运行、可扩展、可测试、可继续开发”的 Laravel 项目基础底座。
> 本章只处理骨架、基础设施、全局约束、底层公共能力，不进入具体业务模块实现。

## 1.1 本章定位

### 本章要解决的问题
- 有可运行的 Laravel 11 工程
- 有清晰目录结构
- 有基础配置能力
- 有 MySQL / Redis / Filesystem 基础接入
- 有统一 API 响应规范
- 有统一异常处理
- 有基础鉴权骨架
- 有文件资源输出骨架
- 有 system_config 基础能力
- 有 files 基础能力
- 有基本测试框架
- 有后续模块可依赖的公共底层

### 本章不做的内容
- 注册 / 登录
- 企业认证
- 职位发布
- 报名
- 私聊
- 评论
- 后台管理页面
- 举报
- 黑名单业务
- 帮助中心业务

## 1.2 本章完成标准
当第 1 章完成时，应满足：
1. Laravel 11 项目可启动
2. `.env.example`、数据库、Redis、文件盘配置可用
3. 已具备统一 API 返回结构
4. 已具备统一异常响应结构
5. 已具备基础 JWT 鉴权框架入口
6. 已具备 `files`、`system_config` 等底层支撑表
7. 已具备文件资源统一输出类
8. 已具备基本测试执行能力
9. 后续模块可以在此基础上直接开发

## 1.3 本章依赖关系
### 上游依赖
无。

### 下游依赖
- 用户与认证
- 简历系统
- 企业认证
- 职位系统
- 报名系统
- 聊天与 WebSocket
- 后台管理
- 安全 / 部署 / 测试

## 1.4 本章任务总览
1. Laravel 工程初始化
2. 目录结构与工程约定
3. 环境配置与基础驱动
4. 基础数据库底座
5. 文件存储底座
6. 统一 API 响应与异常处理
7. 鉴权骨架与中间件预留
8. 系统配置底座
9. 基础测试框架
10. 基础文档与开发规范

## 1.5 可直接执行任务清单

### TASK-ID: BOOT-APP-001
- 状态: ready
- 优先级: P0
- 所属子域: Laravel 工程初始化
- 目标: 初始化 Laravel 11 工程骨架
- 依赖: 无
- 输入:
  - 技术架构文档
  - 部署说明文档
- 输出:
  - 可运行的 Laravel 11 项目
  - 基础目录存在
- 产出物:
  - `app/`
  - `bootstrap/`
  - `config/`
  - `database/`
  - `public/`
  - `routes/`
  - `storage/`
  - `tests/`
  - `artisan`
  - `composer.json`
- 验收标准:
  - `php artisan about` 可执行
  - Laravel 版本为 11.x
  - 项目可正常加载
- 非目标:
  - 不在本任务中配置业务模块
  - 不在本任务中实现数据库表

### TASK-ID: BOOT-APP-002
- 状态: ready
- 优先级: P0
- 所属子域: Laravel 工程初始化
- 目标: 配置项目基础 Composer 依赖
- 依赖:
  - BOOT-APP-001
- 输入:
  - 技术架构文档
  - WebSocket 与 Redis 需求
- 输出:
  - 基础依赖安装完成
- 产出物:
  - `composer.json`
  - `composer.lock`
- 依赖建议覆盖:
  - Laravel 核心依赖
  - Redis 驱动
  - JWT 相关依赖（如选型明确）
  - Workerman / GatewayWorker 所需依赖先加入计划
- 验收标准:
  - `composer install` 成功
  - 自动加载无错误
  - 基础依赖版本与 Laravel 11 兼容
- 非目标:
  - 不要求本任务实现 JWT 业务逻辑
  - 不要求启动 Workerman

### TASK-ID: BOOT-APP-003
- 状态: ready
- 优先级: P0
- 所属子域: Laravel 工程初始化
- 目标: 生成并校验基础环境文件模板
- 依赖:
  - BOOT-APP-001
- 输入:
  - 部署说明
  - 数据库配置要求
  - Redis 配置要求
- 输出:
  - `.env.example` 可用于后续部署
- 产出物:
  - `.env.example`
- 应包含至少以下变量组:
  - APP 基础配置
  - DB 配置
  - REDIS 配置
  - FILESYSTEM 配置
  - QUEUE 配置
  - CACHE 配置
  - JWT 配置预留
  - APP_ENCRYPT_KEY 预留
- 验收标准:
  - 环境变量命名统一
  - 与文档要求一致
  - 不包含真实敏感值
- 非目标:
  - 不在仓库中写入真实生产凭证

### TASK-ID: BOOT-STRUCT-001
- 状态: ready
- 优先级: P0
- 所属子域: 目录结构与工程约定
- 目标: 建立项目应用层目录规范
- 依赖:
  - BOOT-APP-001
- 输入:
  - Laravel 默认结构
  - 项目模块复杂度
- 输出:
  - 适配本项目的应用层目录结构
- 建议目录:
  - `app/Http/Controllers/Api`
  - `app/Http/Controllers/Admin`
  - `app/Http/Requests/Api`
  - `app/Http/Requests/Admin`
  - `app/Http/Resources`
  - `app/Models`
  - `app/Services`
  - `app/Policies`
  - `app/Enums`
  - `app/Exceptions`
  - `app/Support`
  - `app/Jobs`
  - `app/Events`
  - `app/Listeners`
- 验收标准:
  - 目录结构清晰
  - 命名规则统一
  - 能支撑前台/后台/API/异步任务分层

### TASK-ID: BOOT-STRUCT-002
- 状态: ready
- 优先级: P1
- 所属子域: 目录结构与工程约定
- 目标: 建立模块命名与分层约定文档
- 依赖:
  - BOOT-STRUCT-001
- 输入:
  - 项目模块全景
  - Laravel 分层习惯
- 输出:
  - 内部开发约定文档
- 产出物:
  - `docs/development-structure.md` 或等价文档
- 应说明:
  - Controller 职责边界
  - Request 职责边界
  - Service 职责边界
  - Policy/Gate 使用原则
  - Resource 输出原则
  - Job / Event / Listener 使用场景
- 验收标准:
  - 文档能指导后续 AI 开发
  - 不与现有技术文档冲突

### TASK-ID: BOOT-CONFIG-001
- 状态: ready
- 优先级: P0
- 所属子域: 环境配置与基础驱动
- 目标: 配置数据库连接与基础 database 配置
- 依赖:
  - BOOT-APP-001
  - BOOT-APP-003
- 输入:
  - 技术架构文档中的 DB 要求
- 输出:
  - `config/database.php` 与环境变量匹配
- 验收标准:
  - MySQL 8 兼容
  - utf8mb4 / utf8mb4_unicode_ci 配置正确
  - 可正常执行 migration

### TASK-ID: BOOT-CONFIG-002
- 状态: ready
- 优先级: P0
- 所属子域: 环境配置与基础驱动
- 目标: 配置 Redis、缓存与队列驱动
- 依赖:
  - BOOT-APP-001
  - BOOT-APP-003
- 输入:
  - 技术架构文档
  - 部署说明
- 输出:
  - Redis 作为 cache / queue 可用
- 产出物:
  - `config/cache.php`
  - `config/queue.php`
  - `config/database.php` 中 Redis 配置
- 验收标准:
  - Laravel 可识别 Redis 配置
  - 默认队列驱动支持 Redis
  - 默认缓存驱动可切换到 Redis

### TASK-ID: BOOT-CONFIG-003
- 状态: ready
- 优先级: P0
- 所属子域: 环境配置与基础驱动
- 目标: 配置 Filesystem 的 public/private 双盘
- 依赖:
  - BOOT-APP-001
  - BOOT-APP-003
- 输入:
  - 技术文档中的文件访问规则
- 输出:
  - `public` / `private` 磁盘配置完成
- 产出物:
  - `config/filesystems.php`
- 验收标准:
  - public 盘可生成公开 URL
  - private 盘可为后续 temporaryUrl 方案预留
  - 配置命名与文档一致

### TASK-ID: BOOT-CONFIG-004
- 状态: ready
- 优先级: P1
- 所属子域: 环境配置与基础驱动
- 目标: 新建项目级系统配置文件
- 依赖:
  - BOOT-APP-001
- 输入:
  - API 文档中 system config 输出要求
  - 安全文档中的配置项要求
- 输出:
  - 可维护的项目配置文件
- 产出物:
  - `config/system.php`
- 建议先放入默认值:
  - site_name
  - chat_enabled
  - public_chat_enabled
  - captcha_enabled
  - refresh_limit_per_day
  - reapply_limit_hours
  - first_jobs_audit_limit
  - token_expire_days
  - login_fail_max
  - login_lock_minutes
- 验收标准:
  - 配置键命名统一
  - 可与 system_config 表做默认值合并

### TASK-ID: BOOT-DB-001
- 状态: ready
- 优先级: P0
- 所属子域: 基础数据库底座
- 目标: 创建基础迁移目录与命名规范
- 依赖:
  - BOOT-APP-001
  - BOOT-CONFIG-001
- 输入:
  - Laravel migration 规范
- 输出:
  - migration 命名规范明确
- 产出物:
  - `database/migrations/`
  - 迁移命名说明文档（可选）
- 验收标准:
  - 后续表迁移可按统一规则添加

### TASK-ID: BOOT-DB-FILES-001
- 状态: ready
- 优先级: P0
- 所属子域: 基础数据库底座
- 目标: 创建 `files` 表 migration
- 依赖:
  - BOOT-DB-001
  - BOOT-CONFIG-003
- 输入:
  - 文件资源统一结构要求
- 输出:
  - `files` 表可支撑全站文件元数据
- 必含字段建议:
  - id
  - file_id
  - disk
  - path
  - access
  - mime_type
  - size
  - original_name
  - uploader_id
  - type
  - created_at / updated_at
- 索引建议:
  - `file_id` 唯一索引
  - `uploader_id` 普通索引
  - `type` 普通索引
- 验收标准:
  - 能支撑 File Resource 输出
  - 可用于头像、职位图、资质、举报证据等统一建模
- 非目标:
  - 不实现上传逻辑

### TASK-ID: BOOT-DB-SYSCONFIG-001
- 状态: ready
- 优先级: P0
- 所属子域: 基础数据库底座
- 目标: 创建 `system_config` 表 migration
- 依赖:
  - BOOT-DB-001
  - BOOT-CONFIG-004
- 输入:
  - 系统配置文档
  - 管理后台系统配置接口文档
- 输出:
  - `system_config` 表可支撑动态配置
- 必含字段建议:
  - id
  - category
  - config_key
  - config_value
  - config_name
  - config_desc
  - value_type
  - is_editable
  - created_at / updated_at
- 索引建议:
  - `config_key` 唯一索引
  - `category` 普通索引
- 验收标准:
  - 可支撑 basic/function/business/security 四类配置
  - 可与 `config/system.php` 做默认值合并

### TASK-ID: BOOT-DB-CONFIGLOG-001
- 状态: ready
- 优先级: P1
- 所属子域: 基础数据库底座
- 目标: 创建 `config_change_logs` 表 migration
- 依赖:
  - BOOT-DB-SYSCONFIG-001
- 输入:
  - 配置变更审计要求
- 输出:
  - 配置修改有独立日志表
- 必含字段建议:
  - id
  - operator_id
  - request_id
  - change_reason
  - before_snapshot
  - after_snapshot
  - created_at
- 验收标准:
  - 能独立记录配置变更历史
  - 能与 operation_logs 口径联动

### TASK-ID: BOOT-DB-OPLOG-001
- 状态: ready
- 优先级: P0
- 所属子域: 基础数据库底座
- 目标: 创建 `operation_logs` 表 migration
- 依赖:
  - BOOT-DB-001
- 输入:
  - 安全文档中的后台高危操作审计要求
- 输出:
  - 全站后台审计日志底座可用
- 必含字段建议:
  - id
  - operator_type
  - operator_id
  - operation_type
  - operation_module
  - target_type
  - target_id
  - request_id
  - result
  - risk_level
  - reason
  - ip_address
  - user_agent
  - before_snapshot
  - after_snapshot
  - created_at
- 索引建议:
  - operator_id
  - target_type + target_id
  - request_id
  - created_at
  - result
- 验收标准:
  - 满足文档定义的高危操作留痕要求
  - 能支撑按 request_id 追踪操作链
- 非目标:
  - 本任务不实现所有业务写日志调用点

### TASK-ID: BOOT-FILE-RES-001
- 状态: ready
- 优先级: P0
- 所属子域: 文件存储底座
- 目标: 实现统一 File Resource 输出类
- 依赖:
  - BOOT-DB-FILES-001
  - BOOT-CONFIG-003
- 输入:
  - File Resource 统一字段规范
- 输出:
  - 单文件资源输出类
- 产出物:
  - `app/Http/Resources/FileResource.php`
- 验收标准:
  - 输出字段包含 `file_id/disk/path/access/url/temporary_url/mime_type/size/original_name`
  - public/private 两种场景兼容
- 非目标:
  - 不实现上传接口

### TASK-ID: BOOT-FILE-RES-002
- 状态: ready
- 优先级: P1
- 所属子域: 文件存储底座
- 目标: 实现文件集合资源输出类
- 依赖:
  - BOOT-FILE-RES-001
- 输入:
  - 多文件输出需求
- 输出:
  - 文件集合资源类
- 产出物:
  - `app/Http/Resources/FileResourceCollection.php` 或等价实现
- 验收标准:
  - 可复用于职位图片、证据文件、资质材料等场景

### TASK-ID: BOOT-FILE-SVC-001
- 状态: ready
- 优先级: P1
- 所属子域: 文件存储底座
- 目标: 实现文件链接解析基础服务
- 依赖:
  - BOOT-FILE-RES-001
  - BOOT-CONFIG-003
- 输入:
  - public/private 文件访问规则
- 输出:
  - 文件 URL / temporary URL 统一解析服务
- 产出物:
  - `app/Services/FileService.php` 或等价类
- 验收标准:
  - public 文件可生成 url
  - private 文件可预留生成 temporary_url 能力
  - 前端不需要拼接路径
- 非目标:
  - 不实现对象存储接入细节

### TASK-ID: BOOT-API-RESP-001
- 状态: ready
- 优先级: P0
- 所属子域: 统一 API 响应与异常处理
- 目标: 建立统一 API 成功响应封装
- 依赖:
  - BOOT-APP-001
- 输入:
  - 文档统一响应格式 `code/msg/data`
- 输出:
  - 全站统一成功响应辅助能力
- 产出物:
  - `app/Support/ApiResponse.php` 或等价 Helper/Trait
- 验收标准:
  - 所有成功响应可统一输出 `code/msg/data`
  - 支持 list/detail/null 等常见场景

### TASK-ID: BOOT-API-RESP-002
- 状态: ready
- 优先级: P0
- 所属子域: 统一 API 响应与异常处理
- 目标: 建立统一错误响应结构
- 依赖:
  - BOOT-API-RESP-001
- 输入:
  - API 错误码规范
- 输出:
  - 错误响应统一输出
- 验收标准:
  - 非成功场景不一律返回 200
  - 业务错误、参数错误、未授权、限流等能分层返回
  - 响应结构保持统一

### TASK-ID: BOOT-EXCEPTION-001
- 状态: ready
- 优先级: P0
- 所属子域: 统一 API 响应与异常处理
- 目标: 配置 Laravel 11 全局异常处理为 API 统一风格
- 依赖:
  - BOOT-API-RESP-002
- 输入:
  - Laravel 11 异常处理机制
  - API 返回结构规范
- 输出:
  - 常见异常可统一转 JSON
- 产出物:
  - `bootstrap/app.php` 相关异常处理配置
  - 自定义异常类（如需要）
- 验收标准:
  - ValidationException
  - AuthenticationException
  - AuthorizationException
  - ModelNotFoundException
  - ThrottleRequestsException
  - 通用 Throwable
  均有统一输出口径

### TASK-ID: BOOT-ERRORCODE-001
- 状态: ready
- 优先级: P1
- 所属子域: 统一 API 响应与异常处理
- 目标: 建立项目错误码常量或枚举定义
- 依赖:
  - BOOT-API-RESP-002
- 输入:
  - API 文档中涉及的业务错误码
- 输出:
  - 可复用的错误码定义
- 产出物:
  - `app/Enums/ErrorCode.php` 或等价实现
- 验收标准:
  - 覆盖认证、限流、参数、权限、WebSocket 相关基础错误码
  - 后续模块可直接引用

### TASK-ID: BOOT-AUTH-001
- 状态: ready
- 优先级: P0
- 所属子域: 鉴权骨架与中间件预留
- 目标: 确定 JWT 方案并接入基础认证骨架
- 依赖:
  - BOOT-APP-002
- 输入:
  - 技术架构文档中的 JWT 要求
- 输出:
  - JWT 基础接入方案明确
- 验收标准:
  - 已确定具体包或自实现策略
  - 支持用户端与管理员端后续分离
- 非目标:
  - 不在本任务中实现注册登录接口

### TASK-ID: BOOT-AUTH-002
- 状态: ready
- 优先级: P0
- 所属子域: 鉴权骨架与中间件预留
- 目标: 建立用户端 API 认证中间件骨架
- 依赖:
  - BOOT-AUTH-001
- 输入:
  - JWT 方案
  - 用户端 Bearer Token 规范
- 输出:
  - 用户端 API 可接入鉴权中间件
- 产出物:
  - `auth.jwt:user` 或等价中间件
- 验收标准:
  - 能识别 Bearer Token
  - 能为后续业务接口提供用户解析入口
- 非目标:
  - 不要求本任务实现完整 token 黑名单

### TASK-ID: BOOT-AUTH-003
- 状态: ready
- 优先级: P0
- 所属子域: 鉴权骨架与中间件预留
- 目标: 建立管理员端 API 认证中间件骨架
- 依赖:
  - BOOT-AUTH-001
- 输入:
  - 管理端 Bearer Token 规范
- 输出:
  - 管理端独立鉴权骨架
- 产出物:
  - `auth.jwt:admin` 或等价中间件
- 验收标准:
  - 能区分普通用户 token 与管理员 token
  - 为后台接口提供独立鉴权入口

### TASK-ID: BOOT-AUTH-004
- 状态: ready
- 优先级: P1
- 所属子域: 鉴权骨架与中间件预留
- 目标: 预留 token 自动续期中间件骨架
- 依赖:
  - BOOT-AUTH-002
  - BOOT-CONFIG-004
- 输入:
  - `X-New-Token` 续期规则
- 输出:
  - 后续可扩展的续期中间件占位实现
- 验收标准:
  - 中间件位置与调用链明确
  - 后续用户登录模块可直接挂接
- 非目标:
  - 本任务不要求完成续签逻辑

### TASK-ID: BOOT-SYSCONFIG-001
- 状态: ready
- 优先级: P0
- 所属子域: 系统配置底座
- 目标: 创建 `SystemConfig` 模型
- 依赖:
  - BOOT-DB-SYSCONFIG-001
- 输入:
  - system_config 表结构
- 输出:
  - 配置模型可供后续服务调用
- 产出物:
  - `app/Models/SystemConfig.php`
- 验收标准:
  - 字段映射清晰
  - 可支持按 category/config_key 查询

### TASK-ID: BOOT-SYSCONFIG-002
- 状态: ready
- 优先级: P0
- 所属子域: 系统配置底座
- 目标: 实现系统配置读取服务
- 依赖:
  - BOOT-SYSCONFIG-001
  - BOOT-CONFIG-004
- 输入:
  - 数据库配置
  - 默认配置文件 `config/system.php`
- 输出:
  - 配置读取服务可合并 DB 与默认值
- 产出物:
  - `app/Services/SystemConfigService.php`
- 验收标准:
  - 支持按 key 获取配置
  - 支持 DB 配置优先、文件默认兜底
  - 为后续公共配置接口提供底层能力

### TASK-ID: BOOT-SYSCONFIG-003
- 状态: ready
- 优先级: P1
- 所属子域: 系统配置底座
- 目标: 编写系统配置初始化 Seeder
- 依赖:
  - BOOT-DB-SYSCONFIG-001
  - BOOT-CONFIG-004
- 输入:
  - 默认配置项清单
- 输出:
  - 系统初始配置可写入数据库
- 产出物:
  - `database/seeders/SystemConfigSeeder.php`
- 验收标准:
  - 初始化关键配置项
  - 不写入敏感真实生产值

### TASK-ID: BOOT-TEST-001
- 状态: ready
- 优先级: P0
- 所属子域: 基础测试框架
- 目标: 配置 Laravel 测试环境
- 依赖:
  - BOOT-APP-001
  - BOOT-CONFIG-001
- 输入:
  - Laravel 测试机制
- 输出:
  - 测试环境可运行
- 产出物:
  - `phpunit.xml`
  - 测试环境配置
- 验收标准:
  - 可执行基础测试命令
  - 测试环境与开发环境隔离

### TASK-ID: BOOT-TEST-002
- 状态: ready
- 优先级: P1
- 所属子域: 基础测试框架
- 目标: 建立 API 测试基类
- 依赖:
  - BOOT-TEST-001
  - BOOT-API-RESP-001
- 输入:
  - 统一 API 输出规范
- 输出:
  - 后续 Feature Test 可复用基类
- 产出物:
  - `tests/Feature/ApiTestCase.php` 或等价基类
- 验收标准:
  - 可复用 JSON 断言
  - 可复用统一响应断言

### TASK-ID: BOOT-TEST-003
- 状态: ready
- 优先级: P1
- 所属子域: 基础测试框架
- 目标: 编写基础健康检查测试
- 依赖:
  - BOOT-TEST-001
  - BOOT-API-RESP-001
- 输入:
  - 基础路由或健康检查路由
- 输出:
  - 最小可跑测试样例
- 验收标准:
  - 至少有 1 个成功测试证明框架可用

### TASK-ID: BOOT-DOC-001
- 状态: ready
- 优先级: P1
- 所属子域: 基础文档与开发规范
- 目标: 编写项目 README 基础开发说明
- 依赖:
  - BOOT-APP-001
  - BOOT-CONFIG-001
  - BOOT-CONFIG-002
  - BOOT-CONFIG-003
- 输入:
  - 部署说明文档
  - 技术架构文档
- 输出:
  - 开发者可快速启动项目
- 产出物:
  - `README.md`
- 至少包含:
  - 环境要求
  - 安装步骤
  - 本地启动方式
  - 数据库/Redis 配置说明
  - 测试命令
- 验收标准:
  - 新开发者能按 README 启动项目

### TASK-ID: BOOT-DOC-002
- 状态: ready
- 优先级: P1
- 所属子域: 基础文档与开发规范
- 目标: 编写基础开发约定文档
- 依赖:
  - BOOT-STRUCT-002
  - BOOT-API-RESP-002
  - BOOT-EXCEPTION-001
- 输入:
  - 本章的工程约定
- 输出:
  - 可供 AI / 开发者统一遵循的开发规范
- 产出物:
  - `docs/development-guide.md`
- 至少包含:
  - 命名规范
  - Controller / Service / Request / Resource 分层
  - 状态机不可擅改原则
  - API 响应结构
  - 错误处理规范
  - 测试最低要求
- 验收标准:
  - 能减少后续 AI 反复猜测工程约定

## 1.6 本章推荐执行顺序

### 第一批（立即开工，P0）
1. BOOT-APP-001
2. BOOT-APP-002
3. BOOT-APP-003
4. BOOT-CONFIG-001
5. BOOT-CONFIG-002
6. BOOT-CONFIG-003
7. BOOT-STRUCT-001
8. BOOT-DB-001
9. BOOT-DB-FILES-001
10. BOOT-DB-SYSCONFIG-001
11. BOOT-DB-OPLOG-001
12. BOOT-API-RESP-001
13. BOOT-API-RESP-002
14. BOOT-EXCEPTION-001
15. BOOT-AUTH-001
16. BOOT-AUTH-002
17. BOOT-AUTH-003
18. BOOT-SYSCONFIG-001
19. BOOT-SYSCONFIG-002
20. BOOT-TEST-001

### 第二批（主链路支撑增强，P1）
1. BOOT-CONFIG-004
2. BOOT-DB-CONFIGLOG-001
3. BOOT-FILE-RES-001
4. BOOT-FILE-RES-002
5. BOOT-FILE-SVC-001
6. BOOT-ERRORCODE-001
7. BOOT-AUTH-004
8. BOOT-SYSCONFIG-003
9. BOOT-TEST-002
10. BOOT-TEST-003
11. BOOT-STRUCT-002
12. BOOT-DOC-001
13. BOOT-DOC-002

## 1.7 本章完成后的下一个章节入口
当第 1 章完成后，最合理进入：
- 第2章：用户与认证

---

# 第2章：用户与认证

> 目标：建立用户端核心身份体系，让项目具备注册、登录、登出、验证码、token、登录锁定、密码修改/找回、账号基础安全控制能力。
> 本章是后续简历、企业认证、职位、报名、聊天、通知等所有用户侧业务的基础前置模块。

## 2.1 本章定位

### 本章要解决的问题
- 定义并落地用户主表
- 定义用户登录状态相关底层表
- 建立图形验证码能力
- 建立注册/登录/登出接口
- 建立 JWT token 签发与失效基础能力
- 建立 token 自动续期链路
- 建立登录失败次数累计与锁定逻辑
- 建立密码修改与找回密码基础能力
- 为后续简历、企业认证、职位报名等模块提供用户身份基础

### 本章不做的内容
- 简历详情维护
- 企业认证提交
- 职位发布
- 报名
- 私聊
- 后台管理员登录
- 黑名单业务联动处置的完整后台逻辑

## 2.2 本章完成标准
当第 2 章完成时，应满足：
1. `users` 表与基础用户模型可用
2. 用户可通过验证码完成注册
3. 用户可通过账号密码完成登录
4. 登录后可返回合法 Bearer Token
5. 用户可登出并使 token 失效
6. token 自动续期链路已可用
7. 登录失败累计和锁定规则已实现
8. 修改密码能力可用
9. 找回密码基础链路可用
10. 关键认证接口具备基础测试覆盖

## 2.3 本章依赖关系
### 上游依赖
- 第1章：项目骨架与基础设施

### 下游依赖
- 简历系统
- 企业认证
- 职位系统
- 报名系统
- 聊天与 WebSocket
- 通知系统
- 收藏 / 浏览历史

## 2.4 本章任务总览
1. 用户数据模型与表结构
2. 用户模型与枚举
3. 图形验证码
4. 注册能力
5. 登录能力
6. 登出与 token 失效
7. token 自动续期
8. 登录失败锁定
9. 密码修改与找回
10. 用户基础资料接口
11. 用户认证测试

## 2.5 可直接执行任务清单

### 2.5.1 用户数据模型与表结构

### TASK-ID: AUTH-DB-USER-001
- 状态: blocked
- 优先级: P0
- 所属子域: 用户数据模型与表结构
- 目标: 创建 `users` 表 migration
- 依赖:
  - BOOT-DB-001
- 输入:
  - API 文档中的用户相关字段
  - 状态机文档
  - 安全文档中的登录锁定/注销要求
- 输出:
  - `users` 表可支撑用户基础身份、认证状态、登录安全与注销状态
- 建议字段至少包含:
  - id
  - phone
  - email
  - password
  - nickname
  - avatar_file_id
  - role
  - is_certified
  - is_blacklist
  - status
  - login_fail_count
  - locked_until
  - last_login_time
  - last_login_ip
  - must_change_password
  - delete_apply_at
  - deleted_at
  - created_at / updated_at
- 索引建议:
  - phone 唯一索引
  - email 唯一索引（允许空时按数据库策略处理）
  - role 普通索引
  - is_certified 普通索引
  - is_blacklist 普通索引
  - locked_until 普通索引
- 验收标准:
  - 可支撑求职者/雇主统一用户主体
  - 可支撑登录失败锁定
  - 可支撑后续认证状态与黑名单状态
  - 可支撑逻辑删除/注销流程
- 非目标:
  - 不在本任务中创建简历表
  - 不在本任务中创建企业表

### TASK-ID: AUTH-DB-LOGINLOG-001
- 状态: blocked
- 优先级: P1
- 所属子域: 用户数据模型与表结构
- 目标: 创建用户登录日志表 migration
- 依赖:
  - BOOT-DB-001
  - AUTH-DB-USER-001
- 输入:
  - 安全与审计需求
- 输出:
  - 用户登录日志底表可用
- 建议表名:
  - `user_login_logs`
- 建议字段至少包含:
  - id
  - user_id
  - login_type
  - login_ip
  - user_agent
  - device_id
  - status
  - fail_reason
  - created_at
- 验收标准:
  - 能记录成功/失败登录事件
  - 可用于追踪账号异常登录

### TASK-ID: AUTH-DB-DEVICESESSION-001
- 状态: blocked
- 优先级: P0
- 所属子域: 用户数据模型与表结构
- 目标: 创建 `device_sessions` 表 migration
- 依赖:
  - BOOT-DB-001
  - AUTH-DB-USER-001
- 输入:
  - ws_token 与 device_session_id 绑定要求
  - token 失效与踢线要求
- 输出:
  - 设备会话底层表可用
- 建议字段至少包含:
  - id
  - session_id
  - user_id
  - device_id
  - device_type
  - login_ip
  - user_agent
  - token_hash
  - status
  - last_active_at
  - expired_at
  - created_at / updated_at
- 索引建议:
  - session_id 唯一索引
  - user_id 普通索引
  - device_id 普通索引
  - status 普通索引
- 验收标准:
  - 可支撑 token 绑定设备会话
  - 可支撑 WebSocket 鉴权关联
  - 可支撑登出、强制下线、密码修改后会话失效

### TASK-ID: AUTH-DB-CAPTCHA-001
- 状态: blocked
- 优先级: P0
- 所属子域: 用户数据模型与表结构
- 目标: 确定图形验证码存储策略并完成落地
- 依赖:
  - BOOT-CONFIG-002
- 输入:
  - 图形验证码接口要求
- 输出:
  - 验证码存储策略可用
- 实现方式建议:
  - Redis 存储，不强依赖数据库表
- 验收标准:
  - 可根据 `captcha_key` 校验验证码
  - 具备 TTL 与单次消费能力
- 非目标:
  - 本任务不实现对外接口

### 2.5.2 用户模型与枚举

### TASK-ID: AUTH-MODEL-USER-001
- 状态: blocked
- 优先级: P0
- 所属子域: 用户模型与枚举
- 目标: 创建 `User` 模型并补齐基础属性
- 依赖:
  - AUTH-DB-USER-001
- 输入:
  - users 表结构
- 输出:
  - `User` 模型可用
- 产出物:
  - `app/Models/User.php`
- 应补齐:
  - fillable / guarded
  - hidden
  - casts
  - 头像关联
  - 认证状态辅助方法
  - 锁定状态辅助方法
- 验收标准:
  - 模型字段映射清晰
  - 能为后续鉴权和用户资料接口复用

### TASK-ID: AUTH-MODEL-SESSION-001
- 状态: blocked
- 优先级: P1
- 所属子域: 用户模型与枚举
- 目标: 创建 `DeviceSession` 模型
- 依赖:
  - AUTH-DB-DEVICESESSION-001
- 输入:
  - device_sessions 表结构
- 输出:
  - 设备会话模型可用
- 产出物:
  - `app/Models/DeviceSession.php`
- 验收标准:
  - 可支撑登录会话查询、失效、踢线

### TASK-ID: AUTH-ENUM-001
- 状态: blocked
- 优先级: P1
- 所属子域: 用户模型与枚举
- 目标: 建立用户角色与状态相关枚举
- 依赖:
  - AUTH-MODEL-USER-001
- 输入:
  - 角色与状态文档
- 输出:
  - 可复用枚举类
- 建议至少包含:
  - 用户角色枚举
  - 认证状态枚举
  - 用户状态枚举
  - 设备会话状态枚举
- 验收标准:
  - 后续模块不再手写魔法字符串

### 2.5.3 图形验证码

### TASK-ID: AUTH-API-CAPTCHA-001
- 状态: blocked
- 优先级: P0
- 所属子域: 图形验证码
- 目标: 实现 `GET /api/user/captcha`
- 依赖:
  - AUTH-DB-CAPTCHA-001
  - BOOT-API-RESP-001
- 输入:
  - API 文档中的验证码要求
- 输出:
  - 图形验证码接口可用
- 返回建议至少包含:
  - captcha_key
  - captcha_image 或等价可渲染数据
  - expire_seconds
- 验收标准:
  - 可生成验证码
  - Redis 中存在对应 key
  - 验证码有 TTL
  - 响应结构符合统一规范
- 非目标:
  - 不在本任务中实现注册接口

### TASK-ID: AUTH-SVC-CAPTCHA-001
- 状态: blocked
- 优先级: P0
- 所属子域: 图形验证码
- 目标: 实现验证码生成与校验服务
- 依赖:
  - AUTH-DB-CAPTCHA-001
- 输入:
  - 验证码存储策略
- 输出:
  - 验证码服务可复用
- 产出物:
  - `app/Services/CaptchaService.php`
- 验收标准:
  - 支持生成 key/code
  - 支持校验并单次消费
  - 支持过期判定

### 2.5.4 注册能力

### TASK-ID: AUTH-REQ-REG-001
- 状态: blocked
- 优先级: P0
- 所属子域: 注册能力
- 目标: 编写注册接口参数校验 Request
- 依赖:
  - AUTH-DB-USER-001
  - AUTH-SVC-CAPTCHA-001
- 输入:
  - 注册接口文档
- 输出:
  - 注册参数校验类可用
- 建议校验项至少包含:
  - phone
  - password
  - confirm_password（如文档要求）
  - captcha_key
  - captcha
  - role（若注册时区分求职者/雇主）
- 验收标准:
  - 参数错误返回统一结构
  - 手机号格式校验明确
  - 密码强度或长度规则符合文档

### TASK-ID: AUTH-SVC-REGISTER-001
- 状态: blocked
- 优先级: P0
- 所属子域: 注册能力
- 目标: 实现用户注册服务
- 依赖:
  - AUTH-REQ-REG-001
  - AUTH-MODEL-USER-001
  - AUTH-SVC-CAPTCHA-001
- 输入:
  - 注册参数
  - users 表结构
- 输出:
  - 用户注册业务逻辑可用
- 业务要求至少包括:
  - 校验验证码
  - 手机号唯一校验
  - 密码加密存储
  - 初始化默认状态
- 验收标准:
  - 注册成功写入 users 表
  - 密码不明文存储
  - 重复手机号禁止注册

### TASK-ID: AUTH-API-REG-001
- 状态: blocked
- 优先级: P0
- 所属子域: 注册能力
- 目标: 实现 `POST /api/user/register`
- 依赖:
  - AUTH-SVC-REGISTER-001
  - BOOT-API-RESP-001
- 输入:
  - 注册接口文档
- 输出:
  - 注册接口可用
- 验收标准:
  - 成功返回统一 `code/msg/data`
  - 文档要求的用户基础信息或 token 返回正确
  - 错误分支返回统一结构
- 非目标:
  - 本任务不实现登录接口

### 2.5.5 登录能力

### TASK-ID: AUTH-REQ-LOGIN-001
- 状态: blocked
- 优先级: P0
- 所属子域: 登录能力
- 目标: 编写登录接口参数校验 Request
- 依赖:
  - AUTH-SVC-CAPTCHA-001
- 输入:
  - 登录接口文档
- 输出:
  - 登录参数校验类可用
- 建议校验项至少包含:
  - username / phone / email（按文档支持范围）
  - password
  - captcha_key
  - captcha
  - device_id（如需要）
- 验收标准:
  - 参数错误返回统一结构

### TASK-ID: AUTH-SVC-TOKEN-001
- 状态: blocked
- 优先级: P0
- 所属子域: 登录能力
- 目标: 实现用户端 JWT token 签发服务
- 依赖:
  - BOOT-AUTH-001
  - AUTH-MODEL-USER-001
  - AUTH-DB-DEVICESESSION-001
- 输入:
  - JWT 方案
  - token_expire_days 配置
- 输出:
  - 用户端 token 签发能力可用
- 产出物:
  - `app/Services/AuthTokenService.php` 或等价类
- 验收标准:
  - token 包含用户身份信息
  - token 与 device_session 绑定
  - 可返回过期时间
- 非目标:
  - 不实现管理员 token 签发

### TASK-ID: AUTH-SVC-LOGIN-001
- 状态: blocked
- 优先级: P0
- 所属子域: 登录能力
- 目标: 实现用户登录服务
- 依赖:
  - AUTH-REQ-LOGIN-001
  - AUTH-SVC-CAPTCHA-001
  - AUTH-SVC-TOKEN-001
  - AUTH-SVC-LOCK-001
  - AUTH-DB-LOGINLOG-001
- 输入:
  - 登录接口文档
  - users 表
- 输出:
  - 登录业务逻辑可用
- 业务要求至少包括:
  - 校验验证码
  - 校验密码
  - 检查账号锁定状态
  - 重置/累计登录失败次数
  - 创建 device_session
  - 签发 token
  - 写登录日志
- 验收标准:
  - 正确账号密码登录成功
  - 错误密码累计失败次数
  - 锁定账号不可登录

### TASK-ID: AUTH-API-LOGIN-001
- 状态: blocked
- 优先级: P0
- 所属子域: 登录能力
- 目标: 实现 `POST /api/user/login`
- 依赖:
  - AUTH-SVC-LOGIN-001
  - BOOT-API-RESP-001
- 输入:
  - 登录接口文档
- 输出:
  - 登录接口可用
- 成功返回至少包含:
  - token
  - token_expire
  - 用户基础信息
- 验收标准:
  - 响应结构与文档一致
  - 登录成功后后续受保护接口可识别身份

### 2.5.6 登出与 token 失效

### TASK-ID: AUTH-SVC-LOGOUT-001
- 状态: blocked
- 优先级: P0
- 所属子域: 登出与 token 失效
- 目标: 实现用户登出服务
- 依赖:
  - AUTH-SVC-TOKEN-001
  - AUTH-MODEL-SESSION-001
  - BOOT-CONFIG-002
- 输入:
  - 当前 Bearer Token
  - 当前 device_session
- 输出:
  - 登出逻辑可使 token 失效
- 业务要求至少包括:
  - token 拉入黑名单或失效存储
  - 对应 device_session 失效
- 验收标准:
  - 登出后 token 不可继续访问受保护接口

### TASK-ID: AUTH-API-LOGOUT-001
- 状态: blocked
- 优先级: P0
- 所属子域: 登出与 token 失效
- 目标: 实现 `POST /api/user/logout`
- 依赖:
  - AUTH-SVC-LOGOUT-001
  - BOOT-AUTH-002
- 输入:
  - 当前登录态
- 输出:
  - 登出接口可用
- 验收标准:
  - 成功返回统一结构
  - token 立即失效

### 2.5.7 token 自动续期

### TASK-ID: AUTH-SVC-TOKENREFRESH-001
- 状态: blocked
- 优先级: P0
- 所属子域: token 自动续期
- 目标: 实现 token 自动续期服务
- 依赖:
  - AUTH-SVC-TOKEN-001
  - BOOT-CONFIG-004
- 输入:
  - token_expire_days
  - 当前 token 过期时间
- 输出:
  - 可判断是否需要续签并签发新 token
- 验收标准:
  - 距过期不足 1 天时可签发新 token
  - 新 token 有新的到期时间

### TASK-ID: AUTH-MW-TOKENREFRESH-001
- 状态: blocked
- 优先级: P0
- 所属子域: token 自动续期
- 目标: 完成用户端 token 自动续期中间件实现
- 依赖:
  - BOOT-AUTH-004
  - AUTH-SVC-TOKENREFRESH-001
- 输入:
  - 当前受保护请求
- 输出:
  - 响应头可按条件返回 `X-New-Token`
- 验收标准:
  - 接近过期时返回新 token
  - 未接近过期时不误续签

### 2.5.8 登录失败锁定

### TASK-ID: AUTH-SVC-LOCK-001
- 状态: blocked
- 优先级: P0
- 所属子域: 登录失败锁定
- 目标: 实现登录失败累计与账号锁定服务
- 依赖:
  - AUTH-DB-USER-001
  - BOOT-CONFIG-004
- 输入:
  - `login_fail_max`
  - `login_lock_minutes`
- 输出:
  - 锁定策略服务可用
- 业务要求至少包括:
  - 登录失败次数累加
  - 达阈值后设置 `locked_until`
  - 登录成功重置失败次数
  - 可判断当前账号是否处于锁定状态
- 验收标准:
  - 与系统配置联动
  - 锁定时间计算正确

### TASK-ID: AUTH-API-LOCKRESP-001
- 状态: blocked
- 优先级: P1
- 所属子域: 登录失败锁定
- 目标: 实现登录锁定错误响应结构
- 依赖:
  - AUTH-SVC-LOCK-001
  - BOOT-ERRORCODE-001
- 输入:
  - 登录锁定接口文档
- 输出:
  - 锁定时返回标准结构
- 验收标准:
  - 返回 `locked_until`
  - 返回剩余锁定分钟或时间信息

### 2.5.9 密码修改与找回

### TASK-ID: AUTH-REQ-CHANGEPWD-001
- 状态: blocked
- 优先级: P1
- 所属子域: 密码修改与找回
- 目标: 编写修改密码接口参数校验 Request
- 依赖:
  - AUTH-MODEL-USER-001
- 输入:
  - 修改密码接口文档
- 输出:
  - 修改密码参数校验类可用
- 验收标准:
  - 原密码、新密码、确认密码校验明确

### TASK-ID: AUTH-SVC-CHANGEPWD-001
- 状态: blocked
- 优先级: P1
- 所属子域: 密码修改与找回
- 目标: 实现用户修改密码服务
- 依赖:
  - AUTH-REQ-CHANGEPWD-001
  - AUTH-SVC-LOGOUT-001
- 输入:
  - 当前用户
  - 原密码
  - 新密码
- 输出:
  - 修改密码逻辑可用
- 业务要求至少包括:
  - 校验原密码
  - 保存新密码
  - 现有 token / device_session 失效
- 验收标准:
  - 修改后旧 token 不可用
  - 新密码可登录

### TASK-ID: AUTH-API-CHANGEPWD-001
- 状态: blocked
- 优先级: P1
- 所属子域: 密码修改与找回
- 目标: 实现 `POST /api/user/change-password`
- 依赖:
  - AUTH-SVC-CHANGEPWD-001
  - BOOT-AUTH-002
- 输入:
  - 修改密码接口文档
- 输出:
  - 修改密码接口可用
- 验收标准:
  - 成功返回统一结构
  - 旧密码错误时返回标准错误

### TASK-ID: AUTH-REQ-RESETPWD-001
- 状态: blocked
- 优先级: P1
- 所属子域: 密码修改与找回
- 目标: 编写找回密码接口参数校验 Request
- 依赖:
  - AUTH-SVC-CAPTCHA-001
- 输入:
  - 找回密码接口文档
- 输出:
  - 找回密码参数校验类可用
- 验收标准:
  - 账号标识、新密码、验证码等参数校验明确

### TASK-ID: AUTH-SVC-RESETPWD-001
- 状态: blocked
- 优先级: P1
- 所属子域: 密码修改与找回
- 目标: 实现找回密码服务
- 依赖:
  - AUTH-REQ-RESETPWD-001
  - AUTH-SVC-CAPTCHA-001
  - AUTH-SVC-LOGOUT-001
- 输入:
  - 账号标识
  - 验证码
  - 新密码
- 输出:
  - 找回密码逻辑可用
- 验收标准:
  - 验证通过后可重置密码
  - 原会话失效

### TASK-ID: AUTH-API-RESETPWD-001
- 状态: blocked
- 优先级: P1
- 所属子域: 密码修改与找回
- 目标: 实现用户找回密码接口
- 依赖:
  - AUTH-SVC-RESETPWD-001
  - BOOT-API-RESP-001
- 输入:
  - 找回密码接口文档
- 输出:
  - 找回密码接口可用
- 验收标准:
  - 成功/失败响应统一

### 2.5.10 用户基础资料接口

### TASK-ID: AUTH-RES-USER-001
- 状态: blocked
- 优先级: P1
- 所属子域: 用户基础资料接口
- 目标: 实现用户基础资源输出类
- 依赖:
  - AUTH-MODEL-USER-001
  - BOOT-FILE-RES-001
- 输入:
  - 用户返回字段要求
- 输出:
  - `UserResource` 可复用
- 验收标准:
  - 统一输出基础用户信息
  - 头像使用 File Resource 输出

### TASK-ID: AUTH-API-ME-001
- 状态: blocked
- 优先级: P1
- 所属子域: 用户基础资料接口
- 目标: 实现当前登录用户信息接口 `GET /api/user/me`
- 依赖:
  - AUTH-RES-USER-001
  - BOOT-AUTH-002
- 输入:
  - 当前登录态
- 输出:
  - 当前用户基础信息接口可用
- 验收标准:
  - 返回用户基础资料
  - 响应结构统一
- 非目标:
  - 不在本任务中实现完整简历详情

### 2.5.11 用户认证测试

### TASK-ID: AUTH-TEST-CAPTCHA-001
- 状态: blocked
- 优先级: P1
- 所属子域: 用户认证测试
- 目标: 编写验证码接口测试
- 依赖:
  - AUTH-API-CAPTCHA-001
  - BOOT-TEST-002
- 输入:
  - 验证码接口
- 输出:
  - Feature Test
- 验收标准:
  - 返回结构正确
  - 生成的 captcha_key 可在 Redis 中验证

### TASK-ID: AUTH-TEST-REG-001
- 状态: blocked
- 优先级: P0
- 所属子域: 用户认证测试
- 目标: 编写注册接口测试
- 依赖:
  - AUTH-API-REG-001
  - BOOT-TEST-002
- 输入:
  - 注册接口
- 输出:
  - Feature Test
- 验收标准:
  - 正常注册成功
  - 重复手机号失败
  - 验证码错误失败

### TASK-ID: AUTH-TEST-LOGIN-001
- 状态: blocked
- 优先级: P0
- 所属子域: 用户认证测试
- 目标: 编写登录接口测试
- 依赖:
  - AUTH-API-LOGIN-001
  - BOOT-TEST-002
- 输入:
  - 登录接口
- 输出:
  - Feature Test
- 验收标准:
  - 正确密码登录成功
  - 错误密码登录失败
  - 锁定状态登录失败

### TASK-ID: AUTH-TEST-LOGOUT-001
- 状态: blocked
- 优先级: P1
- 所属子域: 用户认证测试
- 目标: 编写登出接口测试
- 依赖:
  - AUTH-API-LOGOUT-001
  - BOOT-TEST-002
- 输入:
  - 登出接口
- 输出:
  - Feature Test
- 验收标准:
  - 登出成功
  - 登出后 token 失效

### TASK-ID: AUTH-TEST-TOKENREFRESH-001
- 状态: blocked
- 优先级: P1
- 所属子域: 用户认证测试
- 目标: 编写 token 自动续期测试
- 依赖:
  - AUTH-MW-TOKENREFRESH-001
  - BOOT-TEST-002
- 输入:
  - 续期中间件
- 输出:
  - Feature Test
- 验收标准:
  - 接近过期时返回 `X-New-Token`
  - 非接近过期时不返回续签头

### TASK-ID: AUTH-TEST-LOCK-001
- 状态: blocked
- 优先级: P0
- 所属子域: 用户认证测试
- 目标: 编写登录失败锁定测试
- 依赖:
  - AUTH-SVC-LOCK-001
  - AUTH-API-LOGIN-001
- 输入:
  - 锁定规则
- 输出:
  - Feature / Unit Test
- 验收标准:
  - 达阈值后账号锁定
  - 锁定期内无法登录
  - 成功登录后失败次数重置

### TASK-ID: AUTH-TEST-CHANGEPWD-001
- 状态: blocked
- 优先级: P1
- 所属子域: 用户认证测试
- 目标: 编写修改密码测试
- 依赖:
  - AUTH-API-CHANGEPWD-001
  - BOOT-TEST-002
- 输入:
  - 修改密码接口
- 输出:
  - Feature Test
- 验收标准:
  - 原密码正确时修改成功
  - 原密码错误时失败
  - 修改后旧 token 失效

## 2.6 本章推荐执行顺序

### 第一批（P0）
1. AUTH-DB-USER-001
2. AUTH-DB-DEVICESESSION-001
3. AUTH-DB-CAPTCHA-001
4. AUTH-MODEL-USER-001
5. AUTH-REQ-REG-001
6. AUTH-SVC-CAPTCHA-001
7. AUTH-API-CAPTCHA-001
8. AUTH-SVC-REGISTER-001
9. AUTH-API-REG-001
10. AUTH-REQ-LOGIN-001
11. AUTH-SVC-LOCK-001
12. AUTH-SVC-TOKEN-001
13. AUTH-SVC-LOGIN-001
14. AUTH-API-LOGIN-001
15. AUTH-SVC-LOGOUT-001
16. AUTH-API-LOGOUT-001
17. AUTH-SVC-TOKENREFRESH-001
18. AUTH-MW-TOKENREFRESH-001
19. AUTH-TEST-REG-001
20. AUTH-TEST-LOGIN-001
21. AUTH-TEST-LOCK-001

### 第二批（P1）
1. AUTH-DB-LOGINLOG-001
2. AUTH-MODEL-SESSION-001
3. AUTH-ENUM-001
4. AUTH-API-LOCKRESP-001
5. AUTH-REQ-CHANGEPWD-001
6. AUTH-SVC-CHANGEPWD-001
7. AUTH-API-CHANGEPWD-001
8. AUTH-REQ-RESETPWD-001
9. AUTH-SVC-RESETPWD-001
10. AUTH-API-RESETPWD-001
11. AUTH-RES-USER-001
12. AUTH-API-ME-001
13. AUTH-TEST-CAPTCHA-001
14. AUTH-TEST-LOGOUT-001
15. AUTH-TEST-TOKENREFRESH-001
16. AUTH-TEST-CHANGEPWD-001

## 2.7 本章完成后的下一个章节入口
当第 2 章完成后，最合理进入：
- 第3章：简历系统
- 或 第4章：企业认证

建议优先顺序：
1. 第3章：简历系统
2. 第4章：企业认证

因为职位发布与报名分别依赖这两条链路。

---

# 第3章：简历系统

> 目标：建立求职者简历主数据结构、简历编辑与查看能力、工作经历与技能结构、简历完整度判定能力，并为报名、雇主查看候选人、人才库等模块提供稳定输入。
> 本章只处理简历域，不直接实现职位发布、报名审核、人才库业务联动。

## 3.1 本章定位

### 本章要解决的问题
- 建立求职者简历主表与扩展表
- 建立简历与用户的一对一关系
- 支持维护基础资料、技能、工作经历、自我介绍等内容
- 建立简历完整度判定规则
- 提供当前用户查看/编辑简历接口
- 提供雇主侧可复用的简历读取基础能力
- 为报名资格校验提供“简历是否完整”的统一判断服务

### 本章不做的内容
- 企业认证资料
- 职位发布
- 报名创建
- 聊天
- 人才库加入逻辑
- 后台简历审核（文档未要求独立后台审核简历）

## 3.2 本章完成标准
当第 3 章完成时，应满足：
1. 简历主表与相关扩展表可用
2. 用户可以查看自己的简历详情
3. 用户可以创建/更新自己的简历资料
4. 用户可以维护技能与工作经历
5. 系统可计算简历完整度
6. 系统可判断用户是否满足报名所需最小简历条件
7. 雇主端与报名模块后续可以复用简历读取服务
8. 关键接口具备基础测试覆盖

## 3.3 本章依赖关系
### 上游依赖
- 第1章：项目骨架与基础设施
- 第2章：用户与认证

### 下游依赖
- 报名系统
- 雇主管理报名列表
- 人才库
- 用户详情摘要展示

## 3.4 本章任务总览
1. 简历数据结构与表设计
2. 简历模型与关联
3. 简历基础资料接口
4. 技能与工种结构
5. 工作经历结构
6. 简历完整度判定
7. 雇主侧复用读取能力
8. 简历测试

## 3.5 可直接执行任务清单

### 3.5.1 简历数据结构与表设计

### TASK-ID: RESUME-DB-BASE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 简历数据结构与表设计
- 目标: 创建 `resumes` 表 migration
- 依赖:
  - BOOT-DB-001
  - AUTH-DB-USER-001
- 输入:
  - 简历相关字段需求
  - 用户详情与报名列表中使用到的简历字段
- 输出:
  - `resumes` 表可支撑用户简历主资料
- 建议字段至少包含:
  - id
  - user_id
  - real_name
  - gender
  - age
  - birthday
  - work_years
  - education
  - current_city
  - expected_city
  - expected_salary
  - self_introduction
  - contact_phone
  - is_public
  - completion_percent
  - last_completed_at
  - created_at / updated_at
- 索引建议:
  - user_id 唯一索引
  - work_years 普通索引
  - education 普通索引
- 验收标准:
  - 每个用户最多一份主简历
  - 可支撑报名、雇主查看、人才库查看的核心展示字段
- 非目标:
  - 不在本任务中创建技能或工作经历明细表

### TASK-ID: RESUME-DB-SKILL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 简历数据结构与表设计
- 目标: 创建简历技能表 migration
- 依赖:
  - RESUME-DB-BASE-001
- 输入:
  - 工种/技能字段需求
  - 报名与人才库展示所需技能字段
- 输出:
  - 简历技能结构可用
- 建议表名:
  - `resume_skills`
- 建议字段至少包含:
  - id
  - resume_id
  - category_id
  - category_name_snapshot
  - level
  - level_name_snapshot
  - years
  - sort_order
  - created_at / updated_at
- 索引建议:
  - resume_id 普通索引
  - category_id 普通索引
  - level 普通索引
  - resume_id + category_id + level 唯一索引
- 验收标准:
  - 同一简历下同工种同级别不重复
  - 可为报名时的工种/级别匹配提供数据基础

### TASK-ID: RESUME-DB-EXP-001
- 状态: blocked
- 优先级: P0
- 所属子域: 简历数据结构与表设计
- 目标: 创建工作经历表 migration
- 依赖:
  - RESUME-DB-BASE-001
- 输入:
  - 页面设计与简历详情展示要求
- 输出:
  - 工作经历明细结构可用
- 建议表名:
  - `resume_work_experiences`
- 建议字段至少包含:
  - id
  - resume_id
  - company_name
  - position_name
  - category_name_snapshot
  - start_date
  - end_date
  - is_current
  - description
  - sort_order
  - created_at / updated_at
- 索引建议:
  - resume_id 普通索引
  - start_date 普通索引
- 验收标准:
  - 支持多段工作经历
  - 支持当前在职标记

### TASK-ID: RESUME-DB-ATTACH-001
- 状态: blocked
- 优先级: P1
- 所属子域: 简历数据结构与表设计
- 目标: 评估并落地简历附件表或附件字段策略
- 依赖:
  - RESUME-DB-BASE-001
  - BOOT-DB-FILES-001
- 输入:
  - 文档中统一文件资源模型要求
- 输出:
  - 简历附件方案明确
- 实现建议:
  - 若需要简历附件，建议单表 `resume_attachments` 或 resumes 上单个 file_id 字段
- 验收标准:
  - 方案明确且不破坏统一 File Resource 输出原则
- 非目标:
  - 若当前文档未强制要求附件上传，可记录为 P1 并保留设计占位

### 3.5.2 简历模型与关联

### TASK-ID: RESUME-MODEL-BASE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 简历模型与关联
- 目标: 创建 `Resume` 模型并补齐基础属性
- 依赖:
  - RESUME-DB-BASE-001
- 输入:
  - resumes 表结构
- 输出:
  - `Resume` 模型可用
- 应补齐:
  - fillable / guarded
  - casts
  - 用户关联
  - 技能关联
  - 工作经历关联
  - 头像/附件读取辅助（如需要）
- 验收标准:
  - 模型能稳定支撑简历详情读取

### TASK-ID: RESUME-MODEL-SKILL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 简历模型与关联
- 目标: 创建 `ResumeSkill` 模型
- 依赖:
  - RESUME-DB-SKILL-001
- 输入:
  - resume_skills 表结构
- 输出:
  - 技能模型可用
- 验收标准:
  - 支持按 resume_id 查询技能列表
  - 支持 level/category 快照输出

### TASK-ID: RESUME-MODEL-EXP-001
- 状态: blocked
- 优先级: P0
- 所属子域: 简历模型与关联
- 目标: 创建 `ResumeWorkExperience` 模型
- 依赖:
  - RESUME-DB-EXP-001
- 输入:
  - 工作经历表结构
- 输出:
  - 工作经历模型可用
- 验收标准:
  - 支持按 resume_id 查询并排序

### TASK-ID: RESUME-MODEL-USERREL-001
- 状态: blocked
- 优先级: P1
- 所属子域: 简历模型与关联
- 目标: 在 `User` 模型中补齐简历关联
- 依赖:
  - AUTH-MODEL-USER-001
  - RESUME-MODEL-BASE-001
- 输入:
  - User / Resume 模型
- 输出:
  - 用户与简历一对一关系可用
- 验收标准:
  - 可通过 `$user->resume` 获取主简历

### 3.5.3 简历基础资料接口

### TASK-ID: RESUME-REQ-UPSERT-001
- 状态: blocked
- 优先级: P0
- 所属子域: 简历基础资料接口
- 目标: 编写简历创建/更新参数校验 Request
- 依赖:
  - RESUME-DB-BASE-001
- 输入:
  - 页面设计与简历字段需求
- 输出:
  - 简历基础资料校验类可用
- 建议校验字段至少包含:
  - real_name
  - gender
  - age
  - work_years
  - education
  - current_city
  - expected_city
  - expected_salary
  - self_introduction
- 验收标准:
  - 参数错误返回统一结构
  - 年龄、年限、枚举字段校验明确

### TASK-ID: RESUME-SVC-UPSERT-001
- 状态: blocked
- 优先级: P0
- 所属子域: 简历基础资料接口
- 目标: 实现简历创建/更新服务
- 依赖:
  - RESUME-REQ-UPSERT-001
  - RESUME-MODEL-BASE-001
- 输入:
  - 当前登录用户
  - 简历基础资料参数
- 输出:
  - 简历保存逻辑可用
- 业务要求至少包括:
  - 用户无简历时创建
  - 用户已有简历时更新
  - 保持 user_id 唯一归属
  - 保存后触发完整度重算
- 验收标准:
  - 能稳定创建和更新简历
  - completion_percent 可同步更新或异步重算

### TASK-ID: RESUME-RES-DETAIL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 简历基础资料接口
- 目标: 实现简历详情资源输出类
- 依赖:
  - RESUME-MODEL-BASE-001
  - RESUME-MODEL-SKILL-001
  - RESUME-MODEL-EXP-001
  - BOOT-FILE-RES-001
- 输入:
  - 简历详情展示字段
- 输出:
  - `ResumeDetailResource` 可用
- 验收标准:
  - 能统一输出基础资料、技能列表、工作经历、完整度
  - 用户头像等文件字段按 File Resource 输出

### TASK-ID: RESUME-API-DETAIL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 简历基础资料接口
- 目标: 实现当前用户简历详情接口 `GET /api/resume/detail`
- 依赖:
  - RESUME-RES-DETAIL-001
  - BOOT-AUTH-002
- 输入:
  - 当前登录用户
- 输出:
  - 当前用户简历详情接口可用
- 验收标准:
  - 未创建简历时返回明确空结构或初始化结构
  - 已创建时返回完整简历详情

### TASK-ID: RESUME-API-UPSERT-001
- 状态: blocked
- 优先级: P0
- 所属子域: 简历基础资料接口
- 目标: 实现简历保存接口 `POST /api/resume/save` 或等价接口
- 依赖:
  - RESUME-SVC-UPSERT-001
  - BOOT-AUTH-002
- 输入:
  - 简历基础资料参数
- 输出:
  - 简历创建/更新接口可用
- 验收标准:
  - 用户可保存简历主资料
  - 响应结构统一
- 非目标:
  - 本任务不负责技能/工作经历批量保存

### 3.5.4 技能与工种结构

### TASK-ID: RESUME-REQ-SKILL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 技能与工种结构
- 目标: 编写简历技能保存参数校验 Request
- 依赖:
  - RESUME-DB-SKILL-001
- 输入:
  - 工种/技能数据结构
- 输出:
  - 技能保存校验类可用
- 建议校验至少包含:
  - skills 数组
  - category_id
  - level
  - years
- 验收标准:
  - skills 数组结构校验明确
  - 同工种同级别重复项可提前拦截

### TASK-ID: RESUME-SVC-SKILL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 技能与工种结构
- 目标: 实现简历技能保存服务
- 依赖:
  - RESUME-REQ-SKILL-001
  - RESUME-MODEL-SKILL-001
- 输入:
  - 当前用户简历
  - 技能数组
- 输出:
  - 技能保存逻辑可用
- 业务要求至少包括:
  - 覆盖式保存或差量保存策略明确
  - 保存 category / level 快照
  - 保存后触发完整度重算
- 验收标准:
  - 技能列表可稳定新增/更新/删除

### TASK-ID: RESUME-API-SKILL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 技能与工种结构
- 目标: 实现简历技能保存接口
- 依赖:
  - RESUME-SVC-SKILL-001
  - BOOT-AUTH-002
- 输入:
  - 技能数组
- 输出:
  - 技能保存接口可用
- 验收标准:
  - 前端可单独保存技能模块
  - 响应结构统一

### 3.5.5 工作经历结构

### TASK-ID: RESUME-REQ-EXP-001
- 状态: blocked
- 优先级: P0
- 所属子域: 工作经历结构
- 目标: 编写工作经历保存参数校验 Request
- 依赖:
  - RESUME-DB-EXP-001
- 输入:
  - 工作经历表结构
- 输出:
  - 工作经历保存校验类可用
- 验收标准:
  - company_name / start_date / end_date / is_current 等字段校验明确

### TASK-ID: RESUME-SVC-EXP-001
- 状态: blocked
- 优先级: P0
- 所属子域: 工作经历结构
- 目标: 实现工作经历保存服务
- 依赖:
  - RESUME-REQ-EXP-001
  - RESUME-MODEL-EXP-001
- 输入:
  - 当前用户简历
  - 工作经历数组
- 输出:
  - 工作经历保存逻辑可用
- 业务要求至少包括:
  - 支持新增/更新/删除
  - 当前在职与结束时间关系校验
  - 保存后触发完整度重算
- 验收标准:
  - 工作经历数据可稳定维护

### TASK-ID: RESUME-API-EXP-001
- 状态: blocked
- 优先级: P0
- 所属子域: 工作经历结构
- 目标: 实现工作经历保存接口
- 依赖:
  - RESUME-SVC-EXP-001
  - BOOT-AUTH-002
- 输入:
  - 工作经历数组
- 输出:
  - 工作经历保存接口可用
- 验收标准:
  - 前端可独立维护工作经历模块
  - 响应结构统一

### 3.5.6 简历完整度判定

### TASK-ID: RESUME-SVC-COMPLETE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 简历完整度判定
- 目标: 实现简历完整度计算服务
- 依赖:
  - RESUME-MODEL-BASE-001
  - RESUME-MODEL-SKILL-001
  - RESUME-MODEL-EXP-001
- 输入:
  - 简历主资料
  - 技能列表
  - 工作经历列表
- 输出:
  - 完整度计算服务可用
- 规则建议至少明确:
  - 基础资料占比
  - 技能占比
  - 工作经历占比
  - 自我介绍占比
- 验收标准:
  - completion_percent 计算稳定、可重复
  - 可复用于保存后自动刷新

### TASK-ID: RESUME-SVC-ELIGIBLE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 简历完整度判定
- 目标: 实现报名可用简历判定服务
- 依赖:
  - RESUME-SVC-COMPLETE-001
- 输入:
  - 当前用户简历
- 输出:
  - 报名模块可复用的“简历是否满足最低要求”判断能力
- 验收标准:
  - 能返回布尔结果
  - 能返回缺失项提示或错误原因
- 非目标:
  - 本任务不直接实现报名接口

### TASK-ID: RESUME-SVC-RECALC-001
- 状态: blocked
- 优先级: P1
- 所属子域: 简历完整度判定
- 目标: 实现简历完整度重算统一入口
- 依赖:
  - RESUME-SVC-COMPLETE-001
- 输入:
  - resume_id
- 输出:
  - 任意简历数据更新后可统一触发重算
- 验收标准:
  - 主资料、技能、工作经历保存后可复用本入口

### 3.5.7 雇主侧复用读取能力

### TASK-ID: RESUME-SVC-READ-EMPLOYER-001
- 状态: blocked
- 优先级: P1
- 所属子域: 雇主侧复用读取能力
- 目标: 实现雇主侧查看求职者简历读取服务
- 依赖:
  - RESUME-RES-DETAIL-001
  - RESUME-MODEL-USERREL-001
- 输入:
  - seeker user_id
- 输出:
  - 雇主侧可复用的简历读取服务
- 验收标准:
  - 可供报名管理、人才库查看复用
  - 不重复在多个模块散写查询逻辑
- 非目标:
  - 本任务不暴露雇主端正式接口

### TASK-ID: RESUME-RES-SUMMARY-001
- 状态: blocked
- 优先级: P1
- 所属子域: 雇主侧复用读取能力
- 目标: 实现简历摘要资源输出类
- 依赖:
  - RESUME-MODEL-BASE-001
  - BOOT-FILE-RES-001
- 输入:
  - 报名管理列表 / 用户详情摘要需求
- 输出:
  - `ResumeSummaryResource` 可用
- 验收标准:
  - 输出年龄、工龄、教育、主要技能摘要
  - 便于报名列表与用户详情页复用

### 3.5.8 简历测试

### TASK-ID: RESUME-TEST-DETAIL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 简历测试
- 目标: 编写简历详情接口测试
- 依赖:
  - RESUME-API-DETAIL-001
  - BOOT-TEST-002
- 输入:
  - 简历详情接口
- 输出:
  - Feature Test
- 验收标准:
  - 无简历时返回结构正确
  - 有简历时返回详情正确

### TASK-ID: RESUME-TEST-UPSERT-001
- 状态: blocked
- 优先级: P0
- 所属子域: 简历测试
- 目标: 编写简历保存接口测试
- 依赖:
  - RESUME-API-UPSERT-001
  - BOOT-TEST-002
- 输入:
  - 简历保存接口
- 输出:
  - Feature Test
- 验收标准:
  - 首次创建成功
  - 再次保存为更新
  - 参数错误返回符合规范

### TASK-ID: RESUME-TEST-SKILL-001
- 状态: blocked
- 优先级: P1
- 所属子域: 简历测试
- 目标: 编写技能保存接口测试
- 依赖:
  - RESUME-API-SKILL-001
  - BOOT-TEST-002
- 输入:
  - 技能保存接口
- 输出:
  - Feature Test
- 验收标准:
  - 技能列表可保存
  - 重复技能项被拦截或规范处理

### TASK-ID: RESUME-TEST-EXP-001
- 状态: blocked
- 优先级: P1
- 所属子域: 简历测试
- 目标: 编写工作经历保存接口测试
- 依赖:
  - RESUME-API-EXP-001
  - BOOT-TEST-002
- 输入:
  - 工作经历保存接口
- 输出:
  - Feature Test
- 验收标准:
  - 工作经历可新增/更新
  - 日期逻辑错误时校验失败

### TASK-ID: RESUME-TEST-COMPLETE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 简历测试
- 目标: 编写简历完整度计算测试
- 依赖:
  - RESUME-SVC-COMPLETE-001
- 输入:
  - 完整度计算服务
- 输出:
  - Unit Test
- 验收标准:
  - 不同字段覆盖率下完整度计算正确

### TASK-ID: RESUME-TEST-ELIGIBLE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 简历测试
- 目标: 编写报名可用简历判定测试
- 依赖:
  - RESUME-SVC-ELIGIBLE-001
- 输入:
  - 简历判定服务
- 输出:
  - Unit Test
- 验收标准:
  - 简历缺关键字段时判定失败
  - 满足最低要求时判定成功

## 3.6 本章推荐执行顺序

### 第一批（P0）
1. RESUME-DB-BASE-001
2. RESUME-DB-SKILL-001
3. RESUME-DB-EXP-001
4. RESUME-MODEL-BASE-001
5. RESUME-MODEL-SKILL-001
6. RESUME-MODEL-EXP-001
7. RESUME-MODEL-USERREL-001
8. RESUME-REQ-UPSERT-001
9. RESUME-SVC-COMPLETE-001
10. RESUME-SVC-UPSERT-001
11. RESUME-RES-DETAIL-001
12. RESUME-API-UPSERT-001
13. RESUME-API-DETAIL-001
14. RESUME-REQ-SKILL-001
15. RESUME-SVC-SKILL-001
16. RESUME-API-SKILL-001
17. RESUME-REQ-EXP-001
18. RESUME-SVC-EXP-001
19. RESUME-API-EXP-001
20. RESUME-SVC-ELIGIBLE-001
21. RESUME-TEST-DETAIL-001
22. RESUME-TEST-UPSERT-001
23. RESUME-TEST-COMPLETE-001
24. RESUME-TEST-ELIGIBLE-001

### 第二批（P1）
1. RESUME-DB-ATTACH-001
2. RESUME-SVC-RECALC-001
3. RESUME-SVC-READ-EMPLOYER-001
4. RESUME-RES-SUMMARY-001
5. RESUME-TEST-SKILL-001
6. RESUME-TEST-EXP-001

## 3.7 本章完成后的下一个章节入口
当第 3 章完成后，最合理进入：
- 第4章：企业认证

原因：
- 雇主发布职位依赖企业认证
- 求职者报名依赖简历与职位链路
- 因此简历完成后，优先把雇主链路补齐最稳

---

# 第4章：企业认证

> 目标：建立雇主/企业/劳务主体的认证提交、认证资料存储、审核状态流转、历史快照、私有文件访问与认证查询基础能力。
> 本章只处理企业认证域及其直接支撑能力，不直接实现职位发布、后台企业审核决策界面、黑名单联动等更大业务链路。

## 4.1 本章定位

### 本章要解决的问题
- 建立企业认证主体表
- 建立认证历史快照表
- 支持企业/劳务/个人雇主提交认证资料
- 支持当前认证详情查询
- 支持重新提交认证资料
- 支持 private 文件关联与签名访问基础约束
- 支持认证状态与用户认证状态联动
- 为后台审核和职位发布资格判断提供认证数据基础

### 本章不做的内容
- 后台管理员审核接口的完整实现
- 职位发布逻辑
- 黑名单或举报联动
- 企业统计与后台运营图表
- 置顶申请

## 4.2 本章完成标准
当第 4 章完成时，应满足：
1. 企业认证主表与历史表可用
2. 雇主可提交认证资料
3. 雇主可查看当前认证详情
4. 被拒后可重新提交认证
5. 认证状态可与 users 表中的认证状态联动
6. 认证资料文件遵循 private 文件访问规范
7. 后续后台审核可以直接基于本章数据结构实现
8. 关键认证接口具备基础测试覆盖

## 4.3 本章依赖关系
### 上游依赖
- 第1章：项目骨架与基础设施
- 第2章：用户与认证

### 下游依赖
- 职位系统
- 后台企业认证审核
- 用户详情中的企业信息摘要

## 4.4 本章任务总览
1. 企业认证数据结构与表设计
2. 企业认证模型与关联
3. 认证资料提交接口
4. 当前认证详情接口
5. 重新提交认证逻辑
6. 认证状态同步与快照
7. private 文件访问约束
8. 企业认证测试

## 4.5 可直接执行任务清单

### 4.5.1 企业认证数据结构与表设计

### TASK-ID: COMPANY-DB-BASE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 企业认证数据结构与表设计
- 目标: 创建 `companies` 表 migration
- 依赖:
  - BOOT-DB-001
  - AUTH-DB-USER-001
  - BOOT-DB-FILES-001
- 输入:
  - 企业认证接口文档
  - 企业认证状态机
  - 文件资源规范
- 输出:
  - `companies` 表可支撑企业/劳务/个人雇主认证主体
- 建议字段至少包含:
  - id
  - user_id
  - company_name
  - unified_social_code
  - company_type
  - business_license_file_id
  - id_card_front_file_id
  - id_card_back_file_id
  - legal_person_name
  - legal_person_id_card
  - legal_person_id_card_front_file_id
  - legal_person_id_card_back_file_id
  - contact_person
  - contact_phone
  - audit_status
  - audit_operator_id
  - audit_time
  - audit_reason
  - audit_version
  - reapply_count
  - certified_at
  - created_at / updated_at
- 索引建议:
  - user_id 唯一索引
  - company_type 普通索引
  - audit_status 普通索引
  - unified_social_code 普通索引
- 验收标准:
  - 一名雇主对应一个当前企业认证主体
  - 能支撑企业/劳务/个人雇主三类主体
  - 能记录当前审核状态与最近审核结果
- 非目标:
  - 不在本任务中创建历史快照表

### TASK-ID: COMPANY-DB-HISTORY-001
- 状态: blocked
- 优先级: P0
- 所属子域: 企业认证数据结构与表设计
- 目标: 创建 `company_audit_histories` 表 migration
- 依赖:
  - COMPANY-DB-BASE-001
- 输入:
  - 认证历史快照要求
- 输出:
  - 企业认证历史表可用
- 建议字段至少包含:
  - id
  - company_id
  - version
  - submitted_at
  - audit_status
  - audit_reason
  - audit_time
  - materials_snapshot
  - created_at / updated_at
- 索引建议:
  - company_id 普通索引
  - company_id + version 唯一索引
  - audit_status 普通索引
- 验收标准:
  - 能保存每次提交/审核的历史快照
  - 能支撑后台查看历史材料

### TASK-ID: COMPANY-DB-TYPEENUM-001
- 状态: blocked
- 优先级: P1
- 所属子域: 企业认证数据结构与表设计
- 目标: 明确企业主体类型与认证状态枚举
- 依赖:
  - COMPANY-DB-BASE-001
- 输入:
  - 文档中的主体类型和审核状态
- 输出:
  - 数据层枚举口径明确
- 建议至少包含:
  - company_type: enterprise / labor / personal_employer（如文档口径需要）
  - audit_status: not_submitted / pending / approved / rejected 或数据库映射枚举
- 验收标准:
  - 状态口径统一，不在不同模块散落魔法值

### 4.5.2 企业认证模型与关联

### TASK-ID: COMPANY-MODEL-BASE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 企业认证模型与关联
- 目标: 创建 `Company` 模型并补齐基础属性
- 依赖:
  - COMPANY-DB-BASE-001
- 输入:
  - companies 表结构
- 输出:
  - `Company` 模型可用
- 应补齐:
  - fillable / guarded
  - casts
  - 用户关联
  - 文件关联辅助
  - 认证状态辅助方法
- 验收标准:
  - 可稳定读取当前认证资料
  - 能为提交、详情、审核模块复用

### TASK-ID: COMPANY-MODEL-HISTORY-001
- 状态: blocked
- 优先级: P0
- 所属子域: 企业认证模型与关联
- 目标: 创建 `CompanyAuditHistory` 模型
- 依赖:
  - COMPANY-DB-HISTORY-001
- 输入:
  - company_audit_histories 表结构
- 输出:
  - 历史模型可用
- 验收标准:
  - 支持按 company_id + version 查询
  - 支持历史材料快照读取

### TASK-ID: COMPANY-MODEL-USERREL-001
- 状态: blocked
- 优先级: P1
- 所属子域: 企业认证模型与关联
- 目标: 在 `User` 模型中补齐企业认证关联
- 依赖:
  - AUTH-MODEL-USER-001
  - COMPANY-MODEL-BASE-001
- 输入:
  - User / Company 模型
- 输出:
  - 用户与企业认证主体一对一关系可用
- 验收标准:
  - 可通过 `$user->company` 或等价关系读取当前认证主体

### 4.5.3 认证资料提交接口

### TASK-ID: COMPANY-REQ-SUBMIT-001
- 状态: blocked
- 优先级: P0
- 所属子域: 认证资料提交接口
- 目标: 编写企业认证提交参数校验 Request
- 依赖:
  - COMPANY-DB-BASE-001
  - BOOT-DB-FILES-001
- 输入:
  - 提交认证接口文档
  - company_type 差异字段要求
- 输出:
  - 企业认证提交校验类可用
- 建议校验至少包含:
  - company_name
  - unified_social_code
  - company_type
  - business_license_file_id
  - id_card_front_file_id / id_card_back_file_id
  - legal_person_name
  - legal_person_id_card
  - legal_person_id_card_front_file_id
  - legal_person_id_card_back_file_id
  - contact_person
  - contact_phone
- 验收标准:
  - 不同主体类型的必填字段校验明确
  - 文件字段按 file_id 校验
  - 身份证/统一社会信用代码格式有基础校验

### TASK-ID: COMPANY-SVC-FILECHECK-001
- 状态: blocked
- 优先级: P0
- 所属子域: 认证资料提交接口
- 目标: 实现认证资料 file_id 合法性校验服务
- 依赖:
  - BOOT-FILE-SVC-001
  - BOOT-DB-FILES-001
- 输入:
  - 提交认证时传入的 file_id 列表
- 输出:
  - 认证资料文件校验服务可用
- 业务要求至少包括:
  - file_id 存在性校验
  - private/public 合法性约束
  - 上传者归属校验
  - type 用途校验（如 cert、id_card 等）
- 验收标准:
  - 非法 file_id 不可进入认证流程

### TASK-ID: COMPANY-SVC-SUBMIT-001
- 状态: blocked
- 优先级: P0
- 所属子域: 认证资料提交接口
- 目标: 实现企业认证提交服务
- 依赖:
  - COMPANY-REQ-SUBMIT-001
  - COMPANY-SVC-FILECHECK-001
  - COMPANY-MODEL-BASE-001
  - COMPANY-SVC-SNAPSHOT-001
  - COMPANY-SVC-USERSTATUS-001
- 输入:
  - 当前登录用户
  - 认证资料参数
- 输出:
  - 企业认证提交逻辑可用
- 业务要求至少包括:
  - 首次提交创建 company 主体
  - 设置 audit_status 为 pending
  - audit_version 递增
  - reapply_count 在重提时累加
  - 同步 users 表认证状态为审核中
  - 写入历史快照
- 验收标准:
  - 提交后可形成当前主体 + 历史快照
  - 用户认证状态同步正确
- 非目标:
  - 不在本任务中实现管理员审核决策

### TASK-ID: COMPANY-API-SUBMIT-001
- 状态: blocked
- 优先级: P0
- 所属子域: 认证资料提交接口
- 目标: 实现企业认证提交接口 `POST /api/company/submit-certification` 或等价接口
- 依赖:
  - COMPANY-SVC-SUBMIT-001
  - BOOT-AUTH-002
- 输入:
  - 当前登录雇主
  - 认证资料参数
- 输出:
  - 企业认证提交接口可用
- 验收标准:
  - 提交成功后返回统一结构
  - 返回当前认证状态
  - 错误响应统一

### 4.5.4 当前认证详情接口

### TASK-ID: COMPANY-RES-DETAIL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 当前认证详情接口
- 目标: 实现企业认证详情资源输出类
- 依赖:
  - COMPANY-MODEL-BASE-001
  - COMPANY-MODEL-HISTORY-001
  - BOOT-FILE-RES-001
- 输入:
  - 企业认证详情返回字段要求
- 输出:
  - `CompanyCertificationDetailResource` 可用
- 验收标准:
  - 当前主体字段输出完整
  - 文件字段按 File Resource 输出
  - 历史记录可组装输出

### TASK-ID: COMPANY-API-DETAIL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 当前认证详情接口
- 目标: 实现当前用户认证详情接口
- 依赖:
  - COMPANY-RES-DETAIL-001
  - BOOT-AUTH-002
- 输入:
  - 当前登录雇主
- 输出:
  - 当前认证详情接口可用
- 验收标准:
  - 未提交时返回明确空结构或默认结构
  - 已提交时返回当前资料与审核状态
  - 如有历史，返回 history 数据

### TASK-ID: COMPANY-RES-SUMMARY-001
- 状态: blocked
- 优先级: P1
- 所属子域: 当前认证详情接口
- 目标: 实现企业认证摘要资源输出类
- 依赖:
  - COMPANY-MODEL-BASE-001
- 输入:
  - 用户详情/职位详情中企业信息摘要要求
- 输出:
  - `CompanyCertificationSummaryResource` 可用
- 验收标准:
  - 输出 company_name / company_type / audit_status / certified_at 等摘要信息

### 4.5.5 重新提交认证逻辑

### TASK-ID: COMPANY-SVC-REAPPLY-001
- 状态: blocked
- 优先级: P0
- 所属子域: 重新提交认证逻辑
- 目标: 明确并实现被拒后重新提交认证逻辑
- 依赖:
  - COMPANY-SVC-SUBMIT-001
- 输入:
  - 当前 company 审核状态
  - 新提交资料
- 输出:
  - 被拒后的重提流程可用
- 业务要求至少包括:
  - 仅允许符合规则的状态重提
  - 重新提交后状态置为 pending
  - version 递增
  - reapply_count 累加
  - 历史快照保留上次材料
- 验收标准:
  - 重提不会覆盖掉历史记录
  - 当前主体更新与历史快照并存

### TASK-ID: COMPANY-API-REAPPLY-001
- 状态: blocked
- 优先级: P1
- 所属子域: 重新提交认证逻辑
- 目标: 如接口语义需要，明确重提是否复用提交接口或独立接口
- 依赖:
  - COMPANY-SVC-REAPPLY-001
  - COMPANY-API-SUBMIT-001
- 输入:
  - API 文档口径
- 输出:
  - 重提接口方案明确
- 验收标准:
  - 若复用提交接口，文档与实现口径一致
  - 若独立接口，给出明确路由与验收

### 4.5.6 认证状态同步与快照

### TASK-ID: COMPANY-SVC-USERSTATUS-001
- 状态: blocked
- 优先级: P0
- 所属子域: 认证状态同步与快照
- 目标: 实现企业认证状态与 users 表认证状态同步服务
- 依赖:
  - AUTH-MODEL-USER-001
  - COMPANY-MODEL-BASE-001
- 输入:
  - 当前认证状态变化
- 输出:
  - 用户认证状态同步逻辑可用
- 业务要求至少包括:
  - 提交时 -> 审核中
  - 审核通过时 -> 已认证
  - 审核拒绝时 -> 被拒绝
- 验收标准:
  - users.is_certified 或等价字段与 company 当前审核状态口径一致

### TASK-ID: COMPANY-SVC-SNAPSHOT-001
- 状态: blocked
- 优先级: P0
- 所属子域: 认证状态同步与快照
- 目标: 实现企业认证资料快照服务
- 依赖:
  - COMPANY-MODEL-HISTORY-001
- 输入:
  - 当前认证主体资料
- 输出:
  - materials_snapshot 统一生成逻辑可用
- 验收标准:
  - 可在每次提交/审核时稳定生成快照
  - 快照字段能支撑后台查看历史材料

### TASK-ID: COMPANY-SVC-CERTCHECK-001
- 状态: blocked
- 优先级: P0
- 所属子域: 认证状态同步与快照
- 目标: 实现“用户是否具备可发布职位认证资格”判断服务
- 依赖:
  - COMPANY-MODEL-BASE-001
  - COMPANY-SVC-USERSTATUS-001
- 输入:
  - 当前用户
- 输出:
  - 职位系统可复用的认证资格判断能力
- 验收标准:
  - 能返回是否认证通过
  - 能返回失败原因或当前状态
- 非目标:
  - 本任务不实现职位发布接口

### 4.5.7 private 文件访问约束

### TASK-ID: COMPANY-SVC-PRIVFILE-001
- 状态: blocked
- 优先级: P0
- 所属子域: private 文件访问约束
- 目标: 实现企业认证资料 private 文件输出约束
- 依赖:
  - BOOT-FILE-SVC-001
  - COMPANY-RES-DETAIL-001
- 输入:
  - 企业认证资料文件字段
- 输出:
  - 当前用户查看自己认证资料时的文件输出规则明确
- 业务要求至少包括:
  - private 文件返回 temporary_url
  - 不暴露原始存储路径
  - 自己查看自己的资料允许访问
- 验收标准:
  - 输出结构符合 File Resource 规范
  - private 文件不直接暴露裸路径

### TASK-ID: COMPANY-SVC-FILEMASK-001
- 状态: blocked
- 优先级: P1
- 所属子域: private 文件访问约束
- 目标: 实现敏感身份字段的脱敏输出策略
- 依赖:
  - COMPANY-RES-DETAIL-001
- 输入:
  - 认证详情返回中的身份证号等字段
- 输出:
  - 对外输出脱敏规则可用
- 验收标准:
  - 普通详情接口不返回完整身份证号明文
  - 与后台高权限查看场景口径分离

### 4.5.8 企业认证测试

### TASK-ID: COMPANY-TEST-SUBMIT-001
- 状态: blocked
- 优先级: P0
- 所属子域: 企业认证测试
- 目标: 编写企业认证提交接口测试
- 依赖:
  - COMPANY-API-SUBMIT-001
  - BOOT-TEST-002
- 输入:
  - 企业认证提交接口
- 输出:
  - Feature Test
- 验收标准:
  - 正常提交成功
  - 缺关键资料提交失败
  - 非法 file_id 提交失败

### TASK-ID: COMPANY-TEST-DETAIL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 企业认证测试
- 目标: 编写企业认证详情接口测试
- 依赖:
  - COMPANY-API-DETAIL-001
  - BOOT-TEST-002
- 输入:
  - 企业认证详情接口
- 输出:
  - Feature Test
- 验收标准:
  - 未提交时返回正确结构
  - 已提交时返回当前资料与状态
  - private 文件按资源结构输出

### TASK-ID: COMPANY-TEST-REAPPLY-001
- 状态: blocked
- 优先级: P1
- 所属子域: 企业认证测试
- 目标: 编写企业认证重提测试
- 依赖:
  - COMPANY-SVC-REAPPLY-001
  - COMPANY-API-SUBMIT-001
- 输入:
  - 重提逻辑
- 输出:
  - Feature / Unit Test
- 验收标准:
  - 被拒状态可重提
  - 重提后 version 递增
  - 历史快照保留

### TASK-ID: COMPANY-TEST-STATUSSYNC-001
- 状态: blocked
- 优先级: P0
- 所属子域: 企业认证测试
- 目标: 编写用户认证状态同步测试
- 依赖:
  - COMPANY-SVC-USERSTATUS-001
- 输入:
  - 状态同步服务
- 输出:
  - Unit Test
- 验收标准:
  - 提交/通过/拒绝三种状态同步正确

### TASK-ID: COMPANY-TEST-CERTCHECK-001
- 状态: blocked
- 优先级: P0
- 所属子域: 企业认证测试
- 目标: 编写认证资格判断服务测试
- 依赖:
  - COMPANY-SVC-CERTCHECK-001
- 输入:
  - 认证资格判断服务
- 输出:
  - Unit Test
- 验收标准:
  - 未认证、审核中、已通过、被拒绝场景判断正确

## 4.6 本章推荐执行顺序

### 第一批（P0）
1. COMPANY-DB-BASE-001
2. COMPANY-DB-HISTORY-001
3. COMPANY-MODEL-BASE-001
4. COMPANY-MODEL-HISTORY-001
5. COMPANY-REQ-SUBMIT-001
6. COMPANY-SVC-FILECHECK-001
7. COMPANY-SVC-SNAPSHOT-001
8. COMPANY-SVC-USERSTATUS-001
9. COMPANY-SVC-SUBMIT-001
10. COMPANY-API-SUBMIT-001
11. COMPANY-RES-DETAIL-001
12. COMPANY-API-DETAIL-001
13. COMPANY-SVC-REAPPLY-001
14. COMPANY-SVC-CERTCHECK-001
15. COMPANY-SVC-PRIVFILE-001
16. COMPANY-TEST-SUBMIT-001
17. COMPANY-TEST-DETAIL-001
18. COMPANY-TEST-STATUSSYNC-001
19. COMPANY-TEST-CERTCHECK-001

### 第二批（P1）
1. COMPANY-DB-TYPEENUM-001
2. COMPANY-MODEL-USERREL-001
3. COMPANY-RES-SUMMARY-001
4. COMPANY-API-REAPPLY-001
5. COMPANY-SVC-FILEMASK-001
6. COMPANY-TEST-REAPPLY-001

## 4.7 本章完成后的下一个章节入口
当第 4 章完成后，最合理进入：
- 第5章：职位系统

原因：
- 职位发布依赖企业认证通过
- 雇主闭环在完成认证后自然进入职位发布与职位管理

---

# 第5章：职位系统

> 目标：建立职位主数据结构、职位列表与详情、发布与草稿、编辑与删除、刷新与状态切换、标记招满、置顶申请、职位审核判定、职位统计等完整职位域基础能力。
> 本章只处理职位域及其直接依赖能力，不直接实现报名创建与报名审核，不直接实现后台职位审核接口本身，但会把后台审核所需的判定与数据结构准备好。

## 5.1 本章定位

### 本章要解决的问题
- 建立职位主表与职位级别明细表
- 建立职位浏览记录表
- 建立职位置顶申请表
- 支持职位列表与详情查询
- 支持发布职位、保存草稿、编辑职位、删除职位、复制职位
- 支持暂停/恢复、刷新、标记招满
- 实现职位审核判定规则
- 支持雇主自己的职位管理列表
- 支持职位统计与雇主整体统计基础能力
- 为后续报名系统和后台职位审核提供稳定数据基础

### 本章不做的内容
- 报名创建与取消
- 报名审核
- 私聊逻辑
- 后台职位审核接口本身
- 评论模块本身

## 5.2 本章完成标准
当第 5 章完成时，应满足：
1. `jobs`、`job_levels`、`job_views`、`job_top_applications` 等底表可用
2. 前台职位列表与详情接口可用
3. 雇主可保存草稿、发布、编辑、删除、刷新、暂停/恢复职位
4. 系统能根据文档规则判定职位进入 `draft/pending/active` 等状态
5. 职位置顶申请与申请状态查询能力可用
6. 雇主可查看自己的职位管理列表
7. 职位统计能力具备基础实现
8. 后续报名系统可以直接依赖职位数据结构和状态机
9. 关键职位接口具备基础测试覆盖

## 5.3 本章依赖关系
### 上游依赖
- 第1章：项目骨架与基础设施
- 第2章：用户与认证
- 第4章：企业认证

### 下游依赖
- 报名系统
- 评论系统
- 举报系统
- 后台职位审核
- 收藏与浏览历史
- 职位相关推荐

## 5.4 本章任务总览
1. 职位数据结构与表设计
2. 职位模型与关联
3. 职位列表与详情
4. 职位发布与草稿
5. 职位编辑、删除、复制
6. 刷新、暂停/恢复、标记招满
7. 置顶申请与申请状态
8. 职位审核判定与编号生成
9. 浏览记录与统计
10. 职位测试

## 5.5 可直接执行任务清单

### 5.5.1 职位数据结构与表设计

### TASK-ID: JOB-DB-BASE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位数据结构与表设计
- 目标: 创建 `jobs` 表 migration
- 依赖:
  - BOOT-DB-001
  - AUTH-DB-USER-001
  - COMPANY-DB-BASE-001
- 输入:
  - 职位接口文档
  - 职位状态机
- 输出:
  - `jobs` 表可支撑职位主资料与状态流转
- 建议字段至少包含:
  - id
  - user_id
  - company_id
  - job_no
  - title
  - work_location
  - work_period
  - work_time
  - settlement_type
  - accommodation
  - meals
  - requirements
  - education
  - age_range
  - tags_json
  - contact_person
  - contact_phone
  - status
  - audit_status
  - audit_reason
  - close_reason
  - is_top
  - top_expire_time
  - views_count
  - comments_count
  - refreshed_at
  - published_at
  - deleted_at
  - created_at / updated_at
- 索引建议:
  - job_no 唯一索引
  - user_id 普通索引
  - company_id 普通索引
  - status 普通索引
  - audit_status 普通索引
  - is_top 普通索引
  - refreshed_at 普通索引
  - published_at 普通索引
- 验收标准:
  - 可支撑草稿、待审核、上线、暂停、关闭、删除等状态
  - 可支撑统计与管理列表
- 非目标:
  - 不在本任务中创建职位级别明细表

### TASK-ID: JOB-DB-LEVEL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位数据结构与表设计
- 目标: 创建 `job_levels` 表 migration
- 依赖:
  - JOB-DB-BASE-001
- 输入:
  - 职位多工种、多级别结构要求
- 输出:
  - `job_levels` 表可支撑职位工种/级别明细
- 建议字段至少包含:
  - id
  - job_id
  - category_id
  - category_name_snapshot
  - level
  - level_name_snapshot
  - salary
  - recruit_count
  - applied_count
  - is_enabled
  - sort_order
  - created_at / updated_at
- 索引建议:
  - job_id 普通索引
  - category_id 普通索引
  - level 普通索引
  - job_id + category_id + level 唯一索引
- 验收标准:
  - 同一职位下同工种同级别不重复
  - 能为报名按 job_id + category_id + level 定位明细

### TASK-ID: JOB-DB-IMAGE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 职位数据结构与表设计
- 目标: 明确职位图片存储策略并完成落地
- 依赖:
  - JOB-DB-BASE-001
  - BOOT-DB-FILES-001
- 输入:
  - 职位图片 File Resource 规范
- 输出:
  - 职位图片与 file_id 的持久化方案明确
- 实现建议:
  - 使用 `job_images` 明细表，避免在 jobs 表内用 JSON 混存多图片 file_id
- 验收标准:
  - 多图场景可稳定输出顺序
  - 保持统一 file_id 模型

### TASK-ID: JOB-DB-VIEW-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位数据结构与表设计
- 目标: 创建 `job_views` 表 migration
- 依赖:
  - JOB-DB-BASE-001
- 输入:
  - 浏览记录与防刷规则
- 输出:
  - 职位浏览记录表可用
- 建议字段至少包含:
  - id
  - job_id
  - viewer_user_id
  - viewer_key
  - ip_address
  - viewed_at
  - created_at
- 索引建议:
  - job_id 普通索引
  - viewer_user_id 普通索引
  - viewer_key 普通索引
  - viewed_at 普通索引
- 验收标准:
  - 可支撑浏览历史、浏览统计、防刷去重辅助

### TASK-ID: JOB-DB-TOPAPP-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位数据结构与表设计
- 目标: 创建 `job_top_applications` 表 migration
- 依赖:
  - JOB-DB-BASE-001
- 输入:
  - 置顶申请接口文档
  - 置顶申请状态机
- 输出:
  - 职位置顶申请表可用
- 建议字段至少包含:
  - id
  - job_id
  - user_id
  - apply_days
  - status
  - reason
  - handled_by
  - handled_at
  - created_at / updated_at
- 索引建议:
  - job_id 普通索引
  - user_id 普通索引
  - status 普通索引
  - job_id + status 组合索引
- 验收标准:
  - 可支撑待审核/通过/拒绝/过期/取消等状态

### 5.5.2 职位模型与关联

### TASK-ID: JOB-MODEL-BASE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位模型与关联
- 目标: 创建 `Job` 模型并补齐基础属性
- 依赖:
  - JOB-DB-BASE-001
- 输入:
  - jobs 表结构
- 输出:
  - `Job` 模型可用
- 应补齐:
  - fillable / guarded
  - casts
  - 用户关联
  - 企业关联
  - 级别关联
  - 图片关联（如独立表）
  - 状态辅助方法
- 验收标准:
  - 能支撑列表、详情、管理、发布等核心查询

### TASK-ID: JOB-MODEL-LEVEL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位模型与关联
- 目标: 创建 `JobLevel` 模型
- 依赖:
  - JOB-DB-LEVEL-001
- 输入:
  - job_levels 表结构
- 输出:
  - 职位级别模型可用
- 验收标准:
  - 支持按职位聚合级别明细

### TASK-ID: JOB-MODEL-VIEW-001
- 状态: blocked
- 优先级: P1
- 所属子域: 职位模型与关联
- 目标: 创建 `JobView` 模型
- 依赖:
  - JOB-DB-VIEW-001
- 输入:
  - job_views 表结构
- 输出:
  - 浏览记录模型可用
- 验收标准:
  - 可用于统计和浏览历史复用

### TASK-ID: JOB-MODEL-TOPAPP-001
- 状态: blocked
- 优先级: P1
- 所属子域: 职位模型与关联
- 目标: 创建 `JobTopApplication` 模型
- 依赖:
  - JOB-DB-TOPAPP-001
- 输入:
  - job_top_applications 表结构
- 输出:
  - 置顶申请模型可用
- 验收标准:
  - 可读取某职位当前申请状态

### 5.5.3 职位列表与详情

### TASK-ID: JOB-REQ-LIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位列表与详情
- 目标: 编写职位列表查询参数校验 Request
- 依赖:
  - JOB-DB-BASE-001
- 输入:
  - 职位列表接口文档
- 输出:
  - 列表查询校验类可用
- 建议校验至少包含:
  - keyword
  - category_id
  - level
  - work_location
  - settlement_type
  - page
  - limit
  - sort
- 验收标准:
  - 查询参数结构明确
  - 分页参数边界明确

### TASK-ID: JOB-RES-SUMMARY-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位列表与详情
- 目标: 实现职位列表摘要资源输出类
- 依赖:
  - JOB-MODEL-BASE-001
  - JOB-MODEL-LEVEL-001
  - BOOT-FILE-RES-001
- 输入:
  - 职位列表返回字段要求
- 输出:
  - `JobSummaryResource` 可用
- 验收标准:
  - 能输出列表页核心字段
  - 支持工种/级别摘要与封面图片等展示

### TASK-ID: JOB-SVC-LIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位列表与详情
- 目标: 实现职位列表查询服务
- 依赖:
  - JOB-REQ-LIST-001
  - JOB-MODEL-BASE-001
- 输入:
  - 职位列表查询参数
- 输出:
  - 职位列表查询逻辑可用
- 业务要求至少包括:
  - 仅返回前台可见状态职位
  - 支持关键词、工种、级别、地点等筛选
  - 支持分页与排序
- 验收标准:
  - 列表查询结果与文档口径一致

### TASK-ID: JOB-API-LIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位列表与详情
- 目标: 实现职位列表接口 `GET /api/job/list`
- 依赖:
  - JOB-SVC-LIST-001
  - JOB-RES-SUMMARY-001
- 输入:
  - 列表查询参数
- 输出:
  - 前台职位列表接口可用
- 验收标准:
  - 响应结构统一
  - 分页字段齐全

### TASK-ID: JOB-RES-DETAIL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位列表与详情
- 目标: 实现职位详情资源输出类
- 依赖:
  - JOB-MODEL-BASE-001
  - JOB-MODEL-LEVEL-001
  - BOOT-FILE-RES-001
- 输入:
  - 职位详情字段要求
- 输出:
  - `JobDetailResource` 可用
- 验收标准:
  - 可输出多工种多级别结构
  - 支持 is_favorited、user_application、top_application 预留字段或后续挂载能力

### TASK-ID: JOB-SVC-DETAIL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位列表与详情
- 目标: 实现职位详情读取服务
- 依赖:
  - JOB-MODEL-BASE-001
  - JOB-SVC-VIEW-001
- 输入:
  - job_id
  - 当前登录用户（可选）
- 输出:
  - 职位详情逻辑可用
- 业务要求至少包括:
  - 状态可见性校验
  - 异步或统一入口写浏览记录
  - 可挂接收藏状态与报名状态读取
- 验收标准:
  - 详情查询与浏览记录逻辑可复用

### TASK-ID: JOB-API-DETAIL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位列表与详情
- 目标: 实现职位详情接口 `GET /api/job/detail`
- 依赖:
  - JOB-SVC-DETAIL-001
  - JOB-RES-DETAIL-001
- 输入:
  - job_id
- 输出:
  - 职位详情接口可用
- 验收标准:
  - 返回字段与文档一致
  - 响应结构统一

### TASK-ID: JOB-API-MYLIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位列表与详情
- 目标: 实现雇主自己的职位列表接口 `GET /api/job/my-list`
- 依赖:
  - JOB-MODEL-BASE-001
  - BOOT-AUTH-002
- 输入:
  - 当前雇主
  - status/page/limit
- 输出:
  - 职位管理列表接口可用
- 验收标准:
  - 仅返回当前雇主自己的职位
  - 支持按状态筛选

### 5.5.4 职位发布与草稿

### TASK-ID: JOB-REQ-PUBLISH-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位发布与草稿
- 目标: 编写发布职位参数校验 Request
- 依赖:
  - JOB-DB-BASE-001
  - JOB-DB-LEVEL-001
  - BOOT-DB-FILES-001
- 输入:
  - 发布职位接口文档
- 输出:
  - 发布参数校验类可用
- 校验建议至少包含:
  - title
  - work_location
  - work_period
  - work_time
  - categories 数组
  - settlement_type
  - accommodation
  - meals
  - requirements
  - education
  - age_range
  - tags
  - image_file_ids
  - contact_person
  - contact_phone
- 验收标准:
  - categories 嵌套结构校验清晰
  - image_file_ids 为 file_id 数组校验通过

### TASK-ID: JOB-REQ-DRAFT-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位发布与草稿
- 目标: 编写保存草稿参数校验 Request
- 依赖:
  - JOB-REQ-PUBLISH-001
- 输入:
  - 草稿接口文档
- 输出:
  - 草稿保存校验类可用
- 验收标准:
  - 草稿允许字段部分为空
  - id 为 null/已有值两种场景支持

### TASK-ID: JOB-SVC-JOBNO-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位发布与草稿
- 目标: 实现职位编号生成服务
- 依赖:
  - JOB-DB-BASE-001
- 输入:
  - 职位编号规则文档
- 输出:
  - `job_no` 生成服务可用
- 验收标准:
  - 编号唯一
  - 格式符合文档约定

### TASK-ID: JOB-SVC-AUDITRULE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位发布与草稿
- 目标: 实现职位审核判定服务
- 依赖:
  - COMPANY-SVC-CERTCHECK-001
  - BOOT-CONFIG-004
- 输入:
  - 企业认证状态
  - 认证通过时长
  - first_jobs_audit_limit
  - 职位内容
- 输出:
  - 职位发布审核判定逻辑可用
- 规则至少包括:
  - 未认证不可正式发布
  - 新认证企业前 N 条进入 pending
  - 认证超过 30 天可直接 active
  - 命中违规规则时进入 pending
- 验收标准:
  - 能输出目标 status 与 audit_status

### TASK-ID: JOB-SVC-PUBLISH-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位发布与草稿
- 目标: 实现发布职位服务
- 依赖:
  - JOB-REQ-PUBLISH-001
  - JOB-SVC-JOBNO-001
  - JOB-SVC-AUDITRULE-001
  - COMPANY-SVC-CERTCHECK-001
  - JOB-SVC-IMAGE-001
- 输入:
  - 当前雇主
  - 发布参数
- 输出:
  - 职位发布逻辑可用
- 业务要求至少包括:
  - 认证资格校验
  - 生成 job_no
  - 创建 jobs 记录
  - 创建 job_levels 明细
  - 绑定图片
  - 设置 status / audit_status
- 验收标准:
  - 事务内完成主表与明细写入
  - 返回 job_id/job_no/status

### TASK-ID: JOB-SVC-DRAFT-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位发布与草稿
- 目标: 实现保存草稿服务
- 依赖:
  - JOB-REQ-DRAFT-001
  - JOB-SVC-IMAGE-001
- 输入:
  - 当前雇主
  - 草稿参数
- 输出:
  - 草稿保存逻辑可用
- 业务要求至少包括:
  - id 为空时新建草稿
  - id 存在时更新草稿
  - 状态强制保持 draft
- 验收标准:
  - 可重复保存草稿
  - 不触发正式发布审核逻辑

### TASK-ID: JOB-API-PUBLISH-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位发布与草稿
- 目标: 实现 `POST /api/job/publish`
- 依赖:
  - JOB-SVC-PUBLISH-001
  - BOOT-AUTH-002
- 输入:
  - 发布参数
- 输出:
  - 发布接口可用
- 验收标准:
  - 返回 job_id/job_no/status
  - 认证未通过时返回标准错误

### TASK-ID: JOB-API-DRAFTSAVE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位发布与草稿
- 目标: 实现 `POST /api/job/save-draft`
- 依赖:
  - JOB-SVC-DRAFT-001
  - BOOT-AUTH-002
- 输入:
  - 草稿参数
- 输出:
  - 草稿保存接口可用
- 验收标准:
  - 支持新建/更新草稿
  - 返回 job_id 与 updated_at

### TASK-ID: JOB-API-DRAFTLIST-001
- 状态: blocked
- 优先级: P1
- 所属子域: 职位发布与草稿
- 目标: 实现草稿列表接口 `GET /api/job/draft-list`
- 依赖:
  - JOB-MODEL-BASE-001
  - BOOT-AUTH-002
- 输入:
  - 当前雇主
- 输出:
  - 草稿列表接口可用
- 验收标准:
  - 仅返回 draft 状态职位
  - 支持 completion_percent 等字段输出

### TASK-ID: JOB-API-DRAFTDETAIL-001
- 状态: blocked
- 优先级: P1
- 所属子域: 职位发布与草稿
- 目标: 实现草稿详情接口 `GET /api/job/draft-detail`
- 依赖:
  - JOB-MODEL-BASE-001
  - BOOT-AUTH-002
- 输入:
  - 草稿职位 id
- 输出:
  - 草稿详情接口可用
- 验收标准:
  - 仅允许当前雇主查看自己的草稿

### 5.5.5 职位编辑、删除、复制

### TASK-ID: JOB-REQ-UPDATE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位编辑、删除、复制
- 目标: 编写编辑职位参数校验 Request
- 依赖:
  - JOB-REQ-PUBLISH-001
- 输入:
  - 编辑接口文档
- 输出:
  - 编辑参数校验类可用
- 验收标准:
  - id 必填
  - 其余字段校验与发布口径一致

### TASK-ID: JOB-SVC-UPDATE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位编辑、删除、复制
- 目标: 实现编辑职位服务
- 依赖:
  - JOB-REQ-UPDATE-001
  - JOB-SVC-AUDITRULE-001
  - JOB-SVC-IMAGE-001
- 输入:
  - 当前雇主
  - 职位 id
  - 编辑参数
- 输出:
  - 职位编辑逻辑可用
- 业务要求至少包括:
  - 仅允许编辑自己的职位
  - 更新 jobs + job_levels + 图片
  - 根据规则决定是否重新进入 pending
- 验收标准:
  - 编辑后数据结构保持一致

### TASK-ID: JOB-API-UPDATE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位编辑、删除、复制
- 目标: 实现 `PUT /api/job/update`
- 依赖:
  - JOB-SVC-UPDATE-001
  - BOOT-AUTH-002
- 输入:
  - 编辑参数
- 输出:
  - 编辑接口可用
- 验收标准:
  - 更新成功返回统一结构

### TASK-ID: JOB-SVC-DELETE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位编辑、删除、复制
- 目标: 实现职位删除服务
- 依赖:
  - JOB-MODEL-BASE-001
- 输入:
  - 当前雇主
  - 职位 id
- 输出:
  - 职位删除逻辑可用
- 业务要求至少包括:
  - 仅允许删除自己的职位
  - 使用逻辑删除或状态置 deleted
- 验收标准:
  - 删除后前台不可见

### TASK-ID: JOB-API-DELETE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位编辑、删除、复制
- 目标: 实现 `DELETE /api/job/delete`
- 依赖:
  - JOB-SVC-DELETE-001
  - BOOT-AUTH-002
- 输入:
  - 职位 id
- 输出:
  - 删除接口可用
- 验收标准:
  - 删除成功返回统一结构

### TASK-ID: JOB-SVC-COPY-001
- 状态: blocked
- 优先级: P1
- 所属子域: 职位编辑、删除、复制
- 目标: 实现职位复制为草稿服务
- 依赖:
  - JOB-MODEL-BASE-001
  - JOB-SVC-JOBNO-001
- 输入:
  - 当前雇主
  - 源职位 id
- 输出:
  - 复制逻辑可用
- 业务要求至少包括:
  - 复制主表、级别、图片引用
  - 新职位状态为 draft
  - 新 job_no 重新生成
- 验收标准:
  - 复制后得到可编辑草稿

### TASK-ID: JOB-API-COPY-001
- 状态: blocked
- 优先级: P1
- 所属子域: 职位编辑、删除、复制
- 目标: 实现 `POST /api/job/copy`
- 依赖:
  - JOB-SVC-COPY-001
  - BOOT-AUTH-002
- 输入:
  - 源职位 id
- 输出:
  - 复制接口可用
- 验收标准:
  - 返回 new_job_id 与 draft 状态

### 5.5.6 刷新、暂停/恢复、标记招满

### TASK-ID: JOB-SVC-REFRESH-001
- 状态: blocked
- 优先级: P0
- 所属子域: 刷新、暂停/恢复、标记招满
- 目标: 实现职位刷新服务
- 依赖:
  - JOB-MODEL-BASE-001
  - BOOT-CONFIG-004
- 输入:
  - 当前雇主
  - 职位 id
  - refresh_limit_per_day
- 输出:
  - 职位刷新逻辑可用
- 验收标准:
  - 刷新时间更新
  - 每日刷新次数限制可校验

### TASK-ID: JOB-API-REFRESH-001
- 状态: blocked
- 优先级: P0
- 所属子域: 刷新、暂停/恢复、标记招满
- 目标: 实现 `PUT /api/job/refresh`
- 依赖:
  - JOB-SVC-REFRESH-001
  - BOOT-AUTH-002
- 输入:
  - 职位 id
- 输出:
  - 刷新接口可用
- 验收标准:
  - 返回 refreshed_at

### TASK-ID: JOB-SVC-TOGGLE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 刷新、暂停/恢复、标记招满
- 目标: 实现职位暂停/恢复服务
- 依赖:
  - JOB-MODEL-BASE-001
- 输入:
  - 当前雇主
  - 职位 id
  - action=pause/resume
- 输出:
  - 职位状态切换逻辑可用
- 验收标准:
  - active -> paused
  - paused -> active
  - 非法状态切换被拦截

### TASK-ID: JOB-API-TOGGLE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 刷新、暂停/恢复、标记招满
- 目标: 实现 `PUT /api/job/toggle-status`
- 依赖:
  - JOB-SVC-TOGGLE-001
  - BOOT-AUTH-002
- 输入:
  - id
  - action
- 输出:
  - 状态切换接口可用
- 验收标准:
  - 返回最新状态

### TASK-ID: JOB-SVC-MARKFILLED-001
- 状态: blocked
- 优先级: P0
- 所属子域: 刷新、暂停/恢复、标记招满
- 目标: 实现职位标记招满服务
- 依赖:
  - JOB-MODEL-BASE-001
- 输入:
  - 当前雇主
  - 职位 id
- 输出:
  - 招满标记逻辑可用
- 业务要求至少包括:
  - status -> closed
  - close_reason -> filled
- 验收标准:
  - 标记后职位不再接受报名

### TASK-ID: JOB-API-MARKFILLED-001
- 状态: blocked
- 优先级: P0
- 所属子域: 刷新、暂停/恢复、标记招满
- 目标: 实现 `PUT /api/job/mark-filled`
- 依赖:
  - JOB-SVC-MARKFILLED-001
  - BOOT-AUTH-002
- 输入:
  - 职位 id
- 输出:
  - 标记招满接口可用
- 验收标准:
  - 返回 closed + close_reason=filled

### TASK-ID: JOB-SVC-AUTOREOPEN-001
- 状态: blocked
- 优先级: P1
- 所属子域: 刷新、暂停/恢复、标记招满
- 目标: 实现“已招满关闭职位的自动恢复判定服务”
- 依赖:
  - JOB-MODEL-BASE-001
  - JOB-MODEL-LEVEL-001
- 输入:
  - 职位当前状态
  - 级别满员情况
- 输出:
  - 报名取消时可复用的自动恢复判断能力
- 验收标准:
  - 仅 filled 场景允许恢复
  - 至少一个级别未满时可恢复 active
- 非目标:
  - 本任务不直接实现报名取消接口

### 5.5.7 置顶申请与申请状态

### TASK-ID: JOB-REQ-TOPAPP-001
- 状态: blocked
- 优先级: P1
- 所属子域: 置顶申请与申请状态
- 目标: 编写职位置顶申请参数校验 Request
- 依赖:
  - JOB-DB-TOPAPP-001
- 输入:
  - 置顶申请接口文档
- 输出:
  - 置顶申请校验类可用
- 校验建议至少包含:
  - job_id
  - apply_days
- 验收标准:
  - apply_days 合法范围明确

### TASK-ID: JOB-SVC-TOPAPP-001
- 状态: blocked
- 优先级: P1
- 所属子域: 置顶申请与申请状态
- 目标: 实现职位置顶申请服务
- 依赖:
  - JOB-REQ-TOPAPP-001
  - JOB-MODEL-TOPAPP-001
- 输入:
  - 当前雇主
  - job_id
  - apply_days
- 输出:
  - 置顶申请逻辑可用
- 业务要求至少包括:
  - 仅允许申请自己的职位
  - 创建 pending 申请
  - 避免重复有效申请冲突
- 验收标准:
  - 返回 application_id/status/apply_days

### TASK-ID: JOB-API-TOPAPP-001
- 状态: blocked
- 优先级: P1
- 所属子域: 置顶申请与申请状态
- 目标: 实现 `POST /api/job/apply-top`
- 依赖:
  - JOB-SVC-TOPAPP-001
  - BOOT-AUTH-002
- 输入:
  - job_id
  - apply_days
- 输出:
  - 置顶申请接口可用
- 验收标准:
  - 返回 pending 状态

### TASK-ID: JOB-SVC-TOPSTATUS-001
- 状态: blocked
- 优先级: P1
- 所属子域: 置顶申请与申请状态
- 目标: 实现置顶申请状态查询服务
- 依赖:
  - JOB-MODEL-TOPAPP-001
- 输入:
  - 当前雇主
  - job_id
- 输出:
  - 置顶申请状态查询逻辑可用
- 验收标准:
  - 有申请/无申请两种结构明确

### TASK-ID: JOB-API-TOPSTATUS-001
- 状态: blocked
- 优先级: P1
- 所属子域: 置顶申请与申请状态
- 目标: 实现 `GET /api/job/top-application-status`
- 依赖:
  - JOB-SVC-TOPSTATUS-001
  - BOOT-AUTH-002
- 输入:
  - job_id
- 输出:
  - 置顶申请状态接口可用
- 验收标准:
  - 返回 has_application 与 application 结构

### 5.5.8 职位审核判定与编号生成

### TASK-ID: JOB-SVC-IMAGE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位审核判定与编号生成
- 目标: 实现职位图片 file_id 绑定服务
- 依赖:
  - BOOT-DB-FILES-001
  - JOB-DB-IMAGE-001
- 输入:
  - image_file_ids
- 输出:
  - 职位图片绑定逻辑可用
- 验收标准:
  - 可校验图片 file_id 合法性
  - 可按顺序保存职位图片关联

### TASK-ID: JOB-SVC-VIOLATION-001
- 状态: blocked
- 优先级: P1
- 所属子域: 职位审核判定与编号生成
- 目标: 预留职位内容违规判定服务
- 依赖:
  - JOB-SVC-AUDITRULE-001
- 输入:
  - title/requirements/contact 信息
- 输出:
  - 审核规则可扩展钩子
- 验收标准:
  - 后续接入违禁词、联系方式异常、风控策略不需要重写发布服务

### 5.5.9 浏览记录与统计

### TASK-ID: JOB-SVC-VIEW-001
- 状态: blocked
- 优先级: P0
- 所属子域: 浏览记录与统计
- 目标: 实现职位浏览记录写入服务
- 依赖:
  - JOB-DB-VIEW-001
  - BOOT-CONFIG-002
- 输入:
  - job_id
  - viewer_user_id 或 viewer_key
- 输出:
  - 浏览记录写入逻辑可用
- 业务要求至少包括:
  - 支持访客/登录用户区分
  - 支持防刷去重 key
  - 支持异步写入扩展
- 验收标准:
  - 可为详情接口复用
  - 可支撑浏览历史与统计

### TASK-ID: JOB-SVC-STATS-001
- 状态: blocked
- 优先级: P1
- 所属子域: 浏览记录与统计
- 目标: 实现单职位统计服务
- 依赖:
  - JOB-DB-VIEW-001
  - JOB-MODEL-BASE-001
- 输入:
  - job_id
  - days
- 输出:
  - 单职位统计逻辑可用
- 验收标准:
  - 可输出 views_total、apply_total、daily_views、daily_applies、level_distribution

### TASK-ID: JOB-API-STATS-001
- 状态: blocked
- 优先级: P1
- 所属子域: 浏览记录与统计
- 目标: 实现职位统计接口 `GET /api/job/statistics`
- 依赖:
  - JOB-SVC-STATS-001
  - BOOT-AUTH-002
- 输入:
  - id
  - days
- 输出:
  - 职位统计接口可用
- 验收标准:
  - 仅允许雇主查看自己的职位统计

### TASK-ID: JOB-SVC-EMPSTATS-001
- 状态: blocked
- 优先级: P1
- 所属子域: 浏览记录与统计
- 目标: 实现雇主整体职位统计服务
- 依赖:
  - JOB-MODEL-BASE-001
  - JOB-DB-VIEW-001
- 输入:
  - 当前雇主
  - days
- 输出:
  - 雇主整体统计逻辑可用
- 验收标准:
  - 可输出 job_total/job_active/views_total/apply_total/pass_rate 等

### TASK-ID: JOB-API-EMPSTATS-001
- 状态: blocked
- 优先级: P1
- 所属子域: 浏览记录与统计
- 目标: 实现雇主整体统计接口 `GET /api/employer/statistics`
- 依赖:
  - JOB-SVC-EMPSTATS-001
  - BOOT-AUTH-002
- 输入:
  - days
- 输出:
  - 雇主整体统计接口可用
- 验收标准:
  - 响应结构与文档一致

### TASK-ID: JOB-SVC-RELATED-001
- 状态: blocked
- 优先级: P2
- 所属子域: 浏览记录与统计
- 目标: 实现相关职位推荐服务
- 依赖:
  - JOB-MODEL-BASE-001
  - JOB-MODEL-LEVEL-001
- 输入:
  - job_id
  - limit
- 输出:
  - 相关推荐逻辑可用
- 验收标准:
  - 基于相同工种、相近地点输出推荐职位

### TASK-ID: JOB-API-RELATED-001
- 状态: blocked
- 优先级: P2
- 所属子域: 浏览记录与统计
- 目标: 实现 `GET /api/job/related`
- 依赖:
  - JOB-SVC-RELATED-001
- 输入:
  - job_id
  - limit
- 输出:
  - 相关职位接口可用
- 验收标准:
  - 返回 list 结构统一

### 5.5.10 职位测试

### TASK-ID: JOB-TEST-LIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位测试
- 目标: 编写职位列表接口测试
- 依赖:
  - JOB-API-LIST-001
  - BOOT-TEST-002
- 输入:
  - 职位列表接口
- 输出:
  - Feature Test
- 验收标准:
  - 基础列表返回成功
  - 筛选条件生效

### TASK-ID: JOB-TEST-DETAIL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位测试
- 目标: 编写职位详情接口测试
- 依赖:
  - JOB-API-DETAIL-001
  - BOOT-TEST-002
- 输入:
  - 职位详情接口
- 输出:
  - Feature Test
- 验收标准:
  - 详情结构正确
  - 浏览记录逻辑触发

### TASK-ID: JOB-TEST-PUBLISH-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位测试
- 目标: 编写职位发布接口测试
- 依赖:
  - JOB-API-PUBLISH-001
  - BOOT-TEST-002
- 输入:
  - 发布接口
- 输出:
  - Feature Test
- 验收标准:
  - 已认证用户可发布
  - 未认证用户不可正式发布
  - 新认证企业前 N 条进入 pending

### TASK-ID: JOB-TEST-DRAFT-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位测试
- 目标: 编写草稿保存与草稿详情测试
- 依赖:
  - JOB-API-DRAFTSAVE-001
  - JOB-API-DRAFTDETAIL-001
- 输入:
  - 草稿接口
- 输出:
  - Feature Test
- 验收标准:
  - 新建草稿成功
  - 更新草稿成功
  - 只能查看自己的草稿

### TASK-ID: JOB-TEST-UPDATE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 职位测试
- 目标: 编写职位编辑接口测试
- 依赖:
  - JOB-API-UPDATE-001
- 输入:
  - 编辑接口
- 输出:
  - Feature Test
- 验收标准:
  - 编辑成功
  - 非本人职位不可编辑

### TASK-ID: JOB-TEST-TOGGLE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 职位测试
- 目标: 编写暂停/恢复/招满测试
- 依赖:
  - JOB-API-TOGGLE-001
  - JOB-API-MARKFILLED-001
- 输入:
  - 状态切换接口
- 输出:
  - Feature Test
- 验收标准:
  - active/paused 切换正确
  - 标记招满后 close_reason=filled

### TASK-ID: JOB-TEST-TOPAPP-001
- 状态: blocked
- 优先级: P1
- 所属子域: 职位测试
- 目标: 编写置顶申请与状态查询测试
- 依赖:
  - JOB-API-TOPAPP-001
  - JOB-API-TOPSTATUS-001
- 输入:
  - 置顶申请接口
- 输出:
  - Feature Test
- 验收标准:
  - 提交申请成功
  - 查询状态结构正确

### TASK-ID: JOB-TEST-STATS-001
- 状态: blocked
- 优先级: P1
- 所属子域: 职位测试
- 目标: 编写职位统计接口测试
- 依赖:
  - JOB-API-STATS-001
  - JOB-API-EMPSTATS-001
- 输入:
  - 统计接口
- 输出:
  - Feature Test
- 验收标准:
  - 单职位统计返回正确结构
  - 雇主整体统计返回正确结构

## 5.6 本章推荐执行顺序

### 第一批（P0）
1. JOB-DB-BASE-001
2. JOB-DB-LEVEL-001
3. JOB-DB-VIEW-001
4. JOB-DB-TOPAPP-001
5. JOB-MODEL-BASE-001
6. JOB-MODEL-LEVEL-001
7. JOB-REQ-LIST-001
8. JOB-RES-SUMMARY-001
9. JOB-SVC-LIST-001
10. JOB-API-LIST-001
11. JOB-RES-DETAIL-001
12. JOB-SVC-VIEW-001
13. JOB-SVC-DETAIL-001
14. JOB-API-DETAIL-001
15. JOB-API-MYLIST-001
16. JOB-REQ-PUBLISH-001
17. JOB-REQ-DRAFT-001
18. JOB-SVC-JOBNO-001
19. JOB-SVC-IMAGE-001
20. JOB-SVC-AUDITRULE-001
21. JOB-SVC-PUBLISH-001
22. JOB-SVC-DRAFT-001
23. JOB-API-PUBLISH-001
24. JOB-API-DRAFTSAVE-001
25. JOB-REQ-UPDATE-001
26. JOB-SVC-UPDATE-001
27. JOB-API-UPDATE-001
28. JOB-SVC-DELETE-001
29. JOB-API-DELETE-001
30. JOB-SVC-REFRESH-001
31. JOB-API-REFRESH-001
32. JOB-SVC-TOGGLE-001
33. JOB-API-TOGGLE-001
34. JOB-SVC-MARKFILLED-001
35. JOB-API-MARKFILLED-001
36. JOB-TEST-LIST-001
37. JOB-TEST-DETAIL-001
38. JOB-TEST-PUBLISH-001
39. JOB-TEST-DRAFT-001

### 第二批（P1/P2）
1. JOB-DB-IMAGE-001
2. JOB-MODEL-VIEW-001
3. JOB-MODEL-TOPAPP-001
4. JOB-API-DRAFTLIST-001
5. JOB-API-DRAFTDETAIL-001
6. JOB-SVC-COPY-001
7. JOB-API-COPY-001
8. JOB-SVC-AUTOREOPEN-001
9. JOB-REQ-TOPAPP-001
10. JOB-SVC-TOPAPP-001
11. JOB-API-TOPAPP-001
12. JOB-SVC-TOPSTATUS-001
13. JOB-API-TOPSTATUS-001
14. JOB-SVC-VIOLATION-001
15. JOB-SVC-STATS-001
16. JOB-API-STATS-001
17. JOB-SVC-EMPSTATS-001
18. JOB-API-EMPSTATS-001
19. JOB-SVC-RELATED-001
20. JOB-API-RELATED-001
21. JOB-TEST-UPDATE-001
22. JOB-TEST-TOGGLE-001
23. JOB-TEST-TOPAPP-001
24. JOB-TEST-STATS-001

## 5.7 本章完成后的下一个章节入口
当第 5 章完成后，最合理进入：
- 第6章：报名系统

原因：
- 报名系统直接依赖职位与简历两条主数据链路
- 完成职位系统后即可进入求职者/雇主交互主闭环

---

# 第6章：报名系统

> 目标：建立求职者职位报名、取消报名、我的报名列表、雇主管理报名列表、单个审核、批量审核、报名幂等控制、名额占用与回收、被拒后再报名限制、报名状态通知等完整报名域基础能力。
> 本章只处理报名域及其直接支撑能力，不直接实现私聊聊天，但会为通过报名后的沟通前置条件准备好可复用判定结果。

## 6.1 本章定位

### 本章要解决的问题
- 建立报名主表
- 支持求职者对职位工种/级别发起报名
- 支持取消报名
- 支持我的报名列表与详情摘要
- 支持雇主查看自己职位的报名列表
- 支持单个审核、批量审核
- 支持报名幂等控制
- 支持名额占用、回收与状态同步
- 支持被拒后再报名限制
- 为通知系统、人才库、聊天开放条件提供基础输入

### 本章不做的内容
- 私聊消息发送
- 人才库加入接口本身
- 后台人工干预报名审核
- 评论、举报逻辑

## 6.2 本章完成标准
当第 6 章完成时，应满足：
1. `applications` 表可用
2. 求职者可按职位工种/级别发起报名
3. 求职者可取消报名
4. 用户可查看“我的报名”列表
5. 雇主可查看职位报名列表
6. 雇主可单个审核与批量审核报名
7. 系统可控制重复报名、再报名限制与幂等
8. 级别报名人数与职位状态可联动更新
9. 后续通知系统可直接复用报名状态变化事件
10. 关键报名接口具备基础测试覆盖

## 6.3 本章依赖关系
### 上游依赖
- 第1章：项目骨架与基础设施
- 第2章：用户与认证
- 第3章：简历系统
- 第5章：职位系统

### 下游依赖
- 通知系统
- 聊天系统开放条件
- 人才库
- 雇主统计

## 6.4 本章任务总览
1. 报名数据结构与表设计
2. 报名模型与关联
3. 创建报名与幂等控制
4. 取消报名
5. 我的报名列表
6. 雇主报名管理列表
7. 单个审核与批量审核
8. 名额占用回收与状态联动
9. 报名通知预留
10. 报名测试

## 6.5 可直接执行任务清单

### 6.5.1 报名数据结构与表设计

### TASK-ID: APPLY-DB-BASE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 报名数据结构与表设计
- 目标: 创建 `applications` 表 migration
- 依赖:
  - BOOT-DB-001
  - AUTH-DB-USER-001
  - JOB-DB-BASE-001
  - JOB-DB-LEVEL-001
- 输入:
  - 报名接口文档
  - 报名状态机
  - 雇主审核与统计需求
- 输出:
  - `applications` 表可支撑报名主流程
- 建议字段至少包含:
  - id
  - user_id
  - employer_id
  - job_id
  - job_level_id
  - category_id
  - category_name_snapshot
  - level
  - level_name_snapshot
  - resume_id
  - status
  - audit_reason
  - applied_at
  - audited_at
  - cancelled_at
  - contact_unlocked_at
  - source
  - created_at / updated_at
- 索引建议:
  - user_id 普通索引
  - employer_id 普通索引
  - job_id 普通索引
  - job_level_id 普通索引
  - status 普通索引
  - user_id + job_id + category_id + level 组合索引
- 验收标准:
  - 可支撑求职者报名、雇主审核、我的报名、职位报名管理、人才库来源溯源
- 非目标:
  - 不在本任务中实现通知明细表

### TASK-ID: APPLY-DB-UNIQUE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 报名数据结构与表设计
- 目标: 明确“重复有效报名”数据库约束策略
- 依赖:
  - APPLY-DB-BASE-001
- 输入:
  - 重复报名限制规则
- 输出:
  - 重复有效报名约束方案明确
- 实现建议:
  - 结合数据库索引 + 服务层状态校验完成，避免仅依赖应用层 if 判断
- 验收标准:
  - 同一用户对同一职位同一工种级别不能存在多条有效报名

### 6.5.2 报名模型与关联

### TASK-ID: APPLY-MODEL-BASE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 报名模型与关联
- 目标: 创建 `Application` 模型并补齐基础属性
- 依赖:
  - APPLY-DB-BASE-001
- 输入:
  - applications 表结构
- 输出:
  - `Application` 模型可用
- 应补齐:
  - fillable / guarded
  - casts
  - 求职者关联
  - 雇主关联
  - 职位关联
  - 职位级别关联
  - 简历关联
  - 状态辅助方法
- 验收标准:
  - 能支撑报名列表、审核逻辑、统计聚合

### TASK-ID: APPLY-MODEL-JOBREL-001
- 状态: blocked
- 优先级: P1
- 所属子域: 报名模型与关联
- 目标: 在 `Job` 模型中补齐报名关联
- 依赖:
  - JOB-MODEL-BASE-001
  - APPLY-MODEL-BASE-001
- 输入:
  - Job / Application 模型
- 输出:
  - 职位与报名关联可用
- 验收标准:
  - 可通过职位聚合读取报名数据

### 6.5.3 创建报名与幂等控制

### TASK-ID: APPLY-REQ-CREATE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 创建报名与幂等控制
- 目标: 编写报名接口参数校验 Request
- 依赖:
  - APPLY-DB-BASE-001
- 输入:
  - 报名接口文档
- 输出:
  - 报名参数校验类可用
- 建议校验至少包含:
  - job_id
  - category_id
  - level
- 验收标准:
  - 参数结构简单明确
  - category_id/level 合法性在后续服务层二次校验

### TASK-ID: APPLY-SVC-LEVELRESOLVE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 创建报名与幂等控制
- 目标: 实现 `job_id + category_id + level` 到 `job_level_id` 的解析服务
- 依赖:
  - JOB-MODEL-LEVEL-001
- 输入:
  - job_id
  - category_id
  - level
- 输出:
  - 职位级别解析能力可用
- 验收标准:
  - 找不到级别时返回明确错误
  - 后续报名、统计、审核逻辑可复用

### TASK-ID: APPLY-SVC-REAPPLYRULE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 创建报名与幂等控制
- 目标: 实现再报名限制判定服务
- 依赖:
  - APPLY-MODEL-BASE-001
  - BOOT-CONFIG-004
- 输入:
  - 当前用户
  - job_id/category_id/level
  - reapply_limit_hours
- 输出:
  - 再报名限制规则可用
- 业务要求至少包括:
  - 被拒后在限制时间内不可重报
  - 已取消后是否允许重报按文档口径判断
- 验收标准:
  - 能返回是否允许重新报名与失败原因

### TASK-ID: APPLY-SVC-IDEMPOTENT-001
- 状态: blocked
- 优先级: P0
- 所属子域: 创建报名与幂等控制
- 目标: 实现报名幂等控制服务
- 依赖:
  - APPLY-DB-UNIQUE-001
  - BOOT-CONFIG-002
- 输入:
  - user_id
  - job_id
  - category_id
  - level
- 输出:
  - 幂等控制能力可用
- 实现建议:
  - 结合 Redis 短期锁 + DB 唯一约束/状态校验
- 验收标准:
  - 高频重复点击不会产生多条有效报名记录

### TASK-ID: APPLY-SVC-CREATE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 创建报名与幂等控制
- 目标: 实现报名创建服务
- 依赖:
  - APPLY-REQ-CREATE-001
  - APPLY-SVC-LEVELRESOLVE-001
  - RESUME-SVC-ELIGIBLE-001
  - APPLY-SVC-REAPPLYRULE-001
  - APPLY-SVC-IDEMPOTENT-001
  - JOB-SVC-APPLYSTATE-001
  - APPLY-SVC-COUNT-001
- 输入:
  - 当前求职者
  - job_id/category_id/level
- 输出:
  - 报名创建逻辑可用
- 业务要求至少包括:
  - 校验职位状态可报名
  - 校验不能报名自己的职位
  - 校验简历完整度
  - 解析 job_level_id
  - 校验重复报名与再报名限制
  - 创建 application 记录
  - 增加 job_level.applied_count
  - 必要时联动职位状态
- 验收标准:
  - 报名成功返回 application_id/status
  - 事务内完成主记录与计数更新

### TASK-ID: APPLY-API-CREATE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 创建报名与幂等控制
- 目标: 实现职位报名接口 `POST /api/application/apply` 或等价接口
- 依赖:
  - APPLY-SVC-CREATE-001
  - BOOT-AUTH-002
- 输入:
  - 报名参数
- 输出:
  - 报名接口可用
- 验收标准:
  - 成功/失败结构统一
  - 能返回当前报名状态

### 6.5.4 取消报名

### TASK-ID: APPLY-SVC-CANCEL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 取消报名
- 目标: 实现取消报名服务
- 依赖:
  - APPLY-MODEL-BASE-001
  - APPLY-SVC-COUNT-001
  - JOB-SVC-APPLYSTATE-001
  - JOB-SVC-AUTOREOPEN-001
- 输入:
  - 当前求职者
  - application_id
- 输出:
  - 取消报名逻辑可用
- 业务要求至少包括:
  - 仅允许取消自己的可取消报名
  - 状态改为 cancelled
  - 回收 job_level.applied_count
  - 如职位因 filled 关闭，必要时触发恢复判定
- 验收标准:
  - 取消后数据与人数同步正确

### TASK-ID: APPLY-API-CANCEL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 取消报名
- 目标: 实现取消报名接口 `PUT /api/application/cancel`
- 依赖:
  - APPLY-SVC-CANCEL-001
  - BOOT-AUTH-002
- 输入:
  - application_id
- 输出:
  - 取消报名接口可用
- 验收标准:
  - 成功后返回最新状态 cancelled

### 6.5.5 我的报名列表

### TASK-ID: APPLY-REQ-MYLIST-001
- 状态: blocked
- 优先级: P1
- 所属子域: 我的报名列表
- 目标: 编写我的报名列表查询参数校验 Request
- 依赖:
  - APPLY-DB-BASE-001
- 输入:
  - 我的报名列表接口文档
- 输出:
  - 列表查询校验类可用
- 建议校验至少包含:
  - status
  - page
  - limit
- 验收标准:
  - 分页与状态筛选清晰

### TASK-ID: APPLY-RES-MYLIST-001
- 状态: blocked
- 优先级: P1
- 所属子域: 我的报名列表
- 目标: 实现我的报名列表资源输出类
- 依赖:
  - APPLY-MODEL-BASE-001
  - JOB-RES-SUMMARY-001
- 输入:
  - 我的报名列表返回字段要求
- 输出:
  - `MyApplicationResource` 可用
- 验收标准:
  - 输出职位摘要、工种级别、报名状态、审核时间等字段

### TASK-ID: APPLY-SVC-MYLIST-001
- 状态: blocked
- 优先级: P1
- 所属子域: 我的报名列表
- 目标: 实现我的报名列表查询服务
- 依赖:
  - APPLY-REQ-MYLIST-001
  - APPLY-MODEL-BASE-001
- 输入:
  - 当前求职者
  - status/page/limit
- 输出:
  - 我的报名列表逻辑可用
- 验收标准:
  - 按时间倒序
  - 支持状态筛选

### TASK-ID: APPLY-API-MYLIST-001
- 状态: blocked
- 优先级: P1
- 所属子域: 我的报名列表
- 目标: 实现我的报名列表接口 `GET /api/application/my-list`
- 依赖:
  - APPLY-SVC-MYLIST-001
  - APPLY-RES-MYLIST-001
  - BOOT-AUTH-002
- 输入:
  - 列表查询参数
- 输出:
  - 我的报名列表接口可用
- 验收标准:
  - 分页字段齐全
  - 响应结构统一

### 6.5.6 雇主报名管理列表

### TASK-ID: APPLY-REQ-EMPLOYERLIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 雇主报名管理列表
- 目标: 编写雇主报名列表查询参数校验 Request
- 依赖:
  - APPLY-DB-BASE-001
- 输入:
  - 雇主报名列表接口文档
- 输出:
  - 报名管理查询校验类可用
- 建议校验至少包含:
  - job_id
  - status
  - category_id
  - level
  - keyword
  - page
  - limit
- 验收标准:
  - 多维筛选参数结构明确

### TASK-ID: APPLY-RES-EMPLOYERLIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 雇主报名管理列表
- 目标: 实现雇主报名管理列表资源输出类
- 依赖:
  - APPLY-MODEL-BASE-001
  - RESUME-RES-SUMMARY-001
- 输入:
  - 雇主报名列表字段要求
- 输出:
  - `EmployerApplicationResource` 可用
- 验收标准:
  - 输出求职者摘要、简历摘要、报名工种级别、报名状态等信息

### TASK-ID: APPLY-SVC-EMPLOYERLIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 雇主报名管理列表
- 目标: 实现雇主报名管理列表查询服务
- 依赖:
  - APPLY-REQ-EMPLOYERLIST-001
  - APPLY-MODEL-BASE-001
- 输入:
  - 当前雇主
  - 查询参数
- 输出:
  - 雇主报名管理列表逻辑可用
- 业务要求至少包括:
  - 仅返回属于当前雇主职位的报名
  - 支持 job_id/status/category_id/level 筛选
- 验收标准:
  - 结果可直接用于职位报名管理页

### TASK-ID: APPLY-API-EMPLOYERLIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 雇主报名管理列表
- 目标: 实现雇主报名管理列表接口 `GET /api/employer/application/list` 或等价接口
- 依赖:
  - APPLY-SVC-EMPLOYERLIST-001
  - APPLY-RES-EMPLOYERLIST-001
  - BOOT-AUTH-002
- 输入:
  - 查询参数
- 输出:
  - 雇主报名列表接口可用
- 验收标准:
  - 响应结构统一
  - 分页可用

### 6.5.7 单个审核与批量审核

### TASK-ID: APPLY-REQ-REVIEW-001
- 状态: blocked
- 优先级: P0
- 所属子域: 单个审核与批量审核
- 目标: 编写单个审核参数校验 Request
- 依赖:
  - APPLY-DB-BASE-001
- 输入:
  - 单个审核接口文档
- 输出:
  - 单个审核校验类可用
- 建议校验至少包含:
  - application_id
  - action=accept/reject
  - reason（拒绝时）
- 验收标准:
  - 审核动作与拒绝原因校验明确

### TASK-ID: APPLY-SVC-COUNT-001
- 状态: blocked
- 优先级: P0
- 所属子域: 单个审核与批量审核
- 目标: 实现报名人数增减统一服务
- 依赖:
  - JOB-MODEL-LEVEL-001
- 输入:
  - job_level_id
  - delta
- 输出:
  - applied_count 统一增减能力可用
- 验收标准:
  - 增减逻辑有边界保护
  - 后续报名创建、取消、审核都复用同一入口

### TASK-ID: JOB-SVC-APPLYSTATE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 单个审核与批量审核
- 目标: 实现职位/级别可报名状态判定服务
- 依赖:
  - JOB-MODEL-BASE-001
  - JOB-MODEL-LEVEL-001
- 输入:
  - job_id
  - job_level_id
- 输出:
  - 报名前置状态判定能力可用
- 业务要求至少包括:
  - 职位必须处于可报名状态
  - 级别必须启用且有剩余名额
- 验收标准:
  - 报名前与审核后状态联动可复用

### TASK-ID: APPLY-SVC-REVIEW-001
- 状态: blocked
- 优先级: P0
- 所属子域: 单个审核与批量审核
- 目标: 实现单个报名审核服务
- 依赖:
  - APPLY-REQ-REVIEW-001
  - APPLY-MODEL-BASE-001
  - JOB-SVC-APPLYSTATE-001
- 输入:
  - 当前雇主
  - application_id
  - action
  - reason
- 输出:
  - 单个审核逻辑可用
- 业务要求至少包括:
  - 仅允许审核自己职位下的报名
  - applied -> accepted/rejected
  - accepted 时设置联系开放时间
  - rejected 时记录原因
  - 审核结果写入 audited_at
- 验收标准:
  - 非法状态不可重复审核
  - 审核通过/拒绝状态更新正确

### TASK-ID: APPLY-API-REVIEW-001
- 状态: blocked
- 优先级: P0
- 所属子域: 单个审核与批量审核
- 目标: 实现单个报名审核接口 `PUT /api/employer/application/review`
- 依赖:
  - APPLY-SVC-REVIEW-001
  - BOOT-AUTH-002
- 输入:
  - application_id
  - action
  - reason
- 输出:
  - 单个审核接口可用
- 验收标准:
  - 返回最新 application 状态

### TASK-ID: APPLY-REQ-BATCHREVIEW-001
- 状态: blocked
- 优先级: P1
- 所属子域: 单个审核与批量审核
- 目标: 编写批量审核参数校验 Request
- 依赖:
  - APPLY-DB-BASE-001
- 输入:
  - 批量审核接口文档
- 输出:
  - 批量审核校验类可用
- 建议校验至少包含:
  - application_ids
  - action
  - reason（拒绝时）
- 验收标准:
  - application_ids 数组结构与上限明确

### TASK-ID: APPLY-SVC-BATCHREVIEW-001
- 状态: blocked
- 优先级: P1
- 所属子域: 单个审核与批量审核
- 目标: 实现批量审核服务
- 依赖:
  - APPLY-REQ-BATCHREVIEW-001
  - APPLY-SVC-REVIEW-001
- 输入:
  - 当前雇主
  - application_ids
  - action
  - reason
- 输出:
  - 批量审核逻辑可用
- 验收标准:
  - 仅处理属于当前雇主的有效报名
  - 返回 success_count / fail_count / failed_items

### TASK-ID: APPLY-API-BATCHREVIEW-001
- 状态: blocked
- 优先级: P1
- 所属子域: 单个审核与批量审核
- 目标: 实现批量审核接口 `PUT /api/employer/application/batch-review`
- 依赖:
  - APPLY-SVC-BATCHREVIEW-001
  - BOOT-AUTH-002
- 输入:
  - application_ids
  - action
  - reason
- 输出:
  - 批量审核接口可用
- 验收标准:
  - 返回聚合处理结果

### 6.5.8 名额占用回收与状态联动

### TASK-ID: APPLY-SVC-LEVELFULL-001
- 状态: blocked
- 优先级: P1
- 所属子域: 名额占用回收与状态联动
- 目标: 实现职位级别满员判定服务
- 依赖:
  - JOB-MODEL-LEVEL-001
- 输入:
  - job_level_id
- 输出:
  - 级别是否满员判断能力可用
- 验收标准:
  - applied_count >= recruit_count 时返回满员

### TASK-ID: APPLY-SVC-JOBCLOSE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 名额占用回收与状态联动
- 目标: 实现职位是否应标记招满关闭的判定服务
- 依赖:
  - JOB-MODEL-BASE-001
  - JOB-MODEL-LEVEL-001
- 输入:
  - job_id
- 输出:
  - 职位满员关闭判定能力可用
- 验收标准:
  - 所有有效级别满员时可判定关闭
- 非目标:
  - 不直接修改职位状态，由调用方决定是否执行关闭

### TASK-ID: APPLY-SVC-REVIEWSIDEEFFECT-001
- 状态: blocked
- 优先级: P1
- 所属子域: 名额占用回收与状态联动
- 目标: 实现审核后续副作用统一处理服务
- 依赖:
  - APPLY-SVC-LEVELFULL-001
  - APPLY-SVC-JOBCLOSE-001
  - JOB-SVC-AUTOREOPEN-001
- 输入:
  - application
  - old_status
  - new_status
- 输出:
  - 审核后职位/级别联动副作用处理能力可用
- 验收标准:
  - 审核通过、取消报名、恢复报名场景下都可复用

### 6.5.9 报名通知预留

### TASK-ID: APPLY-SVC-NOTIFYHOOK-001
- 状态: blocked
- 优先级: P1
- 所属子域: 报名通知预留
- 目标: 预留报名状态变化通知派发入口
- 依赖:
  - APPLY-SVC-CREATE-001
  - APPLY-SVC-REVIEW-001
  - APPLY-SVC-CANCEL-001
- 输入:
  - 报名创建/取消/审核结果
- 输出:
  - 统一通知派发钩子可用
- 验收标准:
  - 后续通知系统接入不需要改写主流程服务
- 非目标:
  - 本任务不直接实现通知发送

### TASK-ID: APPLY-SVC-CONTACTUNLOCK-001
- 状态: blocked
- 优先级: P1
- 所属子域: 报名通知预留
- 目标: 实现报名通过后联系方式开放判定服务
- 依赖:
  - APPLY-MODEL-BASE-001
- 输入:
  - application
- 输出:
  - 聊天/联系开放条件可复用
- 验收标准:
  - 仅 accepted 报名可开放联系条件

### 6.5.10 报名测试

### TASK-ID: APPLY-TEST-CREATE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 报名测试
- 目标: 编写报名接口测试
- 依赖:
  - APPLY-API-CREATE-001
  - BOOT-TEST-002
- 输入:
  - 报名接口
- 输出:
  - Feature Test
- 验收标准:
  - 正常报名成功
  - 简历不完整时报名失败
  - 不能报名自己的职位
  - 重复点击不产生多条有效报名

### TASK-ID: APPLY-TEST-CANCEL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 报名测试
- 目标: 编写取消报名接口测试
- 依赖:
  - APPLY-API-CANCEL-001
  - BOOT-TEST-002
- 输入:
  - 取消报名接口
- 输出:
  - Feature Test
- 验收标准:
  - 可取消自己的有效报名
  - 取消后状态为 cancelled
  - applied_count 正确回收

### TASK-ID: APPLY-TEST-MYLIST-001
- 状态: blocked
- 优先级: P1
- 所属子域: 报名测试
- 目标: 编写我的报名列表接口测试
- 依赖:
  - APPLY-API-MYLIST-001
  - BOOT-TEST-002
- 输入:
  - 我的报名列表接口
- 输出:
  - Feature Test
- 验收标准:
  - 分页与状态筛选生效

### TASK-ID: APPLY-TEST-EMPLOYERLIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 报名测试
- 目标: 编写雇主报名管理列表接口测试
- 依赖:
  - APPLY-API-EMPLOYERLIST-001
  - BOOT-TEST-002
- 输入:
  - 雇主报名列表接口
- 输出:
  - Feature Test
- 验收标准:
  - 只能看到自己职位下的报名
  - job/status/category/level 筛选生效

### TASK-ID: APPLY-TEST-REVIEW-001
- 状态: blocked
- 优先级: P0
- 所属子域: 报名测试
- 目标: 编写单个审核接口测试
- 依赖:
  - APPLY-API-REVIEW-001
  - BOOT-TEST-002
- 输入:
  - 审核接口
- 输出:
  - Feature Test
- 验收标准:
  - 通过/拒绝两种动作正确
  - 非本人职位报名不可审核
  - 重复审核被拦截

### TASK-ID: APPLY-TEST-BATCHREVIEW-001
- 状态: blocked
- 优先级: P1
- 所属子域: 报名测试
- 目标: 编写批量审核接口测试
- 依赖:
  - APPLY-API-BATCHREVIEW-001
  - BOOT-TEST-002
- 输入:
  - 批量审核接口
- 输出:
  - Feature Test
- 验收标准:
  - 批量通过/拒绝可用
  - 返回 success_count/fail_count

### TASK-ID: APPLY-TEST-REAPPLY-001
- 状态: blocked
- 优先级: P0
- 所属子域: 报名测试
- 目标: 编写再报名限制测试
- 依赖:
  - APPLY-SVC-REAPPLYRULE-001
  - APPLY-API-CREATE-001
- 输入:
  - 再报名限制规则
- 输出:
  - Unit / Feature Test
- 验收标准:
  - 被拒后限制时间内不可重报
  - 超过限制时间后可重新报名

### TASK-ID: APPLY-TEST-FILLED-001
- 状态: blocked
- 优先级: P1
- 所属子域: 报名测试
- 目标: 编写名额满员与职位关闭/恢复判定测试
- 依赖:
  - APPLY-SVC-LEVELFULL-001
  - APPLY-SVC-JOBCLOSE-001
  - JOB-SVC-AUTOREOPEN-001
- 输入:
  - 满员判定与恢复判定服务
- 输出:
  - Unit Test
- 验收标准:
  - 全级别满员时职位可判定关闭
  - 取消报名后满足条件可恢复 active

## 6.6 本章推荐执行顺序

### 第一批（P0）
1. APPLY-DB-BASE-001
2. APPLY-DB-UNIQUE-001
3. APPLY-MODEL-BASE-001
4. APPLY-REQ-CREATE-001
5. APPLY-SVC-LEVELRESOLVE-001
6. JOB-SVC-APPLYSTATE-001
7. APPLY-SVC-COUNT-001
8. APPLY-SVC-REAPPLYRULE-001
9. APPLY-SVC-IDEMPOTENT-001
10. APPLY-SVC-CREATE-001
11. APPLY-API-CREATE-001
12. APPLY-SVC-CANCEL-001
13. APPLY-API-CANCEL-001
14. APPLY-REQ-EMPLOYERLIST-001
15. APPLY-RES-EMPLOYERLIST-001
16. APPLY-SVC-EMPLOYERLIST-001
17. APPLY-API-EMPLOYERLIST-001
18. APPLY-REQ-REVIEW-001
19. APPLY-SVC-REVIEW-001
20. APPLY-API-REVIEW-001
21. APPLY-TEST-CREATE-001
22. APPLY-TEST-CANCEL-001
23. APPLY-TEST-EMPLOYERLIST-001
24. APPLY-TEST-REVIEW-001
25. APPLY-TEST-REAPPLY-001

### 第二批（P1）
1. APPLY-MODEL-JOBREL-001
2. APPLY-REQ-MYLIST-001
3. APPLY-RES-MYLIST-001
4. APPLY-SVC-MYLIST-001
5. APPLY-API-MYLIST-001
6. APPLY-REQ-BATCHREVIEW-001
7. APPLY-SVC-BATCHREVIEW-001
8. APPLY-API-BATCHREVIEW-001
9. APPLY-SVC-LEVELFULL-001
10. APPLY-SVC-JOBCLOSE-001
11. APPLY-SVC-REVIEWSIDEEFFECT-001
12. APPLY-SVC-NOTIFYHOOK-001
13. APPLY-SVC-CONTACTUNLOCK-001
14. APPLY-TEST-MYLIST-001
15. APPLY-TEST-BATCHREVIEW-001
16. APPLY-TEST-FILLED-001

## 6.7 本章完成后的下一个章节入口
当第 6 章完成后，最合理进入：
- 第7章：聊天与 WebSocket

原因：
- 用户、简历、企业认证、职位、报名主链路已闭环
- 接下来最自然的是把“报名后沟通”与“实时通知/回执”补齐

---

# 第7章：聊天与 WebSocket

> 目标：建立私聊会话、私聊消息、已读回执、消息撤回/删除、系统通知推送、公共聊天室、在线状态、ws_token 配置接口，以及 Workerman + GatewayWorker 的实时事件基础能力。
> 本章只处理聊天与 WebSocket 域及其直接支撑能力，不直接实现完整通知中心后台运营模块，但会为通知模块提供统一事件入口。

## 7.1 本章定位

### 本章要解决的问题
- 建立私聊会话与私聊消息数据结构
- 建立 ws_token 获取配置接口
- 建立私聊会话列表与消息详情接口
- 建立发送消息、已读回执、撤回、删除能力
- 建立公共聊天室消息基础能力
- 建立在线状态与在线人数统计基础能力
- 明确 WebSocket 事件契约与回执规范
- 建立 Workerman/GatewayWorker 业务分发骨架
- 为通知、聊天开放条件、消息未读数等后续模块提供基础输入

### 本章不做的内容
- 后台广播运营系统
- 完整消息内容审核后台
- 音视频通话
- 群组聊天扩展
- 第三方 IM 服务替换方案

## 7.2 本章完成标准
当第 7 章完成时，应满足：
1. 私聊会话表与私聊消息表可用
2. 用户可获取 ws 配置与 `ws_token`
3. 用户可查看会话列表与会话消息详情
4. 用户可发送私聊消息
5. 用户可标记已读
6. 用户可撤回、删除消息
7. WebSocket 连接、回执与错误事件契约可用
8. 公共聊天室消息基础能力可用
9. 在线状态与在线人数基础能力可用
10. 关键聊天接口与 ws 逻辑具备基础测试或联调任务

## 7.3 本章依赖关系
### 上游依赖
- 第1章：项目骨架与基础设施
- 第2章：用户与认证
- 第5章：职位系统
- 第6章：报名系统

### 下游依赖
- 通知系统
- 人才库沟通链路
- 用户未读消息提示
- 后台消息风控与运营增强

## 7.4 本章任务总览
1. 聊天数据结构与表设计
2. 聊天模型与关联
3. ws 配置与 ws_token
4. 私聊会话列表与消息详情
5. 发送消息与回执
6. 已读、撤回、删除
7. 公共聊天室
8. 在线状态与人数统计
9. WebSocket 网关与业务分发骨架
10. 聊天测试与联调

## 7.5 可直接执行任务清单

### 7.5.1 聊天数据结构与表设计

### TASK-ID: CHAT-DB-CONV-001
- 状态: blocked
- 优先级: P0
- 所属子域: 聊天数据结构与表设计
- 目标: 创建 `conversations` 表 migration
- 依赖:
  - BOOT-DB-001
  - AUTH-DB-USER-001
  - JOB-DB-BASE-001
- 输入:
  - 私聊会话接口文档
  - conversation_key 约定
- 输出:
  - `conversations` 表可支撑私聊会话基础能力
- 建议字段至少包含:
  - id
  - conversation_key
  - user_a_id
  - user_b_id
  - job_id
  - last_message_id
  - last_message_time
  - last_message_preview
  - unread_count_a
  - unread_count_b
  - is_closed
  - created_at / updated_at
- 索引建议:
  - conversation_key 唯一索引
  - user_a_id 普通索引
  - user_b_id 普通索引
  - job_id 普通索引
  - last_message_time 普通索引
- 验收标准:
  - 同一对用户在同一职位维度的会话 key 策略明确
  - 可支撑会话列表与未读统计

### TASK-ID: CHAT-DB-MSG-001
- 状态: blocked
- 优先级: P0
- 所属子域: 聊天数据结构与表设计
- 目标: 创建 `messages_private` 表 migration
- 依赖:
  - CHAT-DB-CONV-001
  - BOOT-DB-FILES-001
- 输入:
  - 私聊消息结构文档
  - WebSocket 事件契约
- 输出:
  - 私聊消息表可用
- 建议字段至少包含:
  - id
  - conversation_id
  - conversation_key
  - sender_id
  - receiver_id
  - job_id
  - client_msg_id
  - message_type
  - content
  - file_id
  - status
  - is_recalled
  - recalled_at
  - deleted_by_sender
  - deleted_by_receiver
  - read_at
  - created_at / updated_at
- 索引建议:
  - conversation_id 普通索引
  - conversation_key 普通索引
  - sender_id 普通索引
  - receiver_id 普通索引
  - client_msg_id 普通索引
  - created_at 普通索引
- 验收标准:
  - 支持 text/image/file 等消息类型
  - 支持幂等 client_msg_id
  - 支持已读、撤回、单边删除语义

### TASK-ID: CHAT-DB-PUBLIC-001
- 状态: blocked
- 优先级: P1
- 所属子域: 聊天数据结构与表设计
- 目标: 创建公共聊天室消息表 migration
- 依赖:
  - BOOT-DB-001
  - AUTH-DB-USER-001
- 输入:
  - 公共聊天室接口文档
- 输出:
  - 公共聊天室消息结构可用
- 建议表名:
  - `messages_public`
- 建议字段至少包含:
  - id
  - sender_id
  - client_msg_id
  - message_type
  - content
  - file_id
  - status
  - created_at / updated_at
- 索引建议:
  - sender_id 普通索引
  - created_at 普通索引
  - client_msg_id 普通索引
- 验收标准:
  - 可支撑公共聊天室历史消息拉取与实时广播

### TASK-ID: CHAT-DB-PRESENCE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 聊天数据结构与表设计
- 目标: 明确在线状态存储策略并完成落地
- 依赖:
  - BOOT-CONFIG-002
  - AUTH-DB-DEVICESESSION-001
- 输入:
  - 在线状态与在线人数需求
- 输出:
  - 在线状态存储策略可用
- 实现建议:
  - Redis 为主，数据库仅保留 last_online_time 到 users/device_sessions
- 验收标准:
  - 支持在线/离线标记
  - 支持 presence.count 统计

### 7.5.2 聊天模型与关联

### TASK-ID: CHAT-MODEL-CONV-001
- 状态: blocked
- 优先级: P0
- 所属子域: 聊天模型与关联
- 目标: 创建 `Conversation` 模型
- 依赖:
  - CHAT-DB-CONV-001
- 输入:
  - conversations 表结构
- 输出:
  - 会话模型可用
- 应补齐:
  - 用户双方关联
  - 职位关联
  - 最新消息关联
  - 未读数辅助方法
- 验收标准:
  - 能支撑会话列表与详情读取

### TASK-ID: CHAT-MODEL-MSG-001
- 状态: blocked
- 优先级: P0
- 所属子域: 聊天模型与关联
- 目标: 创建 `PrivateMessage` 模型
- 依赖:
  - CHAT-DB-MSG-001
- 输入:
  - messages_private 表结构
- 输出:
  - 私聊消息模型可用
- 验收标准:
  - 支持消息详情读取与 sender/receiver 关联

### TASK-ID: CHAT-MODEL-PUBLIC-001
- 状态: blocked
- 优先级: P1
- 所属子域: 聊天模型与关联
- 目标: 创建 `PublicMessage` 模型
- 依赖:
  - CHAT-DB-PUBLIC-001
- 输入:
  - messages_public 表结构
- 输出:
  - 公共消息模型可用
- 验收标准:
  - 支持公共消息历史拉取

### 7.5.3 ws 配置与 ws_token

### TASK-ID: CHAT-SVC-WSTOKEN-001
- 状态: blocked
- 优先级: P0
- 所属子域: ws 配置与 ws_token
- 目标: 实现 ws_token 签发服务
- 依赖:
  - AUTH-SVC-TOKEN-001
  - AUTH-MODEL-SESSION-001
  - BOOT-CONFIG-004
- 输入:
  - 当前登录用户
  - 当前 device_session
- 输出:
  - ws_token 签发能力可用
- 业务要求至少包括:
  - ws_token 短期有效
  - 绑定 device_session_id
  - 区分于 HTTP token
- 验收标准:
  - 可校验有效期与所属会话

### TASK-ID: CHAT-API-WSCONFIG-001
- 状态: blocked
- 优先级: P0
- 所属子域: ws 配置与 ws_token
- 目标: 实现 `GET /api/ws/config`
- 依赖:
  - CHAT-SVC-WSTOKEN-001
  - BOOT-AUTH-002
- 输入:
  - 当前登录用户
- 输出:
  - ws 配置接口可用
- 返回至少包含:
  - ws_url
  - wss_url
  - ws_token
  - expire_time
  - heartbeat_interval
- 验收标准:
  - 响应字段符合文档
  - ws_token 可用于后续握手

### TASK-ID: CHAT-SVC-WSVERIFY-001
- 状态: blocked
- 优先级: P0
- 所属子域: ws 配置与 ws_token
- 目标: 实现 ws_token 校验服务
- 依赖:
  - CHAT-SVC-WSTOKEN-001
- 输入:
  - ws_token
- 输出:
  - 握手时可用的 ws_token 验证能力
- 验收标准:
  - 能解析用户、device_session、过期时间
  - 失效时返回明确错误码

### 7.5.4 私聊会话列表与消息详情

### TASK-ID: CHAT-SVC-CONVKEY-001
- 状态: blocked
- 优先级: P0
- 所属子域: 私聊会话列表与消息详情
- 目标: 实现 conversation_key 生成服务
- 依赖:
  - CHAT-DB-CONV-001
- 输入:
  - user_a_id
  - user_b_id
  - job_id
- 输出:
  - 会话 key 生成规则可用
- 验收标准:
  - 同一会话 key 稳定可重现
  - 双方顺序无关、结果一致

### TASK-ID: CHAT-REQ-CONVLIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 私聊会话列表与消息详情
- 目标: 编写会话列表查询参数校验 Request
- 依赖:
  - CHAT-DB-CONV-001
- 输入:
  - 会话列表接口文档
- 输出:
  - 会话列表查询校验类可用
- 建议校验至少包含:
  - keyword
  - page
  - limit
- 验收标准:
  - 分页参数明确

### TASK-ID: CHAT-RES-CONVLIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 私聊会话列表与消息详情
- 目标: 实现会话列表资源输出类
- 依赖:
  - CHAT-MODEL-CONV-001
  - JOB-RES-SUMMARY-001
  - AUTH-RES-USER-001
- 输入:
  - 会话列表展示字段要求
- 输出:
  - `ConversationResource` 可用
- 验收标准:
  - 输出对方用户摘要、职位摘要、最后消息、未读数、最后时间

### TASK-ID: CHAT-SVC-CONVLIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 私聊会话列表与消息详情
- 目标: 实现会话列表查询服务
- 依赖:
  - CHAT-REQ-CONVLIST-001
  - CHAT-MODEL-CONV-001
- 输入:
  - 当前用户
  - keyword/page/limit
- 输出:
  - 会话列表逻辑可用
- 验收标准:
  - 仅返回当前用户参与的会话
  - 按 last_message_time 倒序

### TASK-ID: CHAT-API-CONVLIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 私聊会话列表与消息详情
- 目标: 实现会话列表接口 `GET /api/chat/conversations`
- 依赖:
  - CHAT-SVC-CONVLIST-001
  - CHAT-RES-CONVLIST-001
  - BOOT-AUTH-002
- 输入:
  - 查询参数
- 输出:
  - 会话列表接口可用
- 验收标准:
  - 分页结构统一

### TASK-ID: CHAT-REQ-MSGHISTORY-001
- 状态: blocked
- 优先级: P0
- 所属子域: 私聊会话列表与消息详情
- 目标: 编写私聊消息详情/历史查询参数校验 Request
- 依赖:
  - CHAT-DB-CONV-001
- 输入:
  - 消息详情接口文档
- 输出:
  - 历史查询校验类可用
- 建议校验至少包含:
  - conversation_key 或 conversation_id
  - before_id
  - limit
- 验收标准:
  - 历史分页参数明确

### TASK-ID: CHAT-RES-MSG-001
- 状态: blocked
- 优先级: P0
- 所属子域: 私聊会话列表与消息详情
- 目标: 实现私聊消息资源输出类
- 依赖:
  - CHAT-MODEL-MSG-001
  - BOOT-FILE-RES-001
- 输入:
  - 私聊消息字段要求
- 输出:
  - `PrivateMessageResource` 可用
- 验收标准:
  - text/file/image 消息统一输出
  - 输出 `is_self`、`conversation_key` 等字段

### TASK-ID: CHAT-SVC-MSGHISTORY-001
- 状态: blocked
- 优先级: P0
- 所属子域: 私聊会话列表与消息详情
- 目标: 实现私聊消息历史查询服务
- 依赖:
  - CHAT-REQ-MSGHISTORY-001
  - CHAT-MODEL-MSG-001
- 输入:
  - 当前用户
  - conversation_key
  - before_id
  - limit
- 输出:
  - 私聊历史查询逻辑可用
- 验收标准:
  - 仅允许读取自己参与会话
  - 按时间顺序或文档要求输出

### TASK-ID: CHAT-API-MSGHISTORY-001
- 状态: blocked
- 优先级: P0
- 所属子域: 私聊会话列表与消息详情
- 目标: 实现私聊消息详情接口 `GET /api/chat/messages`
- 依赖:
  - CHAT-SVC-MSGHISTORY-001
  - CHAT-RES-MSG-001
  - BOOT-AUTH-002
- 输入:
  - 会话查询参数
- 输出:
  - 私聊消息历史接口可用
- 验收标准:
  - 消息列表结构统一

### 7.5.5 发送消息与回执

### TASK-ID: CHAT-REQ-SEND-001
- 状态: blocked
- 优先级: P0
- 所属子域: 发送消息与回执
- 目标: 编写私聊发送消息参数校验 Request
- 依赖:
  - CHAT-DB-MSG-001
  - BOOT-DB-FILES-001
- 输入:
  - 发送消息接口文档
- 输出:
  - 发送消息校验类可用
- 建议校验至少包含:
  - to_user_id
  - job_id
  - conversation_key（如复用）
  - client_msg_id
  - message_type
  - content
  - file_id
- 验收标准:
  - text/file/image 不同类型校验清晰
  - client_msg_id 为必填或按文档强制

### TASK-ID: CHAT-SVC-SENDCHECK-001
- 状态: blocked
- 优先级: P0
- 所属子域: 发送消息与回执
- 目标: 实现私聊发送前置条件校验服务
- 依赖:
  - APPLY-SVC-CONTACTUNLOCK-001
  - COMPANY-SVC-CERTCHECK-001
- 输入:
  - 发送人
  - 接收人
  - job_id
- 输出:
  - 私聊发送前置条件判断能力可用
- 验收标准:
  - 可按报名通过、可联系条件判断是否允许发起会话
- 非目标:
  - 本任务不强制实现全部产品策略细节，可保留扩展点

### TASK-ID: CHAT-SVC-CONVRESOLVE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 发送消息与回执
- 目标: 实现会话创建/获取服务
- 依赖:
  - CHAT-SVC-CONVKEY-001
  - CHAT-MODEL-CONV-001
- 输入:
  - 双方用户 id
  - job_id
- 输出:
  - 会话解析能力可用
- 验收标准:
  - 已有会话复用
  - 无会话时自动创建

### TASK-ID: CHAT-SVC-WSPUSH-001
- 状态: blocked
- 优先级: P0
- 所属子域: 发送消息与回执
- 目标: 实现统一 WebSocket 推送服务
- 依赖:
  - CHAT-SVC-WSVERIFY-001
- 输入:
  - target user_id / conversation / public channel
  - event
  - data
  - request_id
- 输出:
  - WebSocket 推送封装能力可用
- 验收标准:
  - 支持单播、双播、广播基础能力
  - 支持统一事件信封封装

### TASK-ID: CHAT-SVC-SEND-001
- 状态: blocked
- 优先级: P0
- 所属子域: 发送消息与回执
- 目标: 实现私聊发送消息服务
- 依赖:
  - CHAT-REQ-SEND-001
  - CHAT-SVC-SENDCHECK-001
  - CHAT-SVC-CONVRESOLVE-001
  - CHAT-MODEL-MSG-001
  - CHAT-SVC-WSPUSH-001
- 输入:
  - 当前用户
  - 发送参数
- 输出:
  - 私聊发送逻辑可用
- 业务要求至少包括:
  - client_msg_id 幂等控制
  - 创建消息记录
  - 更新会话 last_message*
  - 更新双方未读数
  - 触发 ws 回执与广播
- 验收标准:
  - 发送成功后可收到 `message.sent` 回执与 `message.created` 推送

### TASK-ID: CHAT-API-SEND-001
- 状态: blocked
- 优先级: P0
- 所属子域: 发送消息与回执
- 目标: 实现私聊发送消息 HTTP 接口 `POST /api/chat/send`
- 依赖:
  - CHAT-SVC-SEND-001
  - BOOT-AUTH-002
- 输入:
  - 发送参数
- 输出:
  - HTTP 发送接口可用
- 验收标准:
  - 返回消息资源与当前会话信息

### TASK-ID: CHAT-SVC-WSACK-001
- 状态: blocked
- 优先级: P0
- 所属子域: 发送消息与回执
- 目标: 实现 WebSocket 写操作回执封装服务
- 依赖:
  - CHAT-SVC-WSPUSH-001
- 输入:
  - event
  - request_id
  - data
- 输出:
  - `message.sent` / `system.error` 等回执封装能力可用
- 验收标准:
  - request_id 可原样带回
  - ts 统一输出

### 7.5.6 已读、撤回、删除

### TASK-ID: CHAT-REQ-READ-001
- 状态: blocked
- 优先级: P0
- 所属子域: 已读、撤回、删除
- 目标: 编写标记已读参数校验 Request
- 依赖:
  - CHAT-DB-MSG-001
- 输入:
  - 已读接口文档
- 输出:
  - 已读参数校验类可用
- 建议校验至少包含:
  - conversation_key
  - last_read_msg_id
- 验收标准:
  - 参数结构清晰

### TASK-ID: CHAT-SVC-READ-001
- 状态: blocked
- 优先级: P0
- 所属子域: 已读、撤回、删除
- 目标: 实现消息已读服务
- 依赖:
  - CHAT-REQ-READ-001
  - CHAT-MODEL-MSG-001
  - CHAT-MODEL-CONV-001
  - CHAT-SVC-WSPUSH-001
- 输入:
  - 当前用户
  - conversation_key
  - last_read_msg_id
- 输出:
  - 已读逻辑可用
- 业务要求至少包括:
  - 更新 read_at/最后已读游标
  - 清零对应未读数
  - 推送 `message.read`
- 验收标准:
  - 已读后会话未读数正确

### TASK-ID: CHAT-API-READ-001
- 状态: blocked
- 优先级: P0
- 所属子域: 已读、撤回、删除
- 目标: 实现已读接口 `PUT /api/chat/read`
- 依赖:
  - CHAT-SVC-READ-001
  - BOOT-AUTH-002
- 输入:
  - 已读参数
- 输出:
  - 已读接口可用
- 验收标准:
  - 返回最后已读消息与未读数

### TASK-ID: CHAT-REQ-RECALL-001
- 状态: blocked
- 优先级: P1
- 所属子域: 已读、撤回、删除
- 目标: 编写消息撤回参数校验 Request
- 依赖:
  - CHAT-DB-MSG-001
- 输入:
  - 撤回接口文档
- 输出:
  - 撤回参数校验类可用
- 建议校验至少包含:
  - message_id
- 验收标准:
  - 参数结构明确

### TASK-ID: CHAT-SVC-RECALL-001
- 状态: blocked
- 优先级: P1
- 所属子域: 已读、撤回、删除
- 目标: 实现消息撤回服务
- 依赖:
  - CHAT-REQ-RECALL-001
  - CHAT-MODEL-MSG-001
  - CHAT-SVC-WSPUSH-001
- 输入:
  - 当前用户
  - message_id
- 输出:
  - 撤回逻辑可用
- 业务要求至少包括:
  - 仅允许撤回自己发送的消息
  - 设置 is_recalled/recalled_at
  - 推送 `message.recalled`
- 验收标准:
  - 撤回后历史消息展示状态正确

### TASK-ID: CHAT-API-RECALL-001
- 状态: blocked
- 优先级: P1
- 所属子域: 已读、撤回、删除
- 目标: 实现撤回接口 `PUT /api/chat/recall`
- 依赖:
  - CHAT-SVC-RECALL-001
  - BOOT-AUTH-002
- 输入:
  - message_id
- 输出:
  - 撤回接口可用
- 验收标准:
  - 返回撤回后的消息状态

### TASK-ID: CHAT-REQ-DELETE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 已读、撤回、删除
- 目标: 编写消息删除参数校验 Request
- 依赖:
  - CHAT-DB-MSG-001
- 输入:
  - 删除接口文档
- 输出:
  - 删除参数校验类可用
- 建议校验至少包含:
  - message_id
- 验收标准:
  - 参数结构明确

### TASK-ID: CHAT-SVC-DELETE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 已读、撤回、删除
- 目标: 实现消息删除服务
- 依赖:
  - CHAT-REQ-DELETE-001
  - CHAT-MODEL-MSG-001
  - CHAT-SVC-WSPUSH-001
- 输入:
  - 当前用户
  - message_id
- 输出:
  - 单边删除逻辑可用
- 业务要求至少包括:
  - 仅标记当前用户侧删除
  - 推送 `message.deleted`
- 验收标准:
  - 删除后当前用户历史列表不再显示或按产品规则隐藏

### TASK-ID: CHAT-API-DELETE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 已读、撤回、删除
- 目标: 实现删除消息接口 `DELETE /api/chat/message`
- 依赖:
  - CHAT-SVC-DELETE-001
  - BOOT-AUTH-002
- 输入:
  - message_id
- 输出:
  - 删除接口可用
- 验收标准:
  - 返回成功结构统一

### 7.5.7 公共聊天室

### TASK-ID: CHAT-REQ-PUBLICLIST-001
- 状态: blocked
- 优先级: P1
- 所属子域: 公共聊天室
- 目标: 编写公共聊天室消息列表查询参数校验 Request
- 依赖:
  - CHAT-DB-PUBLIC-001
- 输入:
  - 公共聊天室接口文档
- 输出:
  - 公共消息查询校验类可用
- 验收标准:
  - before_id / limit 等分页参数明确

### TASK-ID: CHAT-RES-PUBLICMSG-001
- 状态: blocked
- 优先级: P1
- 所属子域: 公共聊天室
- 目标: 实现公共聊天室消息资源输出类
- 依赖:
  - CHAT-MODEL-PUBLIC-001
  - BOOT-FILE-RES-001
- 输入:
  - 公共消息输出要求
- 输出:
  - `PublicMessageResource` 可用
- 验收标准:
  - text/image/file 类型统一输出

### TASK-ID: CHAT-SVC-PUBLICLIST-001
- 状态: blocked
- 优先级: P1
- 所属子域: 公共聊天室
- 目标: 实现公共聊天室历史消息查询服务
- 依赖:
  - CHAT-REQ-PUBLICLIST-001
  - CHAT-MODEL-PUBLIC-001
- 输入:
  - before_id
  - limit
- 输出:
  - 公共消息列表逻辑可用
- 验收标准:
  - 支持分页拉取

### TASK-ID: CHAT-API-PUBLICLIST-001
- 状态: blocked
- 优先级: P1
- 所属子域: 公共聊天室
- 目标: 实现公共聊天室历史消息接口
- 依赖:
  - CHAT-SVC-PUBLICLIST-001
  - CHAT-RES-PUBLICMSG-001
  - BOOT-AUTH-002
- 输入:
  - 查询参数
- 输出:
  - 公共聊天室列表接口可用
- 验收标准:
  - 响应结构统一

### TASK-ID: CHAT-REQ-PUBLICSEND-001
- 状态: blocked
- 优先级: P1
- 所属子域: 公共聊天室
- 目标: 编写公共聊天室发言参数校验 Request
- 依赖:
  - CHAT-DB-PUBLIC-001
- 输入:
  - 公共聊天发送接口文档
- 输出:
  - 公共发言校验类可用
- 验收标准:
  - client_msg_id、message_type、content/file_id 校验明确

### TASK-ID: CHAT-SVC-PUBLICSEND-001
- 状态: blocked
- 优先级: P1
- 所属子域: 公共聊天室
- 目标: 实现公共聊天室发送消息服务
- 依赖:
  - CHAT-REQ-PUBLICSEND-001
  - CHAT-MODEL-PUBLIC-001
  - CHAT-SVC-WSPUSH-001
- 输入:
  - 当前用户
  - 公共消息参数
- 输出:
  - 公共聊天室发送逻辑可用
- 验收标准:
  - 可广播 `message.created` 或公共频道专用事件

### TASK-ID: CHAT-API-PUBLICSEND-001
- 状态: blocked
- 优先级: P1
- 所属子域: 公共聊天室
- 目标: 实现公共聊天室发送接口
- 依赖:
  - CHAT-SVC-PUBLICSEND-001
  - BOOT-AUTH-002
- 输入:
  - 公共消息参数
- 输出:
  - 公共聊天室发送接口可用
- 验收标准:
  - 成功返回消息资源

### 7.5.8 在线状态与人数统计

### TASK-ID: CHAT-SVC-PRESENCE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 在线状态与人数统计
- 目标: 实现用户在线状态更新服务
- 依赖:
  - CHAT-DB-PRESENCE-001
  - AUTH-MODEL-SESSION-001
- 输入:
  - user_id
  - device_session_id
  - online/offline
- 输出:
  - 在线状态维护逻辑可用
- 验收标准:
  - 连接建立/断开时状态可更新
  - last_online_time 可回写或缓存维护

### TASK-ID: CHAT-SVC-PRESENCECOUNT-001
- 状态: blocked
- 优先级: P1
- 所属子域: 在线状态与人数统计
- 目标: 实现在线人数统计服务
- 依赖:
  - CHAT-DB-PRESENCE-001
- 输入:
  - 当前在线用户集合
- 输出:
  - `presence.count` 所需聚合数据可用
- 验收标准:
  - 可输出 total/seeker/employer 统计

### TASK-ID: CHAT-API-PUBLICONLINE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 在线状态与人数统计
- 目标: 实现公共聊天室在线人数接口
- 依赖:
  - CHAT-SVC-PRESENCECOUNT-001
  - BOOT-AUTH-002
- 输入:
  - 当前登录用户
- 输出:
  - 在线人数接口可用
- 验收标准:
  - 返回 `total/seeker/employer`

### 7.5.9 WebSocket 网关与业务分发骨架

### TASK-ID: CHAT-WS-BOOT-001
- 状态: blocked
- 优先级: P0
- 所属子域: WebSocket 网关与业务分发骨架
- 目标: 建立 Workerman/GatewayWorker 基础目录与启动骨架
- 依赖:
  - BOOT-APP-002
- 输入:
  - 技术架构文档中的 GatewayWorker 要求
- 输出:
  - websocket 目录与启动入口可用
- 建议产出物:
  - `websocket/start.php`
  - `websocket/Events.php`
  - `websocket/Services/...`
- 验收标准:
  - 能作为后续启动脚本基础

### TASK-ID: CHAT-WS-HANDSHAKE-001
- 状态: blocked
- 优先级: P0
- 所属子域: WebSocket 网关与业务分发骨架
- 目标: 实现 WebSocket 握手鉴权流程
- 依赖:
  - CHAT-WS-BOOT-001
  - CHAT-SVC-WSVERIFY-001
  - CHAT-SVC-PRESENCE-001
- 输入:
  - ws_token
- 输出:
  - 握手鉴权逻辑可用
- 业务要求至少包括:
  - 验证 ws_token
  - 绑定 user_id / device_session_id
  - 鉴权失败返回 9001 并断开
  - 鉴权成功维护在线状态
- 验收标准:
  - 非法 token 无法建立连接
  - 合法 token 可建立连接

### TASK-ID: CHAT-WS-ROUTER-001
- 状态: blocked
- 优先级: P0
- 所属子域: WebSocket 网关与业务分发骨架
- 目标: 实现 WebSocket action 路由分发器
- 依赖:
  - CHAT-WS-BOOT-001
- 输入:
  - action
  - data
  - request_id
- 输出:
  - ping/message.send/message.read/join/leave/recall/delete 等动作可路由
- 验收标准:
  - 未知 action 返回 `system.error`
  - 已知 action 可分发到对应服务

### TASK-ID: CHAT-WS-PING-001
- 状态: blocked
- 优先级: P0
- 所属子域: WebSocket 网关与业务分发骨架
- 目标: 实现 `ping -> pong` 心跳处理
- 依赖:
  - CHAT-WS-ROUTER-001
- 输入:
  - ping action
- 输出:
  - pong 回执可用
- 验收标准:
  - ts/request_id 回传规则符合文档

### TASK-ID: CHAT-WS-ERROR-001
- 状态: blocked
- 优先级: P0
- 所属子域: WebSocket 网关与业务分发骨架
- 目标: 实现统一 `system.error` 事件封装
- 依赖:
  - CHAT-SVC-WSACK-001
  - BOOT-ERRORCODE-001
- 输入:
  - ws 错误场景
- 输出:
  - ws 错误封装能力可用
- 验收标准:
  - 返回 code/msg/request_id/ts
  - 限流场景可返回 retry_after

### TASK-ID: CHAT-WS-KICKOUT-001
- 状态: blocked
- 优先级: P1
- 所属子域: WebSocket 网关与业务分发骨架
- 目标: 实现 `system.kickout` 事件与强制断开逻辑
- 依赖:
  - CHAT-SVC-WSPUSH-001
  - AUTH-MODEL-SESSION-001
- 输入:
  - device_session 失效
  - 密码修改
  - 管理员强制下线等场景
- 输出:
  - 强制下线能力可用
- 验收标准:
  - 先推送 `system.kickout`
  - 再断开连接

### 7.5.10 聊天测试与联调

### TASK-ID: CHAT-TEST-WSCONFIG-001
- 状态: blocked
- 优先级: P0
- 所属子域: 聊天测试与联调
- 目标: 编写 ws 配置接口测试
- 依赖:
  - CHAT-API-WSCONFIG-001
  - BOOT-TEST-002
- 输入:
  - ws 配置接口
- 输出:
  - Feature Test
- 验收标准:
  - 返回 ws_url/ws_token/heartbeat_interval 等字段

### TASK-ID: CHAT-TEST-CONVLIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 聊天测试与联调
- 目标: 编写会话列表接口测试
- 依赖:
  - CHAT-API-CONVLIST-001
  - BOOT-TEST-002
- 输入:
  - 会话列表接口
- 输出:
  - Feature Test
- 验收标准:
  - 仅返回当前用户会话
  - 按最后消息时间排序

### TASK-ID: CHAT-TEST-MSGHISTORY-001
- 状态: blocked
- 优先级: P0
- 所属子域: 聊天测试与联调
- 目标: 编写消息历史接口测试
- 依赖:
  - CHAT-API-MSGHISTORY-001
  - BOOT-TEST-002
- 输入:
  - 消息历史接口
- 输出:
  - Feature Test
- 验收标准:
  - 仅可读取自己参与会话
  - 历史分页正确

### TASK-ID: CHAT-TEST-SEND-001
- 状态: blocked
- 优先级: P0
- 所属子域: 聊天测试与联调
- 目标: 编写发送消息服务/接口测试
- 依赖:
  - CHAT-API-SEND-001
  - BOOT-TEST-002
- 输入:
  - 发送消息接口
- 输出:
  - Feature Test
- 验收标准:
  - 发送成功
  - client_msg_id 幂等生效
  - 不满足联系条件时发送失败

### TASK-ID: CHAT-TEST-READ-001
- 状态: blocked
- 优先级: P1
- 所属子域: 聊天测试与联调
- 目标: 编写已读接口测试
- 依赖:
  - CHAT-API-READ-001
  - BOOT-TEST-002
- 输入:
  - 已读接口
- 输出:
  - Feature Test
- 验收标准:
  - 已读后未读数归零或正确变更

### TASK-ID: CHAT-TEST-RECALL-001
- 状态: blocked
- 优先级: P1
- 所属子域: 聊天测试与联调
- 目标: 编写撤回接口测试
- 依赖:
  - CHAT-API-RECALL-001
  - BOOT-TEST-002
- 输入:
  - 撤回接口
- 输出:
  - Feature Test
- 验收标准:
  - 仅能撤回自己的消息
  - 撤回状态正确

### TASK-ID: CHAT-TEST-PUBLIC-001
- 状态: blocked
- 优先级: P1
- 所属子域: 聊天测试与联调
- 目标: 编写公共聊天室接口测试
- 依赖:
  - CHAT-API-PUBLICLIST-001
  - CHAT-API-PUBLICSEND-001
- 输入:
  - 公共聊天室接口
- 输出:
  - Feature Test
- 验收标准:
  - 历史消息拉取成功
  - 发言成功

### TASK-ID: CHAT-TEST-WSMANUAL-001
- 状态: blocked
- 优先级: P1
- 所属子域: 聊天测试与联调
- 目标: 编写 WebSocket 手工联调说明文档
- 依赖:
  - CHAT-WS-HANDSHAKE-001
  - CHAT-WS-ROUTER-001
- 输入:
  - ws 连接地址与事件契约
- 输出:
  - WebSocket 联调说明文档
- 产出物:
  - `docs/ws-debug-guide.md`
- 验收标准:
  - 能指导开发人员测试 ping/send/read/recall/kickout 等事件

## 7.6 本章推荐执行顺序

### 第一批（P0）
1. CHAT-DB-CONV-001
2. CHAT-DB-MSG-001
3. CHAT-MODEL-CONV-001
4. CHAT-MODEL-MSG-001
5. CHAT-SVC-WSTOKEN-001
6. CHAT-API-WSCONFIG-001
7. CHAT-SVC-WSVERIFY-001
8. CHAT-SVC-CONVKEY-001
9. CHAT-REQ-CONVLIST-001
10. CHAT-RES-CONVLIST-001
11. CHAT-SVC-CONVLIST-001
12. CHAT-API-CONVLIST-001
13. CHAT-REQ-MSGHISTORY-001
14. CHAT-RES-MSG-001
15. CHAT-SVC-MSGHISTORY-001
16. CHAT-API-MSGHISTORY-001
17. CHAT-REQ-SEND-001
18. CHAT-SVC-SENDCHECK-001
19. CHAT-SVC-CONVRESOLVE-001
20. CHAT-SVC-WSPUSH-001
21. CHAT-SVC-WSACK-001
22. CHAT-SVC-SEND-001
23. CHAT-API-SEND-001
24. CHAT-REQ-READ-001
25. CHAT-SVC-READ-001
26. CHAT-API-READ-001
27. CHAT-WS-BOOT-001
28. CHAT-WS-HANDSHAKE-001
29. CHAT-WS-ROUTER-001
30. CHAT-WS-PING-001
31. CHAT-WS-ERROR-001
32. CHAT-TEST-WSCONFIG-001
33. CHAT-TEST-CONVLIST-001
34. CHAT-TEST-MSGHISTORY-001
35. CHAT-TEST-SEND-001

### 第二批（P1）
1. CHAT-DB-PUBLIC-001
2. CHAT-DB-PRESENCE-001
3. CHAT-MODEL-PUBLIC-001
4. CHAT-REQ-RECALL-001
5. CHAT-SVC-RECALL-001
6. CHAT-API-RECALL-001
7. CHAT-REQ-DELETE-001
8. CHAT-SVC-DELETE-001
9. CHAT-API-DELETE-001
10. CHAT-REQ-PUBLICLIST-001
11. CHAT-RES-PUBLICMSG-001
12. CHAT-SVC-PUBLICLIST-001
13. CHAT-API-PUBLICLIST-001
14. CHAT-REQ-PUBLICSEND-001
15. CHAT-SVC-PUBLICSEND-001
16. CHAT-API-PUBLICSEND-001
17. CHAT-SVC-PRESENCE-001
18. CHAT-SVC-PRESENCECOUNT-001
19. CHAT-API-PUBLICONLINE-001
20. CHAT-WS-KICKOUT-001
21. CHAT-TEST-READ-001
22. CHAT-TEST-RECALL-001
23. CHAT-TEST-PUBLIC-001
24. CHAT-TEST-WSMANUAL-001

## 7.7 本章完成后的下一个章节入口
当第 7 章完成后，最合理进入：
- 第8章：评论 / 举报 / 黑名单
- 或 第9章：收藏 / 通知 / 浏览历史

建议优先顺序：
1. 第9章：收藏 / 通知 / 浏览历史
2. 第8章：评论 / 举报 / 黑名单

原因：
- 通知与未读状态会直接承接聊天、报名、职位审核等主链路事件
- 评论/举报/黑名单更偏内容治理增强，可稍后接入

---

# 第9章：收藏 / 通知 / 浏览历史

> 目标：建立职位收藏、人才收藏、浏览历史、系统通知、报名通知、审核通知、聊天相关通知、未读通知数与通知已读/删除等基础能力。
> 本章重点处理“用户感知层”的消息与留痕能力，为前台消息中心、提醒入口和后续运营通知打底。

## 9.1 本章定位

### 本章要解决的问题
- 建立收藏数据结构
- 建立浏览历史数据结构
- 建立系统通知数据结构
- 支持职位收藏/取消收藏
- 支持人才收藏/取消收藏
- 支持浏览历史记录与清空
- 支持通知列表、通知详情、标记已读、全部已读、删除
- 建立主链路事件到通知的派发入口
- 提供通知未读数基础能力

### 本章不做的内容
- 后台公告/广播运营系统
- 评论/举报本身
- 高级推荐算法
- 推送到第三方消息渠道

## 9.2 本章完成标准
当第 9 章完成时，应满足：
1. 收藏、浏览历史、通知表可用
2. 用户可收藏职位并取消收藏
3. 雇主可收藏人才并取消收藏
4. 用户可查看浏览历史并清空
5. 用户可查看通知列表、详情、未读数
6. 用户可标记单条已读、全部已读、删除通知
7. 报名/审核/聊天等事件可挂接到统一通知派发入口
8. 关键接口具备基础测试覆盖

## 9.3 本章依赖关系
### 上游依赖
- 第1章：项目骨架与基础设施
- 第2章：用户与认证
- 第5章：职位系统
- 第6章：报名系统
- 第7章：聊天与 WebSocket

### 下游依赖
- 后台公告与广播
- 用户首页红点/未读提醒
- 运营消息增强

## 9.4 本章任务总览
1. 收藏数据结构与表设计
2. 浏览历史结构与表设计
3. 通知数据结构与表设计
4. 职位收藏
5. 人才收藏
6. 浏览历史
7. 通知列表与详情
8. 通知已读与删除
9. 通知派发入口
10. 测试

## 9.5 可直接执行任务清单

### 9.5.1 收藏数据结构与表设计

### TASK-ID: FAVOR-DB-BASE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 收藏数据结构与表设计
- 目标: 创建 `favorites` 表 migration
- 依赖:
  - BOOT-DB-001
  - AUTH-DB-USER-001
  - JOB-DB-BASE-001
- 输入:
  - 收藏接口文档
- 输出:
  - favorites 表可支撑职位收藏与人才收藏
- 建议字段至少包含:
  - id
  - user_id
  - target_type
  - target_id
  - target_user_id
  - job_id
  - category_id
  - created_at / updated_at
- 索引建议:
  - user_id 普通索引
  - target_type 普通索引
  - target_id 普通索引
  - user_id + target_type + target_id 唯一索引
- 验收标准:
  - 支持职位收藏与人才收藏统一建模
  - 避免重复收藏

### TASK-ID: FAVOR-MODEL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 收藏数据结构与表设计
- 目标: 创建 `Favorite` 模型
- 依赖:
  - FAVOR-DB-BASE-001
- 输入:
  - favorites 表结构
- 输出:
  - Favorite 模型可用
- 验收标准:
  - 可支撑收藏查询与去重

### 9.5.2 浏览历史结构与表设计

### TASK-ID: HISTORY-DB-BASE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 浏览历史结构与表设计
- 目标: 创建 `browse_histories` 表 migration
- 依赖:
  - BOOT-DB-001
  - AUTH-DB-USER-001
  - JOB-DB-BASE-001
- 输入:
  - 浏览历史接口文档
- 输出:
  - browse_histories 表可用
- 建议字段至少包含:
  - id
  - user_id
  - target_type
  - target_id
  - viewed_at
  - created_at / updated_at
- 索引建议:
  - user_id 普通索引
  - target_type 普通索引
  - target_id 普通索引
  - viewed_at 普通索引
- 验收标准:
  - 可支撑职位浏览历史
  - 后续可扩展到人才等其他目标

### TASK-ID: HISTORY-MODEL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 浏览历史结构与表设计
- 目标: 创建 `BrowseHistory` 模型
- 依赖:
  - HISTORY-DB-BASE-001
- 输入:
  - browse_histories 表结构
- 输出:
  - BrowseHistory 模型可用
- 验收标准:
  - 可支撑历史查询与清空

### 9.5.3 通知数据结构与表设计

### TASK-ID: NOTICE-DB-BASE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 通知数据结构与表设计
- 目标: 创建 `notifications` 表 migration
- 依赖:
  - BOOT-DB-001
  - AUTH-DB-USER-001
- 输入:
  - 通知接口文档
  - ws/报名/审核/系统通知需求
- 输出:
  - notifications 表可用
- 建议字段至少包含:
  - id
  - user_id
  - type
  - title
  - content
  - link_url
  - related_type
  - related_id
  - data_json
  - is_read
  - read_at
  - deleted_at
  - created_at / updated_at
- 索引建议:
  - user_id 普通索引
  - type 普通索引
  - is_read 普通索引
  - created_at 普通索引
- 验收标准:
  - 可支撑系统、报名、审核、聊天等通知统一建模

### TASK-ID: NOTICE-MODEL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 通知数据结构与表设计
- 目标: 创建 `Notification` 模型
- 依赖:
  - NOTICE-DB-BASE-001
- 输入:
  - notifications 表结构
- 输出:
  - Notification 模型可用
- 验收标准:
  - 支持用户通知查询、未读统计、标记已读

### 9.5.4 职位收藏

### TASK-ID: FAVOR-REQ-JOB-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位收藏
- 目标: 编写职位收藏参数校验 Request
- 依赖:
  - FAVOR-DB-BASE-001
- 输入:
  - 职位收藏接口文档
- 输出:
  - 职位收藏校验类可用
- 建议校验至少包含:
  - job_id
- 验收标准:
  - 参数结构明确

### TASK-ID: FAVOR-SVC-JOB-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位收藏
- 目标: 实现职位收藏服务
- 依赖:
  - FAVOR-REQ-JOB-001
  - FAVOR-MODEL-001
  - JOB-MODEL-BASE-001
- 输入:
  - 当前用户
  - job_id
- 输出:
  - 职位收藏逻辑可用
- 验收标准:
  - 可新增收藏
  - 重复收藏不会产生重复记录

### TASK-ID: FAVOR-API-JOB-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位收藏
- 目标: 实现职位收藏接口 `POST /api/favorite/job`
- 依赖:
  - FAVOR-SVC-JOB-001
  - BOOT-AUTH-002
- 输入:
  - job_id
- 输出:
  - 职位收藏接口可用
- 验收标准:
  - 返回统一结构

### TASK-ID: FAVOR-SVC-JOBCANCEL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位收藏
- 目标: 实现取消职位收藏服务
- 依赖:
  - FAVOR-MODEL-001
- 输入:
  - 当前用户
  - job_id
- 输出:
  - 取消职位收藏逻辑可用
- 验收标准:
  - 删除或失活当前收藏记录

### TASK-ID: FAVOR-API-JOBCANCEL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位收藏
- 目标: 实现取消职位收藏接口 `DELETE /api/favorite/job`
- 依赖:
  - FAVOR-SVC-JOBCANCEL-001
  - BOOT-AUTH-002
- 输入:
  - job_id
- 输出:
  - 取消职位收藏接口可用
- 验收标准:
  - 返回统一结构

### TASK-ID: FAVOR-API-JOBLIST-001
- 状态: blocked
- 优先级: P1
- 所属子域: 职位收藏
- 目标: 实现职位收藏列表接口
- 依赖:
  - FAVOR-MODEL-001
  - JOB-RES-SUMMARY-001
  - BOOT-AUTH-002
- 输入:
  - page/limit
- 输出:
  - 职位收藏列表接口可用
- 验收标准:
  - 可分页返回收藏职位摘要

### 9.5.5 人才收藏

### TASK-ID: FAVOR-REQ-TALENT-001
- 状态: blocked
- 优先级: P1
- 所属子域: 人才收藏
- 目标: 编写人才收藏参数校验 Request
- 依赖:
  - FAVOR-DB-BASE-001
- 输入:
  - 人才收藏接口文档
- 输出:
  - 人才收藏校验类可用
- 建议校验至少包含:
  - target_user_id
  - category_id（如文档要求）
- 验收标准:
  - 参数结构明确

### TASK-ID: FAVOR-SVC-TALENT-001
- 状态: blocked
- 优先级: P1
- 所属子域: 人才收藏
- 目标: 实现人才收藏服务
- 依赖:
  - FAVOR-REQ-TALENT-001
  - FAVOR-MODEL-001
  - RESUME-SVC-READ-EMPLOYER-001
- 输入:
  - 当前雇主
  - target_user_id
- 输出:
  - 人才收藏逻辑可用
- 验收标准:
  - 仅允许雇主或符合条件角色收藏人才
  - 重复收藏不产生重复记录

### TASK-ID: FAVOR-API-TALENT-001
- 状态: blocked
- 优先级: P1
- 所属子域: 人才收藏
- 目标: 实现人才收藏接口
- 依赖:
  - FAVOR-SVC-TALENT-001
  - BOOT-AUTH-002
- 输入:
  - target_user_id
- 输出:
  - 人才收藏接口可用
- 验收标准:
  - 返回统一结构

### TASK-ID: FAVOR-API-TALENTLIST-001
- 状态: blocked
- 优先级: P1
- 所属子域: 人才收藏
- 目标: 实现人才收藏列表接口
- 依赖:
  - FAVOR-MODEL-001
  - RESUME-RES-SUMMARY-001
  - BOOT-AUTH-002
- 输入:
  - page/limit
- 输出:
  - 人才收藏列表接口可用
- 验收标准:
  - 可分页返回人才摘要

### 9.5.6 浏览历史

### TASK-ID: HISTORY-SVC-WRITE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 浏览历史
- 目标: 实现浏览历史写入服务
- 依赖:
  - HISTORY-MODEL-001
  - JOB-SVC-VIEW-001
- 输入:
  - 当前用户
  - target_type
  - target_id
- 输出:
  - 浏览历史写入逻辑可用
- 验收标准:
  - 可避免短时间重复刷出多条相同历史
  - 可与职位浏览记录协同但不重复冲突

### TASK-ID: HISTORY-API-LIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 浏览历史
- 目标: 实现浏览历史列表接口
- 依赖:
  - HISTORY-MODEL-001
  - JOB-RES-SUMMARY-001
  - BOOT-AUTH-002
- 输入:
  - page/limit
- 输出:
  - 浏览历史列表接口可用
- 验收标准:
  - 可分页返回历史记录
  - 按 viewed_at 倒序

### TASK-ID: HISTORY-API-CLEAR-001
- 状态: blocked
- 优先级: P1
- 所属子域: 浏览历史
- 目标: 实现清空浏览历史接口
- 依赖:
  - HISTORY-MODEL-001
  - BOOT-AUTH-002
- 输入:
  - 当前用户
- 输出:
  - 清空浏览历史接口可用
- 验收标准:
  - 仅清空当前用户历史

### 9.5.7 通知列表与详情

### TASK-ID: NOTICE-REQ-LIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 通知列表与详情
- 目标: 编写通知列表查询参数校验 Request
- 依赖:
  - NOTICE-DB-BASE-001
- 输入:
  - 通知列表接口文档
- 输出:
  - 通知列表查询校验类可用
- 建议校验至少包含:
  - type
  - is_read
  - page
  - limit
- 验收标准:
  - 筛选结构明确

### TASK-ID: NOTICE-RES-ITEM-001
- 状态: blocked
- 优先级: P0
- 所属子域: 通知列表与详情
- 目标: 实现通知资源输出类
- 依赖:
  - NOTICE-MODEL-001
- 输入:
  - 通知字段要求
- 输出:
  - `NotificationResource` 可用
- 验收标准:
  - 输出 type/title/content/link_url/is_read/created_at 等字段

### TASK-ID: NOTICE-SVC-LIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 通知列表与详情
- 目标: 实现通知列表查询服务
- 依赖:
  - NOTICE-REQ-LIST-001
  - NOTICE-MODEL-001
- 输入:
  - 当前用户
  - type/is_read/page/limit
- 输出:
  - 通知列表逻辑可用
- 验收标准:
  - 支持筛选与分页
  - 按创建时间倒序

### TASK-ID: NOTICE-API-LIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 通知列表与详情
- 目标: 实现通知列表接口 `GET /api/notification/list`
- 依赖:
  - NOTICE-SVC-LIST-001
  - NOTICE-RES-ITEM-001
  - BOOT-AUTH-002
- 输入:
  - 查询参数
- 输出:
  - 通知列表接口可用
- 验收标准:
  - 响应结构统一

### TASK-ID: NOTICE-API-DETAIL-001
- 状态: blocked
- 优先级: P1
- 所属子域: 通知列表与详情
- 目标: 实现通知详情接口
- 依赖:
  - NOTICE-MODEL-001
  - NOTICE-RES-ITEM-001
  - BOOT-AUTH-002
- 输入:
  - notification_id
- 输出:
  - 通知详情接口可用
- 验收标准:
  - 仅允许查看自己的通知

### TASK-ID: NOTICE-SVC-UNREAD-001
- 状态: blocked
- 优先级: P0
- 所属子域: 通知列表与详情
- 目标: 实现通知未读数统计服务
- 依赖:
  - NOTICE-MODEL-001
- 输入:
  - 当前用户
- 输出:
  - 未读数统计能力可用
- 验收标准:
  - 可输出 total_unread
  - 如需要，可扩展 type 维度未读数

### TASK-ID: NOTICE-API-UNREAD-001
- 状态: blocked
- 优先级: P0
- 所属子域: 通知列表与详情
- 目标: 实现通知未读数接口
- 依赖:
  - NOTICE-SVC-UNREAD-001
  - BOOT-AUTH-002
- 输入:
  - 当前用户
- 输出:
  - 未读数接口可用
- 验收标准:
  - 返回 total_unread

### 9.5.8 通知已读与删除

### TASK-ID: NOTICE-SVC-READONE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 通知已读与删除
- 目标: 实现单条通知已读服务
- 依赖:
  - NOTICE-MODEL-001
- 输入:
  - 当前用户
  - notification_id
- 输出:
  - 单条已读逻辑可用
- 验收标准:
  - 设置 is_read/read_at
  - 仅允许处理自己的通知

### TASK-ID: NOTICE-API-READONE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 通知已读与删除
- 目标: 实现单条通知已读接口
- 依赖:
  - NOTICE-SVC-READONE-001
  - BOOT-AUTH-002
- 输入:
  - notification_id
- 输出:
  - 单条已读接口可用
- 验收标准:
  - 返回成功结构统一

### TASK-ID: NOTICE-SVC-READALL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 通知已读与删除
- 目标: 实现全部通知已读服务
- 依赖:
  - NOTICE-MODEL-001
- 输入:
  - 当前用户
- 输出:
  - 全部已读逻辑可用
- 验收标准:
  - 当前用户未读通知全部置为已读

### TASK-ID: NOTICE-API-READALL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 通知已读与删除
- 目标: 实现全部通知已读接口
- 依赖:
  - NOTICE-SVC-READALL-001
  - BOOT-AUTH-002
- 输入:
  - 当前用户
- 输出:
  - 全部已读接口可用
- 验收标准:
  - 返回 success / affected_count

### TASK-ID: NOTICE-SVC-DELETE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 通知已读与删除
- 目标: 实现通知删除服务
- 依赖:
  - NOTICE-MODEL-001
- 输入:
  - 当前用户
  - notification_id
- 输出:
  - 通知删除逻辑可用
- 验收标准:
  - 使用逻辑删除或文档要求方式删除

### TASK-ID: NOTICE-API-DELETE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 通知已读与删除
- 目标: 实现通知删除接口
- 依赖:
  - NOTICE-SVC-DELETE-001
  - BOOT-AUTH-002
- 输入:
  - notification_id
- 输出:
  - 删除接口可用
- 验收标准:
  - 返回统一结构

### 9.5.9 通知派发入口

### TASK-ID: NOTICE-SVC-DISPATCH-001
- 状态: blocked
- 优先级: P0
- 所属子域: 通知派发入口
- 目标: 实现统一通知派发服务
- 依赖:
  - NOTICE-MODEL-001
  - CHAT-SVC-WSPUSH-001
- 输入:
  - user_id
  - type
  - title
  - content
  - link_url
  - related_type
  - related_id
  - data_json
- 输出:
  - 通知创建与 ws 推送能力可用
- 验收标准:
  - 创建通知记录
  - 可推送 `notification.created`
  - 后续模块可复用统一入口

### TASK-ID: NOTICE-SVC-TEMPLATES-001
- 状态: blocked
- 优先级: P1
- 所属子域: 通知派发入口
- 目标: 建立核心通知模板组
- 依赖:
  - NOTICE-SVC-DISPATCH-001
- 输入:
  - 报名成功/审核通过/审核拒绝/系统通知等场景
- 输出:
  - 通知模板封装可用
- 验收标准:
  - 避免各模块散写 title/content 文案拼装

### TASK-ID: NOTICE-HOOK-APPLY-001
- 状态: blocked
- 优先级: P1
- 所属子域: 通知派发入口
- 目标: 将报名创建/取消/审核事件接入通知派发入口
- 依赖:
  - APPLY-SVC-NOTIFYHOOK-001
  - NOTICE-SVC-DISPATCH-001
- 输入:
  - 报名状态变化事件
- 输出:
  - 报名链路通知挂接完成
- 验收标准:
  - 报名成功、审核通过、审核拒绝至少三类通知可派发

### TASK-ID: NOTICE-HOOK-CHAT-001
- 状态: blocked
- 优先级: P1
- 所属子域: 通知派发入口
- 目标: 将聊天消息事件接入通知派发入口
- 依赖:
  - CHAT-SVC-SEND-001
  - NOTICE-SVC-DISPATCH-001
- 输入:
  - 新私聊消息事件
- 输出:
  - 聊天消息通知挂接完成
- 验收标准:
  - 离线/未读场景可生成通知记录或红点能力输入

### 9.5.10 测试

### TASK-ID: FAVOR-TEST-JOB-001
- 状态: blocked
- 优先级: P0
- 所属子域: 测试
- 目标: 编写职位收藏接口测试
- 依赖:
  - FAVOR-API-JOB-001
  - FAVOR-API-JOBCANCEL-001
  - BOOT-TEST-002
- 输入:
  - 职位收藏接口
- 输出:
  - Feature Test
- 验收标准:
  - 收藏成功
  - 重复收藏不重复写入
  - 取消收藏成功

### TASK-ID: FAVOR-TEST-TALENT-001
- 状态: blocked
- 优先级: P1
- 所属子域: 测试
- 目标: 编写人才收藏接口测试
- 依赖:
  - FAVOR-API-TALENT-001
  - BOOT-TEST-002
- 输入:
  - 人才收藏接口
- 输出:
  - Feature Test
- 验收标准:
  - 收藏成功
  - 列表查询成功

### TASK-ID: HISTORY-TEST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 测试
- 目标: 编写浏览历史接口测试
- 依赖:
  - HISTORY-API-LIST-001
  - HISTORY-API-CLEAR-001
  - BOOT-TEST-002
- 输入:
  - 浏览历史接口
- 输出:
  - Feature Test
- 验收标准:
  - 列表返回正确
  - 清空仅影响当前用户

### TASK-ID: NOTICE-TEST-LIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 测试
- 目标: 编写通知列表/未读数接口测试
- 依赖:
  - NOTICE-API-LIST-001
  - NOTICE-API-UNREAD-001
  - BOOT-TEST-002
- 输入:
  - 通知列表接口
- 输出:
  - Feature Test
- 验收标准:
  - 列表筛选生效
  - 未读数统计正确

### TASK-ID: NOTICE-TEST-READ-001
- 状态: blocked
- 优先级: P0
- 所属子域: 测试
- 目标: 编写通知已读接口测试
- 依赖:
  - NOTICE-API-READONE-001
  - NOTICE-API-READALL-001
  - BOOT-TEST-002
- 输入:
  - 通知已读接口
- 输出:
  - Feature Test
- 验收标准:
  - 单条已读成功
  - 全部已读成功

### TASK-ID: NOTICE-TEST-DISPATCH-001
- 状态: blocked
- 优先级: P1
- 所属子域: 测试
- 目标: 编写统一通知派发服务测试
- 依赖:
  - NOTICE-SVC-DISPATCH-001
- 输入:
  - 通知派发服务
- 输出:
  - Unit Test
- 验收标准:
  - 正确创建通知记录
  - 推送调用钩子被触发

## 9.6 本章推荐执行顺序

### 第一批（P0）
1. FAVOR-DB-BASE-001
2. FAVOR-MODEL-001
3. HISTORY-DB-BASE-001
4. HISTORY-MODEL-001
5. NOTICE-DB-BASE-001
6. NOTICE-MODEL-001
7. FAVOR-REQ-JOB-001
8. FAVOR-SVC-JOB-001
9. FAVOR-API-JOB-001
10. FAVOR-SVC-JOBCANCEL-001
11. FAVOR-API-JOBCANCEL-001
12. HISTORY-SVC-WRITE-001
13. HISTORY-API-LIST-001
14. NOTICE-REQ-LIST-001
15. NOTICE-RES-ITEM-001
16. NOTICE-SVC-LIST-001
17. NOTICE-API-LIST-001
18. NOTICE-SVC-UNREAD-001
19. NOTICE-API-UNREAD-001
20. NOTICE-SVC-READONE-001
21. NOTICE-API-READONE-001
22. NOTICE-SVC-READALL-001
23. NOTICE-API-READALL-001
24. NOTICE-SVC-DISPATCH-001
25. FAVOR-TEST-JOB-001
26. HISTORY-TEST-001
27. NOTICE-TEST-LIST-001
28. NOTICE-TEST-READ-001

### 第二批（P1）
1. FAVOR-API-JOBLIST-001
2. FAVOR-REQ-TALENT-001
3. FAVOR-SVC-TALENT-001
4. FAVOR-API-TALENT-001
5. FAVOR-API-TALENTLIST-001
6. HISTORY-API-CLEAR-001
7. NOTICE-API-DETAIL-001
8. NOTICE-SVC-DELETE-001
9. NOTICE-API-DELETE-001
10. NOTICE-SVC-TEMPLATES-001
11. NOTICE-HOOK-APPLY-001
12. NOTICE-HOOK-CHAT-001
13. FAVOR-TEST-TALENT-001
14. NOTICE-TEST-DISPATCH-001

## 9.7 本章完成后的下一个章节入口
当第 9 章完成后，最合理进入：
- 第8章：评论 / 举报 / 黑名单

原因：
- 收藏、通知、浏览历史完成后，前台用户感知层已经比较完整
- 接下来补平台治理相关的评论、举报、黑名单更顺

---

# 第8章：评论 / 举报 / 黑名单

> 目标：建立职位评论、评论回复、评论点赞、评论删除、举报职位/评论/用户/企业、黑名单前台展示与基础联动能力，为平台治理与内容安全提供用户侧闭环。
> 本章重点是前台可触达的治理能力与底层数据结构，不直接实现后台举报审核工作台的全部能力，但会把后台处理所需数据基础准备好。

## 8.1 本章定位

### 本章要解决的问题
- 建立评论与评论点赞数据结构
- 支持职位评论、回复评论、点赞/取消点赞、删除评论
- 建立举报数据结构
- 支持举报职位、评论、用户、企业
- 建立黑名单数据结构与前台查询能力
- 为后台举报处理、黑名单公示与风控联动提供数据基础

### 本章不做的内容
- 后台举报审核界面
- 后台黑名单维护界面
- 复杂 NLP 内容审核
- 申诉流程

## 8.2 本章完成标准
当第 8 章完成时，应满足：
1. 评论、评论点赞、举报、黑名单表可用
2. 用户可对职位发表评论和回复
3. 用户可点赞/取消点赞评论
4. 用户可删除自己的评论
5. 用户可发起举报
6. 前台可查询黑名单公示信息
7. 举报与黑名单数据可直接被后台治理模块复用
8. 关键接口具备基础测试覆盖

## 8.3 本章依赖关系
### 上游依赖
- 第1章：项目骨架与基础设施
- 第2章：用户与认证
- 第5章：职位系统
- 第9章：收藏 / 通知 / 浏览历史

### 下游依赖
- 后台举报处理
- 后台黑名单管理
- 内容安全与风控增强

## 8.4 本章任务总览
1. 评论数据结构与表设计
2. 举报与黑名单数据结构
3. 评论列表与详情
4. 发表评论与回复
5. 点赞/取消点赞
6. 删除评论
7. 举报创建与查询
8. 黑名单前台查询
9. 治理通知预留
10. 测试

## 8.5 可直接执行任务清单

### 8.5.1 评论数据结构与表设计

### TASK-ID: COMMENT-DB-BASE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 评论数据结构与表设计
- 目标: 创建 `comments` 表 migration
- 依赖:
  - BOOT-DB-001
  - AUTH-DB-USER-001
  - JOB-DB-BASE-001
- 输入:
  - 评论接口文档
- 输出:
  - comments 表可支撑职位评论与回复
- 建议字段至少包含:
  - id
  - user_id
  - job_id
  - parent_id
  - root_id
  - content
  - status
  - like_count
  - reply_count
  - deleted_at
  - created_at / updated_at
- 索引建议:
  - user_id 普通索引
  - job_id 普通索引
  - parent_id 普通索引
  - root_id 普通索引
  - created_at 普通索引
- 验收标准:
  - 支持一级评论与回复评论
  - 支持逻辑删除

### TASK-ID: COMMENT-DB-LIKE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 评论数据结构与表设计
- 目标: 创建 `comment_likes` 表 migration
- 依赖:
  - COMMENT-DB-BASE-001
  - AUTH-DB-USER-001
- 输入:
  - 评论点赞接口文档
- 输出:
  - comment_likes 表可用
- 建议字段至少包含:
  - id
  - user_id
  - comment_id
  - created_at
- 索引建议:
  - user_id 普通索引
  - comment_id 普通索引
  - user_id + comment_id 唯一索引
- 验收标准:
  - 同一用户对同一评论只能点赞一次

### TASK-ID: COMMENT-MODEL-BASE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 评论数据结构与表设计
- 目标: 创建 `Comment` 模型
- 依赖:
  - COMMENT-DB-BASE-001
- 输入:
  - comments 表结构
- 输出:
  - Comment 模型可用
- 验收标准:
  - 可支撑评论树、用户关联、职位关联

### TASK-ID: COMMENT-MODEL-LIKE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 评论数据结构与表设计
- 目标: 创建 `CommentLike` 模型
- 依赖:
  - COMMENT-DB-LIKE-001
- 输入:
  - comment_likes 表结构
- 输出:
  - CommentLike 模型可用
- 验收标准:
  - 可支撑点赞状态查询与去重

### 8.5.2 举报与黑名单数据结构

### TASK-ID: REPORT-DB-BASE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 举报与黑名单数据结构
- 目标: 创建 `reports` 表 migration
- 依赖:
  - BOOT-DB-001
  - AUTH-DB-USER-001
  - BOOT-DB-FILES-001
- 输入:
  - 举报接口文档
  - 举报状态机
- 输出:
  - reports 表可支撑举报主流程
- 建议字段至少包含:
  - id
  - reporter_id
  - target_type
  - target_id
  - target_user_id
  - reason
  - content
  - evidence_files_json 或独立关联策略
  - status
  - handle_result
  - handled_by
  - handled_at
  - created_at / updated_at
- 索引建议:
  - reporter_id 普通索引
  - target_type 普通索引
  - target_id 普通索引
  - status 普通索引
  - created_at 普通索引
- 验收标准:
  - 支持举报职位/评论/用户/企业
  - 可为后台处理提供完整输入

### TASK-ID: BLACK-DB-BASE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 举报与黑名单数据结构
- 目标: 创建 `blacklists` 表 migration
- 依赖:
  - BOOT-DB-001
  - AUTH-DB-USER-001
- 输入:
  - 黑名单与公示文档
- 输出:
  - blacklists 表可用
- 建议字段至少包含:
  - id
  - target_user_id
  - target_name_snapshot
  - reason
  - description
  - status
  - is_public
  - start_time
  - end_time
  - created_by
  - created_at / updated_at
- 索引建议:
  - target_user_id 普通索引
  - status 普通索引
  - is_public 普通索引
  - end_time 普通索引
- 验收标准:
  - 支持黑名单生效/失效、公示/不公示

### TASK-ID: REPORT-MODEL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 举报与黑名单数据结构
- 目标: 创建 `Report` 模型
- 依赖:
  - REPORT-DB-BASE-001
- 输入:
  - reports 表结构
- 输出:
  - Report 模型可用
- 验收标准:
  - 可支撑举报创建与查询

### TASK-ID: BLACK-MODEL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 举报与黑名单数据结构
- 目标: 创建 `Blacklist` 模型
- 依赖:
  - BLACK-DB-BASE-001
- 输入:
  - blacklists 表结构
- 输出:
  - Blacklist 模型可用
- 验收标准:
  - 可支撑前台公示查询

### 8.5.3 评论列表与详情

### TASK-ID: COMMENT-REQ-LIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 评论列表与详情
- 目标: 编写评论列表查询参数校验 Request
- 依赖:
  - COMMENT-DB-BASE-001
- 输入:
  - 评论列表接口文档
- 输出:
  - 评论列表查询校验类可用
- 建议校验至少包含:
  - job_id
  - page
  - limit
  - sort
- 验收标准:
  - 列表参数结构明确

### TASK-ID: COMMENT-RES-ITEM-001
- 状态: blocked
- 优先级: P0
- 所属子域: 评论列表与详情
- 目标: 实现评论资源输出类
- 依赖:
  - COMMENT-MODEL-BASE-001
  - AUTH-RES-USER-001
- 输入:
  - 评论展示字段要求
- 输出:
  - `CommentResource` 可用
- 验收标准:
  - 输出评论内容、作者摘要、点赞数、回复数、是否点赞等字段

### TASK-ID: COMMENT-SVC-LIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 评论列表与详情
- 目标: 实现职位评论列表查询服务
- 依赖:
  - COMMENT-REQ-LIST-001
  - COMMENT-MODEL-BASE-001
- 输入:
  - job_id
  - page/limit
- 输出:
  - 评论列表逻辑可用
- 验收标准:
  - 支持一级评论分页
  - 可附带回复摘要或完整回复列表策略

### TASK-ID: COMMENT-API-LIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 评论列表与详情
- 目标: 实现评论列表接口 `GET /api/comment/list`
- 依赖:
  - COMMENT-SVC-LIST-001
  - COMMENT-RES-ITEM-001
- 输入:
  - job_id
- 输出:
  - 评论列表接口可用
- 验收标准:
  - 分页结构统一

### 8.5.4 发表评论与回复

### TASK-ID: COMMENT-REQ-CREATE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 发表评论与回复
- 目标: 编写发表评论参数校验 Request
- 依赖:
  - COMMENT-DB-BASE-001
- 输入:
  - 评论创建接口文档
- 输出:
  - 评论创建校验类可用
- 建议校验至少包含:
  - job_id
  - content
  - parent_id（可选）
- 验收标准:
  - 评论与回复两种场景结构明确

### TASK-ID: COMMENT-SVC-CREATE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 发表评论与回复
- 目标: 实现发表评论/回复服务
- 依赖:
  - COMMENT-REQ-CREATE-001
  - COMMENT-MODEL-BASE-001
  - JOB-MODEL-BASE-001
- 输入:
  - 当前用户
  - 评论参数
- 输出:
  - 评论创建逻辑可用
- 业务要求至少包括:
  - 校验职位存在且可评论
  - parent_id 存在时校验父评论归属同一职位
  - 回复时更新父评论 reply_count
  - 更新职位 comments_count
- 验收标准:
  - 一级评论与回复创建都可用

### TASK-ID: COMMENT-API-CREATE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 发表评论与回复
- 目标: 实现发表评论接口 `POST /api/comment/create`
- 依赖:
  - COMMENT-SVC-CREATE-001
  - BOOT-AUTH-002
- 输入:
  - 评论参数
- 输出:
  - 评论创建接口可用
- 验收标准:
  - 返回创建后的评论资源

### 8.5.5 点赞/取消点赞

### TASK-ID: COMMENT-SVC-LIKE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 点赞/取消点赞
- 目标: 实现评论点赞服务
- 依赖:
  - COMMENT-MODEL-LIKE-001
  - COMMENT-MODEL-BASE-001
- 输入:
  - 当前用户
  - comment_id
- 输出:
  - 点赞逻辑可用
- 验收标准:
  - 重复点赞不重复写入
  - like_count 同步增加

### TASK-ID: COMMENT-API-LIKE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 点赞/取消点赞
- 目标: 实现评论点赞接口
- 依赖:
  - COMMENT-SVC-LIKE-001
  - BOOT-AUTH-002
- 输入:
  - comment_id
- 输出:
  - 点赞接口可用
- 验收标准:
  - 返回 is_liked=true

### TASK-ID: COMMENT-SVC-UNLIKE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 点赞/取消点赞
- 目标: 实现评论取消点赞服务
- 依赖:
  - COMMENT-MODEL-LIKE-001
  - COMMENT-MODEL-BASE-001
- 输入:
  - 当前用户
  - comment_id
- 输出:
  - 取消点赞逻辑可用
- 验收标准:
  - 删除点赞记录
  - like_count 同步减少并做下限保护

### TASK-ID: COMMENT-API-UNLIKE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 点赞/取消点赞
- 目标: 实现评论取消点赞接口
- 依赖:
  - COMMENT-SVC-UNLIKE-001
  - BOOT-AUTH-002
- 输入:
  - comment_id
- 输出:
  - 取消点赞接口可用
- 验收标准:
  - 返回 is_liked=false

### 8.5.6 删除评论

### TASK-ID: COMMENT-SVC-DELETE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 删除评论
- 目标: 实现评论删除服务
- 依赖:
  - COMMENT-MODEL-BASE-001
- 输入:
  - 当前用户
  - comment_id
- 输出:
  - 评论删除逻辑可用
- 业务要求至少包括:
  - 仅允许删除自己的评论或按策略允许管理员删除
  - 删除后更新职位 comments_count
  - 父评论 reply_count 需要同步回收
- 验收标准:
  - 删除后列表不再正常展示或按文档口径显示已删除状态

### TASK-ID: COMMENT-API-DELETE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 删除评论
- 目标: 实现评论删除接口
- 依赖:
  - COMMENT-SVC-DELETE-001
  - BOOT-AUTH-002
- 输入:
  - comment_id
- 输出:
  - 评论删除接口可用
- 验收标准:
  - 返回统一结构

### 8.5.7 举报创建与查询

### TASK-ID: REPORT-REQ-CREATE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 举报创建与查询
- 目标: 编写举报创建参数校验 Request
- 依赖:
  - REPORT-DB-BASE-001
  - BOOT-DB-FILES-001
- 输入:
  - 举报接口文档
- 输出:
  - 举报创建校验类可用
- 建议校验至少包含:
  - target_type
  - target_id
  - reason
  - content
  - evidence_file_ids
- 验收标准:
  - target_type 枚举清晰
  - evidence_file_ids 结构明确

### TASK-ID: REPORT-SVC-CREATE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 举报创建与查询
- 目标: 实现举报创建服务
- 依赖:
  - REPORT-REQ-CREATE-001
  - REPORT-MODEL-001
  - COMPANY-SVC-FILECHECK-001
- 输入:
  - 当前用户
  - 举报参数
- 输出:
  - 举报创建逻辑可用
- 业务要求至少包括:
  - 校验目标存在性
  - 校验证据 file_id 合法性
  - 创建 pending 举报
- 验收标准:
  - 可稳定创建举报记录

### TASK-ID: REPORT-API-CREATE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 举报创建与查询
- 目标: 实现举报创建接口 `POST /api/report/create`
- 依赖:
  - REPORT-SVC-CREATE-001
  - BOOT-AUTH-002
- 输入:
  - 举报参数
- 输出:
  - 举报创建接口可用
- 验收标准:
  - 返回 report_id/status

### TASK-ID: REPORT-API-MYLIST-001
- 状态: blocked
- 优先级: P1
- 所属子域: 举报创建与查询
- 目标: 实现“我的举报”列表接口
- 依赖:
  - REPORT-MODEL-001
  - BOOT-AUTH-002
- 输入:
  - page/limit/status
- 输出:
  - 我的举报列表接口可用
- 验收标准:
  - 仅返回当前用户发起的举报

### 8.5.8 黑名单前台查询

### TASK-ID: BLACK-SVC-PUBLICLIST-001
- 状态: blocked
- 优先级: P1
- 所属子域: 黑名单前台查询
- 目标: 实现黑名单公示列表查询服务
- 依赖:
  - BLACK-MODEL-001
- 输入:
  - page/limit
- 输出:
  - 公示黑名单查询逻辑可用
- 验收标准:
  - 仅返回 is_public=true 且当前生效中的记录

### TASK-ID: BLACK-API-PUBLICLIST-001
- 状态: blocked
- 优先级: P1
- 所属子域: 黑名单前台查询
- 目标: 实现黑名单公示列表接口
- 依赖:
  - BLACK-SVC-PUBLICLIST-001
- 输入:
  - page/limit
- 输出:
  - 黑名单公示接口可用
- 验收标准:
  - 返回公示字段，不泄露后台敏感字段

### TASK-ID: BLACK-API-DETAIL-001
- 状态: blocked
- 优先级: P1
- 所属子域: 黑名单前台查询
- 目标: 实现黑名单公示详情接口
- 依赖:
  - BLACK-MODEL-001
- 输入:
  - blacklist_id
- 输出:
  - 黑名单详情接口可用
- 验收标准:
  - 仅返回允许公示的信息

### 8.5.9 治理通知预留

### TASK-ID: REPORT-NOTICE-HOOK-001
- 状态: blocked
- 优先级: P1
- 所属子域: 治理通知预留
- 目标: 预留举报状态变化通知派发入口
- 依赖:
  - NOTICE-SVC-DISPATCH-001
  - REPORT-MODEL-001
- 输入:
  - 举报处理状态变化事件
- 输出:
  - 举报通知挂接入口可用
- 验收标准:
  - 后台处理完成后可直接对举报人派发结果通知

### TASK-ID: BLACK-SVC-USERCHECK-001
- 状态: blocked
- 优先级: P1
- 所属子域: 治理通知预留
- 目标: 实现用户黑名单状态快速判定服务
- 依赖:
  - BLACK-MODEL-001
  - AUTH-MODEL-USER-001
- 输入:
  - user_id
- 输出:
  - 黑名单状态判定能力可用
- 验收标准:
  - 可供登录、发评论、发消息等模块复用

### 8.5.10 测试

### TASK-ID: COMMENT-TEST-CREATE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 测试
- 目标: 编写评论创建接口测试
- 依赖:
  - COMMENT-API-CREATE-001
  - BOOT-TEST-002
- 输入:
  - 评论创建接口
- 输出:
  - Feature Test
- 验收标准:
  - 一级评论成功
  - 回复评论成功
  - 父评论归属错误时失败

### TASK-ID: COMMENT-TEST-LIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 测试
- 目标: 编写评论列表接口测试
- 依赖:
  - COMMENT-API-LIST-001
  - BOOT-TEST-002
- 输入:
  - 评论列表接口
- 输出:
  - Feature Test
- 验收标准:
  - 列表分页成功
  - 回复结构正确

### TASK-ID: COMMENT-TEST-LIKE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 测试
- 目标: 编写评论点赞/取消点赞测试
- 依赖:
  - COMMENT-API-LIKE-001
  - COMMENT-API-UNLIKE-001
- 输入:
  - 点赞接口
- 输出:
  - Feature Test
- 验收标准:
  - 点赞成功
  - 重复点赞不重复计数
  - 取消点赞成功

### TASK-ID: COMMENT-TEST-DELETE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 测试
- 目标: 编写评论删除接口测试
- 依赖:
  - COMMENT-API-DELETE-001
- 输入:
  - 评论删除接口
- 输出:
  - Feature Test
- 验收标准:
  - 仅本人可删除
  - 删除后计数回收正确

### TASK-ID: REPORT-TEST-CREATE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 测试
- 目标: 编写举报创建接口测试
- 依赖:
  - REPORT-API-CREATE-001
  - BOOT-TEST-002
- 输入:
  - 举报创建接口
- 输出:
  - Feature Test
- 验收标准:
  - 举报创建成功
  - 证据 file_id 非法时报错

### TASK-ID: BLACK-TEST-PUBLIC-001
- 状态: blocked
- 优先级: P1
- 所属子域: 测试
- 目标: 编写黑名单公示接口测试
- 依赖:
  - BLACK-API-PUBLICLIST-001
  - BLACK-API-DETAIL-001
- 输入:
  - 黑名单公示接口
- 输出:
  - Feature Test
- 验收标准:
  - 仅返回 public 且有效记录

## 8.6 本章推荐执行顺序

### 第一批（P0）
1. COMMENT-DB-BASE-001
2. COMMENT-DB-LIKE-001
3. COMMENT-MODEL-BASE-001
4. REPORT-DB-BASE-001
5. BLACK-DB-BASE-001
6. REPORT-MODEL-001
7. BLACK-MODEL-001
8. COMMENT-REQ-LIST-001
9. COMMENT-RES-ITEM-001
10. COMMENT-SVC-LIST-001
11. COMMENT-API-LIST-001
12. COMMENT-REQ-CREATE-001
13. COMMENT-SVC-CREATE-001
14. COMMENT-API-CREATE-001
15. REPORT-REQ-CREATE-001
16. REPORT-SVC-CREATE-17. REPORT-API-CREATE-001
18. COMMENT-TEST-CREATE-001
19. COMMENT-TEST-LIST-001
20. REPORT-TEST-CREATE-001

### 第二批（P1）
1. COMMENT-MODEL-LIKE-001
2. COMMENT-SVC-LIKE-001
3. COMMENT-API-LIKE-001
4. COMMENT-SVC-UNLIKE-001
5. COMMENT-API-UNLIKE-001
6. COMMENT-SVC-DELETE-001
7. COMMENT-API-DELETE-001
8. REPORT-API-MYLIST-001
9. BLACK-SVC-PUBLICLIST-001
10. BLACK-API-PUBLICLIST-001
11. BLACK-API-DETAIL-001
12. REPORT-NOTICE-HOOK-001
13. BLACK-SVC-USERCHECK-001
14. COMMENT-TEST-LIKE-001
15. COMMENT-TEST-DELETE-001
16. BLACK-TEST-PUBLIC-001

## 8.7 本章完成后的下一个章节入口
当第 8 章完成后，最合理进入：
- 第10章：工种 / 帮助中心 / 人才库
- 或 第11章：后台管理

建议优先顺序：
1. 第10章：工种 / 帮助中心 / 人才库
2. 第11章：后台管理

原因：
- 工种、帮助中心、人才库仍属于前台/业务增强层，适合在进入超大后台治理章前补齐
- 后台管理会是一个非常大的章节，放在工种/帮助中心/人才库之后更顺

---

# 第10章：工种 / 帮助中心 / 人才库

> 目标：建立工种分类与工种申请、帮助中心分类/文章/搜索/反馈、人才库加入/移除/列表/详情等增强型业务模块，为平台的行业化运营与雇主侧人才沉淀提供完整基础能力。
> 本章重点补齐“业务增强层”，既承接主链路，又为后台运营与治理提供结构化数据输入。

## 10.1 本章定位

### 本章要解决的问题
- 建立工种分类数据结构
- 支持工种列表与工种申请
- 建立帮助中心分类、文章、搜索、反馈与相关文章能力
- 建立人才库数据结构
- 支持从报名记录加入人才库、移除人才库、查看人才库列表与详情
- 为后台工种审核、帮助中心运营、人才库沉淀提供稳定数据基础

### 本章不做的内容
- 后台工种审核界面
- 后台帮助中心管理界面
- 后台复杂搜索推荐系统
- 智能问答机器人

## 10.2 本章完成标准
当第 10 章完成时，应满足：
1. 工种、工种申请、帮助分类、帮助文章、帮助反馈、搜索日志、人才库等底表可用
2. 用户可查询工种列表
3. 用户可提交工种申请并查看自己的申请记录
4. 用户可使用帮助中心分类、详情、搜索、反馈
5. 雇主可从报名记录加入人才库、移除、查看人才库列表与详情
6. 工种/帮助中心/人才库关键接口具备基础测试覆盖

## 10.3 本章依赖关系
### 上游依赖
- 第1章：项目骨架与基础设施
- 第2章：用户与认证
- 第3章：简历系统
- 第5章：职位系统
- 第6章：报名系统

### 下游依赖
- 后台工种管理与申请处理
- 后台帮助中心管理
- 雇主运营增强能力

## 10.4 本章任务总览
1. 工种数据结构与申请
2. 工种接口
3. 帮助中心数据结构
4. 帮助中心接口
5. 人才库数据结构
6. 人才库接口
7. 搜索与反馈增强
8. 测试

## 10.5 可直接执行任务清单

### 10.5.1 工种数据结构与申请

### TASK-ID: CATEGORY-DB-BASE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 工种数据结构与申请
- 目标: 创建 `categories` 表 migration
- 依赖:
  - BOOT-DB-001
- 输入:
  - 工种列表与分类文档
- 输出:
  - categories 表可用
- 建议字段至少包含:
  - id
  - parent_id
  - name
  - code
  - description
  - sort_order
  - status
  - created_at / updated_at
- 索引建议:
  - parent_id 普通索引
  - code 唯一索引
  - status 普通索引
  - sort_order 普通索引
- 验收标准:
  - 支持工种树或平铺分类结构
  - 可支撑后续工种申请审核通过后并入主分类

### TASK-ID: CATEGORY-DB-APPLY-001
- 状态: blocked
- 优先级: P0
- 所属子域: 工种数据结构与申请
- 目标: 创建 `category_applications` 表 migration
- 依赖:
  - CATEGORY-DB-BASE-001
  - AUTH-DB-USER-001
- 输入:
  - 工种申请接口文档
- 输出:
  - category_applications 表可用
- 建议字段至少包含:
  - id
  - user_id
  - category_name
  - parent_suggestion_id
  - reason
  - status
  - audit_reason
  - handled_by
  - handled_at
  - created_at / updated_at
- 索引建议:
  - user_id 普通索引
  - status 普通索引
  - created_at 普通索引
- 验收标准:
  - 可支撑用户申请新增工种
  - 可供后台后续处理

### TASK-ID: CATEGORY-MODEL-BASE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 工种数据结构与申请
- 目标: 创建 `Category` 模型
- 依赖:
  - CATEGORY-DB-BASE-001
- 输入:
  - categories 表结构
- 输出:
  - Category 模型可用
- 验收标准:
  - 支持层级关系与列表排序

### TASK-ID: CATEGORY-MODEL-APPLY-001
- 状态: blocked
- 优先级: P0
- 所属子域: 工种数据结构与申请
- 目标: 创建 `CategoryApplication` 模型
- 依赖:
  - CATEGORY-DB-APPLY-001
- 输入:
  - category_applications 表结构
- 输出:
  - CategoryApplication 模型可用
- 验收标准:
  - 支持工种申请记录查询

### 10.5.2 工种接口

### TASK-ID: CATEGORY-SVC-LIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 工种接口
- 目标: 实现工种列表查询服务
- 依赖:
  - CATEGORY-MODEL-BASE-001
- 输入:
  - status / parent_id 等查询参数
- 输出:
  - 工种列表逻辑可用
- 验收标准:
  - 仅返回启用工种
  - 支持排序与层级结构输出

### TASK-ID: CATEGORY-API-LIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 工种接口
- 目标: 实现工种列表接口 `GET /api/category/list`
- 依赖:
  - CATEGORY-SVC-LIST-001
- 输入:
  - 查询参数
- 输出:
  - 工种列表接口可用
- 验收标准:
  - 返回结构稳定，适合职位发布/简历技能选择复用

### TASK-ID: CATEGORY-REQ-APPLY-001
- 状态: blocked
- 优先级: P0
- 所属子域: 工种接口
- 目标: 编写工种申请参数校验 Request
- 依赖:
  - CATEGORY-DB-APPLY-001
- 输入:
  - 工种申请接口文档
- 输出:
  - 工种申请校验类可用
- 建议校验至少包含:
  - category_name
  - parent_suggestion_id
  - reason
- 验收标准:
  - category_name 长度与重复申请基础校验明确

### TASK-ID: CATEGORY-SVC-APPLY-001
- 状态: blocked
- 优先级: P0
- 所属子域: 工种接口
- 目标: 实现工种申请服务
- 依赖:
  - CATEGORY-REQ-APPLY-001
  - CATEGORY-MODEL-APPLY-001
- 输入:
  - 当前用户
  - 工种申请参数
- 输出:
  - 工种申请逻辑可用
- 验收标准:
  - 创建 pending 申请
  - 可避免短期重复提交同名申请

### TASK-ID: CATEGORY-API-APPLY-001
- 状态: blocked
- 优先级: P0
- 所属子域: 工种接口
- 目标: 实现工种申请接口 `POST /api/category/apply`
- 依赖:
  - CATEGORY-SVC-APPLY-001
  - BOOT-AUTH-002
- 输入:
  - 工种申请参数
- 输出:
  - 工种申请接口可用
- 验收标准:
  - 返回 application_id/status

### TASK-ID: CATEGORY-API-MYAPPLY-001
- 状态: blocked
- 优先级: P1
- 所属子域: 工种接口
- 目标: 实现“我的工种申请”列表接口
- 依赖:
  - CATEGORY-MODEL-APPLY-001
  - BOOT-AUTH-002
- 输入:
  - page/limit/status
- 输出:
  - 我的工种申请列表接口可用
- 验收标准:
  - 仅返回当前用户自己的申请记录

### 10.5.3 帮助中心数据结构

### TASK-ID: HELP-DB-CATEGORY-001
- 状态: blocked
- 优先级: P0
- 所属子域: 帮助中心数据结构
- 目标: 创建 `help_categories` 表 migration
- 依赖:
  - BOOT-DB-001
- 输入:
  - 帮助中心文档
- 输出:
  - help_categories 表可用
- 建议字段至少包含:
  - id
  - name
  - description
  - sort_order
  - status
  - created_at / updated_at
- 验收标准:
  - 支持帮助分类排序与启停

### TASK-ID: HELP-DB-ARTICLE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 帮助中心数据结构
- 目标: 创建 `help_articles` 表 migration
- 依赖:
  - HELP-DB-CATEGORY-001
- 输入:
  - 帮助文章结构要求
- 输出:
  - help_articles 表可用
- 建议字段至少包含:
  - id
  - category_id
  - title
  - content
  - summary
  - keywords_json
  - views_count
  - is_hot
  - sort_order
  - status
  - created_at / updated_at
- 索引建议:
  - category_id 普通索引
  - status 普通索引
  - is_hot 普通索引
  - sort_order 普通索引
- 验收标准:
  - 支持分类文章、热门文章、启停

### TASK-ID: HELP-DB-FEEDBACK-001
- 状态: blocked
- 优先级: P1
- 所属子域: 帮助中心数据结构
- 目标: 创建 `help_article_feedbacks` 表 migration
- 依赖:
  - HELP-DB-ARTICLE-001
  - AUTH-DB-USER-001
- 输入:
  - 帮助反馈接口文档
- 输出:
  - help_article_feedbacks 表可用
- 建议字段至少包含:
  - id
  - article_id
  - user_id
  - is_helpful
  - feedback
  - created_at
- 验收标准:
  - 支持“有帮助/没帮助”及可选反馈意见

### TASK-ID: HELP-DB-SYNONYM-001
- 状态: blocked
- 优先级: P1
- 所属子域: 帮助中心数据结构
- 目标: 创建 `help_search_synonyms` 表 migration
- 依赖:
  - BOOT-DB-001
- 输入:
  - 搜索同义词需求
- 输出:
  - help_search_synonyms 表可用
- 验收标准:
  - 支持搜索词归一化

### TASK-ID: HELP-DB-RELATION-001
- 状态: blocked
- 优先级: P1
- 所属子域: 帮助中心数据结构
- 目标: 创建 `help_article_relations` 表 migration
- 依赖:
  - HELP-DB-ARTICLE-001
- 输入:
  - 相关文章需求
- 输出:
  - help_article_relations 表可用
- 验收标准:
  - 支持文章之间双向或单向关联

### TASK-ID: HELP-DB-SEARCHLOG-001
- 状态: blocked
- 优先级: P1
- 所属子域: 帮助中心数据结构
- 目标: 创建 `help_search_logs` 表 migration
- 依赖:
  - AUTH-DB-USER-001
- 输入:
  - 搜索日志需求
- 输出:
  - help_search_logs 表可用
- 验收标准:
  - 支持记录搜索词、命中数、用户、时间

### TASK-ID: HELP-MODEL-CORE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 帮助中心数据结构
- 目标: 创建帮助中心核心模型组
- 依赖:
  - HELP-DB-CATEGORY-001
  - HELP-DB-ARTICLE-001
  - HELP-DB-FEEDBACK-001
  - HELP-DB-SYNONYM-001
  - HELP-DB-RELATION-001
  - HELP-DB-SEARCHLOG-001
- 输入:
  - 各帮助中心表结构
- 输出:
  - HelpCategory / HelpArticle / HelpArticleFeedback / HelpSearchSynonym / HelpArticleRelation / HelpSearchLog 模型可用
- 验收标准:
  - 模型关系清晰
  - 可支撑分类、详情、搜索、反馈、相关文章

### 10.5.4 帮助中心接口

### TASK-ID: HELP-SVC-HOME-001
- 状态: blocked
- 优先级: P0
- 所属子域: 帮助中心接口
- 目标: 实现帮助中心首页聚合服务
- 依赖:
  - HELP-MODEL-CORE-001
- 输入:
  - 当前用户（可选）
- 输出:
  - 帮助中心首页聚合逻辑可用
- 验收标准:
  - 可输出分类、热门文章、推荐文章等基础结构

### TASK-ID: HELP-API-HOME-001
- 状态: blocked
- 优先级: P0
- 所属子域: 帮助中心接口
- 目标: 实现帮助中心首页接口
- 依赖:
  - HELP-SVC-HOME-001
- 输入:
  - 无或基础参数
- 输出:
  - 帮助中心首页接口可用
- 验收标准:
  - 返回结构适合前端首页直接渲染

### TASK-ID: HELP-API-CATEGORYLIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 帮助中心接口
- 目标: 实现帮助分类列表接口
- 依赖:
  - HELP-MODEL-CORE-001
- 输入:
  - 无或基础参数
- 输出:
  - 帮助分类列表接口可用
- 验收标准:
  - 仅返回启用分类

### TASK-ID: HELP-API-ARTICLELIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 帮助中心接口
- 目标: 实现帮助文章列表接口
- 依赖:
  - HELP-MODEL-CORE-001
- 输入:
  - category_id/page/limit
- 输出:
  - 帮助文章列表接口可用
- 验收标准:
  - 支持分类筛选与分页

### TASK-ID: HELP-SVC-DETAIL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 帮助中心接口
- 目标: 实现帮助文章详情服务
- 依赖:
  - HELP-MODEL-CORE-001
- 输入:
  - article_id
- 输出:
  - 帮助文章详情逻辑可用
- 验收标准:
  - 增加 views_count
  - 可同时返回相关文章

### TASK-ID: HELP-API-DETAIL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 帮助中心接口
- 目标: 实现帮助文章详情接口
- 依赖:
  - HELP-SVC-DETAIL-001
- 输入:
  - article_id
- 输出:
  - 帮助文章详情接口可用
- 验收标准:
  - 返回详情、分类、相关文章等结构

### TASK-ID: HELP-SVC-SEARCH-001
- 状态: blocked
- 优先级: P1
- 所属子域: 帮助中心接口
- 目标: 实现帮助中心搜索服务
- 依赖:
  - HELP-MODEL-CORE-001
- 输入:
  - keyword
  - page/limit
- 输出:
  - 帮助搜索逻辑可用
- 业务要求至少包括:
  - 同义词归一化
  - 搜索日志记录
- 验收标准:
  - 可返回文章结果与命中数

### TASK-ID: HELP-API-SEARCH-001
- 状态: blocked
- 优先级: P1
- 所属子域: 帮助中心接口
- 目标: 实现帮助中心搜索接口
- 依赖:
  - HELP-SVC-SEARCH-001
- 输入:
  - keyword/page/limit
- 输出:
  - 帮助搜索接口可用
- 验收标准:
  - 返回搜索结果结构统一

### TASK-ID: HELP-REQ-FEEDBACK-001
- 状态: blocked
- 优先级: P1
- 所属子域: 帮助中心接口
- 目标: 编写帮助文章反馈参数校验 Request
- 依赖:
  - HELP-DB-FEEDBACK-001
- 输入:
  - 帮助反馈接口文档
- 输出:
  - 帮助反馈校验类可用
- 建议校验至少包含:
  - article_id
  - is_helpful
  - feedback
- 验收标准:
  - 参数结构明确

### TASK-ID: HELP-SVC-FEEDBACK-001
- 状态: blocked
- 优先级: P1
- 所属子域: 帮助中心接口
- 目标: 实现帮助文章反馈服务
- 依赖:
  - HELP-REQ-FEEDBACK-001
  - HELP-MODEL-CORE-001
- 输入:
  - 当前用户
  - 反馈参数
- 输出:
  - 帮助反馈逻辑可用
- 验收标准:
  - 可记录用户反馈
  - 可防止短时间重复刷反馈

### TASK-ID: HELP-API-FEEDBACK-001
- 状态: blocked
- 优先级: P1
- 所属子域: 帮助中心接口
- 目标: 实现帮助文章反馈接口
- 依赖:
  - HELP-SVC-FEEDBACK-001
  - BOOT-AUTH-002
- 输入:
  - 反馈参数
- 输出:
  - 帮助反馈接口可用
- 验收标准:
  - 返回统一结构

### 10.5.5 人才库数据结构

### TASK-ID: TALENT-DB-BASE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 人才库数据结构
- 目标: 创建 `talent_pool` 或等价表 migration
- 依赖:
  - BOOT-DB-001
  - AUTH-DB-USER-001
  - APPLY-DB-BASE-001
- 输入:
  - 人才库文档
- 输出:
  - 人才库底表可用
- 建议字段至少包含:
  - id
  - employer_id
  - seeker_user_id
  - application_id
  - resume_id
  - category_id
  - category_name_snapshot
  - level
  - level_name_snapshot
  - source_job_id
  - remark
  - created_at / updated_at
- 索引建议:
  - employer_id 普通索引
  - seeker_user_id 普通索引
  - application_id 普通索引
  - employer_id + seeker_user_id + category_id + level 唯一索引
- 验收标准:
  - 同一雇主下同求职者同工种级别不重复
  - 可保留来源申请与来源职位快照

### TASK-ID: TALENT-MODEL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 人才库数据结构
- 目标: 创建 `TalentPool` 模型
- 依赖:
  - TALENT-DB-BASE-001
- 输入:
  - 人才库表结构
- 输出:
  - TalentPool 模型可用
- 验收标准:
  - 支持列表、详情、去重与来源追踪

### 10.5.6 人才库接口

### TASK-ID: TALENT-REQ-ADD-001
- 状态: blocked
- 优先级: P0
- 所属子域: 人才库接口
- 目标: 编写加入人才库参数校验 Request
- 依赖:
  - TALENT-DB-BASE-001
- 输入:
  - 加入人才库接口文档
- 输出:
  - 加入人才库校验类可用
- 建议校验至少包含:
  - application_id
  - remark
- 验收标准:
  - 参数结构明确

### TASK-ID: TALENT-SVC-ADD-001
- 状态: blocked
- 优先级: P0
- 所属子域: 人才库接口
- 目标: 实现从报名记录加入人才库服务
- 依赖:
  - TALENT-REQ-ADD-001
  - TALENT-MODEL-001
  - APPLY-MODEL-BASE-001
- 输入:
  - 当前雇主
  - application_id
  - remark
- 输出:
  - 加入人才库逻辑可用
- 业务要求至少包括:
  - 仅允许将自己职位下的报名加入
  - 同工种级别去重
  - 写入 category/level/source_job 等快照
- 验收标准:
  - 加入成功后可在人才库列表中查询

### TASK-ID: TALENT-API-ADD-001
- 状态: blocked
- 优先级: P0
- 所属子域: 人才库接口
- 目标: 实现加入人才库接口
- 依赖:
  - TALENT-SVC-ADD-001
  - BOOT-AUTH-002
- 输入:
  - application_id
  - remark
- 输出:
  - 加入人才库接口可用
- 验收标准:
  - 返回 talent_id

### TASK-ID: TALENT-SVC-REMOVE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 人才库接口
- 目标: 实现移除人才库服务
- 依赖:
  - TALENT-MODEL-001
- 输入:
  - 当前雇主
  - talent_id
- 输出:
  - 移除人才库逻辑可用
- 验收标准:
  - 仅允许移除自己的记录

### TASK-ID: TALENT-API-REMOVE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 人才库接口
- 目标: 实现移除人才库接口
- 依赖:
  - TALENT-SVC-REMOVE-001
  - BOOT-AUTH-002
- 输入:
  - talent_id
- 输出:
  - 移除人才库接口可用
- 验收标准:
  - 返回统一结构

### TASK-ID: TALENT-API-LIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 人才库接口
- 目标: 实现人才库列表接口
- 依赖:
  - TALENT-MODEL-001
  - RESUME-RES-SUMMARY-001
  - BOOT-AUTH-002
- 输入:
  - page/limit/category_id/keyword
- 输出:
  - 人才库列表接口可用
- 验收标准:
  - 支持分页与筛选
  - 返回人才摘要与来源信息

### TASK-ID: TALENT-API-DETAIL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 人才库接口
- 目标: 实现人才库详情接口
- 依赖:
  - TALENT-MODEL-001
  - RESUME-SVC-READ-EMPLOYER-001
  - BOOT-AUTH-002
- 输入:
  - talent_id
- 输出:
  - 人才库详情接口可用
- 验收标准:
  - 仅允许查看自己的记录
  - 返回完整简历与来源信息

### 10.5.7 搜索与反馈增强

### TASK-ID: HELP-SVC-HOT-001
- 状态: blocked
- 优先级: P1
- 所属子域: 搜索与反馈增强
- 目标: 实现热门帮助文章查询服务
- 依赖:
  - HELP-MODEL-CORE-001
- 输入:
  - limit
- 输出:
  - 热门帮助文章逻辑可用
- 验收标准:
  - 支持首页热门问题展示

### TASK-ID: HELP-API-HOT-001
- 状态: blocked
- 优先级: P1
- 所属子域: 搜索与反馈增强
- 目标: 实现热门帮助文章接口
- 依赖:
  - HELP-SVC-HOT-001
- 输入:
  - limit
- 输出:
  - 热门帮助文章接口可用
- 验收标准:
  - 返回热门文章列表结构统一

### 10.5.8 测试

### TASK-ID: CATEGORY-TEST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 测试
- 目标: 编写工种列表与工种申请接口测试
- 依赖:
  - CATEGORY-API-LIST-001
  - CATEGORY-API-APPLY-001
  - BOOT-TEST-002
- 输入:
  - 工种接口
- 输出:
  - Feature Test
- 验收标准:
  - 工种列表返回成功
  - 工种申请成功
  - 重复申请策略生效

### TASK-ID: HELP-TEST-DETAIL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 测试
- 目标: 编写帮助中心列表/详情接口测试
- 依赖:
  - HELP-API-HOME-001
  - HELP-API-CATEGORYLIST-001
  - HELP-API-ARTICLELIST-001
  - HELP-API-DETAIL-001
- 输入:
  - 帮助中心接口
- 输出:
  - Feature Test
- 验收标准:
  - 首页/分类/详情返回正确
  - 详情访问后 views_count 增长

### TASK-ID: HELP-TEST-SEARCH-001
- 状态: blocked
- 优先级: P1
- 所属子域: 测试
- 目标: 编写帮助中心搜索与反馈测试
- 依赖:
  - HELP-API-SEARCH-001
  - HELP-API-FEEDBACK-001
- 输入:
  - 搜索与反馈接口
- 输出:
  - Feature Test
- 验收标准:
  - 搜索结果正确
  - 搜索日志记录成功
  - 反馈提交成功

### TASK-ID: TALENT-TEST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 测试
- 目标: 编写人才库加入/列表/详情接口测试
- 依赖:
  - TALENT-API-ADD-001
  - TALENT-API-LIST-001
  - TALENT-API-DETAIL-001
- 输入:
  - 人才库接口
- 输出:
  - Feature Test
- 验收标准:
  - 从报名记录加入成功
  - 去重策略生效
  - 列表与详情返回正确

## 10.6 本章推荐执行顺序

### 第一批（P0）
1. CATEGORY-DB-BASE-001
2. CATEGORY-DB-APPLY-001
3. CATEGORY-MODEL-BASE-001
4. CATEGORY-MODEL-APPLY-001
5. CATEGORY-SVC-LIST-001
6. CATEGORY-API-LIST-001
7. CATEGORY-REQ-APPLY-001
8. CATEGORY-SVC-APPLY-001
9. CATEGORY-API-APPLY-001
10. HELP-DB-CATEGORY-001
11. HELP-DB-ARTICLE-001
12. HELP-MODEL-CORE-001
13. HELP-SVC-HOME-001
14. HELP-API-HOME-001
15. HELP-API-CATEGORYLIST-001
16. HELP-API-ARTICLELIST-001
17. HELP-SVC-DETAIL-001
18. HELP-API-DETAIL-001
19. TALENT-DB-BASE-001
20. TALENT-MODEL-001
21. TALENT-REQ-ADD-001
22. TALENT-SVC-ADD-001
23. TALENT-API-ADD-001
24. TALENT-API-LIST-001
25. TALENT-API-DETAIL-001
26. CATEGORY-TEST-001
27. HELP-TEST-DETAIL-001
28. TALENT-TEST-001

### 第二批（P1）
1. CATEGORY-API-MYAPPLY-001
2. HELP-DB-FEEDBACK-001
3. HELP-DB-SYNONYM-001
4. HELP-DB-RELATION-001
5. HELP-DB-SEARCHLOG-001
6. HELP-SVC-SEARCH-001
7. HELP-API-SEARCH-001
8. HELP-REQ-FEEDBACK-001
9. HELP-SVC-FEEDBACK-001
10. HELP-API-FEEDBACK-001
11. TALENT-SVC-REMOVE-001
12. TALENT-API-REMOVE-001
13. HELP-SVC-HOT-001
14. HELP-API-HOT-001
15. HELP-TEST-SEARCH-001

## 10.7 本章完成后的下一个章节入口
当第 10 章完成后，最合理进入：
- 第11章：后台管理

原因：
- 到第10章为止，前台主链路与业务增强层已经基本齐备
- 接下来进入后台治理与运营大章最合适

---

# 第11章：后台管理

> 目标：建立管理员登录、子管理员与权限、用户管理、企业审核、职位审核、工种申请处理、举报处理、黑名单管理、公告、广播、违禁词、系统配置、导出任务、操作日志与配置变更日志等完整后台治理与运营能力。
> 本章是平台治理核心章节，重点是把前面所有业务模块统一纳入可审核、可运营、可审计、可配置的后台体系。

## 11.1 本章定位

### 本章要解决的问题
- 建立管理员账号体系与后台登录能力
- 建立子管理员与权限体系
- 建立后台用户管理能力
- 建立企业审核、职位审核、工种申请处理能力
- 建立举报处理与黑名单管理能力
- 建立公告、广播、违禁词等运营能力
- 建立系统配置、导出任务、操作日志、配置变更日志查询能力
- 为上线后的平台治理与日常运营提供完整后台基础

### 本章不做的内容
- 超复杂 BI 看板
- 外部支付结算后台
- 多租户后台
- 机器学习风控系统

## 11.2 本章完成标准
当第 11 章完成时，应满足：
1. 后台管理员登录与权限体系可用
2. 后台可管理用户、审核企业、审核职位、处理工种申请
3. 后台可处理举报与维护黑名单
4. 后台可维护公告、广播、违禁词
5. 后台可维护系统配置、发起导出任务、查看日志
6. 高危后台操作具备审计留痕与权限边界
7. 关键后台接口具备基础测试覆盖

## 11.3 本章依赖关系
### 上游依赖
- 第1章：项目骨架与基础设施
- 第2章：用户与认证
- 第4章：企业认证
- 第5章：职位系统
- 第6章：报名系统
- 第8章：评论 / 举报 / 黑名单
- 第9章：收藏 / 通知 / 浏览历史
- 第10章：工种 / 帮助中心 / 人才库

### 下游依赖
- 安全与部署上线治理
- 日常运营
- 内容风控与数据导出

## 11.4 本章任务总览
1. 后台账号与权限
2. 用户管理
3. 企业审核
4. 职位审核
5. 工种申请处理
6. 举报处理
7. 黑名单管理
8. 公告 / 广播 / 违禁词
9. 系统配置 / 导出 / 日志
10. 测试与执行顺序

## 11.5 可直接执行任务清单

### 11.5.1 后台账号与权限

### TASK-ID: ADMIN-DB-ACCOUNT-001
- 状态: blocked
- 优先级: P0
- 所属子域: 后台账号与权限
- 目标: 创建 `admin_users` 表 migration
- 依赖:
  - BOOT-DB-001
- 输入:
  - 后台管理员登录与子管理员需求
- 输出:
  - admin_users 表可用
- 建议字段至少包含:
  - id
  - username
  - password
  - nickname
  - phone
  - email
  - avatar_file_id
  - status
  - last_login_time
  - last_login_ip
  - created_at / updated_at
- 索引建议:
  - username 唯一索引
  - phone 普通索引
  - status 普通索引
- 验收标准:
  - 可支撑后台管理员与子管理员账号体系

### TASK-ID: ADMIN-DB-PERM-001
- 状态: blocked
- 优先级: P0
- 所属子域: 后台账号与权限
- 目标: 创建 `admin_permissions` 与 `admin_user_permissions` 表 migration
- 依赖:
  - ADMIN-DB-ACCOUNT-001
- 输入:
  - 后台权限需求
- 输出:
  - 权限定义表与管理员权限关联表可用
- 验收标准:
  - 可定义 permission_code
  - 可将多个权限绑定给管理员

### TASK-ID: ADMIN-MODEL-ACCOUNT-001
- 状态: blocked
- 优先级: P0
- 所属子域: 后台账号与权限
- 目标: 创建 `AdminUser` 模型
- 依赖:
  - ADMIN-DB-ACCOUNT-001
- 输入:
  - admin_users 表结构
- 输出:
  - AdminUser 模型可用
- 验收标准:
  - 支持登录、权限读取、状态判断

### TASK-ID: ADMIN-MODEL-PERM-001
- 状态: blocked
- 优先级: P0
- 所属子域: 后台账号与权限
- 目标: 创建后台权限模型组
- 依赖:
  - ADMIN-DB-PERM-001
- 输入:
  - 权限相关表结构
- 输出:
  - AdminPermission / AdminUserPermission 模型可用
- 验收标准:
  - 可支撑权限校验与权限列表输出

### TASK-ID: ADMIN-REQ-LOGIN-001
- 状态: blocked
- 优先级: P0
- 所属子域: 后台账号与权限
- 目标: 编写后台登录参数校验 Request
- 依赖:
  - ADMIN-DB-ACCOUNT-001
- 输入:
  - 后台登录接口文档
- 输出:
  - 后台登录校验类可用
- 验收标准:
  - username/password 等参数校验明确

### TASK-ID: ADMIN-SVC-TOKEN-001
- 状态: blocked
- 优先级: P0
- 所属子域: 后台账号与权限
- 目标: 实现后台 JWT token 签发服务
- 依赖:
  - BOOT-AUTH-001
  - ADMIN-MODEL-ACCOUNT-001
- 输入:
  - 后台账号
- 输出:
  - 后台 token 签发能力可用
- 验收标准:
  - 可区分于普通用户 token
  - 可被 `auth.jwt:admin` 识别

### TASK-ID: ADMIN-SVC-LOGIN-001
- 状态: blocked
- 优先级: P0
- 所属子域: 后台账号与权限
- 目标: 实现后台登录服务
- 依赖:
  - ADMIN-REQ-LOGIN-001
  - ADMIN-SVC-TOKEN-001
- 输入:
  - 登录参数
- 输出:
  - 后台登录逻辑可用
- 验收标准:
  - 账号状态异常时不可登录
  - 登录成功写入 last_login_time/last_login_ip

### TASK-ID: ADMIN-API-LOGIN-001
- 状态: blocked
- 优先级: P0
- 所属子域: 后台账号与权限
- 目标: 实现后台登录接口
- 依赖:
  - ADMIN-SVC-LOGIN-001
- 输入:
  - 登录参数
- 输出:
  - 后台登录接口可用
- 验收标准:
  - 返回 token 与管理员基础信息

### TASK-ID: ADMIN-MW-PERM-001
- 状态: blocked
- 优先级: P0
- 所属子域: 后台账号与权限
- 目标: 实现后台 permission_code 权限中间件
- 依赖:
  - ADMIN-MODEL-PERM-001
  - BOOT-AUTH-003
- 输入:
  - 当前管理员
  - 权限 code
- 输出:
  - 后台权限校验能力可用
- 验收标准:
  - 无权限请求被拒绝
  - 可复用于各后台模块接口

### 11.5.2 用户管理

### TASK-ID: ADMIN-USER-SVC-LIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 用户管理
- 目标: 实现后台用户列表查询服务
- 依赖:
  - AUTH-MODEL-USER-001
  - BOOT-AUTH-003
- 输入:
  - keyword/role/status/is_certified/page/limit
- 输出:
  - 用户列表查询逻辑可用
- 验收标准:
  - 支持多维筛选与分页

### TASK-ID: ADMIN-USER-API-LIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 用户管理
- 目标: 实现后台用户列表接口
- 依赖:
  - ADMIN-USER-SVC-LIST-001
  - ADMIN-MW-PERM-001
- 输入:
  - 查询参数
- 输出:
  - 后台用户列表接口可用
- 验收标准:
  - 返回统一结构与分页

### TASK-ID: ADMIN-USER-API-DETAIL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 用户管理
- 目标: 实现后台用户详情接口
- 依赖:
  - AUTH-MODEL-USER-001
  - RESUME-RES-DETAIL-001
  - COMPANY-RES-DETAIL-001
- 输入:
  - user_id
- 输出:
  - 后台用户详情接口可用
- 验收标准:
  - 返回用户基础信息、简历、企业认证摘要等

### TASK-ID: ADMIN-USER-SVC-RESETPWD-001
- 状态: blocked
- 优先级: P1
- 所属子域: 用户管理
- 目标: 实现后台重置用户密码服务
- 依赖:
  - AUTH-MODEL-USER-001
  - BOOT-DB-OPLOG-001
- 输入:
  - user_id
  - new_password
  - reason
- 输出:
  - 后台重置密码逻辑可用
- 验收标准:
  - 可重置密码
  - 记录高危操作日志

### TASK-ID: ADMIN-USER-API-RESETPWD-001
- 状态: blocked
- 优先级: P1
- 所属子域: 用户管理
- 目标: 实现后台重置用户密码接口
- 依赖:
  - ADMIN-USER-SVC-RESETPWD-001
  - ADMIN-MW-PERM-001
- 输入:
  - user_id/new_password/reason
- 输出:
  - 后台重置密码接口可用
- 验收标准:
  - 返回统一结构

### TASK-ID: ADMIN-USER-SVC-DELETE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 用户管理
- 目标: 实现后台删除/停用用户服务
- 依赖:
  - AUTH-MODEL-USER-001
  - BOOT-DB-OPLOG-001
- 输入:
  - user_id
  - reason
- 输出:
  - 用户处理逻辑可用
- 验收标准:
  - 与系统状态机口径一致
  - 有审计日志

### 11.5.3 企业审核

### TASK-ID: ADMIN-COMPANY-API-LIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 企业审核
- 目标: 实现后台企业认证列表接口
- 依赖:
  - COMPANY-MODEL-BASE-001
  - ADMIN-MW-PERM-001
- 输入:
  - status/page/limit/keyword
- 输出:
  - 企业认证列表接口可用
- 验收标准:
  - 支持状态筛选与分页

### TASK-ID: ADMIN-COMPANY-API-DETAIL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 企业审核
- 目标: 实现后台企业认证详情接口
- 依赖:
  - COMPANY-RES-DETAIL-001
  - ADMIN-MW-PERM-001
- 输入:
  - company_id
- 输出:
  - 企业认证详情接口可用
- 验收标准:
  - 可查看当前资料与历史材料快照

### TASK-ID: ADMIN-COMPANY-SVC-REVIEW-001
- 状态: blocked
- 优先级: P0
- 所属子域: 企业审核
- 目标: 实现后台企业认证审核服务
- 依赖:
  - COMPANY-MODEL-BASE-001
  - COMPANY-SVC-USERSTATUS-001
  - COMPANY-SVC-SNAPSHOT-001
  - BOOT-DB-OPLOG-001
- 输入:
  - company_id
  - action=approve/reject
  - reason
- 输出:
  - 企业认证审核逻辑可用
- 验收标准:
  - 审核通过/拒绝状态同步正确
  - 写操作日志

### TASK-ID: ADMIN-COMPANY-API-REVIEW-001
- 状态: blocked
- 优先级: P0
- 所属子域: 企业审核
- 目标: 实现后台企业认证审核接口
- 依赖:
  - ADMIN-COMPANY-SVC-REVIEW-001
  - ADMIN-MW-PERM-001
- 输入:
  - company_id/action/reason
- 输出:
  - 企业审核接口可用
- 验收标准:
  - 返回最新审核状态

### 11.5.4 职位审核

### TASK-ID: ADMIN-JOB-API-LIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位审核
- 目标: 实现后台职位列表接口
- 依赖:
  - JOB-MODEL-BASE-001
  - ADMIN-MW-PERM-001
- 输入:
  - status/audit_status/page/limit/keyword
- 输出:
  - 后台职位列表接口可用
- 验收标准:
  - 支持状态与审核状态筛选

### TASK-ID: ADMIN-JOB-API-DETAIL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位审核
- 目标: 实现后台职位详情接口
- 依赖:
  - JOB-RES-DETAIL-001
  - ADMIN-MW-PERM-001
- 输入:
  - job_id
- 输出:
  - 后台职位详情接口可用
- 验收标准:
  - 可查看职位主资料、级别、图片、审核信息

### TASK-ID: ADMIN-JOB-SVC-REVIEW-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位审核
- 目标: 实现后台职位审核服务
- 依赖:
  - JOB-MODEL-BASE-001
  - BOOT-DB-OPLOG-001
  - NOTICE-SVC-DISPATCH-001
- 输入:
  - job_id
  - action=approve/reject
  - reason
- 输出:
  - 职位审核逻辑可用
- 验收标准:
  - approve 后职位进入 active
  - reject 后记录 audit_reason
  - 可派发审核通知

### TASK-ID: ADMIN-JOB-API-REVIEW-001
- 状态: blocked
- 优先级: P0
- 所属子域: 职位审核
- 目标: 实现后台职位审核接口
- 依赖:
  - ADMIN-JOB-SVC-REVIEW-001
  - ADMIN-MW-PERM-001
- 输入:
  - job_id/action/reason
- 输出:
  - 职位审核接口可用
- 验收标准:
  - 返回最新审核状态与原因

### TASK-ID: ADMIN-JOB-SVC-TOPSET-001
- 状态: blocked
- 优先级: P1
- 所属子域: 职位审核
- 目标: 实现后台直接设置/取消职位置顶服务
- 依赖:
  - JOB-MODEL-BASE-001
  - BOOT-DB-OPLOG-001
- 输入:
  - job_id
  - is_top
  - top_expire_time
  - reason
- 输出:
  - 后台置顶控制逻辑可用
- 验收标准:
  - 可直接修改置顶状态
  - 有操作日志

### 11.5.5 工种申请处理

### TASK-ID: ADMIN-CATEGORY-API-APPLYLIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 工种申请处理
- 目标: 实现后台工种申请列表接口
- 依赖:
  - CATEGORY-MODEL-APPLY-001
  - ADMIN-MW-PERM-001
- 输入:
  - status/page/limit
- 输出:
  - 工种申请列表接口可用
- 验收标准:
  - 支持分页与状态筛选

### TASK-ID: ADMIN-CATEGORY-SVC-APPLYREVIEW-001
- 状态: blocked
- 优先级: P0
- 所属子域: 工种申请处理
- 目标: 实现后台工种申请处理服务
- 依赖:
  - CATEGORY-MODEL-APPLY-001
  - CATEGORY-MODEL-BASE-001
  - BOOT-DB-OPLOG-001
- 输入:
  - application_id
  - action=approve/reject
  - reason
- 输出:
  - 工种申请处理逻辑可用
- 验收标准:
  - approve 时可创建新工种或绑定现有工种
  - reject 时记录原因

### TASK-ID: ADMIN-CATEGORY-API-APPLYREVIEW-001
- 状态: blocked
- 优先级: P0
- 所属子域: 工种申请处理
- 目标: 实现后台工种申请处理接口
- 依赖:
  - ADMIN-CATEGORY-SVC-APPLYREVIEW-001
  - ADMIN-MW-PERM-001
- 输入:
  - application_id/action/reason
- 输出:
  - 工种申请处理接口可用
- 验收标准:
  - 返回最新处理状态

### 11.5.6 举报处理

### TASK-ID: ADMIN-REPORT-API-LIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 举报处理
- 目标: 实现后台举报列表接口
- 依赖:
  - REPORT-MODEL-001
  - ADMIN-MW-PERM-001
- 输入:
  - status/target_type/page/limit
- 输出:
  - 举报列表接口可用
- 验收标准:
  - 支持状态与目标类型筛选

### TASK-ID: ADMIN-REPORT-API-DETAIL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 举报处理
- 目标: 实现后台举报详情接口
- 依赖:
  - REPORT-MODEL-001
  - ADMIN-MW-PERM-001
- 输入:
  - report_id
- 输出:
  - 举报详情接口可用
- 验收标准:
  - 可查看举报内容、证据、目标信息

### TASK-ID: ADMIN-REPORT-SVC-HANDLE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 举报处理
- 目标: 实现后台举报处理服务
- 依赖:
  - REPORT-MODEL-001
  - REPORT-NOTICE-HOOK-001
  - BOOT-DB-OPLOG-001
- 输入:
  - report_id
  - action=resolve/reject
  - handle_result
  - reason
- 输出:
  - 举报处理逻辑可用
- 验收标准:
  - 更新 status/handle_result/handled_by/handled_at
  - 可触发举报结果通知

### TASK-ID: ADMIN-REPORT-API-HANDLE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 举报处理
- 目标: 实现后台举报处理接口
- 依赖:
  - ADMIN-REPORT-SVC-HANDLE-001
  - ADMIN-MW-PERM-001
- 输入:
  - report_id/action/handle_result/reason
- 输出:
  - 举报处理接口可用
- 验收标准:
  - 返回最新处理状态

### 11.5.7 黑名单管理

### TASK-ID: ADMIN-BLACK-API-LIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 黑名单管理
- 目标: 实现后台黑名单列表接口
- 依赖:
  - BLACK-MODEL-001
  - ADMIN-MW-PERM-001
- 输入:
  - status/is_public/page/limit
- 输出:
  - 黑名单列表接口可用
- 验收标准:
  - 支持分页与状态筛选

### TASK-ID: ADMIN-BLACK-SVC-CREATE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 黑名单管理
- 目标: 实现后台新增黑名单服务
- 依赖:
  - BLACK-MODEL-001
  - BOOT-DB-OPLOG-001
- 输入:
  - target_user_id
  - reason
  - description
  - is_public
  - start_time
  - end_time
- 输出:
  - 新增黑名单逻辑可用
- 验收标准:
  - 可创建黑名单记录
  - 可同步用户黑名单状态字段（如设计需要）

### TASK-ID: ADMIN-BLACK-API-CREATE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 黑名单管理
- 目标: 实现后台新增黑名单接口
- 依赖:
  - ADMIN-BLACK-SVC-CREATE-001
  - ADMIN-MW-PERM-001
- 输入:
  - 黑名单参数
- 输出:
  - 新增黑名单接口可用
- 验收标准:
  - 返回 blacklist_id

### TASK-ID: ADMIN-BLACK-SVC-UPDATE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 黑名单管理
- 目标: 实现后台编辑黑名单服务
- 依赖:
  - BLACK-MODEL-001
  - BOOT-DB-OPLOG-001
- 输入:
  - blacklist_id
  - 更新字段
- 输出:
  - 黑名单编辑逻辑可用
- 验收标准:
  - 可修改公示状态、生效区间、描述等字段

### TASK-ID: ADMIN-BLACK-SVC-REMOVE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 黑名单管理
- 目标: 实现后台移除/失效黑名单服务
- 依赖:
  - BLACK-MODEL-001
  - BOOT-DB-OPLOG-001
- 输入:
  - blacklist_id
  - reason
- 输出:
  - 黑名单移除逻辑可用
- 验收标准:
  - 可结束黑名单生效状态
  - 记录操作日志

### 11.5.8 公告 / 广播 / 违禁词

### TASK-ID: ADMIN-DB-ANNOUNCE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 公告 / 广播 / 违禁词
- 目标: 创建 `announcements` 表 migration
- 依赖:
  - BOOT-DB-001
- 输入:
  - 公告接口文档
- 输出:
  - announcements 表可用
- 验收标准:
  - 支持公告新增、启停、排序与详情展示

### TASK-ID: ADMIN-DB-BROADCAST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 公告 / 广播 / 违禁词
- 目标: 创建 `system_broadcasts` 表 migration
- 依赖:
  - BOOT-DB-001
- 输入:
  - 广播接口文档
- 输出:
  - system_broadcasts 表可用
- 验收标准:
  - 支持广播消息留痕与状态管理

### TASK-ID: ADMIN-DB-BANNEDWORD-001
- 状态: blocked
- 优先级: P0
- 所属子域: 公告 / 广播 / 违禁词
- 目标: 创建 `banned_words` 表 migration
- 依赖:
  - BOOT-DB-001
- 输入:
  - 违禁词接口文档
- 输出:
  - banned_words 表可用
- 验收标准:
  - 支持词条启停与分类（如有）

### TASK-ID: ADMIN-MODEL-OPS-001
- 状态: blocked
- 优先级: P0
- 所属子域: 公告 / 广播 / 违禁词
- 目标: 创建公告/广播/违禁词模型组
- 依赖:
  - ADMIN-DB-ANNOUNCE-001
  - ADMIN-DB-BROADCAST-001
  - ADMIN-DB-BANNEDWORD-001
- 输入:
  - 各表结构
- 输出:
  - Announcement / SystemBroadcast / BannedWord 模型可用
- 验收标准:
  - 可支撑后台 CRUD 与前台读取

### TASK-ID: ADMIN-ANNOUNCE-API-CRUD-001
- 状态: blocked
- 优先级: P0
- 所属子域: 公告 / 广播 / 违禁词
- 目标: 实现后台公告 CRUD 接口组
- 依赖:
  - ADMIN-MODEL-OPS-001
  - ADMIN-MW-PERM-001
- 输入:
  - 公告接口文档
- 输出:
  - 公告列表/详情/新增/编辑/删除/启停接口可用
- 验收标准:
  - 基本运营管理能力完整

### TASK-ID: ADMIN-BROADCAST-API-CRUD-001
- 状态: blocked
- 优先级: P1
- 所属子域: 公告 / 广播 / 违禁词
- 目标: 实现后台广播接口组
- 依赖:
  - ADMIN-MODEL-OPS-001
  - NOTICE-SVC-DISPATCH-001
  - ADMIN-MW-PERM-001
- 输入:
  - 广播接口文档
- 输出:
  - 广播列表/详情/发送/删除接口可用
- 验收标准:
  - 广播发送有留痕

### TASK-ID: ADMIN-BANNEDWORD-API-CRUD-001
- 状态: blocked
- 优先级: P0
- 所属子域: 公告 / 广播 / 违禁词
- 目标: 实现后台违禁词 CRUD 接口组
- 依赖:
  - ADMIN-MODEL-OPS-001
  - ADMIN-MW-PERM-001
- 输入:
  - 违禁词接口文档
- 输出:
  - 违禁词列表/新增/编辑/删除/启停接口可用
- 验收标准:
  - 可供职位、评论、聊天内容审核复用

### 11.5.9 系统配置 / 导出 / 日志

### TASK-ID: ADMIN-CONFIG-API-LIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 系统配置 / 导出 / 日志
- 目标: 实现后台系统配置查询接口
- 依赖:
  - BOOT-SYSCONFIG-002
  - ADMIN-MW-PERM-001
- 输入:
  - category
- 输出:
  - 系统配置查询接口可用
- 验收标准:
  - 支持按分类获取配置

### TASK-ID: ADMIN-CONFIG-SVC-UPDATE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 系统配置 / 导出 / 日志
- 目标: 实现后台系统配置更新服务
- 依赖:
  - BOOT-SYSCONFIG-002
  - BOOT-DB-CONFIGLOG-001
  - BOOT-DB-OPLOG-001
- 输入:
  - 配置更新数组
  - reason
- 输出:
  - 系统配置更新逻辑可用
- 验收标准:
  - 可批量更新配置
  - 写 config_change_logs 与 operation_logs

### TASK-ID: ADMIN-CONFIG-API-UPDATE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 系统配置 / 导出 / 日志
- 目标: 实现后台系统配置更新接口
- 依赖:
  - ADMIN-CONFIG-SVC-UPDATE-001
  - ADMIN-MW-PERM-001
- 输入:
  - 配置更新参数
- 输出:
  - 配置更新接口可用
- 验收标准:
  - 返回更新结果

### TASK-ID: ADMIN-DB-EXPORT-001
- 状态: blocked
- 优先级: P0
- 所属子域: 系统配置 / 导出 / 日志
- 目标: 创建 `export_tasks` 表 migration
- 依赖:
  - BOOT-DB-001
  - ADMIN-DB-ACCOUNT-001
- 输入:
  - 导出任务接口文档
- 输出:
  - export_tasks 表可用
- 验收标准:
  - 支持 pending/processing/completed/failed 状态

### TASK-ID: ADMIN-EXPORT-MODEL-001
- 状态: blocked
- 优先级: P0
- 所属子域: 系统配置 / 导出 / 日志
- 目标: 创建 `ExportTask` 模型
- 依赖:
  - ADMIN-DB-EXPORT-001
- 输入:
  - export_tasks 表结构
- 输出:
  - ExportTask 模型可用
- 验收标准:
  - 可支撑导出任务列表与状态查询

### TASK-ID: ADMIN-EXPORT-SVC-CREATE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 系统配置 / 导出 / 日志
- 目标: 实现后台导出任务创建服务
- 依赖:
  - ADMIN-EXPORT-MODEL-001
  - BOOT-DB-OPLOG-001
- 输入:
  - export_type
  - filters
  - reason
- 输出:
  - 导出任务创建逻辑可用
- 验收标准:
  - 创建 pending 任务
  - 有操作日志

### TASK-ID: ADMIN-EXPORT-API-CREATE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 系统配置 / 导出 / 日志
- 目标: 实现后台导出任务创建接口
- 依赖:
  - ADMIN-EXPORT-SVC-CREATE-001
  - ADMIN-MW-PERM-001
- 输入:
  - 导出参数
- 输出:
  - 导出任务创建接口可用
- 验收标准:
  - 返回 task_id/status

### TASK-ID: ADMIN-EXPORT-API-LIST-001
- 状态: blocked
- 优先级: P1
- 所属子域: 系统配置 / 导出 / 日志
- 目标: 实现后台导出任务列表接口
- 依赖:
  - ADMIN-EXPORT-MODEL-001
  - ADMIN-MW-PERM-001
- 输入:
  - page/limit/status
- 输出:
  - 导出任务列表接口可用
- 验收标准:
  - 可分页返回导出任务状态

### TASK-ID: ADMIN-OPLOG-API-LIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 系统配置 / 导出 / 日志
- 目标: 实现操作日志查询接口
- 依赖:
  - BOOT-DB-OPLOG-001
  - ADMIN-MW-PERM-001
- 输入:
  - operator_id/module/operation_type/target_type/result/page/limit
- 输出:
  - 操作日志查询接口可用
- 验收标准:
  - 支持多维筛选与分页

### TASK-ID: ADMIN-CONFIGLOG-API-LIST-001
- 状态: blocked
- 优先级: P1
- 所属子域: 系统配置 / 导出 / 日志
- 目标: 实现配置变更日志查询接口
- 依赖:
  - BOOT-DB-CONFIGLOG-001
  - ADMIN-MW-PERM-001
- 输入:
  - page/limit/operator_id
- 输出:
  - 配置变更日志查询接口可用
- 验收标准:
  - 可查看 before/after snapshot

### 11.5.10 测试与执行顺序

### TASK-ID: ADMIN-TEST-AUTH-001
- 状态: blocked
- 优先级: P0
- 所属子域: 测试与执行顺序
- 目标: 编写后台登录与权限测试
- 依赖:
  - ADMIN-API-LOGIN-001
  - ADMIN-MW-PERM-001
- 输入:
  - 后台登录与权限中间件
- 输出:
  - Feature Test
- 验收标准:
  - 登录成功
  - 无权限访问被拦截

### TASK-ID: ADMIN-TEST-COMPANY-001
- 状态: blocked
- 优先级: P0
- 所属子域: 测试与执行顺序
- 目标: 编写企业审核接口测试
- 依赖:
  - ADMIN-COMPANY-API-REVIEW-001
- 输入:
  - 企业审核接口
- 输出:
  - Feature Test
- 验收标准:
  - approve/reject 成功
  - 用户认证状态同步正确

### TASK-ID: ADMIN-TEST-JOB-001
- 状态: blocked
- 优先级: P0
- 所属子域: 测试与执行顺序
- 目标: 编写职位审核接口测试
- 依赖:
  - ADMIN-JOB-API-REVIEW-001
- 输入:
  - 职位审核接口
- 输出:
  - Feature Test
- 验收标准:
  - 审核通过/拒绝逻辑正确

### TASK-ID: ADMIN-TEST-REPORT-001
- 状态: blocked
- 优先级: P1
- 所属子域: 测试与执行顺序
- 目标: 编写举报处理接口测试
- 依赖:
  - ADMIN-REPORT-API-HANDLE-001
- 输入:
  - 举报处理接口
- 输出:
  - Feature Test
- 验收标准:
  - 举报状态更新正确
  - 通知挂接可触发

### TASK-ID: ADMIN-TEST-CONFIG-001
- 状态: blocked
- 优先级: P1
- 所属子域: 测试与执行顺序
- 目标: 编写系统配置更新与日志测试
- 依赖:
  - ADMIN-CONFIG-API-UPDATE-001
  - ADMIN-CONFIGLOG-API-LIST-001
- 输入:
  - 系统配置接口
- 输出:
  - Feature Test
- 验收标准:
  - 更新成功
  - config_change_logs 写入成功

## 11.6 本章推荐执行顺序

### 第一批（P0）
1. ADMIN-DB-ACCOUNT-001
2. ADMIN-DB-PERM-001
3. ADMIN-MODEL-ACCOUNT-001
4. ADMIN-MODEL-PERM-001
5. ADMIN-REQ-LOGIN-001
6. ADMIN-SVC-TOKEN-001
7. ADMIN-SVC-LOGIN-001
8. ADMIN-API-LOGIN-001
9. ADMIN-MW-PERM-001
10. ADMIN-USER-SVC-LIST-001
11. ADMIN-USER-API-LIST-001
12. ADMIN-USER-API-DETAIL-001
13. ADMIN-COMPANY-API-LIST-001
14. ADMIN-COMPANY-API-DETAIL-001
15. ADMIN-COMPANY-SVC-REVIEW-001
16. ADMIN-COMPANY-API-REVIEW-001
17. ADMIN-JOB-API-LIST-001
18. ADMIN-JOB-API-DETAIL-001
19. ADMIN-JOB-SVC-REVIEW-001
20. ADMIN-JOB-API-REVIEW-001
21. ADMIN-CATEGORY-API-APPLYLIST-001
22. ADMIN-CATEGORY-SVC-APPLYREVIEW-001
23. ADMIN-CATEGORY-API-APPLYREVIEW-001
24. ADMIN-REPORT-API-LIST-001
25. ADMIN-REPORT-API-DETAIL-001
26. ADMIN-REPORT-SVC-HANDLE-001
27. ADMIN-REPORT-API-HANDLE-001
28. ADMIN-BLACK-API-LIST-001
29. ADMIN-BLACK-SVC-CREATE-001
30. ADMIN-BLACK-API-CREATE-001
31. ADMIN-DB-ANNOUNCE-001
32. ADMIN-DB-BROADCAST-001
33. ADMIN-DB-BANNEDWORD-001
34. ADMIN-MODEL-OPS-001
35. ADMIN-ANNOUNCE-API-CRUD-001
36. ADMIN-BANNEDWORD-API-CRUD-001
37. ADMIN-CONFIG-API-LIST-001
38. ADMIN-CONFIG-SVC-UPDATE-001
39. ADMIN-CONFIG-API-UPDATE-001
40. ADMIN-DB-EXPORT-001
41. ADMIN-EXPORT-MODEL-001
42. ADMIN-OPLOG-API-LIST-001
43. ADMIN-TEST-AUTH-001
44. ADMIN-TEST-COMPANY-001
45. ADMIN-TEST-JOB-001

### 第二批（P1）
1. ADMIN-USER-SVC-RESETPWD-001
2. ADMIN-USER-API-RESETPWD-001
3. ADMIN-USER-SVC-DELETE-001
4. ADMIN-JOB-SVC-TOPSET-001
5. ADMIN-BLACK-SVC-UPDATE-001
6. ADMIN-BLACK-SVC-REMOVE-001
7. ADMIN-BROADCAST-API-CRUD-001
8. ADMIN-EXPORT-SVC-CREATE-001
9. ADMIN-EXPORT-API-CREATE-001
10. ADMIN-EXPORT-API-LIST-001
11. ADMIN-CONFIGLOG-API-LIST-001
12. ADMIN-TEST-REPORT-001
13. ADMIN-TEST-CONFIG-001

## 11.7 本章完成后的下一个章节入口
当第 11 章完成后，最合理进入：
- 第12章：安全 / 测试 / 部署 / 上线维护

原因：
- 到第11章为止，前台主链路、业务增强层、后台治理层都已经具备完整施工清单
- 最后一章应聚焦收口：安全加固、测试补齐、部署上线、监控与维护

---

# 第12章：安全 / 测试 / 部署 / 上线维护

> 目标：为 ZPW 项目建立完整的安全基线、测试体系、部署方案、上线检查流程、监控告警与日常维护机制，确保从“功能可开发”真正落到“系统可上线、可稳定运行、可持续维护”。
> 本章是全项目收口章，重点不是新增业务功能，而是给前 1～11 章提供质量保证、运行保证与上线保证。

## 12.1 本章定位

### 本章要解决的问题
- 建立认证、鉴权、限流、内容安全、文件安全、数据安全基线
- 建立接口测试、服务测试、WebSocket 联调、回归测试、发布前验收清单
- 建立 Laravel + MySQL + Redis + Workerman/GatewayWorker + UniApp 后端部署方案
- 建立队列、定时任务、日志、监控、备份与恢复方案
- 建立上线前检查、灰度发布、回滚与上线后巡检机制

### 本章不做的内容
- 复杂云原生多集群架构
- 自动化压测平台
- 自研可观测性平台
- 商业级 WAF / SOC 平台建设

## 12.2 本章完成标准
当第 12 章完成时，应满足：
1. 核心认证与接口安全基线明确并有执行任务
2. 文件、内容、权限、限流、安全日志等关键安全措施可落地
3. 测试矩阵完整，覆盖主链路与后台治理链路
4. 部署文档、环境变量、进程管理、日志路径、备份方案明确
5. 上线检查清单、回滚方案、上线后巡检流程明确
6. 全项目具备从开发到上线维护的完整执行闭环

## 12.3 本章依赖关系
### 上游依赖
- 第1章～第11章全部模块
- docs/07-安全与性能.md
- docs/08-部署说明.md

### 下游依赖
- 实际开发排期
- 测试执行排期
- 首次上线与后续版本发布

## 12.4 本章任务总览
1. 认证与接口安全
2. 内容与文件安全
3. 数据安全与审计
4. 队列 / 定时任务 / WebSocket 运行安全
5. 测试矩阵与验收
6. 部署结构与环境配置
7. 监控、备份、恢复
8. 上线检查、回滚、巡检

## 12.5 可直接执行任务清单

### 12.5.1 认证与接口安全

### TASK-ID: SEC-AUTH-JWT-001
- 状态: blocked
- 优先级: P0
- 所属子域: 认证与接口安全
- 目标: 落实用户端与后台端 JWT 安全策略
- 依赖:
  - BOOT-AUTH-001
  - ADMIN-SVC-TOKEN-001
- 输入:
  - 技术架构文档
  - 安全文档
- 输出:
  - JWT 签发、过期、刷新、黑名单/失效策略设计落地
- 验收标准:
  - 前台 token 与后台 token 能明确区分
  - 失效 token 不可继续访问
  - 密码修改/账号封禁后 token 可失效

### TASK-ID: SEC-AUTH-DEVICE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 认证与接口安全
- 目标: 完善设备会话与多端登录安全策略
- 依赖:
  - AUTH-MODEL-SESSION-001
  - CHAT-WS-KICKOUT-001
- 输入:
  - 登录、ws、设备会话文档
- 输出:
  - 设备会话安全策略说明与实现任务
- 验收标准:
  - 可支持单设备踢下线或多设备并存策略
  - 会话失效与 ws 连接状态联动

### TASK-ID: SEC-RATE-API-001
- 状态: blocked
- 优先级: P0
- 所属子域: 认证与接口安全
- 目标: 落实接口限流策略
- 依赖:
  - BOOT-APP-002
  - BOOT-ERRORCODE-001
- 输入:
  - 安全文档中的频控建议
- 输出:
  - 登录、注册、验证码、聊天发送、评论、举报等接口限流方案可用
- 验收标准:
  - 高频敏感接口有限流配置
  - 超限返回统一错误码与提示

### TASK-ID: SEC-RATE-WS-001
- 状态: blocked
- 优先级: P1
- 所属子域: 认证与接口安全
- 目标: 落实 WebSocket 动作频控策略
- 依赖:
  - CHAT-WS-ROUTER-001
  - CHAT-WS-ERROR-001
- 输入:
  - ws action 列表
- 输出:
  - ping/message.send/join/leave/read 等 ws 频控策略可用
- 验收标准:
  - 超频 action 返回统一错误事件

### TASK-ID: SEC-PERM-ADMIN-001
- 状态: blocked
- 优先级: P0
- 所属子域: 认证与接口安全
- 目标: 收敛后台权限点矩阵
- 依赖:
  - ADMIN-MW-PERM-001
- 输入:
  - 第11章后台接口清单
- 输出:
  - 后台权限 code 清单文档与初始化数据可用
- 验收标准:
  - 企业审核、职位审核、举报处理、配置修改等高危权限独立拆分

### 12.5.2 内容与文件安全

### TASK-ID: SEC-CONTENT-BANNED-001
- 状态: blocked
- 优先级: P0
- 所属子域: 内容与文件安全
- 目标: 将违禁词检测接入职位、评论、聊天、帮助反馈等写操作
- 依赖:
  - ADMIN-BANNEDWORD-API-CRUD-001
  - CHAT-SVC-SEND-001
  - COMMENT-SVC-CREATE-001
- 输入:
  - 违禁词表
  - 各内容写入服务
- 输出:
  - 基础违禁词检测能力可复用
- 验收标准:
  - 敏感文本可拦截或按策略替换/打标
  - 高风险内容有统一错误返回

### TASK-ID: SEC-FILE-UPLOAD-001
- 状态: blocked
- 优先级: P0
- 所属子域: 内容与文件安全
- 目标: 落实文件上传安全限制
- 依赖:
  - BOOT-DB-FILES-001
  - BOOT-FILE-RES-001
- 输入:
  - 文件上传设计
- 输出:
  - 文件大小、类型、扩展名、MIME、命名与访问控制策略可用
- 验收标准:
  - 禁止危险脚本类文件
  - 文件路径不暴露真实磁盘结构
  - 图片/附件访问策略清晰

### TASK-ID: SEC-FILE-SCAN-001
- 状态: blocked
- 优先级: P1
- 所属子域: 内容与文件安全
- 目标: 预留文件安全扫描或人工审核钩子
- 依赖:
  - SEC-FILE-UPLOAD-001
- 输入:
  - 上传成功后的文件记录
- 输出:
  - 文件扫描钩子或状态字段设计可用
- 验收标准:
  - 后续可接入第三方扫描能力

### TASK-ID: SEC-PRIVACY-DESENS-001
- 状态: blocked
- 优先级: P1
- 所属子域: 内容与文件安全
- 目标: 梳理前后台敏感字段脱敏输出规则
- 依赖:
  - AUTH-RES-USER-001
  - COMPANY-RES-DETAIL-001
  - ADMIN-USER-API-DETAIL-001
- 输入:
  - 用户、企业、简历、联系方式字段
- 输出:
  - 脱敏输出规则文档与资源层改造任务
- 验收标准:
  - 非授权场景不直接泄露手机号、身份证号、证件图片链接等信息

### 12.5.3 数据安全与审计

### TASK-ID: SEC-DB-INDEX-001
- 状态: blocked
- 优先级: P0
- 所属子域: 数据安全与审计
- 目标: 全量复核核心表索引与唯一约束
- 依赖:
  - 第1章至第11章所有 migration 任务
- 输入:
  - 全部表结构清单
- 输出:
  - 索引复核表与修正任务清单可用
- 验收标准:
  - 高频查询表具备必要索引
  - 幂等/去重场景具备唯一约束

### TASK-ID: SEC-DB-TRANS-001
- 状态: blocked
- 优先级: P0
- 所属子域: 数据安全与审计
- 目标: 识别并补齐关键事务边界
- 依赖:
  - APPLY-SVC-* 相关任务
  - CHAT-SVC-SEND-001
  - ADMIN-COMPANY-SVC-REVIEW-001
  - ADMIN-JOB-SVC-REVIEW-001
- 输入:
  - 主链路写操作服务
- 输出:
  - 关键事务清单与改造任务可用
- 验收标准:
  - 报名、审核、聊天发送、通知派发等关键流程事务边界明确

### TASK-ID: SEC-AUDIT-OPLOG-001
- 状态: blocked
- 优先级: P0
- 所属子域: 数据安全与审计
- 目标: 统一高危操作审计口径
- 依赖:
  - BOOT-DB-OPLOG-001
  - ADMIN-CONFIG-SVC-UPDATE-001
- 输入:
  - 后台高危动作清单
- 输出:
  - 审计动作枚举、记录字段、日志写入规范可用
- 验收标准:
  - 配置变更、用户封禁、审核拒绝、黑名单新增等动作有审计留痕

### TASK-ID: SEC-DATA-BACKUP-001
- 状态: blocked
- 优先级: P0
- 所属子域: 数据安全与审计
- 目标: 定义数据库与上传文件备份策略
- 依赖:
  - DEPLOY-OPS-BACKUP-001
- 输入:
  - 数据目录、上传目录、数据库实例信息
- 输出:
  - 备份频率、保留策略、恢复演练要求文档可用
- 验收标准:
  - 明确全量/增量或至少每日全量策略
  - 明确恢复验证要求

### 12.5.4 队列 / 定时任务 / WebSocket 运行安全

### TASK-ID: OPS-QUEUE-BASE-001
- 状态: blocked
- 优先级: P0
- 所属子域: 队列 / 定时任务 / WebSocket 运行安全
- 目标: 梳理并落地队列任务基线
- 依赖:
  - NOTICE-SVC-DISPATCH-001
  - ADMIN-EXPORT-SVC-CREATE-001
- 输入:
  - 异步任务清单
- 输出:
  - 队列任务分类、重试、超时、失败处理策略可用
- 验收标准:
  - 通知、导出、广播等异步场景有明确队列策略

### TASK-ID: OPS-CRON-BASE-001
- 状态: blocked
- 优先级: P1
- 所属子域: 队列 / 定时任务 / WebSocket 运行安全
- 目标: 梳理定时任务清单
- 依赖:
  - JOB-SVC-TOPAUTO-001
  - ADMIN-BLACK-SVC-REMOVE-001
- 输入:
  - 过期下架、置顶失效、黑名单到期、广播发送等任务
- 输出:
  - 定时任务列表与执行频率方案可用
- 验收标准:
  - 每类定时任务有明确入口与执行周期

### TASK-ID: OPS-WS-PROC-001
- 状态: blocked
- 优先级: P0
- 所属子域: 队列 / 定时任务 / WebSocket 运行安全
- 目标: 制定 Workerman/GatewayWorker 进程部署与重启策略
- 依赖:
  - CHAT-WS-BOOT-001
  - docs/08-部署说明.md
- 输入:
  - websocket 启动脚本与部署说明
- 输出:
  - websocket 进程管理策略文档可用
- 验收标准:
  - 包括启动、停止、重启、日志、异常恢复建议

### TASK-ID: OPS-WS-CONN-001
- 状态: blocked
- 优先级: P1
- 所属子域: 队列 / 定时任务 / WebSocket 运行安全
- 目标: 明确 ws 连接数、心跳、断线重连与在线状态一致性策略
- 依赖:
  - CHAT-SVC-PRESENCE-001
  - CHAT-WS-PING-001
- 输入:
  - ws 协议与 presence 逻辑
- 输出:
  - 连接治理策略文档可用
- 验收标准:
  - 心跳超时与离线状态回收策略明确

### 12.5.5 测试矩阵与验收

### TASK-ID: QA-TEST-MATRIX-001
- 状态: blocked
- 优先级: P0
- 所属子域: 测试矩阵与验收
- 目标: 建立全项目测试矩阵
- 依赖:
  - 第1章～第11章所有测试任务
- 输入:
  - 所有模块清单
- 输出:
  - 模块 × 用例 × 责任人 × 状态 的测试矩阵文档可用
- 验收标准:
  - 覆盖用户端、企业端、后台端、ws、定时任务、导出任务

### TASK-ID: QA-TEST-AUTO-001
- 状态: blocked
- 优先级: P0
- 所属子域: 测试矩阵与验收
- 目标: 整理 PHPUnit/Feature/Unit 自动化测试执行基线
- 依赖:
  - BOOT-TEST-002
- 输入:
  - Laravel 测试结构
- 输出:
  - 自动化测试目录规范、命名规范、执行命令文档可用
- 验收标准:
  - 新增模块都能按统一规范补测试

### TASK-ID: QA-TEST-WSMANUAL-001
- 状态: blocked
- 优先级: P1
- 所属子域: 测试矩阵与验收
- 目标: 建立 WebSocket 联调与回归检查单
- 依赖:
  - CHAT-TEST-WSMANUAL-001
- 输入:
  - ws 事件契约
- 输出:
  - ws 回归测试清单可用
- 验收标准:
  - 覆盖握手、发送、已读、撤回、kickout、公共聊天室

### TASK-ID: QA-UAT-CHECKLIST-001
- 状态: blocked
- 优先级: P0
- 所属子域: 测试矩阵与验收
- 目标: 建立上线前 UAT 验收清单
- 依赖:
  - QA-TEST-MATRIX-001
- 输入:
  - 业务流程图
  - 全模块接口清单
- 输出:
  - UAT 验收 checklist 可用
- 验收标准:
  - 至少覆盖注册登录、简历、企业认证、职位、报名、聊天、通知、后台审核等主链路

### 12.5.6 部署结构与环境配置

### TASK-ID: DEPLOY-DOC-ENV-001
- 状态: blocked
- 优先级: P0
- 所属子域: 部署结构与环境配置
- 目标: 梳理 `.env` 与环境变量清单
- 依赖:
  - docs/08-部署说明.md
- 输入:
  - Laravel、MySQL、Redis、WS、上传、短信、地图等配置项需求
- 输出:
  - 环境变量清单文档可用
- 验收标准:
  - 开发/测试/生产环境关键变量完整列出

### TASK-ID: DEPLOY-DOC-NGINX-001
- 状态: blocked
- 优先级: P0
- 所属子域: 部署结构与环境配置
- 目标: 生成 Nginx 站点与反向代理部署说明
- 依赖:
  - docs/08-部署说明.md
  - OPS-WS-PROC-001
- 输入:
  - HTTP 服务与 WS 服务端口规划
- 输出:
  - Nginx 配置样板与部署说明文档可用
- 验收标准:
  - 包括静态文件、PHP 入口、ws 反代、上传目录访问策略

### TASK-ID: DEPLOY-DOC-PROC-001
- 状态: blocked
- 优先级: P0
- 所属子域: 部署结构与环境配置
- 目标: 生成 PHP-FPM / Queue / Scheduler / Workerman 进程管理说明
- 依赖:
  - OPS-QUEUE-BASE-001
  - OPS-CRON-BASE-001
  - OPS-WS-PROC-001
- 输入:
  - 各运行进程清单
- 输出:
  - Supervisor 或 systemd 管理说明可用
- 验收标准:
  - 各进程启动命令、重启策略、日志路径明确

### TASK-ID: DEPLOY-DOC-BUILD-001
- 状态: blocked
- 优先级: P1
- 所属子域: 部署结构与环境配置
- 目标: 梳理前端 uniapp 与后端 Laravel 构建/发布步骤
- 依赖:
  - docs/08-部署说明.md
- 输入:
  - 构建命令、产物目录、环境区分规则
- 输出:
  - 构建发布步骤文档可用
- 验收标准:
  - 前后端发布顺序与注意事项明确

### 12.5.7 监控、备份、恢复

### TASK-ID: OPS-MON-LOG-001
- 状态: blocked
- 优先级: P0
- 所属子域: 监控、备份、恢复
- 目标: 统一应用日志、ws 日志、队列日志、Nginx 日志路径与保留策略
- 依赖:
  - DEPLOY-DOC-PROC-001
- 输入:
  - 各进程日志输出方式
- 输出:
  - 日志路径规范文档可用
- 验收标准:
  - 问题排查时能快速定位日志来源

### TASK-ID: OPS-MON-ALERT-001
- 状态: blocked
- 优先级: P1
- 所属子域: 监控、备份、恢复
- 目标: 定义基础告警策略
- 依赖:
  - OPS-MON-LOG-001
- 输入:
  - 关键错误场景
- 输出:
  - 数据库连接异常、Redis 异常、队列堆积、ws 进程退出、磁盘不足等告警策略文档可用
- 验收标准:
  - 每类关键故障有明确告警触发条件

### TASK-ID: DEPLOY-OPS-BACKUP-001
- 状态: blocked
- 优先级: P0
- 所属子域: 监控、备份、恢复
- 目标: 制定数据库与上传目录备份执行方案
- 依赖:
  - DEPLOY-DOC-ENV-001
- 输入:
  - 数据库连接信息
  - 上传目录路径
- 输出:
  - 备份脚本/备份流程文档可用
- 验收标准:
  - 明确执行频率、存储位置、保留周期

### TASK-ID: DEPLOY-OPS-RECOVERY-001
- 状态: blocked
- 优先级: P1
- 所属子域: 监控、备份、恢复
- 目标: 制定恢复演练流程
- 依赖:
  - DEPLOY-OPS-BACKUP-001
  - SEC-DATA-BACKUP-001
- 输入:
  - 备份产物
- 输出:
  - DB 恢复与文件恢复演练说明可用
- 验收标准:
  - 明确恢复步骤、恢复后验证项、恢复责任人

### 12.5.8 上线检查、回滚、巡检

### TASK-ID: RELEASE-CHECK-001
- 状态: blocked
- 优先级: P0
- 所属子域: 上线检查、回滚、巡检
- 目标: 建立首次上线检查清单
- 依赖:
  - QA-UAT-CHECKLIST-001
  - DEPLOY-DOC-ENV-001
  - DEPLOY-DOC-NGINX-001
  - DEPLOY-DOC-PROC-001
- 输入:
  - 全部部署与测试文档
- 输出:
  - 上线前 checklist 可用
- 验收标准:
  - 覆盖环境变量、数据库迁移、缓存清理、队列、ws、日志、权限、域名、证书等项

### TASK-ID: RELEASE-ROLLBACK-001
- 状态: blocked
- 优先级: P0
- 所属子域: 上线检查、回滚、巡检
- 目标: 建立版本回滚方案
- 依赖:
  - DEPLOY-DOC-BUILD-001
  - DEPLOY-OPS-BACKUP-001
- 输入:
  - 发布方式与产物结构
- 输出:
  - 代码回滚、数据库回滚、配置回滚、静态资源回滚方案可用
- 验收标准:
  - 明确哪些变更可直接回滚，哪些需要前置备份

### TASK-ID: RELEASE-INSPECT-001
- 状态: blocked
- 优先级: P1
- 所属子域: 上线检查、回滚、巡检
- 目标: 建立上线后 1h / 24h 巡检清单
- 依赖:
  - OPS-MON-ALERT-001
  - RELEASE-CHECK-001
- 输入:
  - 核心指标与关键日志入口
- 输出:
  - 上线后巡检清单可用
- 验收标准:
  - 至少覆盖登录、发布职位、报名、聊天、后台审核、错误日志、磁盘、队列、ws 进程状态

## 12.6 本章推荐执行顺序

### 第一批（P0）
1. SEC-AUTH-JWT-001
2. SEC-RATE-API-001
3. SEC-PERM-ADMIN-001
4. SEC-CONTENT-BANNED-001
5. SEC-FILE-UPLOAD-001
6. SEC-DB-INDEX-001
7. SEC-DB-TRANS-001
8. SEC-AUDIT-OPLOG-001
9. OPS-QUEUE-BASE-001
10. OPS-WS-PROC-001
11. QA-TEST-MATRIX-001
12. QA-TEST-AUTO-001
13. QA-UAT-CHECKLIST-001
14. DEPLOY-DOC-ENV-001
15. DEPLOY-DOC-NGINX-001
16. DEPLOY-DOC-PROC-001
17. OPS-MON-LOG-001
18. DEPLOY-OPS-BACKUP-001
19. SEC-DATA-BACKUP-001
20. RELEASE-CHECK-001
21. RELEASE-ROLLBACK-001

### 第二批（P1）
1. SEC-AUTH-DEVICE-001
2. SEC-RATE-WS-001
3. SEC-FILE-SCAN-001
4. SEC-PRIVACY-DESENS-001
5. OPS-CRON-BASE-001
6. OPS-WS-CONN-001
7. QA-TEST-WSMANUAL-001
8. DEPLOY-DOC-BUILD-001
9. OPS-MON-ALERT-001
10. DEPLOY-OPS-RECOVERY-001
11. RELEASE-INSPECT-001

## 12.7 本章完成后的项目收口入口
当第 12 章完成后，整个 ZPW 项目的“AI 可执行开发总清单”即进入收口阶段。

建议紧接着补两份附录型文件：
1. 《ZPW_开发阶段排期表.md》
2. 《ZPW_联调与测试用例总表.md》

原因：
- 当前主文件已经完成“做什么、按什么顺序做”的全量拆解
- 下一步最有价值的是把这些任务进一步压缩成排期视图与测试执行视图
