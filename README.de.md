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

**Massenhaftes Erstellen von 24hoursAPI-Konten mit automatisch generierten API-Schlüsseln — OpenAI-kompatible Inferenz + 9Router-Connector.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-61%20served-412991?style=for-the-badge)](https://24hoursapi.com)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Schnellstart](#-quick-start) · [🎁 Kostenlose Stufe](#-free-tier--how-much-you-get) · [🤖 Modelle](#-models--all-61-of-them) · [🧪 Die API verwenden](#-how-to-use-the-api) · [✅ Verifiziert](#-verified-working--every-key-is-tested) · [🧭 So funktioniert es](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ Optionen](#️-cli-options) · [🛰️ Proxys](#️-behind-a-proxy) · [🐞 Issues](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Warum es das gibt

24hoursAPI verteilt **1.000.000 Quota-Einheiten (≈ 2 $ bei qpu=500.000) an jedes neue Konto** plus einen täglichen Check-in (+2.000…+9.752 gemessen). Die `default`-Gruppe des Panels ist **veraltet** (`403 分组 default 已被弃用`) — ihre aktiven Kanäle liegen in **`qwen 官直0.8`** (`qwen-plus-latest`) und **`官转 0.8`** (`deepseek-v4.1-flash`, Reasoning-Modell). Die Registrierung erfolgt per E-Mail + Code, ohne Captcha; das Panel schließt Anmeldungen zeitweise von der Admin-Seite aus, und dieses Tool sagt es, wenn das passiert.

## 🎁 Kostenlose Stufe — wie viel du bekommst

| Was | Wert |
| --- | --- |
| Anmeldeguthaben | **1.000.000 Einheiten ≈ 2 $** pro Konto (gemessen an allen 52 Konten; qpu=500.000) |
| Täglicher Check-in | **+2.000 … +9.752 Einheiten/Tag** (ausgezahlt an 50 von 52 Konten im Lauf) |
| Funktionierende Gruppen | **`qwen 官直0.8`** → `qwen-plus-latest` · **`官转 0.8`** → `deepseek-v4.1-flash` |
| `default`-Gruppe | **in diesem Fork veraltet** (`403 分组 default 已被弃用`) — Schlüssel müssen in einer aktiven Gruppe erstellt werden |
| Registrierung | E-Mail + 6-stelliger Code, **kein Captcha**; die Registrierung kann vom Admin geschlossen werden (`New user registration has been disabled by administrator`) — die CLI meldet den genauen Body |
| Katalog | 61 Modelleinträge auf dem aktiven Preisblatt |

> 💡 Die kostenlose Stufe wird von 24hoursAPI festgelegt und kann sich ändern. Prüfe die aktuellen Zahlen auf [24hoursapi.com](https://24hoursapi.com) — und lass das Gate des Tools selbst entscheiden:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Modelle — alle 61

Der aktive Katalog enthält **61 Modelleinträge**. Beide verifizierten Zeilen, in ihren eigenen Gruppen:

| Modell-ID | Gruppe | Verifiziert |
| --- | --- | --- |
| `qwen-plus-latest` | `qwen 官直0.8` | **49 Schlüssel** chat-200 während des Mints; unabhängige Nachprüfung **3/3** |
| `deepseek-v4.1-flash` | `官转 0.8` | **3 Schlüssel** chat-200 (Reasoning-Modell — Inhalt kann in `reasoning_content` liegen) |

```bash
curl https://24hoursapi.com/v1/chat/completions \
  -H "Authorization: Bearer JnSI3L…" -H "Content-Type: application/json" \
  -d '{"model":"qwen-plus-latest","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 So verwendest du die API

Basis-URL **`https://24hoursapi.com/v1`** — OpenAI-kompatibel.

```python
from openai import OpenAI
c = OpenAI(base_url="https://24hoursapi.com/v1", api_key="JnSI3L…")
print(c.chat.completions.create(model="qwen-plus-latest",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 Schnellstart

```bash
cd 24hoursapi-bulk-apikey
printf 'socks5://host:port\n' > proxies.txt   # optional — without it the tool runs direct
node src/launcher.js        # interactive: mint / verify / screen / settings
# or: node src/cli.js --count 30    (check-in runs before the chat gate)
```

Ein Schlüssel wird **nur** dann in `keys.txt` geschrieben, wenn `/v1/chat/completions` mit 200 und echtem Inhalt antwortet; `accounts.json` speichert Benutzername / E-Mail / quotaStart → quotaAfterCheckin / Gruppe.

### 🖱️ Der Launcher

Der Launcher fragt nach der Proxy-Liste (optional — ohne eine läuft die CLI direkt) und der Anzahl, merkt sie sich und führt die CLI aus — Mint, Verify, Screen-Exits, Einstellungen, Hilfe, Connect to 9Router.

## ✅ Verifiziert funktionierend — jeder Schlüssel wird getestet

| Prüfung | Ergebnis |
| --- | --- |
| `GET /api/status` | **200** — E-Mail-Verifizierung an, Check-in an, Captcha aus (dieser Fork veröffentlicht kein `register_enabled`; der Ablehnungs-Body ist das Signal) |
| `POST /api/user/checkin` | **200** — Quota bewegt sich von 1.000.000 → bis zu 1.009.752 (50/52 Konten) |
| `POST /api/token/` in `qwen 官直0.8` / `官转 0.8` | **200** — Schlüssel werden einmalig zurückgegeben |
| `POST /v1/chat/completions` pro Schlüssel, bevor er geschrieben wird | **200** — 49× `qwen-plus-latest` + 3× `deepseek-v4.1-flash` |
| unabhängige Nachprüfung (frischer Exit, Stichprobe) | **3/3** Schlüssel `GET /v1/models` 200 + `qwen-plus-latest`-Completion 200 |
| Ausfall auf Panel-Seite (während des Rebuilds beobachtet) | Chat-Aufrufe antworteten mit `500 Database error, please contact the administrator` — ein **panel-seitiger** Fehler; führe `--verify-only` erneut aus, wenn das Panel gesund ist |

```bash
node src/cli.js --verify-only
node src/cli.js --screen
```

## 🧭 So funktioniert es

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: 24hoursAPI page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://24hoursapi.com/v1/chat/completions → real content = the key is live
```

| Schritt | Aufruf | Hinweise |
| --- | --- | --- |
| Status | `GET /api/status` | Mail- + Check-in-Gates; Register-Flag in diesem Fork nicht veröffentlicht |
| Mailbox | Emailnator `POST /api/generate-email` | nach dem Mailtrap-Limit der Flotte ist Gmail die Schiene |
| Code senden | `GET /api/verification?email=…` | 6-stelliger Code |
| Registrieren | `POST /api/user/register` | wenn der Admin Anmeldungen schließt, sagt der Body es wörtlich |
| Anmelden | `POST /api/user/login` | Session-Token |
| **Check-in** | `POST /api/user/checkin` | Tagesbonus zusätzlich zum 2-$-Anmeldeguthaben |
| Schlüssel | `POST /api/token/` in `qwen 官直0.8` (Fallback `官转 0.8`) | Gruppen-Durchlauf fängt leere Rolls der Primärgruppe ab |
| Nachweis | `POST /v1/chat/completions` | 200 mit Inhalt = der Schlüssel ist aktiv |

## ⚙️ CLI-Optionen

```bash
node src/cli.js --count N        # mint N keys (check-in + chat gate per account)
node src/cli.js --group "qwen 官直0.8" --alt-groups "官转 0.8"
node src/cli.js --verify-only    # re-verify every key in keys.txt
node src/cli.js --screen         # exit pool self-test
node src/cli.js --connect-9router
node src/cli.js --help
```

| Env | Standard | Bedeutung |
| --- | --- | --- |
| `H24API_PROXIES` | `proxies.txt` | Exit-Pool, eine Zeile pro Proxy — optional: ohne Pool verbindet sich die CLI direkt |
| `H24API_MODEL` | `qwen-plus-latest` | Modell für die Live-Verifizierung |

## 🛰️ Hinter einem Proxy

**Optional — bei Skalierung empfohlen (Limits pro IP, Bans).** Das Tool funktioniert auch ohne Proxy.

- **Mit einem Pool:** übergib `--pool <file>` (oder setze `H24API_PROXIES`), und jede Anfrage geht über einen Exit aus dieser Liste — eine tote Leitung lässt diesen Versuch fehlschlagen, und die CLI versucht es mit dem nächsten Exit erneut (Register/Login sind rate-limitiert: `login 429` bei Bursts).
- **Ohne einen Pool:** die CLI gibt `running WITHOUT a proxy (direct)` aus und verbindet sich direkt. An Registrierung, Check-in, Schlüssel-Minting oder Verifizierung ändert sich nichts — nur daran, wo die Pakete rausgehen.
- Führe `--screen` gegen beide Modi aus: Es testet die Exits, die du hast (oder die direkte Route), gegen 24hoursAPI vor einem echten Lauf.

## 📄 Dateien

| Pfad | Inhalt |
| --- | --- |
| `keys.txt` | ein live-verifizierter Schlüssel pro Zeile — von Git ignoriert |
| `accounts.json` | Benutzername / E-Mail / quotaStart / quotaAfterCheckin / Gruppe |
| `proxies.txt` | deine Exits — von Git ignoriert |
| `outputs/` | Lauf-Logs — von Git ignoriert |

## 🔌 Mit 9Router verbinden

[9Router](https://9router.com) ist ein selbst gehosteter, OpenAI-kompatibler Router:

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

Standardwerte: `--prefix 24hoursapi`, `--base-url https://24hoursapi.com/v1`, `--model qwen-plus-latest`. Modelle werden als `24hoursapi/<model id>` bereitgestellt.

## 🔑 Was ein Schlüssel dir bringt

| | |
| --- | --- |
| 🏢 Anbieter | **24hoursAPI** — [24hoursapi.com](https://24hoursapi.com) |
| 🎁 Was der Anbieter gibt | **2 $ (1.000.000 Einheiten)** bei Anmeldung + täglicher Check-in |
| 🔑 Was ein Schlüssel dir bringt | ein Schlüssel auf einem eigenen Konto in `qwen 官直0.8` / `官转 0.8`, chat-verifiziert |
| 🌐 API-Form | OpenAI-kompatibel: `POST https://24hoursapi.com/v1/chat/completions` |
| 🔒 Deine Daten | nur `24hoursapi.com` und dein Inbox-Backend werden kontaktiert |

## 🐞 Issues

Wenn 24hoursAPI die Registrierung schließt, seine Gruppen ändert oder die Check-in-Belohnung rotiert, stoppt das Tool dort und sagt es. **[Öffne ein Issue](../../issues)** mit dem Befehl und der genauen Ausgabe.

## 📄 Lizenz

MIT © 2026 — siehe [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 24hoursapi-bulk-apikey contributors

</div>