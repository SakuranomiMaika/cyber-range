<p align="center">
  <h1 align="center">🏴‍☠️ Cyber Range</h1>
  <p align="center">
    Web 安全训练靶场 — SQL 注入 / XSS / 文件上传
    <br />
    全部原创关卡，Docker Compose 一键部署，适合安全学习者实战练习
  </p>
  <p align="center">
    <a href="#快速启动"><strong>快速启动 »</strong></a>
    <br />
    <br />
    <a href="#关卡列表">关卡列表</a>
    ·
    <a href="#项目结构">项目结构</a>
    ·
    <a href="#技术栈">技术栈</a>
    ·
    <a href="https://github.com/SakuranomiyaYuka/cyber-range/issues">报告问题</a>
  </p>
  <p align="center">
    <img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="License: MIT">
    <img src="https://img.shields.io/badge/python-3.11+-brightgreen.svg" alt="Python 3.11+">
    <img src="https://img.shields.io/badge/flask-3.0+-orange.svg" alt="Flask 3.0+">
    <img src="https://img.shields.io/badge/docker-compose-2496ED?logo=docker" alt="Docker Compose">
    <img src="https://img.shields.io/badge/关卡-15-blueviolet" alt="15 Levels">
  </p>
</p>

---

> ⚠️ **免责声明**：本项目仅供授权的安全学习和测试使用。请勿用于非法用途。

## 📋 目录

- [项目介绍](#项目介绍)
- [快速启动](#快速启动)
- [关卡列表](#关卡列表)
- [通关参考](#通关参考)
- [数据库结构](#数据库结构)
- [项目结构](#项目结构)
- [技术栈](#技术栈)
- [常见问题](#常见问题)
- [贡献指南](#贡献指南)
- [License](#license)

## 📖 项目介绍

**Cyber Range** 是一个面向 Web 安全初学者的训练靶场，涵盖三大经典漏洞类型，共 **15 个原创关卡**。每个系列从基础到进阶逐步深入：

- **SQL 注入** — GET 注入 → 登录绕过 → 布尔盲注 → 时间盲注 → UPDATE 注入
- **XSS 跨站脚本** — 反射型 → 标签绕过 → 存储型 → DOM 型 → CSP 绕过
- **文件上传** — 无过滤 → 后缀黑名单 → MIME 检查 → 幻数检查 → 多重过滤

设计理念：**每个关卡只教一个知识点**，页面提供明确提示。适合自学、培训、CTF 赛前练习。

## 🚀 快速启动

### Docker Compose（推荐）

```bash
git clone https://github.com/SakuranomiyaYuka/cyber-range.git
cd cyber-range
docker compose up -d
```

启动后访问 http://localhost:5000

停止：`docker compose down`

### 直接运行（无需 Docker）

```bash
cd cyber-range/app
pip install flask gunicorn
python app.py
```

访问 http://localhost:5000

## 🎯 关卡列表

### SQL 注入系列（5关）

| 关卡 | 类型 | 难度 | 攻击面 | 核心技巧 |
|------|------|:----:|--------|----------|
| Lv1 | GET 注入 | ★☆☆ | URL 参数 `?id=` | 单引号闭合，`OR 1=1` 爆出全部数据 |
| Lv2 | POST 登录绕过 | ★☆☆ | 登录表单 | 注释符 `--` 绕过密码验证 |
| Lv3 | 布尔盲注 | ★★☆ | URL 参数 `?id=` | 页面仅返回存在/不存在，逐位猜解 |
| Lv4 | 时间盲注 | ★★★ | URL 参数 `?id=` | 通过 `SLEEP()` 延时判断真伪 |
| Lv5 | UPDATE 注入 | ★★★ | 修改密码表单 | 在 UPDATE 语句中嵌入子查询读取 secrets 表 |

### XSS 跨站脚本系列（5关）

| 关卡 | 类型 | 难度 | 攻击面 | 核心技巧 |
|------|------|:----:|--------|----------|
| Lv1 | 反射型（无过滤） | ★☆☆ | 搜索框 | 直接输出 `<script>alert(1)</script>` |
| Lv2 | 反射型（标签绕过） | ★☆☆ | 搜索框 | `<script>` 被过滤，用 `<img src=x onerror=alert(1)>` 绕过 |
| Lv3 | 存储型（留言板） | ★★☆ | 评论表单 | 内容存入数据库，每次刷新都触发 |
| Lv4 | DOM 型 | ★★☆ | URL hash | 前端 `innerHTML` 直接插入，不经服务端渲染 |
| Lv5 | 存储型 + CSP 绕过 | ★★★ | 留言板 | CSP 限制内联脚本，需上传 `.js` 文件并引用 |

### 文件上传系列（5关）

| 关卡 | 类型 | 难度 | 攻击面 | 核心技巧 |
|------|------|:----:|--------|----------|
| Lv1 | 无过滤 | ★☆☆ | 文件选择 | 直接上传任意后缀文件 |
| Lv2 | 后缀黑名单 | ★☆☆ | 文件名 | 黑名单不全，改 `.php5` / `.phtml` / 大小写绕过 |
| Lv3 | MIME 类型检查 | ★★☆ | Content-Type | 仅检查 Content-Type，改 `image/jpeg` |
| Lv4 | 文件头检查 | ★★★ | 文件内容 | 检查幻数，在文件头加 `GIF89a` 后再放 payload |
| Lv5 | 多重过滤 | ★★★ | 组合检查 | 同时检查后缀、MIME、幻数，需组合多种技巧 |

## 💡 通关参考

### SQLi Lv1 — 基础 GET 注入

传入 `?id=1 OR 1=1`，底层 SQL 变为：

```sql
SELECT id, username, email, role FROM users WHERE id = 1 OR 1=1
```

`OR 1=1` 使条件恒成立，返回全部记录。

### XSS Lv1 — 反射型 XSS

在搜索框输入 `<script>alert('XSS')</script>`，页面弹出对话框。

### Upload Lv2 — 后缀黑名单绕过

将 `.php` 改为 `.php5` / `.phtml` / `.Php` 即可绕过。

上传后访问 `http://localhost:5000/upload/uploads/shell.php5`。

## 🗄️ 数据库结构

SQLite 数据库 `app/data.db`，包含三张表：

```sql
CREATE TABLE users (id INTEGER PRIMARY KEY, username TEXT, password TEXT, email TEXT, role TEXT DEFAULT 'user');
CREATE TABLE comments (id INTEGER PRIMARY KEY, name TEXT, content TEXT, created_at DATETIME DEFAULT CURRENT_TIMESTAMP);
CREATE TABLE secrets (id INTEGER PRIMARY KEY, name TEXT, value TEXT);
```

secrets 表预置两个 flag：

| name | value |
|------|-------|
| flag_sqli_level5 | flag{f1rst_0rd3r_upd4t3_1nj3ct10n} |
| flag_xss_level5 | flag{st0r3d_xss_w1th_csp_byp4ss} |

## 📁 项目结构

```
cyber-range/
├── Dockerfile
├── docker-compose.yml
├── LICENSE
├── README.md
└── app/
    ├── app.py
    ├── db.py
    ├── levels/     # sqli.py, xss.py, upload.py
    └── templates/  # Jinja2 模板
```

## 🛠️ 技术栈

Python Flask 3.x / SQLite / Docker Compose / Gunicorn / 原生前端

## ❓ 常见问题

**Q: 启动后 5000 端口无响应？** 检查 `docker compose logs -f`

**Q: 数据库报错？** `rm app/data.db && docker compose restart`

**Q: Docker 下载慢？** 配置国内镜像加速

## 🤝 贡献指南

Fork → 特性分支 → PR，遵循 PEP 8。

## 📄 License

MIT — Copyright (c) 2026 SakuranomiyaYuka