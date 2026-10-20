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

**Массовое создание аккаунтов 24hoursAPI с автоматически сгенерированными API-ключами — инференс, совместимый с OpenAI, + коннектор 9Router.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-61%20served-412991?style=for-the-badge)](https://24hoursapi.com)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Быстрый старт](#-quick-start) · [🎁 Бесплатный тариф](#-free-tier--how-much-you-get) · [🤖 Модели](#-models--all-61-of-them) · [🧪 Использование API](#-how-to-use-the-api) · [✅ Проверено](#-verified-working--every-key-is-tested) · [🧭 Как это работает](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ Опции](#️-cli-options) · [🛰️ Прокси](#️-behind-a-proxy) · [🐞 Проблемы](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Зачем это нужно

24hoursAPI выдаёт **1 000 000 единиц квоты (≈ $2 при qpu=500 000) каждому новому аккаунту** плюс ежедневный чек-ин (+2 000…+9 752 по измерениям). Группа `default` в панели **устарела** (`403 分组 default 已被弃用`) — её рабочие каналы находятся в **`qwen 官直0.8`** (`qwen-plus-latest`) и **`官转 0.8`** (`deepseek-v4.1-flash`, reasoning-модель). Регистрация — e-mail + код, без капчи; панель периодически закрывает регистрацию со стороны администратора, и этот инструмент сообщает об этом, когда это происходит.

## 🎁 Бесплатный тариф — сколько вы получаете

| Что | Значение |
| --- | --- |
| Кредит при регистрации | **1 000 000 единиц ≈ $2** на аккаунт (измерено на всех 52 аккаунтах; qpu=500 000) |
| Ежедневный чек-ин | **+2 000 … +9 752 единиц/день** (начислено на 50 из 52 аккаунтов в потоке) |
| Рабочие группы | **`qwen 官直0.8`** → `qwen-plus-latest` · **`官转 0.8`** → `deepseek-v4.1-flash` |
| Группа `default` | **устарела в этом форке** (`403 分组 default 已被弃用`) — ключи должны создаваться в рабочей группе |
| Регистрация | e-mail + 6-символьный код, **без капчи**; регистрация может быть закрыта администратором (`New user registration has been disabled by administrator`) — CLI сообщает точный текст ответа |
| Каталог | 61 запись модели в актуальном прайс-листе |

> 💡 Бесплатный тариф устанавливается 24hoursAPI и может меняться. Проверяйте актуальные цифры на [24hoursapi.com](https://24hoursapi.com) — и пусть собственный гейт инструмента решает:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Модели — все 61

Актуальный каталог содержит **61 запись модели**. Обе проверенные линии, в своих группах:

| ID модели | Группа | Проверено |
| --- | --- | --- |
| `qwen-plus-latest` | `qwen 官直0.8` | **49 ключей** chat-200 во время создания; независимая перепроверка **3/3** |
| `deepseek-v4.1-flash` | `官转 0.8` | **3 ключа** chat-200 (reasoning-модель — содержимое может находиться в `reasoning_content`) |

```bash
curl https://24hoursapi.com/v1/chat/completions \
  -H "Authorization: Bearer JnSI3L…" -H "Content-Type: application/json" \
  -d '{"model":"qwen-plus-latest","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 Как использовать API

Базовый URL **`https://24hoursapi.com/v1`** — совместим с OpenAI.

```python
from openai import OpenAI
c = OpenAI(base_url="https://24hoursapi.com/v1", api_key="JnSI3L…")
print(c.chat.completions.create(model="qwen-plus-latest",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 Быстрый старт

```bash
cd 24hoursapi-bulk-apikey
printf 'socks5://host:port\n' > proxies.txt   # optional — without it the tool runs direct
node src/launcher.js        # interactive: mint / verify / screen / settings
# or: node src/cli.js --count 30    (check-in runs before the chat gate)
```

Ключ записывается в `keys.txt` **только** после того, как `/v1/chat/completions` вернёт 200 с реальным содержимым; `accounts.json` хранит username / e-mail / quotaStart → quotaAfterCheckin / group.

### 🖱️ Лаунчер

Лаунчер запрашивает список прокси (необязательно — без него CLI работает напрямую) и количество, запоминает их и запускает CLI — создание, проверка, тест выходов, настройки, справка, Connect to 9Router.

## ✅ Проверено — каждый ключ протестирован

| Проверка | Результат |
| --- | --- |
| `GET /api/status` | **200** — подтверждение e-mail включено, чек-ин включён, капча выключена (этот форк не публикует `register_enabled`; сигналом служит текст отказа) |
| `POST /api/user/checkin` | **200** — квота растёт с 1 000 000 до 1 009 752 (50/52 аккаунтов) |
| `POST /api/token/` в `qwen 官直0.8` / `官转 0.8` | **200** — ключи возвращаются один раз |
| `POST /v1/chat/completions` для каждого ключа перед записью | **200** — 49× `qwen-plus-latest` + 3× `deepseek-v4.1-flash` |
| независимая перепроверка (свежий выход, выборка) | **3/3** ключа `GET /v1/models` 200 + завершение `qwen-plus-latest` 200 |
| сбой на стороне панели (наблюдался во время пересборки) | вызовы чата отвечали `500 Database error, please contact the administrator` — сбой **на стороне панели**; повторите `--verify-only`, когда панель исправна |

```bash
node src/cli.js --verify-only
node src/cli.js --screen
```

## 🧭 Как это работает

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: 24hoursAPI page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://24hoursapi.com/v1/chat/completions → real content = the key is live
```

| Шаг | Вызов | Примечания |
| --- | --- | --- |
| status | `GET /api/status` | гейты почты + чек-ина; флаг регистрации не публикуется в этом форке |
| почтовый ящик | Emailnator `POST /api/generate-email` | после исчерпания лимита Mailtrap во флоте, gmail становится основным путём |
| отправка кода | `GET /api/verification?email=…` | 6-символьный код |
| регистрация | `POST /api/user/register` | когда администратор закрывает регистрацию, текст ответа сообщает об этом дословно |
| вход | `POST /api/user/login` | токен сессии |
| **чек-ин** | `POST /api/user/checkin` | ежедневный бонус поверх $2 кредита при регистрации |
| ключ | `POST /api/token/` в `qwen 官直0.8` (резерв `官转 0.8`) | обход групп перехватывает пустые результаты в основной группе |
| подтверждение | `POST /v1/chat/completions` | 200 с содержимым = ключ рабочий |

## ⚙️ Опции CLI

```bash
node src/cli.js --count N        # mint N keys (check-in + chat gate per account)
node src/cli.js --group "qwen 官直0.8" --alt-groups "官转 0.8"
node src/cli.js --verify-only    # re-verify every key in keys.txt
node src/cli.js --screen         # exit pool self-test
node src/cli.js --connect-9router
node src/cli.js --help
```

| Переменная | По умолчанию | Значение |
| --- | --- | --- |
| `H24API_PROXIES` | `proxies.txt` | пул выходов, по одной строке на прокси — необязательно: без пула CLI подключается напрямую |
| `H24API_MODEL` | `qwen-plus-latest` | модель для живой проверки |

## 🛰️ За прокси

**Необязательно — рекомендуется при масштабировании (лимиты на IP, блокировки).** Инструмент работает и без прокси.

- **С пулом:** передайте `--pool <file>` (или задайте `H24API_PROXIES`), и каждый запрос идёт через выход из этого списка — мёртвая линия проваливает эту попытку, и CLI повторяет с следующим выходом (регистрация/вход ограничены по частоте: `login 429` при всплесках).
- **Без пула:** CLI выводит `running WITHOUT a proxy (direct)` и подключается напрямую. Ничего в регистрации, чек-ине, создании ключей или проверке не меняется — только то, откуда уходят пакеты.
- Запустите `--screen` в любом режиме: он тестирует имеющиеся выходы (или прямое соединение) против 24hoursAPI перед реальным запуском.

## 📄 Файлы

| Путь | Содержимое |
| --- | --- |
| `keys.txt` | один проверенный ключ на строку — игнорируется git |
| `accounts.json` | username / e-mail / quotaStart / quotaAfterCheckin / group |
| `proxies.txt` | ваши выходы — игнорируется git |
| `outputs/` | логи запусков — игнорируется git |

## 🔌 Подключение к 9Router

[9Router](https://9router.com) — это самостоятельно размещаемый роутер, совместимый с OpenAI:

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

По умолчанию: `--prefix 24hoursapi`, `--base-url https://24hoursapi.com/v1`, `--model qwen-plus-latest`. Модели подаются как `24hoursapi/<model id>`.

## 🔑 Что даёт один ключ

| | |
| --- | --- |
| 🏢 Провайдер | **24hoursAPI** — [24hoursapi.com](https://24hoursapi.com) |
| 🎁 Что даёт провайдер | **$2 (1 000 000 единиц)** при регистрации + ежедневный чек-ин |
| 🔑 Что даёт один ключ | один ключ на собственном аккаунте в `qwen 官直0.8` / `官转 0.8`, проверенный через чат |
| 🌐 Форма API | Совместим с OpenAI: `POST https://24hoursapi.com/v1/chat/completions` |
| 🔒 Ваши данные | связь только с `24hoursapi.com` и вашим почтовым бэкендом |

## 🐞 Проблемы

Если 24hoursAPI закрывает регистрацию, меняет свои группы или изменяет награду за чек-ин, инструмент останавливается и сообщает об этом. **[Откройте issue](../../issues)** с командой и точным выводом.

## 📄 Лицензия

MIT © 2026 — см. [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 24hoursapi-bulk-apikey contributors

</div>