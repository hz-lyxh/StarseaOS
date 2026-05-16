# 星海量子通用操作系统 - 配置终端

**版本**：1.2.0  
**发布日期**：2026-06-15  
**开发者**：蓝域星河有限公司集团  

星海量子通用操作系统是一个多用户、多语言、高安全性的量子操作系统配置与管理终端。  
它提供了系统初始化、用户管理、终端命令交互、虚拟文件系统、应用/插件市场、定时任务、升级维护、安全审计等完整功能，并支持图形化/Web/命令行多种启动模式。

---

## v1.2.0 新特性

- **应用/插件市场远程列表**：`pm list-remote` / `app list-remote` 直接查看仓库所有可用条目，按类型自动筛选
- **分类帮助系统**：`help` 按类别显示命令，`help <关键词>` 智能搜索
- **图形界面双引擎**：优先 PyQt5 现代界面，自动回退 Tkinter
- **静默安装模式**：支持 `--unattended` 参数实现无人值守部署
- **统一启动器**：7 种启动模式（CLI/UI/Web 任意组合）
- **零硬编码密钥**：产品密钥通过环境变量或交互输入，彻底消除硬编码
- **Web 面板局域网访问**：监听 `0.0.0.0`，支持局域网内其他设备访问

---

## 快速开始

### 环境要求

- Python 3.8+
- 依赖库：见 `requirements.txt`

### 安装与运行

```bash
cd 组件
pip install -r requirements.txt
python main.py
```

首次使用请运行 **图形化安装向导**（自动弹出）或执行 `python install_wizard.py`，按提示输入产品密钥、创建管理员账户、设置终端类型及序号。安装完成后即可通过 `main.py` 选择启动模式（命令行/图形界面/Web面板）。

---

## 功能特性

### 核心特性
- **多语言支持**：内置 8 种语言，界面文本实时切换
- **多种启动模式**：CLI、PyQt5 图形界面（回退 Tkinter）、Flask Web 面板
- **三级权限体系**：user / admin / developer
- **加密安全**：配置文件 Fernet 加密，bcrypt 验证，产品密钥零硬编码
- **应用/插件市场**：统一市场源，`pm`/`app` 命令搜索、安装、更新、卸载，`list-remote` 查看远程仓库

### 终端体验
- **80+ 内置命令**：系统管理、文件操作、网络诊断、VFS 等
- **智能帮助**：`help` 分类显示，`help <关键词>` 搜索
- **Tab 补全**、历史记录、别名、收藏夹
- **多会话管理**：独立 VFS 和工作目录
- **7 种 ANSI 颜色主题**

### 系统管理
- **虚拟文件系统**：目录、文件、权限、软链接
- **系统维护**：完整性检查、还原点、版本化备份
- **定时任务**：cron 表达式，自动备份与报告
- **自愈守护进程**：监控调度器、自动压缩日志
- **升级支持**：本地迁移 + GitHub Release 自动更新
- **双因素认证**（TOTP）、硬件绑定许可证

### 扩展与集成
- **插件系统**：动态加载 `plugins/` 目录下的 Python 脚本
- **脚本引擎**：执行 `.ssea` 批处理脚本
- **网络工具**：ping、traceroute、SCP、邮件通知
- **沙盒执行**、独立 SDK

---

## 项目结构

```
星海量子操作系统/
├── 组件/
│   ├── main.py               # 统一启动器
│   ├── install_wizard.py     # 图形化/静默安装向导
│   ├── requirements.txt
│   ├── market_config.json    # 市场配置
│   ├── 关于/ 支付码/ 示例文档/ 数据集/
│   ├── 应用/                 # 应用安装目录
│   ├── 插件/                 # 插件安装目录
│   └── system/               # 核心模块（50+）
└── G_D_蓝域星河的数据储存库/
```

---

## 使用示例

### 1. 启动系统
```bash
python main.py
```
菜单选项：
```
1. 仅启动命令行终端
2. 仅启动图形界面
3. 仅启动 Web 面板
4. 命令行终端 + 图形界面
5. 图形界面 + Web 面板
6. 命令行终端 + Web 面板
7. 启动全部
0. 退出
```

### 2. 在终端中使用市场命令
```bash
pm list-remote          # 查看远程插件
pm install joke_plugin  # 安装插件
plugin run joke_plugin  # 运行插件

app list-remote         # 查看远程应用
app install calcapp     # 安装应用（需重启终端）
calcapp                 # 运行应用
```

### 3. 查看帮助
```bash
help           # 分类显示
help user      # 搜索用户相关命令
```

### 4. 静默安装（无人值守）
```bash
python install_wizard.py --unattended \
    --product-key "YOUR_KEY" \
    --admin-user "admin" \
    --admin-pass "Admin123!" \
    --term-type "KZ" \
    --term-index "A"
```

### 5. Web 面板局域网访问
启动 Web 面板后，本机访问 `http://127.0.0.1:5000`，局域网其他设备访问 `http://<本机IP>:5000`（如 `http://192.168.1.100:5000`）。

---

## 市场配置

默认市场配置文件 `market_config.json` 使用国内镜像：

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

远程仓库 JSON 格式示例：
```json
{
  "plugins": {
    "plugin_id": {
      "name": "插件名称",
      "version": "1.0",
      "desc": "描述",
      "type": "plugin",
      "file": "plugin_id.py"
    },
    "app_id": {
      "name": "应用名称",
      "type": "app",
      "command": "app_command",
      "file": "app_id.py"
    }
  }
}
```

---

## 安全说明

- **产品密钥**：通过环境变量 `STARSEA_PRODUCT_KEY` 或首次启动时交互输入
- **配置文件加密**：Fernet 加密（AES-128 CBC + HMAC）
- **登录保护**：错误 5 次锁定 15 分钟
- **双因素认证**：TOTP 可选
- **审计日志**：所有敏感操作记录在 `audit.log`

---

## 常见问题

**Q: 首次运行提示“没有已初始化的语言系统”？**  
A: 运行 `python install_wizard.py` 完成初始化。

**Q: `app list-remote` 超时？**  
A: 网络问题，可设置代理或更换镜像源。执行 `app refresh` 重试。

**Q: 安装应用后无法运行？**  
A: 需重启终端（`exit` 后重新登录）。

**Q: 图形界面闪退？**  
A: 安装 PyQt5：`pip install PyQt5`，系统会自动回退到 Tkinter。

**Q: 其他设备无法访问 Web 面板？**  
A: 确保防火墙允许 5000 端口，并使用本机局域网 IP 访问。

---

## 许可证

专有许可证，仅供授权使用。  
版权所有 © 蓝域星河有限公司集团。

---

## 联系我们

- 公司官网：[https://hz-lyxh.github.io/bloge/](https://hz-lyxh.github.io/bloge/)
- 系统官网：[https://hz-lyxh.github.io/starsea-os/](https://hz-lyxh.github.io/starsea-os/)

**愿星海量子伴您遨游数字宇宙！**