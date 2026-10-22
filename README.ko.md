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

**자동 생성된 API 키로 24hoursAPI 계정을 대량 생성 — OpenAI 호환 추론 + 9Router 커넥터.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-61%20served-412991?style=for-the-badge)](https://24hoursapi.com)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 빠른 시작](#-quick-start) · [🎁 무료 티어](#-free-tier--how-much-you-get) · [🤖 모델](#-models--all-61-of-them) · [🧪 API 사용하기](#-how-to-use-the-api) · [✅ 검증됨](#-verified-working--every-key-is-tested) · [🧭 작동 방식](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ 옵션](#️-cli-options) · [🛰️ 프록시](#️-behind-a-proxy) · [🐞 이슈](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ 이 도구가 존재하는 이유

24hoursAPI는 **모든 신규 계정에 1,000,000 할당량 단위(≈ $2, qpu=500,000 기준)** 를 지급하며, 여기에 매일 출석 체크(+2,000…+9,752 측정됨)가 더해집니다. 패널의 `default` 그룹은 **더 이상 사용되지 않습니다**(`403 分组 default 已被弃用`) — 활성 채널은 **`qwen 官直0.8`**(`qwen-plus-latest`)과 **`官转 0.8`**(`deepseek-v4.1-flash`, 추론 모델)에 있습니다. 가입은 이메일 + 코드 방식이며 캡차가 없습니다. 패널은 관리자 측에서 주기적으로 가입을 닫으며, 이 도구는 그런 경우 이를 알려줍니다.

## 🎁 무료 티어 — 얼마나 받을 수 있나

| 항목 | 값 |
| --- | --- |
| 가입 크레딧 | 계정당 **1,000,000 단위 ≈ $2** (52개 계정 모두에서 측정됨; qpu=500,000) |
| 매일 출석 체크 | **+2,000 … +9,752 단위/일** (해당 레인의 52개 계정 중 50개에서 지급됨) |
| 작동하는 그룹 | **`qwen 官直0.8`** → `qwen-plus-latest` · **`官转 0.8`** → `deepseek-v4.1-flash` |
| `default` 그룹 | **이 포크에서는 더 이상 사용되지 않음** (`403 分组 default 已被弃用`) — 키는 활성 그룹에서 생성해야 함 |
| 가입 | 이메일 + 6자리 코드, **캡차 없음**; 관리자가 가입을 닫을 수 있음 (`New user registration has been disabled by administrator`) — CLI가 정확한 본문을 보고함 |
| 카탈로그 | 라이브 가격표에 61개 모델 항목 |

> 💡 무료 티어는 24hoursAPI가 정하며 변경될 수 있습니다. 실시간 수치는 [24hoursapi.com](https://24hoursapi.com)에서 확인하세요 — 그리고 도구 자체의 게이트가 판단하게 하세요:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 모델 — 61개 전부

라이브 카탈로그에는 **61개 모델 항목**이 있습니다. 각자의 그룹에서 검증된 두 라인:

| 모델 ID | 그룹 | 검증됨 |
| --- | --- | --- |
| `qwen-plus-latest` | `qwen 官直0.8` | 발급 중 **49개 키** chat-200; 독립 재확인 **3/3** |
| `deepseek-v4.1-flash` | `官转 0.8` | **3개 키** chat-200 (추론 모델 — 콘텐츠가 `reasoning_content`에 있을 수 있음) |

```bash
curl https://24hoursapi.com/v1/chat/completions \
  -H "Authorization: Bearer JnSI3L…" -H "Content-Type: application/json" \
  -d '{"model":"qwen-plus-latest","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 API 사용 방법

Base URL **`https://24hoursapi.com/v1`** — OpenAI 호환.

```python
from openai import OpenAI
c = OpenAI(base_url="https://24hoursapi.com/v1", api_key="JnSI3L…")
print(c.chat.completions.create(model="qwen-plus-latest",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 빠른 시작

```bash
cd 24hoursapi-bulk-apikey
printf 'socks5://host:port\n' > proxies.txt   # optional — without it the tool runs direct
node src/launcher.js        # interactive: mint / verify / screen / settings

# or: node src/cli.js --count 30    (check-in runs before the chat gate)
```

키는 `/v1/chat/completions`가 실제 콘텐츠와 함께 200을 응답한 **후에만** `keys.txt`에 기록됩니다. `accounts.json`은 사용자 이름 / 이메일 / quotaStart → quotaAfterCheckin / 그룹을 보관합니다.

### 🖱️ 런처

런처는 프록시 목록(선택 사항 — 없으면 CLI는 직접 연결로 실행됨)과 개수를 묻고, 이를 기억한 뒤 CLI를 실행합니다 — 발급, 검증, 출구 스크리닝, 설정, 도움말, 9Router 연결.

## ✅ 작동 검증됨 — 모든 키가 테스트됨

| 검사 | 결과 |
| --- | --- |
| `GET /api/status` | **200** — 이메일 인증 켜짐, 출석 체크 켜짐, 캡차 꺼짐 (이 포크는 `register_enabled`를 공개하지 않음; 거부 본문이 신호임) |
| `POST /api/user/checkin` | **200** — 할당량이 1,000,000 → 최대 1,009,752로 이동 (52개 계정 중 50개) |
| `qwen 官直0.8` / `官转 0.8`에서 `POST /api/token/` | **200** — 키가 한 번 반환됨 |
| 기록되기 전 각 키에 대한 `POST /v1/chat/completions` | **200** — 49× `qwen-plus-latest` + 3× `deepseek-v4.1-flash` |
| 독립 재확인 (새 출구, 샘플) | **3/3** 키 `GET /v1/models` 200 + `qwen-plus-latest` 완성 200 |
| 패널 측 장애 (재구축 중 관찰됨) | 채팅 호출이 `500 Database error, please contact the administrator`를 응답함 — **패널 측** 실패; 패널이 정상일 때 `--verify-only`를 다시 실행하세요 |

```bash
node src/cli.js --verify-only
node src/cli.js --screen
```

## 🧭 작동 방식

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: 24hoursAPI page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://24hoursapi.com/v1/chat/completions → real content = the key is live
```

| 단계 | 호출 | 비고 |
| --- | --- | --- |
| status | `GET /api/status` | 메일 + 출석 체크 게이트; 이 포크에서는 register 플래그가 공개되지 않음 |
| mailbox | Emailnator `POST /api/generate-email` | 플릿의 Mailtrap 한도 이후, gmail이 레일임 |
| send code | `GET /api/verification?email=…` | 6자리 코드 |
| register | `POST /api/user/register` | 관리자가 가입을 닫으면 본문이 그대로 알려줌 |
| login | `POST /api/user/login` | 세션 토큰 |
| **check-in** | `POST /api/user/checkin` | $2 가입 크레딧 위에 매일 보너스 |
| key | `qwen 官直0.8`에서 `POST /api/token/` (대체 `官转 0.8`) | 그룹 순회로 빈 기본 그룹 배정을 잡아냄 |
| proof | `POST /v1/chat/completions` | 콘텐츠와 함께 200 = 키가 활성 상태 |

## ⚙️ CLI 옵션

```bash
node src/cli.js --count N        # mint N keys (check-in + chat gate per account)
node src/cli.js --group "qwen 官直0.8" --alt-groups "官转 0.8"
node src/cli.js --verify-only    # re-verify every key in keys.txt
node src/cli.js --screen         # exit pool self-test
node src/cli.js --connect-9router
node src/cli.js --help
```

| 환경 변수 | 기본값 | 의미 |
| --- | --- | --- |
| `H24API_PROXIES` | `proxies.txt` | 출구 풀, 프록시당 한 줄 — 선택 사항: 풀이 없으면 CLI는 직접 연결 |
| `H24API_MODEL` | `qwen-plus-latest` | 라이브 검증에 사용할 모델 |

## 🛰️ 프록시 뒤에서

**선택 사항 — 대규모에서는 권장됨 (IP당 제한, 차단).** 이 도구는 프록시 없이도 작동합니다.

- **풀이 있는 경우:** `--pool <file>`을 전달하거나 `H24API_PROXIES`를 설정하면 모든 요청이 해당 목록의 출구를 통해 나갑니다 — 죽은 회선은 해당 시도를 실패시키고 CLI는 다음 출구로 재시도합니다 (register/login은 속도 제한이 있음: 버스트 시 `login 429`).
- **풀이 없는 경우:** CLI는 `running WITHOUT a proxy (direct)`를 출력하고 직접 연결합니다. 가입, 출석 체크, 키 발급, 검증에는 아무 변화가 없습니다 — 패킷이 나가는 위치만 달라집니다.
- 어느 모드에서든 `--screen`을 실행하세요: 실제 실행 전에 보유한 출구(또는 직접 경로)를 24hoursAPI에 대해 테스트합니다.

## 📄 파일

| 경로 | 내용 |
| --- | --- |
| `keys.txt` | 라이브 검증된 키 한 줄에 하나 — git-ignored |
| `accounts.json` | 사용자 이름 / 이메일 / quotaStart / quotaAfterCheckin / 그룹 |
| `proxies.txt` | 사용자의 출구 — git-ignored |
| `outputs/` | 실행 로그 — git-ignored |

## 🔌 9Router에 연결

[9Router](https://9router.com)는 자체 호스팅, OpenAI 호환 라우터입니다:

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

기본값: `--prefix 24hoursapi`, `--base-url https://24hoursapi.com/v1`, `--model qwen-plus-latest`. 모델은 `24hoursapi/<model id>`로 제공됩니다.

## 🔑 키 하나로 얻는 것

| | |
| --- | --- |
| 🏢 제공자 | **24hoursAPI** — [24hoursapi.com](https://24hoursapi.com) |
| 🎁 제공자가 주는 것 | 가입 시 **$2 (1,000,000 단위)** + 매일 출석 체크 |
| 🔑 키 하나로 얻는 것 | `qwen 官直0.8` / `官转 0.8`에서 자체 계정의 키 하나, 채팅 검증됨 |
| 🌐 API 형태 | OpenAI 호환: `POST https://24hoursapi.com/v1/chat/completions` |
| 🔒 데이터 | `24hoursapi.com`과 사용자의 받은편지함 백엔드만 접촉됨 |

## 🐞 이슈

24hoursAPI가 가입을 닫거나, 그룹을 변경하거나, 출석 체크 보상을 교체하면 도구는 그 지점에서 멈추고 그 사실을 알려줍니다. 명령과 정확한 출력을 포함하여 **[이슈를 열어주세요](../../issues)**.

## 📄 라이선스

MIT © 2026 — [LICENSE](LICENSE) 참조.

<div align="center">

**[MIT](LICENSE)** © 2026 24hoursapi-bulk-apikey contributors

</div>