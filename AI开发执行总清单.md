# ZPW 项目 AI 开发执行总清单

> 文档目标：把《自动化装备招聘系统》从“设计文档项目”转换成“AI 可直接施工的开发清单”。
>
> 使用原则：**AI 不需要再猜，就能直接开工。**
>
> 当前项目现状：仓库内只有技术文档，没有实际前后端代码、数据库迁移、测试、部署脚本。
>
> 本文件定位：
> - 不是产品需求文档
> - 不是泛泛开发计划
> - 不是阶段口号
> - 是可以直接拿来指挥 AI 开发的 **单文件总控施工表**
>
> 标签说明：
> - `【文档明确】`：原技术文档中已明确要求或已明确出现的内容，属于必须覆盖范围
> - `【实施建议】`：为让 AI 能顺利落地而补充的工程化执行建议，不代表原文档逐字要求
> - `【超计划风险】`：如果实现时把此类建议误当成强制范围，可能导致偏离原文档，应谨慎控制

---

# 0. 当前进展

## 0.1 已完成

- 已完整通读项目仓库内全部技术文档
- 已确认项目边界、角色、数据库设计、流程、接口、安全与部署方案
- 已完成 AI 开发清单框架设计
- 已收敛为 **单文件方案**，不再按一个模块一个文件拆分
- 已生成本文件：`AI开发执行总清单.md`

## 0.2 当前结论

这个项目目前具备：

- 产品定义
- 技术架构
- 数据库设计
- 页面设计
- 功能流程
- API 规范
- 安全方案
- 部署说明

但目前 **不具备**：

- Laravel 工程代码
- uniapp 前端代码
- migration 文件
- Workerman 启动代码
- 测试代码
- 真实可运行实现

因此，AI 开发第一目标不是“改现有代码”，而是：

> **从 0 建立可运行工程骨架，并严格按文档实现。**

## 0.3 文档一致性审查结论

### 审查结论（当前版）

- **主干方向符合技术文档**：角色、技术栈、核心模块、业务主链路、后台审核方向基本一致
- **存在漏项**：有一些在原技术文档里已经明确存在的表、模块和任务，在总清单里最初没有显式列出，需要补齐
- **存在少量实施层扩展**：为了让 AI 能直接开工，加入了部分工程化建议；这些建议是合理的，但不应被误认成原始文档的硬性范围

### 本次审查认定的“原文档明确漏项”

以下内容在原文档中明确出现，必须纳入总清单：

- `operation_logs`
- `config_change_logs`
- `announcements`
- `system_broadcasts`
- `category_applications`
- `export_tasks`
- `help_categories`
- `help_articles`
- `help_article_feedbacks`
- `help_search_synonyms`
- `help_article_relations`
- `help_search_logs`

### 本次审查认定的“实施建议，不算原文档强制项”

以下内容可以保留，但必须按 `【实施建议】` 理解，而不是按“文档原始强制范围”理解：

- 目录规范细分（如 `app/Services`、`app/Policies`、`app/Exceptions`）
- 测试目录与测试策略骨架
- `.env.production.example`
- 当前阶段优先后端、后做前端页面的执行顺序
- 某些统一辅助层命名（如解析器、helper、trait 的具体命名方式）

### 控制原则

后续执行时必须遵循：

1. **先满足 `【文档明确】`**
2. 再选择性实现 `【实施建议】`
3. 如某项建议会扩大范围、拖慢主线、改变原文档口径，应视为 `【超计划风险】` 并降级处理

---

# 1. 本文件怎么用

## 1.1 这份文件要解决的事

它只解决 7 件事：

1. 这个项目总共要做什么
2. 应该先做什么后做什么
3. 每个模块真正需要做哪些事
4. 每一项是否已经具备开工条件
5. 每一项任务的输入输出是什么
6. AI 做完之后如何回填状态
7. 如何保证下一次 AI 接手时不需要重新猜

## 1.2 任务粒度标准

每条任务应尽量满足以下条件：

- 只有一个明确目标
- 有明确输入
- 有明确输出
- 有明确依赖
- 有明确验收标准
- AI 一次可以独立完成，或显著推进

### 不合格示例

- 做用户系统
- 做职位模块
- 做聊天功能
- 完成后台

### 合格示例

- 创建 `users` 表 migration，并补齐唯一索引、锁定字段、注销字段
- 创建 `User` 模型，并补齐 casts、hidden、关联关系
- 实现 `GET /api/user/captcha`
- 实现 `POST /api/user/register`
- 实现登录失败累计和账号锁定逻辑
- 为注册接口编写 Feature Test

---

# 2. 全局执行规则

## 2.1 统一状态

- `todo`：未开始
- `ready`：依赖满足，可直接开工
- `doing`：开发中
- `blocked`：被依赖或条件阻塞
- `review`：代码完成，待复核
- `testing`：测试中
- `done`：开发与验收完成
- `cancelled`：取消

## 2.2 统一优先级

- `P0`：阻塞主链路，必须优先完成
- `P1`：核心能力增强，建议尽快完成
- `P2`：体验增强、运营增强
- `P3`：优化项、后续增强项

## 2.3 统一任务编号

格式：

`模块-子域-序号`

示例：

- `BOOT-APP-001`
- `AUTH-API-REG-001`
- `JOB-DB-001`
- `APPLY-API-REVIEW-001`
- `CHAT-WS-001`
- `ADMIN-COMPANY-AUDIT-001`

要求：

- 一个任务编号只对应一个任务
- 一个任务不要承载多个无关目标

## 2.4 AI 接任务前必须检查

1. 当前任务状态是否为 `ready`
2. 依赖任务是否已完成
3. 输入文档是否明确
4. 目标输出路径是否明确
5. 验收标准是否明确
6. 是否会与其他进行中的任务产生文件冲突

如果不满足，任务应先标记为：

- `blocked`

并注明阻塞原因。

## 2.5 AI 完成任务后必须回填

每条任务完成后至少记录：

- 状态变更为 `review` 或 `done`
- 实际改动文件
- 未完成点
- 风险点
- 后续建议任务
- 是否与文档有偏差

## 2.6 AI 禁止事项

### 禁止扩 scope

比如任务是：

- 实现注册接口

AI 不应顺手做：

- 登录接口
- 设备管理
- 邮件找回密码
- WebSocket 配置

除非任务明确要求。

### 禁止偷偷改口径

不得擅自修改：

- 状态枚举
- 错误码
- 接口响应结构
- 字段命名
- 核心业务状态机

如需调整，必须记录“偏差说明”。

### 禁止只报喜不报忧

如果任务存在：

- 依赖缺失
- 文档冲突
- 结构不支持
- 测试未通过

必须明确写出，不能直接标 `done`。

---

# 3. 全局统一约束

## 3.1 后端技术基线

按文档执行：

- 后端：Laravel 11 `【文档明确】`
- PHP：8.2+ `【文档明确】`
- 数据库：MySQL 8.0+ `【文档明确】`
- 缓存/队列：Redis 6.0+ `【文档明确】`
- 实时通讯：Workerman + GatewayWorker `【文档明确】`
- 鉴权：JWT Bearer Token `【文档明确】`
- 文件：Laravel Filesystem，public/private 双盘 `【文档明确】`

## 3.2 前端技术基线

按文档执行：

- 前端：uniapp `【文档明确】`
- UI：拟态风格 `【文档明确】`
- “第一阶段优先后端接口骨架、前端页面后置”属于实施策略，记为 `【实施建议】`，不是原文档硬性要求

## 3.3 接口统一约束

- 所有 API 统一输出 `code / msg / data` `【文档明确】`
- 除成功外，HTTP 状态码不能一律返回 200 `【文档明确】`
- 文件字段统一返回 File Resource `【文档明确】`
- 私有文件必须返回临时签名地址，不直接暴露裸路径 `【文档明确】`
- Token 自动续期通过 `X-New-Token` `【文档明确】`
- 限流命中统一走 429 + 业务错误码 `【文档明确】`

## 3.4 核心状态机不能擅改

### 用户状态
- 正常
- 审核中 / 已认证 / 被拒绝（认证维度）
- 注销申请中
- 已注销
- 黑名单/封禁

### 职位状态
- `draft`
- `pending`
- `rejected`
- `active`
- `paused`
- `expired`
- `closed`
- `deleted`

### 报名状态
- `applied`
- `accepted`
- `rejected`
- `cancelled`

### 企业认证状态
- `not_submitted`
- `pending`
- `approved`
- `rejected`

---

# 4. 总开发路线

## 阶段 A：项目骨架与基础设施
目标：让项目具备可开发、可运行、可测试骨架。

## 阶段 B：用户基础闭环
目标：让用户能注册、登录、维护资料和会话。

## 阶段 C：雇主基础闭环
目标：让雇主能认证、发职位、收报名。

## 阶段 D：求职者基础闭环
目标：让求职者能找职位、报名、查看结果。

## 阶段 E：互动与实时能力
目标：让私聊、通知、评论、举报跑通。

## 阶段 F：平台治理与后台运营
目标：让审核、风控、公示、配置、导出跑通。

## 阶段 G：测试、部署、上线维护
目标：让项目可验收、可上线、可持续维护。

---

# 5. 推荐开发顺序

1. 项目骨架与基础设施（P0）
2. 用户与认证（P0）
3. 简历系统（P0）
4. 企业认证（P0）
5. 职位系统（P0）
6. 报名系统（P0）
7. 通知系统（P1）
8. 聊天与 WebSocket（P1）
9. 评论 / 举报 / 黑名单（P1）
10. 收藏 / 浏览历史 / 帮助中心 / 人才库（P2）
11. 后台管理（P0/P1）
12. 安全 / 部署 / 测试 / 监控（P0）

原因：

- 用户、简历、企业认证、职位、报名，是业务主链路
- 审核后台会反向阻塞企业认证和职位上线，不能拖太后
- 聊天与帮助中心不是第一阻塞项，但属于高优先增强项

---

# 6. 总控看板

| 模块 | 状态 | 优先级 | 当前说明 |
|---|---|---:|---|
| 项目骨架与基础设施 | ready | P0 | 可以立即开工 |
| 用户与认证 | blocked | P0 | 依赖项目骨架 |
| 简历系统 | blocked | P0 | 依赖用户系统 |
| 企业认证 | blocked | P0 | 依赖用户系统、文件系统 |
| 职位系统 | blocked | P0 | 依赖企业认证、文件系统 |
| 报名系统 | blocked | P0 | 依赖职位系统、简历系统 |
| 聊天与 WebSocket | blocked | P1 | 依赖用户系统、会话模型 |
| 评论/举报/黑名单 | blocked | P1 | 依赖职位系统、用户系统 |
| 收藏/通知/帮助中心/人才库 | blocked | P2 | 依赖用户系统与核心业务 |
| 后台管理 | blocked | P0 | 依赖项目骨架、权限体系 |
| 安全/部署/测试/监控 | blocked | P0 | 需基础模块逐步落地 |

---

# 7. 第一批必须立即开工的任务

> 这些任务是当前最应该做的，不是泛方向，而是真正的第一批施工项。

## 7.1 Ready 任务（可直接开工）

### TASK-ID: BOOT-APP-001
- 状态: ready
- 优先级: P0
- 目标: 初始化 Laravel 11 工程骨架
- 输入: 技术架构文档、部署文档
- 输出:
  - Laravel 项目初始化完成
  - 基础目录结构建立
- 产出物:
  - `app/`
  - `bootstrap/`
  - `config/`
  - `routes/`
  - `storage/`
  - `tests/`
- 验收标准:
  - 项目可通过 artisan 基础命令启动
  - 测试目录存在
  - API 路由文件存在

### TASK-ID: BOOT-APP-002
- 状态: ready
- 优先级: P0
- 目标: 建立后端目录规范
- 输入: Laravel 11 架构约定
- 输出:
  - 控制器、请求、资源、服务、策略、异常目录建立
- 产出物:
  - `app/Http/Controllers/Api`
  - `app/Http/Controllers/Admin`
  - `app/Http/Requests`
  - `app/Http/Resources`
  - `app/Services`
  - `app/Policies`
  - `app/Exceptions`
- 验收标准:
  - 目录结构可用于后续业务落地

### TASK-ID: BOOT-APP-003
- 状态: ready
- 优先级: P0
- 目标: 配置 `.env.example` 基础项
- 输入: 部署说明、技术架构
- 输出:
  - MySQL / Redis / Queue / Cache / Mail / App Key 占位项完整
- 产出物:
  - `.env.example`
- 验收标准:
  - 新环境复制后可完成基础配置

### TASK-ID: BOOT-RESP-001
- 状态: ready
- 优先级: P0
- 目标: 建立统一 JSON 响应结构
- 输入: API 规范第六章
- 输出:
  - success / error 统一响应工具
- 产出物:
  - 响应助手类
  - 基础响应 trait 或 helper
- 验收标准:
  - 返回结构统一为 `code/msg/data`

### TASK-ID: BOOT-EXC-001
- 状态: ready
- 优先级: P0
- 目标: 建立统一异常映射机制
- 输入: 错误码规范
- 输出:
  - 参数错误/认证错误/验证错误/限流错误统一输出
- 产出物:
  - 异常类
  - Handler 映射逻辑
- 验收标准:
  - 非成功请求不会乱返回结构

### TASK-ID: BOOT-CODE-001
- 状态: ready
- 优先级: P0
- 目标: 建立错误码常量/枚举
- 输入: 第六章错误码规范
- 输出:
  - 系统/参数/认证/业务/验证/限流/内容安全/管理端错误码文件
- 产出物:
  - 错误码类或枚举
- 验收标准:
  - 后续接口不再手写魔法数字

### TASK-ID: BOOT-TEST-001
- 状态: ready
- 优先级: P0
- 目标: 配置测试基础骨架
- 输入: 工程结构要求
- 输出:
  - Feature / Unit 测试目录可用
- 产出物:
  - `tests/Feature/Api`
  - `tests/Feature/Admin`
  - `tests/Unit`
- 验收标准:
  - 可执行基础测试命令

### TASK-ID: BOOT-FILE-001
- 状态: ready
- 优先级: P0
- 目标: 创建 `files` 表 migration
- 输入: 数据库设计第 3 章
- 输出:
  - 文件元数据表结构
- 产出物:
  - migration 文件
- 验收标准:
  - 包含 file_id、disk、path、mime_type、size、original_name、uploader_id、type 等字段

### TASK-ID: BOOT-FILE-002
- 状态: ready
- 优先级: P0
- 目标: 创建 `File` 模型与 File Resource
- 输入: 文件资源输出规范
- 输出:
  - 文件模型
  - 文件资源对象输出
- 产出物:
  - `File` 模型
  - `FileResource`
- 验收标准:
  - 支持 public/private 文件统一输出

### TASK-ID: BOOT-FILE-003
- 状态: ready
- 优先级: P0
- 目标: 实现 `POST /api/upload/image`
- 输入: 第六章上传接口规范
- 输出:
  - 图片上传接口
  - 元数据落库
- 产出物:
  - 上传控制器
  - 上传请求校验
  - 上传服务
- 验收标准:
  - 支持 type 校验、大小校验、格式校验、落库返回 File Resource

### TASK-ID: BOOT-CONFIG-001
- 状态: ready
- 优先级: P0
- 目标: 创建 `system_config` 表 migration
- 输入: 数据库设计第 3 章
- 输出:
  - 系统配置表结构
- 产出物:
  - migration 文件
- 验收标准:
  - 包含 config_key、config_value、value_type、options 等字段

### TASK-ID: BOOT-CONFIG-002
- 状态: ready
- 优先级: P0
- 目标: 创建 `SystemConfig` 模型和配置读取服务
- 输入: 配置设计文档
- 输出:
  - 配置模型
  - 配置读取服务
  - 缓存读取逻辑
- 产出物:
  - `SystemConfig` 模型
  - 配置服务类
- 验收标准:
  - 支持按 key 读取配置并带缓存

### TASK-ID: BOOT-CONFIG-003
- 状态: ready
- 优先级: P0
- 目标: 实现 `GET /api/system/config`
- 输入: 第六章公开配置接口规范
- 输出:
  - 公开配置接口
- 产出物:
  - 控制器
  - Resource
- 验收标准:
  - 只输出允许公开的配置项

---

# 8. 全量开发清单

下面开始给出全量任务。状态默认以当前项目现状为基础：

- 没骨架的，一律 `blocked` 或 `todo`
- 可立即做的基础项，已在上面标为 `ready`

---

# 8.1 项目骨架与基础设施

## 工程初始化
- `BOOT-APP-001` 初始化 Laravel 11 工程骨架
- `BOOT-APP-002` 建立目录规范
- `BOOT-APP-003` 配置 `.env.example`
- `BOOT-ROUTE-001` 配置 API 路由分组：用户端/管理端
- `BOOT-LOG-001` 配置日志通道：api/admin/security/queue
- `BOOT-TEST-001` 配置测试基础骨架

## 统一响应与异常
- `BOOT-RESP-001` 建立统一成功响应工具
- `BOOT-RESP-002` 建立统一失败响应工具
- `BOOT-EXC-001` 建立业务异常基类
- `BOOT-EXC-002` 建立全局异常映射
- `BOOT-CODE-001` 建立错误码常量/枚举

## JWT 与鉴权
- `BOOT-AUTH-001` 选定 JWT 实现方案
- `BOOT-AUTH-002` 设计用户 token payload
- `BOOT-AUTH-003` 设计管理员 token payload
- `BOOT-AUTH-004` 增加 `device_session_id` claim
- `BOOT-AUTH-005` 实现用户鉴权中间件
- `BOOT-AUTH-006` 实现管理员鉴权中间件
- `BOOT-AUTH-007` 实现 token 黑名单机制
- `BOOT-AUTH-008` 实现 token 自动续期中间件
- `BOOT-AUTH-009` 实现 `X-New-Token` 输出逻辑
- `BOOT-AUTH-010` 当前用户解析器
- `BOOT-AUTH-011` 当前管理员解析器

## 文件系统
- `BOOT-FILE-001` 创建 `files` 表 migration
- `BOOT-FILE-002` 创建 `File` 模型
- `BOOT-FILE-003` 封装 File Resource
- `BOOT-FILE-004` 封装 public URL 生成逻辑
- `BOOT-FILE-005` 封装 private temporary URL 逻辑
- `BOOT-FILE-006` 实现 `POST /api/upload/image`
- `BOOT-FILE-007` 上传图片格式校验
- `BOOT-FILE-008` 上传图片大小校验
- `BOOT-FILE-009` 上传用途 type 校验
- `BOOT-FILE-010` 上传元数据落库
- `BOOT-FILE-011` 上传接口测试

## 系统配置
- `BOOT-CONFIG-001` 创建 `system_config` 表 migration
- `BOOT-CONFIG-002` 创建 `SystemConfig` 模型
- `BOOT-CONFIG-003` 封装配置读取服务
- `BOOT-CONFIG-004` 封装配置缓存服务
- `BOOT-CONFIG-005` 编写基础配置 seed
- `BOOT-CONFIG-006` 实现 `GET /api/system/config`
- `BOOT-CONFIG-007` 配置接口测试

---

# 8.2 用户与认证

## 数据层
- `AUTH-DB-001` 创建 `users` 表 migration
- `AUTH-DB-002` 创建 `login_sessions` 表 migration
- `AUTH-DB-003` 创建 `password_reset_requests` 表 migration
- `AUTH-MODEL-001` 创建 `User` 模型
- `AUTH-MODEL-002` 创建 `LoginSession` 模型
- `AUTH-MODEL-003` 创建 `PasswordResetRequest` 模型
- `AUTH-MODEL-004` 为 User 配置 casts
- `AUTH-MODEL-005` 为 User 配置 hidden
- `AUTH-MODEL-006` 为 User 增加状态辅助方法
- `AUTH-MODEL-007` 配置 User 关联：resume/company/loginSessions

## 图形验证码
- `AUTH-CAPTCHA-001` 实现 `GET /api/user/captcha`
- `AUTH-CAPTCHA-002` 生成随机验证码字符串
- `AUTH-CAPTCHA-003` 生成验证码图片
- `AUTH-CAPTCHA-004` 验证码写入 Redis
- `AUTH-CAPTCHA-005` 验证码一次性消费逻辑
- `AUTH-CAPTCHA-006` 验证码过期处理
- `AUTH-CAPTCHA-007` 验证码测试

## 注册
- `AUTH-API-REG-001` 实现 `POST /api/user/register`
- `AUTH-REQ-REG-001` 编写 Register FormRequest
- `AUTH-VAL-REG-001` 校验手机号格式
- `AUTH-VAL-REG-002` 校验手机号唯一
- `AUTH-VAL-REG-003` 校验验证码
- `AUTH-VAL-REG-004` 校验密码规则
- `AUTH-VAL-REG-005` 校验密码确认一致
- `AUTH-SVC-REG-001` 写入 users 表
- `AUTH-SVC-REG-002` 注册成功后自动登录
- `AUTH-SVC-REG-003` 注册成功写入 login_sessions
- `AUTH-TEST-REG-001` 注册成功测试
- `AUTH-TEST-REG-002` 重复手机号测试
- `AUTH-TEST-REG-003` 验证码错误测试
- `AUTH-TEST-REG-004` 密码确认不一致测试

## 登录
- `AUTH-API-LOGIN-001` 实现 `POST /api/user/login`
- `AUTH-REQ-LOGIN-001` 编写 Login FormRequest
- `AUTH-VAL-LOGIN-001` 校验验证码
- `AUTH-VAL-LOGIN-002` 校验手机号存在
- `AUTH-VAL-LOGIN-003` 校验密码正确性
- `AUTH-SEC-LOGIN-001` 登录失败累计
- `AUTH-SEC-LOGIN-002` 账号锁定逻辑
- `AUTH-SEC-LOGIN-003` 锁定剩余时间返回
- `AUTH-SVC-LOGIN-001` 登录成功重置失败计数
- `AUTH-SVC-LOGIN-002` 生成用户 token
- `AUTH-SVC-LOGIN-003` 写入/更新 login_sessions
- `AUTH-SVC-LOGIN-004` 实现 3 设备上限控制
- `AUTH-SVC-LOGIN-005` 顶替最早不活跃设备
- `AUTH-TEST-LOGIN-001` 登录成功测试
- `AUTH-TEST-LOGIN-002` 密码错误测试
- `AUTH-TEST-LOGIN-003` 账号锁定测试
- `AUTH-TEST-LOGIN-004` 封禁/注销测试

## 登出与会话
- `AUTH-API-LOGOUT-001` 实现 `POST /api/user/logout`
- `AUTH-SVC-LOGOUT-001` token 加入黑名单
- `AUTH-SVC-LOGOUT-002` 当前 `device_session_id` 标记失效
- `AUTH-SVC-LOGOUT-003` 当前 ws_token 失效
- `AUTH-SVC-LOGOUT-004` 预留 WebSocket 踢出联动
- `AUTH-TEST-LOGOUT-001` 退出登录测试

## 用户资料
- `AUTH-API-INFO-001` 实现 `GET /api/user/info`
- `AUTH-API-INFO-002` 实现 `PUT /api/user/update`
- `AUTH-REQ-INFO-001` 编写 UpdateUser FormRequest
- `AUTH-VAL-INFO-001` 校验头像 file_id 合法性
- `AUTH-VAL-INFO-002` 校验邮箱格式
- `AUTH-TEST-INFO-001` 用户资料测试

## 密码管理与找回
- `AUTH-API-PWD-001` 实现 `PUT /api/user/change-password`
- `AUTH-VAL-PWD-001` 校验旧密码
- `AUTH-VAL-PWD-002` 校验新旧密码不同
- `AUTH-SVC-PWD-001` 改密后全部会话失效
- `AUTH-API-FORGOT-001` 实现 `POST /api/user/forgot-password`
- `AUTH-SVC-FORGOT-001` 写入客服找回申请记录
- `AUTH-API-FORGOT-002` 实现 `POST /api/user/forgot-password-email`
- `AUTH-SVC-FORGOT-002` 发送邮件任务
- `AUTH-API-FORGOT-003` 实现 `POST /api/user/reset-password-email`
- `AUTH-VAL-FORGOT-001` 校验 reset_token 有效期
- `AUTH-SVC-FORGOT-003` 重置后会话失效
- `AUTH-TEST-PWD-001` 密码管理测试

## 修改手机号
- `AUTH-API-PHONE-001` 实现 `PUT /api/user/change-phone` step1
- `AUTH-SVC-PHONE-001` 生成 verify_token
- `AUTH-SVC-PHONE-002` verify_token 写入 Redis
- `AUTH-API-PHONE-002` 实现 `PUT /api/user/change-phone` step2
- `AUTH-VAL-PHONE-001` 校验 verify_token
- `AUTH-VAL-PHONE-002` 校验新手机号未注册
- `AUTH-SVC-PHONE-003` 修改后会话失效
- `AUTH-TEST-PHONE-001` 修改手机号测试

## 设备管理
- `AUTH-API-DEVICE-001` 实现 `GET /api/user/devices`
- `AUTH-SVC-DEVICE-001` 当前设备识别逻辑
- `AUTH-API-DEVICE-002` 实现 `DELETE /api/user/device/{device_id}`
- `AUTH-API-DEVICE-003` 实现 `POST /api/user/logout-others`
- `AUTH-SVC-DEVICE-002` 联动 token/ws_token/device_session_id 失效
- `AUTH-TEST-DEVICE-001` 设备管理测试

## 账号注销
- `AUTH-API-DEL-001` 实现 `POST /api/user/delete`
- `AUTH-VAL-DEL-001` 校验密码确认
- `AUTH-SVC-DEL-001` 写入注销申请时间
- `AUTH-SVC-DEL-002` 计算生效时间
- `AUTH-API-DEL-002` 实现 `POST /api/user/cancel-delete`
- `AUTH-CMD-DEL-001` 编写冷静期到期处理命令
- `AUTH-TEST-DEL-001` 注销申请测试
- `AUTH-TEST-DEL-002` 取消注销测试

## 未读数
- `AUTH-API-UNREAD-001` 实现 `GET /api/user/unread-count`
- `AUTH-SVC-UNREAD-001` 聚合私聊未读
- `AUTH-SVC-UNREAD-002` 聚合通知未读
- `AUTH-SVC-UNREAD-003` 聚合报名变化未读
- `AUTH-TEST-UNREAD-001` 未读数测试

---

# 8.3 简历系统

## 数据层
- `RESUME-DB-001` 创建 `resumes` 表 migration
- `RESUME-DESIGN-001` 决定工作经历是否拆表，若不拆，定义 JSON 结构
- `RESUME-MODEL-001` 创建 `Resume` 模型
- `RESUME-MODEL-002` 配置 casts
- `RESUME-MODEL-003` 配置与 User 关联

## 接口层
- `RESUME-API-001` 实现 `POST /api/user/complete-profile`
- `RESUME-API-002` 实现 `GET /api/resume/detail`
- `RESUME-API-003` 实现 `POST /api/resume/save`
- `RESUME-API-004` 实现 `GET /api/resume/view`
- `RESUME-REQ-001` 编写 SaveResume FormRequest
- `RESUME-SVC-001` 实现简历公开/隐藏逻辑
- `RESUME-SVC-002` 实现简历完成度校验
- `RESUME-POLICY-001` 雇主查看简历权限判断
- `RESUME-TEST-001` 简历测试

---

# 8.4 企业认证

## 数据层
- `COMPANY-DB-001` 创建 `companies` 表 migration
- `COMPANY-DB-002` 创建 `company_audit_histories` 表 migration
- `COMPANY-MODEL-001` 创建 `Company` 模型
- `COMPANY-MODEL-002` 创建 `CompanyAuditHistory` 模型
- `COMPANY-MODEL-003` 配置关联与状态辅助方法

## 提交认证
- `COMPANY-API-SUBMIT-001` 实现 `POST /api/company/submit`
- `COMPANY-REQ-SUBMIT-001` 编写 SubmitCompany FormRequest
- `COMPANY-VAL-SUBMIT-001` 校验企业/劳务/个人雇主差异字段
- `COMPANY-VAL-SUBMIT-002` 校验 cert file_id 归属
- `COMPANY-SEC-SUBMIT-001` 敏感字段加密存储
- `COMPANY-SVC-SUBMIT-001` 写入 companies 当前版本
- `COMPANY-SVC-SUBMIT-002` 写入 company_audit_histories 快照
- `COMPANY-SVC-SUBMIT-003` 更新 users.is_certified=1
- `COMPANY-TEST-SUBMIT-001` 企业认证提交测试

## 认证详情
- `COMPANY-API-DETAIL-001` 实现 `GET /api/company/detail`
- `COMPANY-SVC-DETAIL-001` 输出 private 文件 temporary_url
- `COMPANY-SVC-DETAIL-002` 身份证号脱敏
- `COMPANY-TEST-DETAIL-001` 详情测试

## 重新认证
- `COMPANY-SVC-REAPPLY-001` 实现审核拒绝后的重新提交流程
- `COMPANY-SVC-REAPPLY-002` audit_version +1
- `COMPANY-SVC-REAPPLY-003` reapply_count +1
- `COMPANY-SVC-REAPPLY-004` 历史快照写入新版本
- `COMPANY-TEST-REAPPLY-001` 重新认证测试

---

# 8.5 职位系统

## 数据层
- `JOB-DB-001` 创建 `job_categories` 表 migration
- `JOB-DB-002` 创建 `jobs` 表 migration
- `JOB-DB-003` 创建 `job_levels` 表 migration
- `JOB-DB-004` 创建 `job_views` 表 migration
- `JOB-DB-005` 创建 `job_top_applications` 表 migration
- `JOB-MODEL-001` 创建相关模型与关联
- `JOB-MODEL-002` 建立职位状态枚举/常量

## 工种
- `JOB-CATEGORY-001` 实现 `GET /api/category/list`
- `JOB-CATEGORY-002` 工种缓存
- `JOB-CATEGORY-003` 工种测试

## 职位编号
- `JOB-NO-001` 实现 JOB 编号生成服务
- `JOB-NO-002` 实现按日重置序号
- `JOB-NO-003` 实现并发安全控制
- `JOB-NO-004` 编号测试

## 列表 / 搜索 / 详情
- `JOB-API-LIST-001` 实现 `GET /api/job/list`
- `JOB-API-SEARCH-001` 实现 `GET /api/job/search`
- `JOB-API-HOT-001` 实现 `GET /api/job/hot-words`
- `JOB-API-DETAIL-001` 实现 `GET /api/job/detail`
- `JOB-SVC-LIST-001` 实现置顶优先排序
- `JOB-SVC-LIST-002` 实现收藏状态注入
- `JOB-SVC-LIST-003` 实现报名状态注入
- `JOB-SVC-VIEW-001` 实现浏览记录异步写入
- `JOB-TEST-LIST-001` 列表/搜索/详情测试

## 草稿
- `JOB-API-DRAFT-001` 实现 `POST /api/job/save-draft`
- `JOB-API-DRAFT-002` 实现 `GET /api/job/draft-list`
- `JOB-API-DRAFT-003` 实现 `GET /api/job/draft-detail`
- `JOB-SVC-DRAFT-001` 草稿更新逻辑
- `JOB-SVC-DRAFT-002` 草稿完成度计算
- `JOB-TEST-DRAFT-001` 草稿测试

## 发布
- `JOB-API-PUBLISH-001` 实现 `POST /api/job/publish`
- `JOB-REQ-PUBLISH-001` 编写 PublishJob FormRequest
- `JOB-VAL-PUBLISH-001` 校验 image_file_ids
- `JOB-VAL-PUBLISH-002` 校验 categories/levels 结构
- `JOB-VAL-PUBLISH-003` 校验企业认证状态
- `JOB-SVC-PUBLISH-001` 写入 jobs + job_levels 事务
- `JOB-SVC-PUBLISH-002` 审核判定服务
- `JOB-SVC-PUBLISH-003` 新认证企业前 N 条审核逻辑
- `JOB-SVC-PUBLISH-004` 发布通知
- `JOB-TEST-PUBLISH-001` 发布测试

## 编辑 / 删除 / 状态切换
- `JOB-API-UPDATE-001` 实现 `PUT /api/job/update`
- `JOB-API-DELETE-001` 实现 `DELETE /api/job/delete`
- `JOB-API-STATUS-001` 实现 `PUT /api/job/toggle-status`
- `JOB-API-COPY-001` 实现 `POST /api/job/copy`
- `JOB-API-FILLED-001` 实现 `PUT /api/job/mark-filled`
- `JOB-SVC-STATUS-001` close_reason 规则
- `JOB-TEST-STATUS-001` 状态切换测试

## 刷新与置顶
- `JOB-API-REFRESH-001` 实现 `PUT /api/job/refresh`
- `JOB-SVC-REFRESH-001` 刷新限流
- `JOB-API-TOP-001` 实现 `POST /api/job/apply-top`
- `JOB-API-TOP-002` 实现 `GET /api/job/top-application-status`
- `JOB-CMD-TOP-001` 置顶到期清理任务
- `JOB-TEST-TOP-001` 刷新与置顶测试

## 统计与推荐
- `JOB-API-STAT-001` 实现 `GET /api/job/statistics`
- `JOB-API-STAT-002` 实现 `GET /api/employer/statistics`
- `JOB-API-RELATED-001` 实现 `GET /api/job/related`
- `JOB-TEST-STAT-001` 统计测试

---

# 8.6 报名系统

## 数据层
- `APPLY-DB-001` 创建 `applications` 表 migration
- `APPLY-MODEL-001` 创建 `Application` 模型
- `APPLY-MODEL-002` 配置状态与关联

## 报名
- `APPLY-API-001` 实现 `POST /api/application/apply`
- `APPLY-REQ-001` 编写 ApplyJob FormRequest
- `APPLY-SVC-001` 通过 `job_id + category_id + level` 反查 `job_level_id`
- `APPLY-VAL-001` 校验职位状态
- `APPLY-VAL-002` 校验不是自己职位
- `APPLY-VAL-003` 校验简历已完善
- `APPLY-VAL-004` 校验重报限制
- `APPLY-SVC-002` 实现 `client_apply_id` 幂等
- `APPLY-SVC-003` 写入报名记录
- `APPLY-SVC-004` applied_count +1
- `APPLY-SVC-005` 满员后关闭职位
- `APPLY-SVC-006` 报名通知
- `APPLY-TEST-001` 报名测试

## 取消报名
- `APPLY-API-002` 实现 `PUT /api/application/cancel`
- `APPLY-VAL-005` 校验取消合法性
- `APPLY-SVC-007` applied_count -1
- `APPLY-SVC-008` 满员关闭职位自动恢复
- `APPLY-SVC-009` 取消报名通知
- `APPLY-TEST-002` 取消报名测试

## 列表与审核
- `APPLY-API-003` 实现 `GET /api/application/my-list`
- `APPLY-API-004` 实现 `GET /api/application/manage-list`
- `APPLY-API-005` 实现 `PUT /api/application/review`
- `APPLY-SVC-010` employer_message
- `APPLY-SVC-011` reject_reason / can_reapply_time
- `APPLY-API-006` 实现 `PUT /api/application/batch-review`
- `APPLY-SVC-012` request_id / 批次号
- `APPLY-TEST-003` 审核测试

---

# 8.7 聊天与 WebSocket

## 数据层
- `CHAT-DB-001` 创建 `conversations` 表 migration
- `CHAT-DB-002` 创建 `messages_private` 表 migration
- `CHAT-DB-003` 创建 `messages_public` 表 migration
- `CHAT-MODEL-001` 创建模型与关联

## 会话与私聊接口
- `CHAT-CONV-001` conversation_key 生成规则
- `CHAT-CONV-002` 会话创建/复用服务
- `CHAT-API-001` 实现 `GET /api/message/private-list`
- `CHAT-API-002` 实现 `GET /api/message/private-detail`
- `CHAT-API-003` 实现 `POST /api/message/send-private`
- `CHAT-SEC-001` 违禁词过滤
- `CHAT-SEC-002` 联系方式过滤
- `CHAT-SVC-001` client_msg_id 幂等
- `CHAT-SVC-002` 未读数更新
- `CHAT-API-004` 实现 `PUT /api/message/read`
- `CHAT-API-005` 实现 `PUT /api/message/read-all`
- `CHAT-API-006` 实现 `POST /api/message/private/recall`
- `CHAT-API-007` 实现 `DELETE /api/message/private/delete`
- `CHAT-TEST-001` 私聊接口测试

## 公共聊天室
- `CHAT-API-008` 实现 `GET /api/message/public-list`
- `CHAT-API-009` 实现 `POST /api/message/send-public`
- `CHAT-SVC-003` 公共聊天限流
- `CHAT-API-010` 实现 `GET /api/chat/online-count`
- `CHAT-TEST-002` 公共聊天室测试

## WebSocket 接入
- `CHAT-WS-001` 实现 `GET /api/ws/config`
- `CHAT-WS-002` 生成 ws_token
- `CHAT-WS-003` 绑定 device_session_id
- `CHAT-WS-004` 编写 Workerman/GatewayWorker 启动脚本
- `CHAT-WS-005` 握手鉴权
- `CHAT-WS-006` ping/pong
- `CHAT-WS-007` message.send 事件
- `CHAT-WS-008` read 事件
- `CHAT-WS-009` recall/delete 事件
- `CHAT-WS-010` kickout 事件
- `CHAT-WS-011` 联调清单

---

# 8.8 评论、举报、黑名单、违禁词

## 评论
- `COMMENT-DB-001` 创建 `comments` 表 migration
- `COMMENT-DB-002` 创建 `comment_likes` 表 migration
- `COMMENT-API-001` 实现 `GET /api/job/comments`
- `COMMENT-API-002` 实现 `POST /api/job/comment`
- `COMMENT-API-003` 实现 `POST /api/job/comment/reply`
- `COMMENT-API-004` 实现 `POST /api/job/comment/like`
- `COMMENT-API-005` 实现 `DELETE /api/job/comment/delete`
- `COMMENT-SEC-001` 评论内容安全过滤
- `COMMENT-TEST-001` 评论测试

## 举报
- `REPORT-DB-001` 创建 `reports` 表 migration
- `REPORT-MODEL-001` 创建 `Report` 模型
- `REPORT-API-001` 实现 `POST /api/report/submit`
- `REPORT-VAL-001` evidence_file_ids 校验
- `REPORT-TEST-001` 举报提交测试

## 黑名单与违禁词
- `BLACKLIST-DB-001` 创建 `blacklist` 表 migration
- `BLACKLIST-MODEL-001` 创建 `Blacklist` 模型
- `BLACKLIST-API-001` 实现 `GET /api/blacklist/public`
- `BLACKLIST-API-002` 实现 `GET /api/blacklist/check`
- `BANNED-DB-001` 创建 `banned_words` 表 migration
- `BANNED-MODEL-001` 创建 `BannedWord` 模型
- `BANNED-SVC-001` 违禁词检测服务
- `BANNED-SVC-002` filter/warn/block 动作
- `BANNED-SVC-003` 缓存与热更新
- `BLACKLIST-TEST-001` 黑名单测试
- `BANNED-TEST-001` 违禁词测试

---

# 8.9 收藏、通知、浏览历史、帮助中心、人才库

## 收藏
- `FAV-DB-001` 创建 `favorites` 表 migration
- `FAV-API-001` 实现 `POST /api/favorite/add`
- `FAV-API-002` 实现 `DELETE /api/favorite/remove`
- `FAV-API-003` 实现 `GET /api/favorite/list`
- `FAV-TEST-001` 收藏测试

## 通知
- `NOTIFY-DB-001` 创建 `notifications` 表 migration
- `NOTIFY-MODEL-001` 创建 `Notification` 模型
- `NOTIFY-API-001` 实现 `GET /api/notifications`
- `NOTIFY-API-002` 实现 `PUT /api/notifications/read`
- `NOTIFY-API-003` 实现 `DELETE /api/notifications/{id}`
- `NOTIFY-API-004` 实现 `DELETE /api/notifications`
- `NOTIFY-SVC-001` 未读数缓存同步
- `NOTIFY-TEST-001` 通知测试

## 浏览历史
- `HISTORY-API-001` 实现 `GET /api/user/browse-history`
- `HISTORY-API-002` 实现 `DELETE /api/user/browse-history/clear`
- `HISTORY-SVC-001` 浏览历史聚合逻辑
- `HISTORY-TEST-001` 浏览历史测试

## 帮助中心
- `HELP-DB-001` 创建 `help_categories` 表 migration `【文档明确】`
- `HELP-DB-002` 创建 `help_articles` 表 migration `【文档明确】`
- `HELP-DB-003` 创建 `help_article_feedbacks` 表 migration `【文档明确】`
- `HELP-DB-004` 创建 `help_search_synonyms` 表 migration `【文档明确】`
- `HELP-DB-005` 创建 `help_article_relations` 表 migration `【文档明确】`
- `HELP-DB-006` 创建 `help_search_logs` 表 migration `【文档明确】`
- `HELP-MODEL-001` 创建 `HelpCategory` 模型 `【文档明确】`
- `HELP-MODEL-002` 创建 `HelpArticle` 模型 `【文档明确】`
- `HELP-MODEL-003` 创建 `HelpArticleFeedback` 模型 `【文档明确】`
- `HELP-MODEL-004` 创建 `HelpSearchSynonym` 模型 `【文档明确】`
- `HELP-MODEL-005` 创建 `HelpArticleRelation` 模型 `【文档明确】`
- `HELP-MODEL-006` 创建 `HelpSearchLog` 模型 `【文档明确】`
- `HELP-API-001` 实现 `GET /api/help/home`
- `HELP-API-002` 实现 `GET /api/help/categories`
- `HELP-API-003` 实现 `GET /api/help/articles`
- `HELP-API-004` 实现 `GET /api/help/articles/{id}`
- `HELP-API-005` 实现 `GET /api/help/search`
- `HELP-API-006` 实现 `GET /api/help/hot`
- `HELP-API-007` 实现 `POST /api/help/articles/{id}/feedback`
- `HELP-SVC-001` `help_search_logs` 写入
- `HELP-TEST-001` 帮助中心测试

## 人才库
- `TALENT-DB-001` 创建 `talent_pool` 表 migration
- `TALENT-MODEL-001` 创建 `TalentPool` 模型
- `TALENT-API-001` 实现 `GET /api/employer/talent-pool`
- `TALENT-API-002` 实现 `POST /api/employer/talent-pool/add`
- `TALENT-API-003` 实现 `DELETE /api/employer/talent-pool/remove`
- `TALENT-API-004` 实现 `GET /api/employer/talent-pool/resume`
- `TALENT-SVC-001` 同工种去重逻辑
- `TALENT-TEST-001` 人才库测试

---

# 8.10 后台管理

## 管理员认证
- `ADMIN-AUTH-001` 创建 `admin_users` 表 migration
- `ADMIN-AUTH-002` 创建 `admin_login_logs` 表 migration
- `ADMIN-AUTH-003` 创建 `admin_user_permissions` 表 migration
- `ADMIN-AUTH-004` 实现 `POST /api/admin/login`
- `ADMIN-AUTH-005` 实现 `POST /api/admin/logout`
- `ADMIN-AUTH-006` 管理员锁定逻辑
- `ADMIN-AUTH-007` 管理员 token 与权限装载
- `ADMIN-AUTH-TEST-001` 管理员登录测试

## 概览与用户管理
- `ADMIN-OVERVIEW-001` 实现 `GET /api/admin/stat/overview`
- `ADMIN-USER-001` 实现 `GET /api/admin/user/list`
- `ADMIN-USER-002` 实现 `GET /api/admin/user/detail`
- `ADMIN-USER-003` 实现 `POST /api/admin/user/reset-password`
- `ADMIN-USER-004` 实现 `DELETE /api/admin/user/delete`
- `ADMIN-USER-TEST-001` 用户管理测试

## 企业认证审核
- `ADMIN-COMPANY-001` 实现 `GET /api/admin/company-certifications`
- `ADMIN-COMPANY-002` 实现 `GET /api/admin/company-certifications/{id}/detail`
- `ADMIN-COMPANY-003` 实现 `POST /api/admin/company-certifications/{id}/audit`
- `ADMIN-COMPANY-004` 历史材料组装
- `ADMIN-COMPANY-TEST-001` 企业审核测试

## 职位与工种管理
- `ADMIN-JOB-001` 实现 `GET /api/admin/job/list`
- `ADMIN-JOB-002` 实现 `GET /api/admin/job/detail`
- `ADMIN-JOB-003` 实现 `PUT /api/admin/job/audit`
- `ADMIN-JOB-004` 实现 `DELETE /api/admin/job/delete`
- `ADMIN-JOB-005` 实现 `POST /api/admin/job/toggle-top`
- `ADMIN-CAT-001` 实现工种管理接口
- `ADMIN-CAT-002` 实现工种申请处理接口
- `ADMIN-JOB-TEST-001` 后台职位与工种测试

## 举报 / 黑名单 / 公告 / 广播
- `ADMIN-REPORT-001` 举报列表接口
- `ADMIN-REPORT-002` 举报详情接口
- `ADMIN-REPORT-003` 举报处理接口
- `ADMIN-BLACKLIST-001` 黑名单列表接口
- `ADMIN-BLACKLIST-002` 黑名单详情接口
- `ADMIN-BLACKLIST-003` 黑名单新增接口
- `ADMIN-BLACKLIST-004` 黑名单解除接口
- `ADMIN-BLACKLIST-005` 黑名单公示接口
- `ADMIN-BLACKLIST-006` 黑名单编辑接口
- `ADMIN-ANN-DB-001` 创建 `announcements` 表 migration `【文档明确】`
- `ADMIN-ANN-MODEL-001` 创建 `Announcement` 模型 `【文档明确】`
- `ADMIN-ANN-001` 公告列表接口
- `ADMIN-ANN-002` 公告详情接口
- `ADMIN-ANN-003` 公告新增接口
- `ADMIN-ANN-004` 公告编辑接口
- `ADMIN-ANN-005` 公告删除接口
- `ADMIN-ANN-006` 公告启停接口
- `ADMIN-BROADCAST-DB-001` 创建 `system_broadcasts` 表 migration `【文档明确】`
- `ADMIN-BROADCAST-MODEL-001` 创建 `SystemBroadcast` 模型 `【文档明确】`
- `ADMIN-BROADCAST-001` 广播列表接口
- `ADMIN-BROADCAST-002` 广播详情接口
- `ADMIN-BROADCAST-003` 广播发送接口
- `ADMIN-BROADCAST-004` 广播删除接口
- `ADMIN-GOV-TEST-001` 治理后台测试

## 工种申请 / 违禁词 / 子管理员 / 配置 / 日志 / 导出
- `ADMIN-CAT-APP-DB-001` 创建 `category_applications` 表 migration `【文档明确】`
- `ADMIN-CAT-APP-MODEL-001` 创建 `CategoryApplication` 模型 `【文档明确】`
- `ADMIN-BANNED-DB-001` 创建 `banned_words` 表 migration `【文档明确】`
- `ADMIN-BANNED-MODEL-001` 创建 `BannedWord` 模型 `【文档明确】`
- `ADMIN-BANNED-001` 违禁词管理接口
- `ADMIN-SUBADMIN-001` 子管理员管理接口
- `ADMIN-CONFIG-LOG-DB-001` 创建 `config_change_logs` 表 migration `【文档明确】`
- `ADMIN-CONFIG-LOG-MODEL-001` 创建 `ConfigChangeLog` 模型 `【文档明确】`
- `ADMIN-CONFIG-001` 系统配置列表接口
- `ADMIN-CONFIG-002` 系统配置更新接口
- `ADMIN-LOG-DB-001` 创建 `operation_logs` 表 migration `【文档明确】`
- `ADMIN-LOG-MODEL-001` 创建 `OperationLog` 模型 `【文档明确】`
- `ADMIN-LOG-001` 操作日志查询接口
- `ADMIN-EXPORT-DB-001` 创建 `export_tasks` 表 migration `【文档明确】`
- `ADMIN-EXPORT-MODEL-001` 创建 `ExportTask` 模型 `【文档明确】`
- `ADMIN-EXPORT-001` 导出任务创建接口
- `ADMIN-EXPORT-002` 导出状态接口
- `ADMIN-EXPORT-003` 导出记录列表接口
- `ADMIN-MISC-TEST-001` 后台配置与导出测试

---

# 8.11 安全、部署、测试、监控

## 安全
- `SEC-001` 敏感字段加密工具
- `SEC-002` 内容安全统一服务
- `SEC-003` 接口限流策略
- `SEC-004` 高危操作审计服务
- `SEC-005` 导出审计
- `SEC-006` private 文件权限访问控制

## 定时任务
- `SCHEDULE-001` 清理过期私聊消息命令
- `SCHEDULE-002` 清理过期公共消息命令
- `SCHEDULE-003` 清理过期置顶命令
- `SCHEDULE-004` 清理过期草稿命令
- `SCHEDULE-005` 处理注销到期命令
- `SCHEDULE-006` 清理旧日志命令
- `SCHEDULE-007` 配置 Laravel schedule

## 队列
- `QUEUE-001` 配置 Redis 队列
- `QUEUE-002` 定义 notifications 队列
- `QUEUE-003` 定义 audit 队列
- `QUEUE-004` 定义 export 队列
- `QUEUE-005` 定义 chat 队列
- `QUEUE-006` 配置 queue worker

## 部署
- `DEPLOY-001` 编写 `.env.production.example`
- `DEPLOY-002` 编写部署说明
- `DEPLOY-003` 编写 Nginx 配置样例
- `DEPLOY-004` 编写 PHP-FPM 配置注意项
- `DEPLOY-005` 编写 Redis 配置注意项
- `DEPLOY-006` 编写 MySQL 初始化说明
- `DEPLOY-007` 编写 Workerman 启动说明
- `DEPLOY-008` 编写 Supervisor/Systemd 样例

## 测试
- `TEST-PLAN-001` 建立 Feature Test 清单
- `TEST-PLAN-002` 建立 Unit Test 清单
- `TEST-PLAN-003` 建立接口联调清单
- `TEST-PLAN-004` 建立后台联调清单
- `TEST-PLAN-005` 建立 WebSocket 联调清单
- `TEST-PLAN-006` 建立上线前验收清单

---

# 9. 任务模板

后续如要继续维护本文件中的某条任务，推荐使用下面这个结构补充细节。

```md
### TASK-ID: AUTH-API-REG-001
- 状态: ready
- 优先级: P0
- 负责人: ai
- 依赖:
  - AUTH-DB-001
  - AUTH-CAPTCHA-001
- 目标:
  - 实现用户注册接口 `POST /api/user/register`
- 输入:
  - 第三章数据库设计：users 表
  - 第五章流程：5.1 用户注册流程
  - 第六章接口：6.3.2 用户注册
- 输出:
  - 路由
  - Controller
  - FormRequest
  - Service/Action
- 产出物:
  - `routes/api.php`
  - `app/Http/Controllers/Api/UserAuthController.php`
  - `app/Http/Requests/RegisterRequest.php`
  - `app/Services/Auth/RegisterService.php`
- 验收标准:
  - 手机号重复返回约定错误码
  - 验证码错误返回约定错误码
  - 注册成功写入 users 表
  - 注册成功后返回 token
- 测试要求:
  - 成功注册测试
  - 重复手机号测试
  - 验证码错误测试
  - 密码确认不一致测试
- 完成回填:
  - 实际改动文件:
  - 风险点:
  - 未完成项:
  - 后续建议:
  - 偏差说明:
```

---

# 10. 当前推荐下一步动作

如果现在就要进入真正开发，我建议按下面顺序推进：

## 第一步：先只做 12 个基础 ready 任务
也就是第 7 节列出的这些基础任务。

它们做完后，项目就从：

- 只有文档

变成：

- 有 Laravel 骨架
- 有统一响应
- 有错误码
- 有文件系统
- 有配置系统
- 可以开始做用户模块

## 第二步：再推进用户与认证主链路
顺序建议：

1. `AUTH-DB-*`
2. `AUTH-CAPTCHA-*`
3. `AUTH-API-REG-*`
4. `AUTH-API-LOGIN-*`
5. `AUTH-API-LOGOUT-*`
6. `AUTH-API-INFO-*`
7. `AUTH-API-PWD-*`
8. `AUTH-API-DEVICE-*`

## 第三步：推进业务核心闭环
顺序建议：

1. 简历系统
2. 企业认证
3. 职位系统
4. 报名系统
5. 后台审核最小闭环

---

# 11. 最终标准

本文件最终要达到的效果是：

> 任意一条 `ready` 状态任务，交给 AI 后，AI 不需要再问“我该先写什么”，就能直接开始干。

如果 AI 还需要再猜这些问题：

- 改哪个文件？
- 写哪个接口？
- 需要建哪张表？
- 返回结构是什么？
- 如何验收？

那说明任务还没拆到位。

---

# 12. 一致性审查后的说明

## 12.1 本轮审查结果

目前这份总清单已经完成过一次“是否符合原技术文档、是否有超计划”审查。

结论：

- **主线符合原技术文档**
- **原文档明确存在的漏项已补齐到清单中**
- **少量工程化建议仍保留，但已标记为 `【实施建议】`，避免 AI 把它们当成原文档强制项**

## 12.2 你后续怎么看“是否超计划”

判断规则很简单：

### 属于正常范围
- 原文档里明确提到的表、接口、流程、日志、广播、帮助中心、导出、安全规则
- 为实现这些内容而必须建立的 migration、model、controller、request、service、resource、test

### 属于实施建议
- 目录结构规范
- 测试分层策略
- 某些命名方式
- 阶段优先顺序
- 某些部署辅助文件

### 属于超计划风险
- 在未完成 P0 主链路前，大量扩展体验层/优化层能力
- 把工程偏好当成功能必做项
- 擅自新增原文档里没有的业务模块
- 改写原文档的状态机、字段口径、接口契约

## 12.3 执行建议

后续所有 AI 开发动作都建议遵循：

1. 先查本任务是否属于 `【文档明确】`
2. 如果只是 `【实施建议】`，要确认是否会影响主链路优先级
3. 如果会扩大范围或拖慢主线，就暂缓，视为 `【超计划风险】`

这样这份文件才能既 **能执行**，又 **不跑偏**。
