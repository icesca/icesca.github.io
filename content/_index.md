---
bookToc: false
title: home
type: docs
---

**博客前端演示入口请跳转**

- <a href="http://116.62.33.139:50000" target="_blank" rel="noopener noreferrer">http://116.62.33.139:50000</a>
（前端界面使用 AI 辅助生成）

**项目介绍**

- 简要陈述
  1. **网络库**：使用 C++ 和 socket 编程接口，基于 multi-Reactor 模型
  2. **HTTP 框架**：利用网络库接口，实现协议解析、静态和动态路由注册、会话管理
  3. **博客平台后端**：提供基于 HTTP 的 api，实现用户认证（登录/注册）、博文展示、发布与修改功能
  4. 独立模块：基于阻塞队列的**异步日志**系统；基于生产者-消费者模型的 MySQL **连接池**

- 分模块解析
  - [multi-Reactor 网络库](/docs/multi-reactor)
  - [HTTP 框架](/docs/http)
  - [博客平台后端](/docs/web-app)
  - [日志系统和数据库连接池](/docs/logger-dbpool)

**代码库**

- <a href="https://github.com/icesca" target="_blank" rel="noopener noreferrer">github</a>
