<div align="center">

```
██████╗ ██╗  ██╗██╗  ██╗ ██████╗ ██╗   ██╗██████╗ ███████╗ █████╗ ██████╗ ██╗
╚════██╗██║  ██║██║  ██║██╔═══██╗██║   ██║██╔══██╗██╔════╝██╔══██╗██╔══██╗██║
 █████╔╝███████║███████║██║   ██║██║   ██║██████╔╝███████╗███████║██████╔╝██║
██╔═══╝ ╚════██║██╔══██║██║   ██║██║   ██║██╔══██╗╚════██║██╔══██║██╔═══╝ ██║
███████╗     ██║██║  ██║╚██████╔╝╚██████╔╝██║  ██║███████║██║  ██║██║     ██║
╚══════╝     ╚═╝╚═╝  ╚═╝ ╚═════╝  ╚═════╝ ╚═╝  ╚═╝╚══════╝╚═╝  ╚═╝╚═╝     ╚═╝
```

# 24hoursapi-bulk-apikey

**批量创建 24hoursAPI 账户并自动生成 API 密钥 — 兼容 OpenAI 的推理 + 9Router 连接器。**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-61%20served-412991?style=for-the-badge)](https://24hoursapi.com)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![OmniRoute](https://img.shields.io/badge/OmniRoute-connector-8B5CF6?style=for-the-badge)](#-connect-to-omniroute)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 快速开始](#-quick-start) · [🎁 免费额度](#-free-tier--how-much-you-get) · [🤖 模型](#-models--all-61-of-them) · [🧪 使用 API](#-how-to-use-the-api) · [✅ 已验证](#-verified-working--every-key-is-tested) · [🧭 工作原理](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [🔌 OmniRoute](#-connect-to-omniroute) · [⚙️ 选项](#️-cli-options) · [🛰️ 代理](#️-behind-a-proxy) · [🐞 问题](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ 为什么会有这个项目

24hoursAPI 为**每个新账户发放 1,000,000 配额单位（在 qpu=500,000 时约合 $2）**，外加每日签到（实测 +2,000…+9,752）。面板的 `default` 分组**已被弃用**（`403 分组 default 已被弃用`）——其可用渠道位于 **`qwen 官直0.8`**（`qwen-plus-latest`）和 **`官转 0.8`**（`deepseek-v4.1-flash`，推理模型）。注册只需邮箱 + 验证码，无需验证码；面板会定期从管理端关闭注册，本工具在遇到这种情况时会如实报告。

## 🎁 免费额度 — 你能获得多少

| 项目 | 数值 |
| --- | --- |
| 注册赠送 | **每个账户 1,000,000 单位 ≈ $2**（在全部 52 个账户上实测；qpu=500,000） |
| 每日签到 | **+2,000 … +9,752 单位/天**（该批次 52 个账户中有 50 个获得发放） |
| 可用分组 | **`qwen 官直0.8`** → `qwen-plus-latest` · **`官转 0.8`** → `deepseek-v4.1-flash` |
| `default` 分组 | **在此分支上已弃用**（`403 分组 default 已被弃用`）——密钥必须在可用分组中创建 |
| 注册 | 邮箱 + 6 位验证码，**无需验证码**；注册可能被管理员关闭（`New user registration has been disabled by administrator`）——CLI 会报告确切的响应正文 |
| 目录 | 实时价格表上有 61 个模型条目 |

> 💡 免费额度由 24hoursAPI 设定，可能发生变化。请在 [24hoursapi.com](https://24hoursapi.com) 查看实时数值——并让工具自身的校验来决定：

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 模型 — 全部 61 个

实时目录包含 **61 个模型条目**。两条已验证的线路，各自位于其分组中：

| 模型 ID | 分组 | 已验证 |
| --- | --- | --- |
| `qwen-plus-latest` | `qwen 官直0.8` | 生成期间每个密钥均 chat-200；独立复检 **3/3** |
| `deepseek-v4.1-flash` | `官转 0.8` | 每个密钥均 chat-200（推理模型——内容可能位于 `reasoning_content` 中） |

```bash
curl https://24hoursapi.com/v1/chat/completions \
  -H "Authorization: Bearer JnSI3L…" -H "Content-Type: application/json" \
  -d '{"model":"qwen-plus-latest","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 如何使用 API

基础 URL **`https://24hoursapi.com/v1`** — 兼容 OpenAI。

```python
from openai import OpenAI
c = OpenAI(base_url="https://24hoursapi.com/v1", api_key="JnSI3L…")
print(c.chat.completions.create(model="qwen-plus-latest",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 快速开始

```bash
cd 24hoursapi-bulk-apikey
printf 'socks5://host:port\n' > proxies.txt   # optional — without it the tool runs direct
node src/launcher.js        # interactive: mint / verify / screen / settings

# or: node src/cli.js --count 30    (check-in runs before the chat gate)
```

只有当 `/v1/chat/completions` 返回 200 且带有真实内容后，密钥才会被写入 `keys.txt`；`accounts.json` 保存用户名 / 邮箱 / quotaStart → quotaAfterCheckin / 分组。

### 🖱️ 启动器

启动器会询问代理列表（可选——不提供则 CLI 直接运行）和数量，记住它们，并运行 CLI——生成、验证、检测出口、设置、帮助、连接到 9Router。

## ✅ 已验证可用 — 每个密钥都经过测试

| 检查 | 结果 |
| --- | --- |
| `GET /api/status` | **200** — 邮箱验证开启，签到开启，验证码关闭（此分支不发布 `register_enabled`；拒绝响应正文即为信号） |
| `POST /api/user/checkin` | **200** — 配额从 1,000,000 变为最高 1,009,752（50/52 个账户） |
| 在 `qwen 官直0.8` / `官转 0.8` 中 `POST /api/token/` | **200** — 密钥仅返回一次 |
| 每个密钥写入前 `POST /v1/chat/completions` | **200** — 49× `qwen-plus-latest` + 3× `deepseek-v4.1-flash` |
| 独立复检（全新出口，抽样） | **3/3** 个密钥 `GET /v1/models` 200 + `qwen-plus-latest` 补全 200 |
| 面板侧故障（重建期间观察到） | chat 调用返回 `500 Database error, please contact the administrator`——**面板侧**故障；面板恢复健康后重新运行 `--verify-only` |

```bash
node src/cli.js --verify-only
node src/cli.js --screen
```

> **质量门禁。** 这里的每个密钥在写入 `keys.txt` 之前都通过了实时 `chat-200`
> （绝不交付失效密钥）。路由器组合在模型层面遵循同样的规则：仅当模型
> 返回 `chat-200` 且能调用工具时，才会添加该条目——不支持工具、密钥失效或
> 提供方失效的模型会被丢弃而非添加。可随时重新运行验证步骤；
> 失败会被报告，绝不隐藏。

## 🧭 工作原理

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: 24hoursAPI page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://24hoursapi.com/v1/chat/completions → real content = the key is live
```

| 步骤 | 调用 | 说明 |
| --- | --- | --- |
| status | `GET /api/status` | 邮件 + 签到门禁；此分支不发布注册标志 |
| mailbox | Emailnator `POST /api/generate-email` | 在集群的 Mailtrap 上限之后，gmail 成为备用通道 |
| send code | `GET /api/verification?email=…` | 6 位验证码 |
| register | `POST /api/user/register` | 当管理员关闭注册时，响应正文会原样说明 |
| login | `POST /api/user/login` | 会话令牌 |
| **check-in** | `POST /api/user/checkin` | 在 $2 注册赠送之外的每日奖励 |
| key | 在 `qwen 官直0.8` 中 `POST /api/token/`（回退 `官转 0.8`） | 分组遍历可捕获主分组空分配的情况 |
| proof | `POST /v1/chat/completions` | 返回 200 且带内容 = 密钥可用 |

## ⚙️ CLI 选项

```bash
node src/cli.js --count N        # mint N keys (check-in + chat gate per account)
node src/cli.js --group "qwen 官直0.8" --alt-groups "官转 0.8"
node src/cli.js --verify-only    # re-verify every key in keys.txt
node src/cli.js --screen         # exit pool self-test
node src/cli.js --connect-9router
node src/cli.js --help
```

| 环境变量 | 默认值 | 含义 |
| --- | --- | --- |
| `H24API_PROXIES` | `proxies.txt` | 出口池，每行一个代理——可选：没有池时 CLI 直接连接 |
| `H24API_MODEL` | `qwen-plus-latest` | 用于实时验证的模型 |

## 🛰️ 在代理之后

**可选——大规模使用时推荐（按 IP 限制、封禁）。** 本工具在没有代理时也能工作。

- **有池时：** 传入 `--pool <file>`（或设置 `H24API_PROXIES`），每个请求都会经过该列表中的一个出口——失效线路会使该次尝试失败，CLI 会用下一个出口重试（注册/登录有速率限制：突发时会出现 `login 429`）。
- **无池时：** CLI 会打印 `running WITHOUT a proxy (direct)` 并直接连接。注册、签到、密钥生成或验证都不会有任何变化——只是数据包发出的位置不同。
- 对任一模式运行 `--screen`：它会在真实运行前，用你拥有的出口（或直连路由）针对 24hoursAPI 进行测试。

## 📄 文件

| 路径 | 内容 |
| --- | --- |
| `keys.txt` | 每行一个经过实时验证的密钥——已被 git 忽略 |
| `accounts.json` | 用户名 / 邮箱 / quotaStart / quotaAfterCheckin / 分组 |
| `proxies.txt` | 你的出口——已被 git 忽略 |
| `outputs/` | 运行日志——已被 git 忽略 |

## 🔌 连接到 9Router

[9Router](https://9router.com) 是一个自托管、兼容 OpenAI 的路由器：

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

默认值：`--prefix 24hoursapi`、`--base-url https://24hoursapi.com/v1`、`--model qwen-plus-latest`。模型以 `24hoursapi/<model id>` 形式提供。

## 🔌 连接到 OmniRoute

[OmniRoute](https://omniroute.online) 是一个自托管、兼容 OpenAI 的路由器：

```bash
node src/connect-omniroute.js --base http://localhost:20128 --password 123456
```

默认值：`--prefix 24hoursapi`、`--base-url https://24hoursapi.com/v1`、`--model qwen-plus-latest`。模型以 `24hoursapi/<model id>` 形式提供。

## 🔑 一个密钥能带给你什么

| | |
| --- | --- |
| 🏢 提供方 | **24hoursAPI** — [24hoursapi.com](https://24hoursapi.com) |
| 🎁 提供方赠送 | 注册时 **$2（1,000,000 单位）** + 每日签到 |
| 🔑 一个密钥能带给你什么 | 在其自身账户上的一个密钥，位于 `qwen 官直0.8` / `官转 0.8`，已通过 chat 验证 |
| 🌐 API 形态 | 兼容 OpenAI：`POST https://24hoursapi.com/v1/chat/completions` |
| 🔒 你的数据 | 仅会联系 `24hoursapi.com` 和你的收件箱后端 |

## 🐞 问题

如果 24hoursAPI 关闭注册、更改其分组或调整签到奖励，本工具会在此停止并如实报告。请**[提交 issue](../../issues)**，附上命令和确切的输出。

## 📄 许可证

MIT © 2026 — 参见 [LICENSE](LICENSE)。

<div align="center">

**[MIT](LICENSE)** © 2026 24hoursapi-bulk-apikey contributors

</div>