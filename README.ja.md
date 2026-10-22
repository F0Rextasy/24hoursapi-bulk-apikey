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

**自動生成された API キーで 24hoursAPI アカウントを一括作成 — OpenAI 互換の推論 + 9Router コネクタ。**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-61%20served-412991?style=for-the-badge)](https://24hoursapi.com)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 クイックスタート](#-quick-start) · [🎁 無料枠](#-free-tier--how-much-you-get) · [🤖 モデル](#-models--all-61-of-them) · [🧪 API の使い方](#-how-to-use-the-api) · [✅ 検証済み](#-verified-working--every-key-is-tested) · [🧭 仕組み](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ オプション](#️-cli-options) · [🛰️ プロキシ](#️-behind-a-proxy) · [🐞 問題](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ これが存在する理由

24hoursAPI は**新規アカウントごとに 1,000,000 クォータユニット（qpu=500,000 で約 $2）**を付与し、さらに毎日のチェックイン（+2,000…+9,752 を計測）も提供します。パネルの `default` グループは**非推奨**（`403 分组 default 已被弃用`）で、稼働中のチャネルは **`qwen 官直0.8`**（`qwen-plus-latest`）と **`官转 0.8`**（`deepseek-v4.1-flash`、推論モデル）にあります。登録はメールアドレス + コードで、キャプチャはありません。パネルは管理者側で定期的に新規登録を締め切ることがあり、その場合はこのツールがその旨を通知します。

## 🎁 無料枠 — どれだけ得られるか

| 項目 | 値 |
| --- | --- |
| 登録クレジット | **1,000,000 ユニット ≈ $2** / アカウント（全 52 アカウントで計測、qpu=500,000） |
| 毎日のチェックイン | **+2,000 … +9,752 ユニット/日**（レーンの 52 アカウント中 50 で付与） |
| 稼働中のグループ | **`qwen 官直0.8`** → `qwen-plus-latest` · **`官转 0.8`** → `deepseek-v4.1-flash` |
| `default` グループ | **このフォークでは非推奨**（`403 分组 default 已被弃用`）— キーは稼働中のグループで作成する必要があります |
| 登録 | メールアドレス + 6 文字コード、**キャプチャなし**。管理者が登録を締め切ることがあります（`New user registration has been disabled by administrator`）— CLI は正確なレスポンスボディを報告します |
| カタログ | 稼働中の料金表に 61 のモデルエントリ |

> 💡 無料枠は 24hoursAPI が設定するもので、変更される可能性があります。最新の数値は [24hoursapi.com](https://24hoursapi.com) で確認してください — そしてツール自身のゲートに判断させましょう:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 モデル — 全 61 種

稼働中のカタログには **61 のモデルエントリ**があります。検証済みの 2 系統を、それぞれのグループで示します:

| モデル ID | グループ | 検証済み |
| --- | --- | --- |
| `qwen-plus-latest` | `qwen 官直0.8` | ミント時に **49 キー**が chat-200、独立再チェック **3/3** |
| `deepseek-v4.1-flash` | `官转 0.8` | **3 キー**が chat-200（推論モデル — コンテンツは `reasoning_content` にある場合があります） |

```bash
curl https://24hoursapi.com/v1/chat/completions \
  -H "Authorization: Bearer JnSI3L…" -H "Content-Type: application/json" \
  -d '{"model":"qwen-plus-latest","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 API の使い方

ベース URL **`https://24hoursapi.com/v1`** — OpenAI 互換。

```python
from openai import OpenAI
c = OpenAI(base_url="https://24hoursapi.com/v1", api_key="JnSI3L…")
print(c.chat.completions.create(model="qwen-plus-latest",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 クイックスタート

```bash
cd 24hoursapi-bulk-apikey
printf 'socks5://host:port\n' > proxies.txt   # optional — without it the tool runs direct
node src/launcher.js        # interactive: mint / verify / screen / settings

# or: node src/cli.js --count 30    (check-in runs before the chat gate)
```

キーは `/v1/chat/completions` が実際のコンテンツとともに 200 を返した**後にのみ** `keys.txt` に書き込まれます。`accounts.json` には username / e-mail / quotaStart → quotaAfterCheckin / group が保持されます。

### 🖱️ ランチャー

ランチャーはプロキシリスト（任意 — 指定しない場合 CLI は直接実行）と件数を尋ね、それらを記憶して CLI を実行します — mint、verify、screen exits、settings、help、Connect to 9Router。

## ✅ 検証済み — すべてのキーがテストされます

| チェック | 結果 |
| --- | --- |
| `GET /api/status` | **200** — メール検証オン、チェックインオン、キャプチャオフ（このフォークは `register_enabled` を公開しません。拒否時のレスポンスボディがシグナルです） |
| `POST /api/user/checkin` | **200** — クォータが 1,000,000 → 最大 1,009,752 に増加（50/52 アカウント） |
| `qwen 官直0.8` / `官转 0.8` での `POST /api/token/` | **200** — キーは一度だけ返されます |
| 書き込み前の各キーに対する `POST /v1/chat/completions` | **200** — 49× `qwen-plus-latest` + 3× `deepseek-v4.1-flash` |
| 独立再チェック（新しい出口、サンプル） | **3/3** キーが `GET /v1/models` 200 + `qwen-plus-latest` 補完 200 |
| パネル側の障害（再構築中に観測） | チャット呼び出しが `500 Database error, please contact the administrator` を返す — **パネル側**の障害です。パネルが正常なときに `--verify-only` を再実行してください |

```bash
node src/cli.js --verify-only
node src/cli.js --screen
```

## 🧭 仕組み

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: 24hoursAPI page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://24hoursapi.com/v1/chat/completions → real content = the key is live
```

| ステップ | 呼び出し | 備考 |
| --- | --- | --- |
| status | `GET /api/status` | メール + チェックインのゲート。このフォークでは register フラグは非公開 |
| mailbox | Emailnator `POST /api/generate-email` | フリートの Mailtrap 上限後、gmail がレールとなります |
| send code | `GET /api/verification?email=…` | 6 文字コード |
| register | `POST /api/user/register` | 管理者が登録を締め切ると、レスポンスボディがそのまま示します |
| login | `POST /api/user/login` | セッショントークン |
| **check-in** | `POST /api/user/checkin` | $2 の登録クレジットに加えて毎日のボーナス |
| key | `qwen 官直0.8` での `POST /api/token/`（フォールバック `官转 0.8`） | グループウォークがプライマリグループの空ロールを捕捉します |
| proof | `POST /v1/chat/completions` | コンテンツ付きの 200 = キーが稼働中 |

## ⚙️ CLI オプション

```bash
node src/cli.js --count N        # mint N keys (check-in + chat gate per account)
node src/cli.js --group "qwen 官直0.8" --alt-groups "官转 0.8"
node src/cli.js --verify-only    # re-verify every key in keys.txt
node src/cli.js --screen         # exit pool self-test
node src/cli.js --connect-9router
node src/cli.js --help
```

| 環境変数 | デフォルト | 意味 |
| --- | --- | --- |
| `H24API_PROXIES` | `proxies.txt` | 出口プール、1 行につき 1 プロキシ — 任意: プールがなければ CLI は直接接続します |
| `H24API_MODEL` | `qwen-plus-latest` | ライブ検証に使用するモデル |

## 🛰️ プロキシの背後で

**任意 — 規模が大きい場合に推奨（IP ごとの制限、BAN）。** このツールはプロキシなしでも動作します。

- **プールあり:** `--pool <file>` を渡す（または `H24API_PROXIES` を設定する）と、すべてのリクエストがそのリストの出口を経由します — 死んだ回線はその試行を失敗させ、CLI は次の出口で再試行します（register/login はレート制限あり: バースト時に `login 429`）。
- **プールなし:** CLI は `running WITHOUT a proxy (direct)` と表示し、直接接続します。サインアップ、チェックイン、キーのミント、検証のいずれも変わりません — 変わるのはパケットの出発点だけです。
- どちらのモードでも `--screen` を実行してください: 実際の実行前に、利用可能な出口（または直接ルート）を 24hoursAPI に対してテストします。

## 📄 ファイル

| パス | 内容 |
| --- | --- |
| `keys.txt` | ライブ検証済みのキーを 1 行に 1 つ — git 管理外 |
| `accounts.json` | username / e-mail / quotaStart / quotaAfterCheckin / group |
| `proxies.txt` | あなたの出口 — git 管理外 |
| `outputs/` | 実行ログ — git 管理外 |

## 🔌 9Router に接続

[9Router](https://9router.com) はセルフホスト型の OpenAI 互換ルーターです:

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

デフォルト: `--prefix 24hoursapi`、`--base-url https://24hoursapi.com/v1`、`--model qwen-plus-latest`。モデルは `24hoursapi/<model id>` として提供されます。

## 🔑 1 つのキーで得られるもの

| | |
| --- | --- |
| 🏢 プロバイダー | **24hoursAPI** — [24hoursapi.com](https://24hoursapi.com) |
| 🎁 プロバイダーが提供するもの | 登録時に **$2（1,000,000 ユニット）** + 毎日のチェックイン |
| 🔑 1 つのキーで得られるもの | `qwen 官直0.8` / `官转 0.8` 内の専用アカウント上の 1 キー、チャット検証済み |
| 🌐 API の形式 | OpenAI 互換: `POST https://24hoursapi.com/v1/chat/completions` |
| 🔒 あなたのデータ | 接続先は `24hoursapi.com` とあなたの受信トレイバックエンドのみ |

## 🐞 問題

24hoursAPI が登録を締め切ったり、グループを変更したり、チェックイン報酬をローテーションしたりした場合、ツールはそこで停止しその旨を通知します。コマンドと正確な出力を添えて **[issue を作成してください](../../issues)**。

## 📄 ライセンス

MIT © 2026 — [LICENSE](LICENSE) を参照。

<div align="center">

**[MIT](LICENSE)** © 2026 24hoursapi-bulk-apikey contributors

</div>