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

**24hoursAPI hesaplarını otomatik oluşturulan API anahtarlarıyla toplu oluşturun — OpenAI uyumlu çıkarım + 9Router bağlayıcısı.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-61%20served-412991?style=for-the-badge)](https://24hoursapi.com)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![OmniRoute](https://img.shields.io/badge/OmniRoute-connector-8B5CF6?style=for-the-badge)](#-connect-to-omniroute)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Hızlı başlangıç](#-quick-start) · [🎁 Ücretsiz katman](#-free-tier--how-much-you-get) · [🤖 Modeller](#-models--all-61-of-them) · [🧪 API'yi kullanın](#-how-to-use-the-api) · [✅ Doğrulandı](#-verified-working--every-key-is-tested) · [🧭 Nasıl çalışır](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [🔌 OmniRoute](#-connect-to-omniroute) · [⚙️ Seçenekler](#️-cli-options) · [🛰️ Proxy'ler](#️-behind-a-proxy) · [🐞 Sorunlar](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Bu neden var

24hoursAPI her yeni hesaba **1.000.000 kota birimi (qpu=500.000'de ≈ $2)** ve günlük giriş ödülü (+2.000…+9.752 ölçüldü) verir. Panelin `default` grubu **kullanımdan kaldırılmıştır** (`403 分组 default 已被弃用`) — canlı kanalları **`qwen 官直0.8`** (`qwen-plus-latest`) ve **`官转 0.8`** (`deepseek-v4.1-flash`, akıl yürütme modeli) içinde yer alır. Kayıt e-posta + kod ile yapılır, captcha yoktur; panel yönetici tarafından kayıtları periyodik olarak kapatır ve bu araç bu durumda bunu bildirir.

## 🎁 Ücretsiz katman — ne kadar alırsınız

| Ne | Değer |
| --- | --- |
| Kayıt kredisi | Hesap başına **1.000.000 birim ≈ $2** (52 hesabın tamamında ölçüldü; qpu=500.000) |
| Günlük giriş | **+2.000 … +9.752 birim/gün** (şeritteki 52 hesabın 50'sinde ödendi) |
| Çalışan gruplar | **`qwen 官直0.8`** → `qwen-plus-latest` · **`官转 0.8`** → `deepseek-v4.1-flash` |
| `default` grubu | **bu çatalda kullanımdan kaldırıldı** (`403 分组 default 已被弃用`) — anahtarlar canlı bir grupta oluşturulmalıdır |
| Kayıt | e-posta + 6 karakterli kod, **captcha yok**; kayıt yönetici tarafından kapatılabilir (`New user registration has been disabled by administrator`) — CLI tam gövdeyi bildirir |
| Katalog | Canlı fiyatlandırma sayfasında 61 model girdisi |

> 💡 Ücretsiz katman 24hoursAPI tarafından belirlenir ve değişebilir. Canlı sayıları [24hoursapi.com](https://24hoursapi.com) adresinden kontrol edin — ve aracın kendi kapısının karar vermesine izin verin:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Modeller — 61'inin tamamı

Canlı katalog **61 model girdisi** içerir. Kendi gruplarında doğrulanmış iki satır:

| Model ID | Grup | Doğrulandı |
| --- | --- | --- |
| `qwen-plus-latest` | `qwen 官直0.8` | üretim sırasında her anahtar chat-200; bağımsız yeniden kontrol **3/3** |
| `deepseek-v4.1-flash` | `官转 0.8` | her anahtar chat-200 (akıl yürütme modeli — içerik `reasoning_content` içinde olabilir) |

```bash
curl https://24hoursapi.com/v1/chat/completions \
  -H "Authorization: Bearer JnSI3L…" -H "Content-Type: application/json" \
  -d '{"model":"qwen-plus-latest","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 API nasıl kullanılır

Temel URL **`https://24hoursapi.com/v1`** — OpenAI uyumlu.

```python
from openai import OpenAI
c = OpenAI(base_url="https://24hoursapi.com/v1", api_key="JnSI3L…")
print(c.chat.completions.create(model="qwen-plus-latest",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 Hızlı başlangıç

```bash
cd 24hoursapi-bulk-apikey
printf 'socks5://host:port\n' > proxies.txt   # optional — without it the tool runs direct
node src/launcher.js        # interactive: mint / verify / screen / settings

# or: node src/cli.js --count 30    (check-in runs before the chat gate)
```

Bir anahtar `keys.txt` dosyasına **yalnızca** `/v1/chat/completions` gerçek içerikle 200 yanıt verdikten sonra yazılır; `accounts.json` kullanıcı adı / e-posta / quotaStart → quotaAfterCheckin / grup bilgilerini saklar.

### 🖱️ Başlatıcı

Başlatıcı proxy listesini (isteğe bağlı — olmadan CLI doğrudan çalışır) ve sayıyı sorar, bunları hatırlar ve CLI'yi çalıştırır — üret, doğrula, çıkışları tara, ayarlar, yardım, 9Router'a Bağlan.

## ✅ Çalıştığı doğrulandı — her anahtar test edilir

| Kontrol | Sonuç |
| --- | --- |
| `GET /api/status` | **200** — e-posta doğrulama açık, giriş açık, captcha kapalı (bu çatal `register_enabled` yayınlamaz; reddetme gövdesi sinyaldir) |
| `POST /api/user/checkin` | **200** — kota 1.000.000 → 1.009.752'ye kadar hareket eder (50/52 hesap) |
| `qwen 官直0.8` / `官转 0.8` içinde `POST /api/token/` | **200** — anahtarlar bir kez döndürülür |
| Yazılmadan önce her anahtar için `POST /v1/chat/completions` | **200** — 49× `qwen-plus-latest` + 3× `deepseek-v4.1-flash` |
| bağımsız yeniden kontrol (yeni çıkış, örnek) | **3/3** anahtar `GET /v1/models` 200 + `qwen-plus-latest` tamamlama 200 |
| panel tarafı kesinti (yeniden oluşturma sırasında gözlemlendi) | sohbet çağrıları `500 Database error, please contact the administrator` yanıtladı — bir **panel tarafı** hatası; panel sağlıklı olduğunda `--verify-only` komutunu yeniden çalıştırın |

```bash
node src/cli.js --verify-only
node src/cli.js --screen
```

> **Kalite kapıları.** Buradaki her anahtar `keys.txt` dosyasına yazılmadan önce canlı bir `chat-200` geçti
> (ölü anahtarlar asla gönderilmez). Yönlendirici kombinasyonları model düzeyinde aynı kuralı izler: bir girdi
> **yalnızca model `chat-200` yanıtlıyorsa ve araçları çağırabiliyorsa** eklenir — araç desteği olmayan bir model,
> ölü bir anahtar veya ölü bir sağlayıcı eklenmek yerine çıkarılır. Doğrulama adımını istediğiniz zaman yeniden çalıştırın;
> hatalar bildirilir, asla gizlenmez.

## 🧭 Nasıl çalışır

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: 24hoursAPI page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://24hoursapi.com/v1/chat/completions → real content = the key is live
```

| Adım | Çağrı | Notlar |
| --- | --- | --- |
| durum | `GET /api/status` | posta + giriş kapıları; bu çatalda kayıt bayrağı yayınlanmaz |
| posta kutusu | Emailnator `POST /api/generate-email` | filonun Mailtrap sınırından sonra gmail rayı kullanılır |
| kod gönder | `GET /api/verification?email=…` | 6 karakterli kod |
| kayıt | `POST /api/user/register` | yönetici kayıtları kapattığında gövde bunu birebir söyler |
| giriş | `POST /api/user/login` | oturum belirteci |
| **giriş ödülü** | `POST /api/user/checkin` | $2 kayıt kredisinin üzerine günlük bonus |
| anahtar | `qwen 官直0.8` içinde `POST /api/token/` (yedek `官转 0.8`) | grup dolaşımı boş birincil grup atışlarını yakalar |
| kanıt | `POST /v1/chat/completions` | içerikle 200 = anahtar canlı |

## ⚙️ CLI seçenekleri

```bash
node src/cli.js --count N        # mint N keys (check-in + chat gate per account)
node src/cli.js --group "qwen 官直0.8" --alt-groups "官转 0.8"
node src/cli.js --verify-only    # re-verify every key in keys.txt
node src/cli.js --screen         # exit pool self-test
node src/cli.js --connect-9router
node src/cli.js --help
```

| Ortam | Varsayılan | Anlam |
| --- | --- | --- |
| `H24API_PROXIES` | `proxies.txt` | çıkış havuzu, her satırda bir proxy — isteğe bağlı: havuz olmadan CLI doğrudan bağlanır |
| `H24API_MODEL` | `qwen-plus-latest` | canlı doğrulama için model |

## 🛰️ Proxy arkasında

**İsteğe bağlı — ölçekte önerilir (IP başına sınırlar, yasaklar).** Araç proxy olmadan da çalışır.

- **Havuzla:** `--pool <file>` geçin (veya `H24API_PROXIES` ayarlayın) ve her istek o listeden bir çıkıştan geçer — ölü bir hat o denemeyi başarısız kılar ve CLI bir sonraki çıkışla yeniden dener (kayıt/giriş hız sınırlıdır: yoğunlukta `login 429`).
- **Havuz olmadan:** CLI `running WITHOUT a proxy (direct)` yazdırır ve doğrudan bağlanır. Kayıt, giriş ödülü, anahtar üretimi veya doğrulama hakkında hiçbir şey değişmez — yalnızca paketlerin nereden çıktığı değişir.
- Her iki modda da `--screen` çalıştırın: gerçek bir çalıştırmadan önce sahip olduğunuz çıkışları (veya doğrudan rotayı) 24hoursAPI'ye karşı test eder.

## 📄 Dosyalar

| Yol | İçerik |
| --- | --- |
| `keys.txt` | satır başına bir canlı doğrulanmış anahtar — git tarafından yok sayılır |
| `accounts.json` | kullanıcı adı / e-posta / quotaStart / quotaAfterCheckin / grup |
| `proxies.txt` | çıkışlarınız — git tarafından yok sayılır |
| `outputs/` | çalıştırma günlükleri — git tarafından yok sayılır |

## 🔌 9Router'a Bağlan

[9Router](https://9router.com) kendi kendine barındırılan, OpenAI uyumlu bir yönlendiricidir:

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

Varsayılanlar: `--prefix 24hoursapi`, `--base-url https://24hoursapi.com/v1`, `--model qwen-plus-latest`. Modeller `24hoursapi/<model id>` olarak sunulur.

## 🔌 OmniRoute'a Bağlan

[OmniRoute](https://omniroute.online) kendi kendine barındırılan, OpenAI uyumlu bir yönlendiricidir:

```bash
node src/connect-omniroute.js --base http://localhost:20128 --password 123456
```

Varsayılanlar: `--prefix 24hoursapi`, `--base-url https://24hoursapi.com/v1`, `--model qwen-plus-latest`. Modeller `24hoursapi/<model id>` olarak sunulur.

## 🔑 Bir anahtar size ne kazandırır

| | |
| --- | --- |
| 🏢 Sağlayıcı | **24hoursAPI** — [24hoursapi.com](https://24hoursapi.com) |
| 🎁 Sağlayıcının verdikleri | Kayıtta **$2 (1.000.000 birim)** + günlük giriş ödülü |
| 🔑 Bir anahtar size ne kazandırır | `qwen 官直0.8` / `官转 0.8` içinde kendi hesabında bir anahtar, sohbet doğrulanmış |
| 🌐 API biçimi | OpenAI uyumlu: `POST https://24hoursapi.com/v1/chat/completions` |
| 🔒 Verileriniz | yalnızca `24hoursapi.com` ve gelen kutunuzun arka ucuyla iletişim kurulur |

## 🐞 Sorunlar

24hoursAPI kaydı kapatırsa, gruplarını değiştirirse veya giriş ödülünü döndürürse, araç orada durur ve bunu bildirir. Komut ve tam çıktıyla birlikte **[bir sorun açın](../../issues)**.

## 📄 Lisans

MIT © 2026 — bkz. [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 24hoursapi-bulk-apikey katkıda bulunanlar

</div>