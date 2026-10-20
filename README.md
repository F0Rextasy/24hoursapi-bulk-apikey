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

**Bulk-create 24hoursAPI accounts with auto-generated API keys — OpenAI-compatible inference + 9Router connector.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-61%20served-412991?style=for-the-badge)](https://24hoursapi.com)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Quick start](#-quick-start) · [🎁 Free tier](#-free-tier--how-much-you-get) · [🤖 Models](#-models--all-61-of-them) · [🧪 Use the API](#-how-to-use-the-api) · [✅ Verified](#-verified-working--every-key-is-tested) · [🧭 How it works](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ Options](#️-cli-options) · [🛰️ Proxies](#️-behind-a-proxy) · [🐞 Issues](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Why this exists

24hoursAPI hands out **1,000,000 quota units (≈ $2 at qpu=500,000) to every new account** plus a daily check-in (+2,000…+9,752 measured). The panel's `default` group is **deprecated** (`403 分组 default 已被弃用`) — its live channels sit in **`qwen 官直0.8`** (`qwen-plus-latest`) and **`官转 0.8`** (`deepseek-v4.1-flash`, reasoning model). Registration is e-mail + code, captcha-free; the panel periodically closes signups from the admin side, and this tool says so when it happens.

## 🎁 Free tier — how much you get

| What | Value |
| --- | --- |
| Signup credit | **1,000,000 units ≈ $2** per account (measured on all 52 accounts; qpu=500,000) |
| Daily check-in | **+2,000 … +9,752 units/day** (paid on 50 of 52 accounts in the lane) |
| Working groups | **`qwen 官直0.8`** → `qwen-plus-latest` · **`官转 0.8`** → `deepseek-v4.1-flash` |
| `default` group | **deprecated on this fork** (`403 分组 default 已被弃用`) — keys must be created in a live group |
| Signup | e-mail + 6-char code, **no captcha**; registration can be closed by the admin (`New user registration has been disabled by administrator`) — the CLI reports the exact body |
| Catalogue | 61 model entries on the live pricing sheet |

> 💡 The free tier is set by 24hoursAPI and can change. Check the live numbers at [24hoursapi.com](https://24hoursapi.com) — and let the tool's own gate decide:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Models — all 61 of them

The live catalogue carries **61 model entries**. Both verified lines, on their own groups:

| Model ID | Group | Verified |
| --- | --- | --- |
| `qwen-plus-latest` | `qwen 官直0.8` | **49 keys** chat-200 during the mint; independent re-check **3/3** |
| `deepseek-v4.1-flash` | `官转 0.8` | **3 keys** chat-200 (reasoning model — content may sit in `reasoning_content`) |

```bash
curl https://24hoursapi.com/v1/chat/completions \
  -H "Authorization: Bearer JnSI3L…" -H "Content-Type: application/json" \
  -d '{"model":"qwen-plus-latest","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 How to use the API

Base URL **`https://24hoursapi.com/v1`** — OpenAI-compatible.

```python
from openai import OpenAI
c = OpenAI(base_url="https://24hoursapi.com/v1", api_key="JnSI3L…")
print(c.chat.completions.create(model="qwen-plus-latest",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 Quick start

```bash
cd 24hoursapi-bulk-apikey
printf 'socks5://host:port\n' > proxies.txt   # optional — without it the tool runs direct
node src/launcher.js        # interactive: mint / verify / screen / settings
# or: node src/cli.js --count 30    (check-in runs before the chat gate)
```

A key is written to `keys.txt` **only** after `/v1/chat/completions` answers 200 with real content; `accounts.json` keeps username / e-mail / quotaStart → quotaAfterCheckin / group.

### 🖱️ The launcher

The launcher asks for the proxy list (optional — without one the CLI runs direct) and count, remembers them, and runs the CLI — mint, verify, screen exits, settings, help, Connect to 9Router.

## ✅ Verified working — every key is tested

| Check | Result |
| --- | --- |
| `GET /api/status` | **200** — e-mail verification on, check-in on, captcha off (this fork does not publish `register_enabled`; the refusal body is the signal) |
| `POST /api/user/checkin` | **200** — quota moves 1,000,000 → up to 1,009,752 (50/52 accounts) |
| `POST /api/token/` in `qwen 官直0.8` / `官转 0.8` | **200** — keys returned once |
| `POST /v1/chat/completions` per key before it is written | **200** — 49× `qwen-plus-latest` + 3× `deepseek-v4.1-flash` |
| independent re-check (fresh exit, sample) | **3/3** keys `GET /v1/models` 200 + `qwen-plus-latest` completion 200 |
| panel-side outage (observed during rebuild) | chat calls answered `500 Database error, please contact the administrator` — a **panel-side** failure; re-run `--verify-only` when the panel is healthy |

```bash
node src/cli.js --verify-only
node src/cli.js --screen
```

## 🧭 How it works

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: 24hoursAPI page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://24hoursapi.com/v1/chat/completions → real content = the key is live
```

| Step | Call | Notes |
| --- | --- | --- |
| status | `GET /api/status` | mail + check-in gates; register flag unpublished on this fork |
| mailbox | Emailnator `POST /api/generate-email` | after the fleet's Mailtrap cap, gmail is the rail |
| send code | `GET /api/verification?email=…` | 6-char code |
| register | `POST /api/user/register` | when the admin closes signups, the body says so verbatim |
| login | `POST /api/user/login` | session token |
| **check-in** | `POST /api/user/checkin` | daily bonus on top of the $2 signup credit |
| key | `POST /api/token/` in `qwen 官直0.8` (fallback `官转 0.8`) | group-walk catches empty primary-group rolls |
| proof | `POST /v1/chat/completions` | 200 with content = the key is live |

## ⚙️ CLI options

```bash
node src/cli.js --count N        # mint N keys (check-in + chat gate per account)
node src/cli.js --group "qwen 官直0.8" --alt-groups "官转 0.8"
node src/cli.js --verify-only    # re-verify every key in keys.txt
node src/cli.js --screen         # exit pool self-test
node src/cli.js --connect-9router
node src/cli.js --help
```

| Env | Default | Meaning |
| --- | --- | --- |
| `H24API_PROXIES` | `proxies.txt` | exit pool, one line per proxy — optional: without a pool the CLI connects directly |
| `H24API_MODEL` | `qwen-plus-latest` | model for the live verification |

## 🛰️ Behind a proxy

**Optional — recommended at scale (per-IP limits, bans).** The tool works without a proxy too.

- **With a pool:** pass `--pool <file>` (or set `H24API_PROXIES`) and every request goes through an exit from that list — a dead line fails that attempt and the CLI retries with the next exit (register/login are rate-limited: `login 429` under bursts).
- **Without a pool:** the CLI prints `running WITHOUT a proxy (direct)` and connects directly. Nothing about sign-up, check-in, key minting or verification changes — only where the packets leave from.
- Run `--screen` against either mode: it tests the exits you have (or the direct route) against 24hoursAPI before a real run.

## 📄 Files

| Path | Contents |
| --- | --- |
| `keys.txt` | one live-verified key per line — git-ignored |
| `accounts.json` | username / e-mail / quotaStart / quotaAfterCheckin / group |
| `proxies.txt` | your exits — git-ignored |
| `outputs/` | run logs — git-ignored |

## 🔌 Connect to 9Router

[9Router](https://9router.com) is a self-hosted, OpenAI-compatible router:

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

Defaults: `--prefix 24hoursapi`, `--base-url https://24hoursapi.com/v1`, `--model qwen-plus-latest`. Models are served as `24hoursapi/<model id>`.

## 🔑 What one key gets you

| | |
| --- | --- |
| 🏢 Provider | **24hoursAPI** — [24hoursapi.com](https://24hoursapi.com) |
| 🎁 What the provider gives | **$2 (1,000,000 units)** on signup + daily check-in |
| 🔑 What one key gets you | one key on its own account in `qwen 官直0.8` / `官转 0.8`, chat-verified |
| 🌐 API shape | OpenAI-compatible: `POST https://24hoursapi.com/v1/chat/completions` |
| 🔒 Your data | only `24hoursapi.com` and your inbox backend are contacted |

## 🐞 Issues

If 24hoursAPI closes registration, changes its groups or rotates the check-in reward, the tool stops there and says so. **[Open an issue](../../issues)** with the command and the exact output.

## 📄 License

MIT © 2026 — see [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 24hoursapi-bulk-apikey contributors

</div>
