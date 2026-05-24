# 论坛系统

全栈论坛系统，基于 Spring Boot + Vue.js 构建。
最初来自于 码神之路 的 个人博客系统。这里对 java版本 进行了升级

## 技术栈

### 后端
| 技术 | 版本 | 说明 |
|------|------|------|
| Java | 21 | 运行环境 |
| Spring Boot | 3.5.4 | 应用框架 |
| MyBatis-Plus | 3.5.10 | ORM 框架 |
| MySQL | 8.x | 关系数据库 |
| Redis | - | 缓存 / Token 存储 / 限流 |
| Spring Security | - | 后台认证 |
| JWT (jjwt) | 0.12.6 | API 认证 |
| Log4j2 | - | 日志框架 |
| Lombok | 1.18.38 | 代码简化 |
| FastJSON2 | 2.0.58 | JSON 序列化 |
| 七牛云 SDK | 7.19.x | 图片上传 |

### 前端
| 技术 | 说明 |
|------|------|
| Vue 2.5 | 前端框架 |
| Vue Router | 路由管理 |
| Vuex | 状态管理 |
| Element UI | UI 组件库 |
| Mavon Editor | Markdown 编辑器 |
| Axios | HTTP 客户端 |
| Webpack | 模块打包 |

## 项目结构

```
Java_Blog/
├── blog-java/                    # 后端
│   ├── blog.sql                  # 数据库初始化脚本
│   └── blog-parent/
│       ├── blog-api/             # 公开 API 模块（端口 8888）
│       └── blog-admin/           # 后台管理模块（端口 8889）
├── blog-app-new/                 # 前端（端口 8080）
└── images/                       # 图片资源
```

## 功能模块

### 公开 API
- **文章管理** — Markdown 编辑器撰写文章，支持分类和标签
- **评论系统** — 两级嵌套评论
- **用户认证** — JWT 登录 + Redis Token 存储
- **用户注册** — 自助注册
- **图片上传** — 七牛云存储
- **文章归档** — 按年/月分组
- **首页小部件** — 热门文章、最新文章、热门标签、归档
- **速率限制** — Redis 滑动窗口限流
- **AOP 缓存** — 基于 Redis 的自定义 `@Cache` 注解
- **AOP 日志** — 操作日志记录（模块、操作、耗时）

### 后台管理
- **管理员认证** — Spring Security + BCrypt
- **权限管理** — 基于角色的 CRUD 权限控制

## 快速开始

### 环境要求

- JDK 21
- Maven 3.6+
- MySQL 8.x
- Redis
- Node.js 8+

### 1. 数据库初始化

创建数据库并导入初始化脚本：

```sql
CREATE DATABASE blogdb DEFAULT CHARACTER SET utf8mb4;
USE blogdb;
SOURCE blog-java/blog.sql;
```

### 2. 修改配置

编辑 `blog-java/blog-parent/blog-api/src/main/resources/application.properties`：

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/blogdb
spring.datasource.username=root
spring.datasource.password=你的密码

spring.data.redis.host=localhost
spring.data.redis.port=6379
spring.data.redis.password=你的Redis密码
```

### 3. 启动后端

```bash
cd blog-java/blog-parent
mvn clean install
# 启动 API 模块（端口 8888）
cd blog-api
mvn spring-boot:run
# 启动后台管理模块（端口 8889）
cd ../blog-admin
mvn spring-boot:run
```

### 4. 启动前端

```bash
cd blog-app-new
npm install
npm run dev
```

访问 `http://localhost:8080` 查看博客首页。

### 默认账号

| 账号 | 密码 | 角色 |
|------|------|------|
| admin | admin | 管理员 |
| mszlu | 123456 | 管理员 |

## 接口文档

### 文章相关

| 方法 | 路径 | 说明 |
|------|------|------|
| POST | `/articles` | 发布文章 |
| GET | `/articles` | 文章列表（分页） |
| GET | `/articles/hot` | 热门文章 |
| GET | `/articles/new` | 最新文章 |
| GET | `/articles/view/{id}` | 文章详情 |
| GET | `/articles/archives` | 文章归档 |

### 用户相关

| 方法 | 路径 | 说明 |
|------|------|------|
| POST | `/login` | 用户登录 |
| POST | `/register` | 用户注册 |
| GET | `/logout` | 退出登录 |
| GET | `/users/currentUser` | 当前用户信息 |

### 评论相关

| 方法 | 路径 | 说明 |
|------|------|------|
| GET | `/comments/article/{id}` | 文章评论列表 |
| POST | `/comments/create/change` | 发表评论 |

### 分类 & 标签

| 方法 | 路径 | 说明 |
|------|------|------|
| GET | `/categorys` | 所有分类 |
| GET | `/categorys/detail` | 分类详情（含文章数） |
| GET | `/tags` | 所有标签 |
| GET | `/tags/hot` | 热门标签 |
| GET | `/tags/detail` | 标签详情（含文章数） |

## 数据库表

| 表名 | 说明 |
|------|------|
| `ms_article` | 文章表 |
| `ms_article_body` | 文章内容表 |
| `ms_article_tag` | 文章-标签关联表 |
| `ms_category` | 分类表 |
| `ms_tag` | 标签表 |
| `ms_comment` | 评论表 |
| `ms_sys_user` | 用户表 |
| `ms_admin` | 管理员表 |
| `ms_permission` | 权限表 |
