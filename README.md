# JMBot-NcatBot

> [!CAUTION]
> **请勿大批量、高频率下载漫画**
>
> 本工具仅面向**个人自用、按需下载**的场景。大批量或高频率的连续下载会给禁漫站点服务器带来沉重负担，影响其他用户的正常访问，同时极易触发站点风控，导致你的 JM 账号被限流乃至封禁。
>
> 请自觉控制下载的数量和频率。因滥用造成的一切后果（包括但不限于账号封禁、IP 限制），由使用者自行承担。

基于 [NcatBot](https://docs.ncatbot.xyz/) + [NapCat](https://napcat.napneko.icu/) 的禁漫（JM Comic）独立下载机器人：在群聊或私聊中发送漫画 ID，机器人即自动完成下载、生成 PDF 并回传文件。支持批量下载、JM 账号登录、PDF 加密、下载路径自定义与产物定期清理。

本项目是一个**独立、轻量、不依赖 AstrBot** 的机器人部署形态。如果你需要 WebUI 可视化配置、站内搜索（`/jms`、`/jma`）、本子详情查询（`/jmv`）、超分辨率下载（`-h/-hr/-hw`）等更多功能，请使用姊妹项目 **[Sanshui755/JMBot-Pro](https://github.com/Sanshui755/JMBot-Pro)**（NapCat + AstrBot 插件版）。

架构链路：**NapCat（QQ 协议端）← WebSocket → NcatBot（Python 框架）← 插件 → JMBot**。

---

## ⚖️ 使用须知与免责声明

1. **内容性质**：本项目涉及成人向（R18）内容站点，**禁止未满 18 周岁的用户安装或使用**。
2. **用途限制**：本项目仅用于编程技术、网络通信与自动化方向的学习与研究。使用者应确保其行为符合所在国家或地区的法律法规，以及目标站点的服务条款。
3. **合理使用**：请**勿大批量、高频率下载**漫画。请按需、少量下载，自觉控制频率。
4. **版权提示**：通过本项目获取的任何漫画、图片及其衍生文件，其著作权均归原作者或相应权利人所有。请**支持正版**，并在获取后 **24 小时内删除**，不得用于传播、分享、二次上传或任何商业用途。
5. **责任界定**：本项目为开源技术工具，作者不提供任何内容，也不对使用者的任何下载、存储、传播行为承担法律责任。
6. **侵权处理**：如权利人认为本项目存在侵权情形，请通过 GitHub Issue 联系，核实后将及时处理。

---

## 功能特性

- `/jm <id>` 下载漫画并生成 PDF 回传（群聊 / 私聊）
- **批量下载**：`/jm 350234 350235 350236`；兼容 `/jm350234`（无空格）、`/jm 350234备注`（数字后紧跟文字）
- **JM 账号登录**：聊天指令保存账号凭据（仅存于本机），启动时自动登录
- **PDF 加密**：群聊回传的 PDF 可自动添加打开密码（私聊默认不加密）
- **下载路径可配置**：聊天指令修改，切换目录时旧文件自动迁移
- **自动清理**：下载产物默认保留 3 天，后台任务每 6 小时清理一次
- **错误处理**：本子不存在、ID 错误等场景返回明确中文提示

> 需要站内搜索、作者搜索、详情查询、超分辨率下载等功能，请使用 [JMBot-Pro（AstrBot 插件版）](https://github.com/Sanshui755/JMBot-Pro)。

---

## 1. 环境要求

- Windows 10/11（其他平台请自行调整 NapCat 启动方式）
- Python 3.10 或更高版本（开发环境为 Python 3.13）
- [NapCat.Shell](https://napcat.napneko.icu/guide/boot-shell)（免安装版 QQ 协议端，需本机已安装 QQNT）

## 2. 安装依赖

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

> 若 PowerShell 提示“禁止运行脚本”，请先执行一次：
> `Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`

## 3. 修改配置

```powershell
Copy-Item config.example.yaml config.yaml
```

编辑 `config.yaml`，将 `bot_uin`（机器人 QQ 号）与 `root`（超管 QQ 号）替换为你自己的号码。

随后在 NapCat 中配置 WebSocket 服务端：打开 NapCat WebUI（默认 `localhost:6099`）→ 网络配置 → 新建 **WebSocket 服务器**，监听端口填 `3001`，Token 留空（需与 `config.yaml` 中的 `ws_token: ""` 保持一致）。

## 4. 启动

**方式 A：一键启动脚本（推荐）**

编辑 `start_all.bat` 顶部的变量（`NAPCAT_DIR`、`BOT_QQ`、`NCATBOT`），双击运行。脚本将依次完成：启动 NapCat → 等待 3001 端口就绪 → 启动机器人。

**方式 B：手动启动**

```powershell
# 终端 1：启动 NapCat（附加 QQ 号参数可使用快速登录）
cd C:\NapCat.Shell
.\launcher-user.bat 你的QQ号

# 终端 2：启动机器人
.\.venv\Scripts\ncatbot.exe run
```

首次启动需使用手机 QQ 扫描二维码登录，此后带 QQ 号参数启动即可快速登录。

## 5. 快速验证

私聊机器人依次发送：

```
设置JM账号 你的用户名
设置JM密码 你的密码
JM状态
/jm 350234
```

群聊使用前，先由超管私聊机器人发送 `开启JMBot`。

## 6. 命令一览

| 命令                                | 功能                                         | 权限                       |
| ----------------------------------- | -------------------------------------------- | -------------------------- |
| `/jm <id> [id ...]`                 | 下载并回传 PDF，支持多个 ID                  | 群聊需先开启，私聊默认可用 |
| `/jm help`                          | 查看使用帮助                                 | 所有人                     |
| `开启JMBot` / `关闭JMBot`           | 开启 / 关闭群聊下载                          | 超管                       |
| `测试JMBot`                         | 检查插件运行状态                             | 超管                       |
| `打开加密` / `关闭加密`             | 开关群聊 PDF 加密                            | 超管                       |
| `PDF密码` / `设置PDF密码 xxx`       | 查看 / 设置 PDF 打开密码                     | 超管                       |
| `设置JM账号 xxx` / `设置JM密码 xxx` | 保存 JM 凭据并自动登录                       | 超管                       |
| `JM登录` / `JM状态` / `清除JM账号`  | 手动登录 / 查看状态 / 清除凭据               | 超管                       |
| `JM帮助`                            | 查看帮助信息                                 | 超管                       |

> 建议每条设置命令单独发送一条消息，避免参数被换行内容污染。
>
> 修改代码后可私聊机器人发送 `!reload JMBot` 热重载。

---

## 目录结构

```
JMBot-NcatBot
├── config.example.yaml      # 主配置模板（复制为 config.yaml 后使用）
├── start_all.bat            # 一键启动脚本（NapCat + 机器人）
├── test_jm_login.py         # JM 登录链路自测脚本
├── requirements.txt         # Python 依赖
└── plugins/JMBot/           # 插件本体
    ├── plugin.py            #   插件主逻辑
    ├── manifest.toml        #   NcatBot 插件清单
    ├── set_password.py      #   PDF 加密
    └── config/config.yml    #   jmcomic 下载配置（域名/线程/PDF 插件）
```

敏感配置（`config.yaml`、`jm_account.json`、`pdf_password.txt` 等）与下载产物均不会进入版本库，详见 [`.gitignore`](.gitignore)。

---

## 常见问题

- **提示域名解析失败**：JM 域名可能因网络环境无法解析。jmcomic 会自动获取可用域名；必要时可在 jmcomic 配置（`plugins/JMBot/config/config.yml`）的 `client.domain` 中手动指定可用域名，或为终端配置代理。
- **登录返回 401**：请核对账号与密码，并确保每条设置命令单独发送。
- **群聊中无响应**：群聊下载默认关闭，请先由超管私聊机器人发送 `开启JMBot`。

---

## 🙏 致谢（Acknowledgements）

- **[Estecsky/JMBot](https://github.com/Estecsky/JMBot)** —— 本项目的直接改编来源，沿用其“聊天发送 `/jm <id>` → 自动下载 → 生成 PDF → 回传文件”的核心设计与命令交互。
- **[Sora-o-tobu/JM-NcatBot](https://github.com/Sora-o-tobu/JM-NcatBot)** —— Estecsky/JMBot 的上游项目，衍生脉络为：
  `JM-NcatBot` → `Estecsky/JMBot` → **JMBot-NcatBot（本项目）**
- **[hect0x7/JMComic-Crawler-Python](https://github.com/hect0x7/JMComic-Crawler-Python)**（MIT License）—— 提供禁漫站点访问、域名解析、图片混淆还原、账号登录等全部核心下载能力。
- [NapCatQQ](https://napcat.napneko.icu/) —— QQ 协议端
- [NcatBot](https://docs.ncatbot.xyz/) —— 本项目使用的 Python 机器人框架

> **许可说明**：[Estecsky/JMBot](https://github.com/Estecsky/JMBot) 与 Sora-o-tobu/JM-NcatBot 未在其仓库中声明开源许可证。本项目在充分署名的前提下发布衍生代码，若相关权利人对改编或发布有异议，请随时联系，将第一时间配合处理。

---

## 许可证（License）

- 本项目**新增与改编的代码**以 [MIT License](LICENSE) 开源。
- 核心依赖 [JMComic-Crawler-Python](https://github.com/hect0x7/JMComic-Crawler-Python) 遵循其自身的 MIT License。
- 本项目不包含任何受版权保护的漫画内容。
