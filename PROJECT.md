# 星海量子通用操作系统 - 项目文档

**版本**：1.2.0  
**发布日期**：2026-06-15  
**开发者**：蓝域星河有限公司集团  

---

## 一、项目概述

星海量子通用操作系统配置终端是一个多用户、多语言、高安全性的量子操作系统管理工具。  
它提供了系统初始化、用户管理、终端命令交互、虚拟文件系统、应用/插件市场、定时任务、升级维护、安全审计等完整功能，并支持图形化/Web/命令行多种启动模式。

v1.2.0 聚焦于**市场生态完善、部署自动化、用户体验打磨**，新增静默安装、远程市场列表、分类帮助、图形界面回退机制等核心特性。

系统采用 **Fernet 加密** 保护所有配置文件，用户密码使用 SHA-256 加盐存储，系统密钥采用 bcrypt 验证并支持暴力锁定。  
内置 **admin / user / developer** 三级权限体系，开发者拥有最高运维权限（需独立激活密钥）。

> **安全提示**：产品密钥不再硬编码，首次启动通过环境变量 `STARSEA_PRODUCT_KEY` 或交互输入获取。

---

## 二、系统架构

### 2.1 整体目录结构

```
星海量子操作系统/
├── 组件/                                   # 系统运行根目录
│   ├── main.py                             # 统一启动器（7种模式）
│   ├── install_wizard.py                   # 图形化/静默安装向导
│   ├── requirements.txt                    # Python依赖
│   ├── README.md                           # 项目说明
│   ├── PROJECT.md                          # 项目文档（本文件）
│   ├── 更新日志.md                         # 版本更新记录
│   ├── market_config.json                  # 插件/应用市场配置（共用）
│   ├── 关于/                               # 产品说明文档
│   │   ├── 版本号.txt
│   │   ├── 关于.txt
│   │   ├── 系统教程.txt
│   │   ├── 系统框架.txt
│   │   └── 制作名单.txt
│   ├── 支付码/                             # 赞赏码占位图
│   ├── 示例文档/                           # 16类空白模板文件
│   ├── 数据集/                             # 参考数据
│   ├── 应用/                               # 官方应用安装目录（app install下载）
│   ├── 插件/                               # 插件安装目录（pm install下载）
│   └── system/                             # 核心系统模块（50+个Python模块）
│       ├── __init__.py
│       ├── config.py                       # 全局常量、8语言包、密钥哈希
│       ├── utils.py                        # 加密、文件操作、用户管理
│       ├── init_handler.py                 # 系统初始化流程（含依赖检查）
│       ├── login_handler.py                # 用户登录验证（自动加密初始化）
│       ├── daily_handler.py                # 日常检测菜单
│       ├── terminal.py                     # 终端交互核心（80+命令，pm/app支持）
│       ├── user_manager.py                 # 管理员用户管理子菜单
│       ├── upgrade.py                      # 本地版本迁移
│       ├── github_updater.py               # GitHub Release自动更新
│       ├── system_monitor.py               # 系统资源监控
│       ├── scheduler.py                    # 定时任务调度器
│       ├── virtual_fs.py                   # 虚拟文件系统
│       ├── script_engine.py                # .ssea脚本执行
│       ├── report_generator.py             # HTML报告生成
│       ├── diagnostics.py                  # 系统诊断工具
│       ├── theme.py                        # ANSI颜色主题
│       ├── task_manager.py                 # 任务管理界面
│       ├── system_maintenance.py           # 系统维护工具
│       ├── crypto_tool.py                  # 配置文件加解密工具
│       ├── notifications.py                # 通知中心
│       ├── audit.py                        # 安全审计日志
│       ├── restore_points.py               # 还原点管理
│       ├── password_reset.py               # 密码重置（安全问题）
│       ├── totp_auth.py                    # TOTP双因素认证
│       ├── network_tools.py                # ping/traceroute
│       ├── secure_transfer.py              # SCP文件传输
│       ├── mail_notify.py                  # 邮件通知
│       ├── process_manager.py              # 进程查看与控制
│       ├── settings_menu.py                # 系统设置菜单
│       ├── disk_usage.py                   # 磁盘使用统计
│       ├── password_policy.py              # 密码复杂度策略
│       ├── groups.py                       # 用户组管理
│       ├── session_manager.py              # 在线会话管理
│       ├── file_search.py                  # 文件搜索引擎
│       ├── backup_versioning.py            # 版本化备份
│       ├── profiler.py                     # 性能分析器
│       ├── env_manager.py                  # 环境变量持久化
│       ├── term_sessions.py                # 多会话管理
│       ├── clock.py                        # 实时时钟广播
│       ├── highlight.py                    # 语法高亮
│       ├── sandbox.py                      # 沙盒执行
│       ├── guardian.py                     # 自愈守护进程
│       ├── favorites.py                    # 收藏夹与别名
│       ├── license_bind.py                 # 硬件绑定许可证
│       ├── vfs_advanced.py                 # VFS权限扩展
│       ├── broadcast.py                    # 消息广播
│       ├── chart.py                        # ASCII资源图表
│       ├── plugin_manager.py               # 插件管理器
│       ├── plugin_market.py                # 插件市场（类型筛选、远程列表）
│       ├── app_market.py                   # 应用市场（类型筛选、远程列表）
│       ├── app_registry.py                 # 应用注册表
│       ├── web_server.py                   # Flask Web面板（无硬编码，监听0.0.0.0）
│       ├── gui_launcher.py                 # 图形面板（PyQt5优先，Tkinter回退）
│       ├── system_settings.py              # 系统设置独立模块
│       ├── kernel_interface.py             # 内核信息抽象层
│       ├── linux_kernel.py / windows_kernel.py / darwin_kernel.py
│       ├── system_management.py            # 服务/软件包管理
│       ├── ai_assistant.py                 # AI助手集成
│       ├── microservice.py                 # 微服务通信
│       ├── crypto_upgrade.py               # 密钥轮换
│       ├── config_watcher.py               # 配置热加载
│       ├── rbac.py                         # 基于角色的权限控制
│       └── tests/                          # 自动化测试
│           ├── conftest.py
│           ├── test_utils.py
│           ├── test_config.py
│           ├── test_user_management.py
│           ├── test_encryption.py
│           ├── test_terminal.py
│           ├── test_virtual_fs.py
│           ├── test_restore_points.py
│           └── test_diagnostics.py
├── sdk/                                    # 独立SDK包
│   ├── setup.py
│   └── starsea_sdk/
│       ├── __init__.py
│       ├── client.py
│       ├── crypto.py
│       └── errors.py
├── G_D_蓝域星河的数据储存库/               # 用户数据仓库（示例占位）
│   ├── 01.图片/
│   ├── 02.音频/
│   ├── 03.视频/
│   ├── 04.文档/
│   ├── 05.压缩包/
│   └── 06.其他/
├── LICENSE.txt                             # 专有软件许可证
└── 项目文档.md                              # 本文件（PROJECT.md）
```

### 2.2 核心模块依赖关系

```text
启动器 (main.py) 或 安装向导 (install_wizard.py)
    │
    ├─→ system.utils (加密、配置读写)
    │       └─→ cryptography, bcrypt, json
    ├─→ system.config (常量、语言包)
    ├─→ system.login_handler
    │       └─→ system.terminal (终端主循环)
    │               ├─→ system.virtual_fs
    │               ├─→ system.plugin_manager
    │               ├─→ system.plugin_market
    │               ├─→ system.app_market
    │               └─→ system.app_registry
    ├─→ system.gui_launcher (图形界面)
    └─→ system.web_server (Web面板)
            └─→ flask
```

---

## 三、v1.2.0 新增/强化功能

### 3.1 应用/插件市场生态完善
- **统一市场源**：`market_config.json` 同时用于插件和应用，通过 `type` 字段区分
- **远程列表**：`pm list-remote` / `app list-remote` 直接查看仓库所有可用条目
- **类型筛选**：插件市场仅显示 `type=plugin`，应用市场仅显示 `type=app`
- **下载增强**：超时延长至 30 秒，支持 3 次自动重试，失败时使用本地缓存

### 3.2 终端体验优化
- **分类帮助**：`help` 按类别显示常用命令，`help <关键词>` 智能搜索相关命令
- **命令补全**：支持 `pm` / `app` 所有子命令（`list-remote`, `search`, `install`, `update`, `refresh`, `list`, `uninstall`）
- **安装提示**：应用安装成功后自动提示“请重启终端”，避免命令无法立即识别

### 3.3 图形界面现代化与回退机制
- **双引擎支持**：优先使用 PyQt5（现代化界面），自动回退 Tkinter（无额外依赖）
- **完整功能**：9 个标签页覆盖系统状态、用户管理、VFS、命令执行、市场、插件、备份、日志、设置
- **多语言**：中英文界面实时切换

### 3.4 安装向导升级
- **图形化分步**：环境检查 → 目录准备 → 产品密钥 → 管理员账户 → 终端配置 → 执行安装
- **静默模式**：`--unattended` 参数实现无人值守安装，适合批量部署
- **安全回滚**：安装前自动备份现有配置，失败时完整恢复

### 3.5 Web 面板独立化与局域网访问
- 移除外部 client 依赖，直接调用系统模块
- 新增市场 API：`/api/market/plugins`、`/api/market/apps`
- `/api/exec` 支持 `pm` / `app` 命令
- **监听 `0.0.0.0`**，支持局域网内其他设备访问

### 3.6 启动器多模式
- 7 种组合：仅CLI、仅UI、仅Web、CLI+UI、UI+Web、CLI+Web、全部
- 通过环境变量 `STARSEA_PRODUCT_KEY` 传递产品密钥，彻底消除硬编码

### 3.7 安全与依赖
- 所有脚本中产品密钥硬编码已清除
- `init_handler.py` 自动检查 Python 版本和依赖包，缺失时提示安装命令
- 首次运行自动创建必要目录和默认 `market_config.json`
- 更新 `requirements.txt`，增加 `psutil`, `pyotp`, `paramiko`, `croniter`, `flask`, `PyQt5`, `pytest` 等依赖

---

## 四、核心功能详解

### 4.1 系统初始化
- 验证系统产品密钥（格式 XXXXX-XXXXX-XXXXX-XXXXX-XXXXX）
- 设置管理员账户、终端类型（FWQ/KZ/GL/DB）及序号
- 自动创建加密配置文件 `.lyxh.enc`
- 设置安全问题（用于密码重置）
- 初始化后自动重命名数据储存库文件夹

### 4.2 用户与权限管理
- 三级角色：user（普通）、admin（管理员）、developer（开发者）
- 管理员可增删用户、修改密码、变更角色
- 开发者拥有所有管理员权限 + 修改系统根密钥、查看原始配置等
- 角色变更至 developer 需独立激活密钥

### 4.3 量子终端
- 超过 80 个内置命令（系统管理、文件操作、网络、监控、虚拟文件系统等）
- Tab 命令补全、历史记录、别名、收藏夹
- 多会话管理（独立虚拟文件系统和真实目录）
- 颜色主题切换（7 种 ANSI 主题）
- 性能分析（`time` 命令）、沙盒执行
- 实时时钟显示、消息广播

### 4.4 虚拟文件系统（VFS）
- 支持创建/删除目录和文件
- 支持读写文件内容
- 支持权限（`vchmod`）和属主（`vchown`）设置
- 支持软链接（`vln`）
- 多会话间相互隔离

### 4.5 应用与插件市场
- **插件市场**：`pm search`、`pm install`、`pm update`、`pm list-remote`、`pm refresh`
- **应用市场**：`app search`、`app install`、`app update`、`app list-remote`、`app uninstall`
- 安装的应用注册为终端命令，安装后需重启终端
- 插件通过 `plugin run` 执行

### 4.6 安全机制
- 配置文件全程加密（AES-128 CBC + HMAC）
- 系统密钥 bcrypt 验证，错误 5 次锁定 15 分钟
- 用户密码 SHA-256 哈希存储，支持密码策略强制
- 双因素认证（TOTP）可选
- 硬件绑定许可证（可选）
- 安全审计日志记录所有敏感操作
- **产品密钥零硬编码**（v1.2.0 新增）

### 4.7 升级与维护
- 本地版本迁移（`upgrade` 命令）
- GitHub Release 自动检测、下载、应用升级包
- 系统还原点管理（创建 / 恢复快照）
- 文件完整性检查、临时文件清理、虚拟文件系统修复
- 自动备份（每 10 分钟）与定时报告（每小时）
- 自愈守护进程（监控调度器、日志自动压缩）

### 4.8 扩展性
- 插件系统：`plugins/` 目录下的 Python 模块可通过 `plugin` 命令动态加载和执行
- 脚本引擎：支持执行 `.ssea` 批处理脚本
- Cron 任务：支持标准 cron 表达式添加定时任务
- 独立 SDK：提供 `starsea_sdk` 包，外部程序可直接调用系统 API

### 4.9 多语言支持
内置 8 种语言：中文、English、日本語、Français、Deutsch、Español、Русский、العربية  
所有界面提示均根据所选语言显示，配置文件按语言独立存储。

---

## 五、安装与运行

### 5.1 环境要求
- Python 3.8+
- 依赖库：见 `requirements.txt`

### 5.2 安装步骤
```bash
cd 组件
pip install -r requirements.txt
python install_wizard.py   # 图形化安装向导
```

### 5.3 静默安装
```bash
python install_wizard.py --unattended \
    --product-key "YOUR_PRODUCT_KEY" \
    --admin-user "admin" \
    --admin-pass "YourStrongPassword123!" \
    --term-type "KZ" \
    --term-index "A"
```

### 5.4 启动系统
```bash
python main.py
```
菜单选择：
- `1` - 仅启动命令行终端
- `2` - 仅启动图形界面
- `3` - 仅启动 Web 面板
- `4` - 命令行 + 图形界面
- `5` - 图形界面 + Web 面板
- `6` - 命令行 + Web 面板
- `7` - 全部启动
- `0` - 退出

---

## 六、使用指南

### 6.1 终端登录
```bash
用户名: admin
密码: ******
星海量子>
```

### 6.2 常用命令
| 类别 | 命令 | 说明 |
|------|------|------|
| 系统 | `status`, `sysinfo`, `monitor` | 查看状态信息 |
| 文件 | `ls`, `cat`, `cd` | 真实文件操作 |
| VFS | `vls`, `vcd`, `vread`, `vwrite` | 虚拟文件系统 |
| 用户 | `users`, `useradd`, `userdel` | 用户管理（管理员） |
| 备份 | `backup`, `restore`, `create_rp` | 备份与还原 |
| 市场 | `pm list-remote`, `app list-remote` | 查看远程插件/应用 |
| 市场 | `pm install <id>`, `app install <id>` | 安装 |
| 插件 | `plugin run <id>` | 运行插件 |
| 帮助 | `help`, `help <关键词>` | 分类帮助 |

### 6.3 图形界面操作
- 启动 `python main.py` → 选择 `2`
- 9 个标签页：系统状态、用户管理、VFS、命令执行、市场管理、插件管理、备份还原、日志、设置
- 在“市场管理”标签页可直接浏览远程插件/应用并安装

### 6.4 Web 面板操作
- 启动 `python main.py` → 选择 `3`
- 本机访问 `http://127.0.0.1:5000`
- 局域网其他设备访问 `http://<本机IP>:5000`
- 支持 API 调用：`/api/status`, `/api/users`, `/api/market/plugins`, `/api/exec` 等

---

## 七、市场配置

### 7.1 配置文件 `market_config.json`
```json
{
  "repositories": [
    {
      "name": "官方仓库镜像",
      "index_url": "https://ghproxy.net/https://raw.githubusercontent.com/hz-lyxh/starsea-market/main/plugins.json",
      "download_base": "https://ghproxy.net/https://raw.githubusercontent.com/hz-lyxh/starsea-market/main/plugins/"
    }
  ]
}
```

### 7.2 远程仓库格式要求
```json
{
  "plugins": {
    "plugin_id": {
      "name": "插件名称",
      "version": "1.0",
      "desc": "描述",
      "type": "plugin",
      "file": "plugin_id.py",
      "signature": "sha256"
    },
    "app_id": {
      "name": "应用名称",
      "version": "1.0",
      "desc": "描述",
      "type": "app",
      "command": "app_command",
      "file": "app_id.py"
    }
  }
}
```

### 7.3 添加自定义仓库
在 `market_config.json` 的 `repositories` 数组中添加多个仓库，系统会合并所有仓库索引。

---

## 八、开发与扩展

### 8.1 插件开发
插件需实现 `run(args, lang, lang_code, current_user)` 函数，放入 `plugins/` 目录。

示例 `hello.py`：
```python
def run(args, lang, lang_code, current_user):
    name = args[0] if args else current_user['username']
    print(f"Hello, {name}!")
```
运行：`plugin run hello [name]`

### 8.2 应用开发
应用安装后会自动注册为终端命令，需在远程仓库 JSON 中声明 `type: "app"` 和 `command` 字段。

### 8.3 使用 SDK
```python
from starsea_sdk import StarseaClient
client = StarseaClient("产品密钥")
users = client.get_users("system_config_zh.lyxh.enc")
```

### 8.4 自动化测试
```bash
cd 组件
pytest tests/ -v
```
测试覆盖：工具函数、配置常量、用户管理、加密解密、终端命令、VFS、还原点、诊断。

---

## 九、常见问题

**Q: 首次运行提示“没有已初始化的语言系统”？**  
A: 运行 `python install_wizard.py` 完成初始化。

**Q: `app list-remote` 超时或失败？**  
A: 网络问题，可设置代理或更换 `market_config.json` 中的镜像源。执行 `app refresh` 重试。

**Q: 安装应用后无法运行？**  
A: 应用安装后需重启终端（`exit` 退出后重新登录）。

**Q: 图形界面闪退或报错？**  
A: 安装 PyQt5：`pip install PyQt5`。若无 PyQt5，系统会自动回退到 Tkinter。

**Q: 如何更换产品密钥？**  
A: 修改环境变量 `STARSEA_PRODUCT_KEY`，或删除现有配置文件后重新初始化。

**Q: 插件市场和应用市场可以分开配置吗？**  
A: 目前共用 `market_config.json`，通过 `type` 字段区分。未来版本可能支持独立配置。

**Q: 静默安装需要哪些参数？**  
A: 必须提供 `--product-key`, `--admin-user`, `--admin-pass`, `--term-type`, `--term-index`。

**Q: 如何让其他设备访问 Web 面板？**  
A: 确保防火墙允许 5000 端口，启动 Web 面板后使用本机局域网 IP 访问（如 `http://192.168.1.100:5000`）。

**Q: 为什么 Web 面板监听 `0.0.0.0` 后无法用 `0.0.0.0:5000` 访问？**  
A: `0.0.0.0` 是监听地址，不是访问地址。请使用本机 IP 或 `127.0.0.1` 访问。

---

## 十、许可证

本项目采用专有许可证，仅供授权使用。  
版权所有 © 蓝域星河有限公司集团。  
详细条款请参阅项目根目录下的 `LICENSE.txt` 文件。

---

## 十一、联系方式

- 公司官网：[https://hz-lyxh.github.io/bloge/](https://hz-lyxh.github.io/bloge/)
- 系统官网：[https://hz-lyxh.github.io/starsea-os/](https://hz-lyxh.github.io/starsea-os/)
- 技术支持：仅限授权用户（通过内部渠道）

**愿星海量子助您开启智能运维新时代！**