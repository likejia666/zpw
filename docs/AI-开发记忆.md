# AI 开发记忆

> 本文件记录 ZPW 项目开发过程中的关键信息、经验总结和代码约定
> 每次开发完成后必须更新本文件
> 版本：v1.0 | 创建日期：2026-05-02

---

## 一、项目基础信息

### 1.1 项目概述

- **项目名称**：自动化装备招聘系统
- **技术栈**：Laravel 11.x / PHP 8.2 / MySQL 8.0+ / Redis 6.0+ / UniApp
- **文档版本**：v2.4
- **项目路径**：`/public/home/scnttztjzs/openclaw/workspace/ZPW`

### 1.2 文档目录结构

```
/docs/
├── 01-项目概述.md
├── 02-技术架构.md
├── 03-数据库设计.md
├── 04-页面设计与交互.md
├── 05-功能流程图.md
├── 06-API接口规范-上.md
├── 06-API接口规范-下.md
├── 07-SVG图标设计提示词.md
├── 08-安全与性能.md
├── 09-部署说明.md
├── 项目进度跟踪文档.md
├── AI-DEVELOPMENT-GUIDE.md    # AI 开发约束协议
└── AI-开发记忆.md              # 本文件
```

### 1.3 核心约束

**强制约束**：所有开发必须基于 `/docs` 目录下的技术文档，禁止跳过文档进行猜测开发。

---

## 二、开发规范

### 2.1 Laravel 开发规范

- 控制器位于 `app/Http/Controllers/Api/` 目录
- 使用 FormRequest 进行参数校验
- Service 层负责核心业务逻辑
- Policy 负责资源级权限判断
- Resource 统一响应字段结构和脱敏逻辑
- 使用路由模型绑定：`Route::apiResource('jobs', JobController::class)`

### 2.2 数据库规范

- 表名使用下划线命名法：`users`, `job_categories`, `login_sessions`
- 字段名使用下划线命名法：`is_certified`, `created_at`, `job_no`
- 外键命名：`user_id`, `category_id`, `company_id`
- 软删除使用 `deleted_at` 字段
- 唯一索引：`(user_id, favorite_type, target_id)`

### 2.3 API 响应规范

统一 JSON 格式：
```json
{
  "code": 200,
  "msg": "success",
  "data": {}
}
```

HTTP 状态码与业务 code 约定：
- HTTP 200 = 业务成功
- HTTP 400 = 参数错误（业务 code 20xxx）
- HTTP 401 = 未认证（业务 code 3xxx）
- HTTP 403 = 无权限（业务 code 3xxx）
- HTTP 404 = 资源不存在（业务 code 4xxx）
- HTTP 429 = 频率限制（业务 code 6xxx）
- HTTP 500 = 服务端异常（业务 code 1xxx）

### 2.4 文件上传规范

- 统一通过 `POST /api/upload/image` 上传
- 参数：multipart/form-data，包含 file 和 type
- type 可选值：avatar / job_image / cert / resume / help
- 返回标准 File Resource 对象
- 文件元数据存入 `files` 表

---

## 三、关键业务规则

### 3.1 用户认证

- 登录失败次数限制：`login_fail_max`（默认 5 次）
- 账号锁定时间：`login_lock_minutes`（默认 30 分钟）
- Token 有效期：`token_expire_days`（默认 7 天）
- 设备数量限制：同一账号最多 3 个设备同时登录

### 3.2 企业认证

- 认证状态：`is_certified`
  - 0：未认证
  - 1：审核中
  - 2：已认证
  - 3：认证被拒绝
- 新认证企业前 10 条职位需审核（可通过 `first_jobs_audit_limit` 配置）

### 3.3 职位管理

- 状态：`status`
  - draft：草稿
  - pending：待审核
  - rejected：审核拒绝
  - active：招聘中
  - paused：已暂停
  - expired：已过期
  - closed：已关闭
  - deleted：已删除
- 置顶规则：`is_top=1` 且 `top_expire_time > 当前时间` 的职位始终排在最前

### 3.4 报名规则

- 状态：`status`
  - pending：待审核
  - approved：已通过
  - rejected：已拒绝
  - canceled：已取消
- 重新报名限制：`reapply_limit_hours`（默认 72 小时）

---

## 四、常见问题与解决方案

（待开发过程中逐步补充）

---

## 五、开发记录

### 2026-05-02

**开发内容**：建立 AI 开发约束协议和开发记忆系统

**完成工作**：
1. 创建 `AI-DEVELOPMENT-GUIDE.md` - AI 开发约束协议
2. 创建 `AI-开发记忆.md` - 开发记忆文档

**备注**：这是首次 AI 参与项目开发，建立了基础约束机制和记忆系统，后续开发将遵循此协议并更新记忆文档。