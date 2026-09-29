website
分享几个简单实用的公司网站
[README.md](https://github.com/user-attachments/files/32782715/README.md)
# 星核科技 · 科技公司官网

一套「科技感前台 + 内容管理后台」的公司官网，专为**宝塔面板（PHP 8.0 + SQLite）**环境设计，无需 MySQL、无任何 Composer 依赖，上传即可使用。

## 功能概览

| 模块 | 说明 |
| --- | --- |
| 前台首页 `index.php` | 暗色科技风：粒子网络动效、打字机标语、数字滚动、滚动显现动画 |
| 产品服务 | 展示后台维护的产品/服务卡片，支持图标与排序 |
| 关于我们 | 公司介绍 + 终端风格装饰面板 |
| 新闻 `news.php` | 列表（分页）+ 详情页，内容后台可管理 |
| 联系我们 `contact.php` | 在线留言表单，提交后写入 SQLite，后台可查看 |
| 后台 `admin/` | 登录鉴权、仪表盘、新闻/产品/留言管理、站点信息与密码设置 |

- 深度适配移动端（H5）：响应式栅格、汉堡菜单、触控友好的按钮与表单
- 安全：密码 `password_hash` 加密、全站输出转义、后台 CSRF 校验、PDO 预处理防注入
- 零依赖：纯原生 PHP + CSS + JS，无 CDN、无外部字体，国内服务器加载无忧

## 目录结构

```
website/
├── index.php            # 前台首页
├── news.php             # 新闻列表 + 详情
├── contact.php          # 联系我们（在线留言）
├── install.php          # 安装向导（装完删除）
├── includes/
│   ├── db.php           # SQLite 连接 / 建表 / 演示数据
│   ├── functions.php    # 转义、设置、鉴权、CSRF、图标
│   └── header.php / footer.php
├── assets/
│   ├── css/style.css    # 前台样式（暗色科技风）
│   └── js/main.js      # 粒子背景、打字机、数字滚动等动效
├── admin/
│   ├── login.php / logout.php
│   ├── index.php        # 仪表盘
│   ├── news.php         # 新闻管理
│   ├── products.php     # 产品管理
│   ├── messages.php     # 留言管理
│   ├── settings.php     # 站点设置 + 密码修改
│   ├── inc/top.php, inc/bottom.php
│   └── assets/admin.css
└── data/                # SQLite 数据目录（已带 .htaccess 禁止访问）
```

## 宝塔面板部署

1. **创建站点**：宝塔 → 网站 → 添加站点（PHP 站点），PHP 版本选择 **8.0**（8.1+ 均可）。
2. **上传代码**：将 `website/` 目录内的全部文件上传到站点根目录（如 `/www/wwwroot/你的域名/`）。
3. **检查扩展**：宝塔 → 软件商店 → PHP 8.0 → 设置 → 安装扩展，确认已启用 `pdo_sqlite`（宝塔默认启用）。
4. **目录权限**：
   ```bash
   chown -R www:www /www/wwwroot/你的域名
   chmod -R 755 /www/wwwroot/你的域名/data
   ```
5. **初始化**：浏览器访问 `http://你的域名/install.php`，设置管理员账号密码，完成安装。
6. **安全加固（Nginx，重要）**：站点设置 → 配置文件，在 `server { }` 内加入：
   ```nginx[website.zip](https://github.com/user-attachments/files/32782734/website.zip)
[website.zip](https://github.com/user-attachments/files/32782727/website.zip)

   location ^~ /data/ {
       deny all;
   }
   location = /install.php {
       deny all;   # 或直接删除该文件
   }
   ```
   （Apache 用户项目已自带 `data/.htaccess` 保护，无需额外配置。）
7. 登录 `http://你的域名/admin/`，在「系统设置」中修改公司名称、标语、联系方式、ICP 备案号，并发布新闻、维护产品。

## 使用说明

- **改公司名/联系方式**：后台 → 系统设置，保存后前台立即生效；
- **首页打字机标语**：第一句取自「主标语」设置，其余内置，可在 `index.php` 的 `$phrases` 中调整；
- **留言通知**：留言存入数据库，后台侧边栏会显示未读数量角标；
- **重置系统**：删除 `data/site.db` 后重新访问 `install.php`。

## 安全建议

- 安装完成后立即删除 `install.php`；
- 后台密码请使用 12 位以上强密码；
- 建议在宝塔一键申请 SSL 证书启用 HTTPS。
