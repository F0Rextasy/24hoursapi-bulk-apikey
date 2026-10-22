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

**Crea cuentas de 24hoursAPI en masa con claves API autogeneradas — inferencia compatible con OpenAI + conector 9Router.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-61%20served-412991?style=for-the-badge)](https://24hoursapi.com)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![OmniRoute](https://img.shields.io/badge/OmniRoute-connector-8B5CF6?style=for-the-badge)](#-connect-to-omniroute)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Inicio rápido](#-quick-start) · [🎁 Nivel gratuito](#-free-tier--how-much-you-get) · [🤖 Modelos](#-models--all-61-of-them) · [🧪 Usa la API](#-how-to-use-the-api) · [✅ Verificado](#-verified-working--every-key-is-tested) · [🧭 Cómo funciona](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ Opciones](#️-cli-options) · [🛰️ Proxies](#️-behind-a-proxy) · [🐞 Problemas](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Por qué existe esto

24hoursAPI reparte **1.000.000 de unidades de cuota (≈ $2 con qpu=500.000) a cada cuenta nueva** más un check-in diario (+2.000…+9.752 medidos). El grupo `default` del panel está **obsoleto** (`403 分组 default 已被弃用`) — sus canales activos están en **`qwen 官直0.8`** (`qwen-plus-latest`) y **`官转 0.8`** (`deepseek-v4.1-flash`, modelo de razonamiento). El registro es correo electrónico + código, sin captcha; el panel cierra periódicamente los registros desde el lado del administrador, y esta herramienta lo indica cuando ocurre.

## 🎁 Nivel gratuito — cuánto obtienes

| Qué | Valor |
| --- | --- |
| Crédito de registro | **1.000.000 de unidades ≈ $2** por cuenta (medido en las 52 cuentas; qpu=500.000) |
| Check-in diario | **+2.000 … +9.752 unidades/día** (pagado en 50 de 52 cuentas del carril) |
| Grupos activos | **`qwen 官直0.8`** → `qwen-plus-latest` · **`官转 0.8`** → `deepseek-v4.1-flash` |
| Grupo `default` | **obsoleto en este fork** (`403 分组 default 已被弃用`) — las claves deben crearse en un grupo activo |
| Registro | correo electrónico + código de 6 caracteres, **sin captcha**; el registro puede cerrarse por el administrador (`New user registration has been disabled by administrator`) — la CLI informa el cuerpo exacto |
| Catálogo | 61 entradas de modelos en la hoja de precios activa |

> 💡 El nivel gratuito lo define 24hoursAPI y puede cambiar. Consulta los números activos en [24hoursapi.com](https://24hoursapi.com) — y deja que la propia puerta de la herramienta decida:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Modelos — los 61

El catálogo activo contiene **61 entradas de modelos**. Ambas líneas verificadas, en sus propios grupos:

| ID del modelo | Grupo | Verificado |
| --- | --- | --- |
| `qwen-plus-latest` | `qwen 官直0.8` | **49 claves** chat-200 durante la creación; reverificación independiente **3/3** |
| `deepseek-v4.1-flash` | `官转 0.8` | **3 claves** chat-200 (modelo de razonamiento — el contenido puede estar en `reasoning_content`) |

```bash
curl https://24hoursapi.com/v1/chat/completions \
  -H "Authorization: Bearer JnSI3L…" -H "Content-Type: application/json" \
  -d '{"model":"qwen-plus-latest","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 Cómo usar la API

URL base **`https://24hoursapi.com/v1`** — compatible con OpenAI.

```python
from openai import OpenAI
c = OpenAI(base_url="https://24hoursapi.com/v1", api_key="JnSI3L…")
print(c.chat.completions.create(model="qwen-plus-latest",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 Inicio rápido

```bash
cd 24hoursapi-bulk-apikey
printf 'socks5://host:port\n' > proxies.txt   # optional — without it the tool runs direct
node src/launcher.js        # interactive: mint / verify / screen / settings

# or: node src/cli.js --count 30    (check-in runs before the chat gate)
```

Una clave se escribe en `keys.txt` **solo** después de que `/v1/chat/completions` responda 200 con contenido real; `accounts.json` conserva usuario / correo electrónico / quotaStart → quotaAfterCheckin / grupo.

### 🖱️ El lanzador

El lanzador solicita la lista de proxies (opcional — sin una, la CLI se ejecuta directamente) y la cantidad, las recuerda y ejecuta la CLI — crear, verificar, revisar salidas, configuración, ayuda, Conectar a 9Router.

## ✅ Verificado y funcionando — cada clave se prueba

| Comprobación | Resultado |
| --- | --- |
| `GET /api/status` | **200** — verificación por correo activada, check-in activado, captcha desactivado (este fork no publica `register_enabled`; el cuerpo de rechazo es la señal) |
| `POST /api/user/checkin` | **200** — la cuota pasa de 1.000.000 → hasta 1.009.752 (50/52 cuentas) |
| `POST /api/token/` en `qwen 官直0.8` / `官转 0.8` | **200** — las claves se devuelven una vez |
| `POST /v1/chat/completions` por clave antes de escribirse | **200** — 49× `qwen-plus-latest` + 3× `deepseek-v4.1-flash` |
| reverificación independiente (salida nueva, muestra) | **3/3** claves `GET /v1/models` 200 + finalización `qwen-plus-latest` 200 |
| caída del lado del panel (observada durante la reconstrucción) | las llamadas de chat respondieron `500 Database error, please contact the administrator` — un fallo **del lado del panel**; vuelve a ejecutar `--verify-only` cuando el panel esté sano |

```bash
node src/cli.js --verify-only
node src/cli.js --screen
```

## 🧭 Cómo funciona

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: 24hoursAPI page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://24hoursapi.com/v1/chat/completions → real content = the key is live
```

| Paso | Llamada | Notas |
| --- | --- | --- |
| estado | `GET /api/status` | puertas de correo + check-in; indicador de registro no publicado en este fork |
| buzón | Emailnator `POST /api/generate-email` | tras el límite de Mailtrap de la flota, gmail es el carril |
| enviar código | `GET /api/verification?email=…` | código de 6 caracteres |
| registrar | `POST /api/user/register` | cuando el administrador cierra los registros, el cuerpo lo dice textualmente |
| iniciar sesión | `POST /api/user/login` | token de sesión |
| **check-in** | `POST /api/user/checkin` | bonificación diaria además del crédito de registro de $2 |
| clave | `POST /api/token/` en `qwen 官直0.8` (alternativa `官转 0.8`) | el recorrido de grupos captura tiradas vacías del grupo primario |
| prueba | `POST /v1/chat/completions` | 200 con contenido = la clave está activa |

## ⚙️ Opciones de la CLI

```bash
node src/cli.js --count N        # mint N keys (check-in + chat gate per account)
node src/cli.js --group "qwen 官直0.8" --alt-groups "官转 0.8"
node src/cli.js --verify-only    # re-verify every key in keys.txt
node src/cli.js --screen         # exit pool self-test
node src/cli.js --connect-9router
node src/cli.js --help
```

| Variable de entorno | Predeterminado | Significado |
| --- | --- | --- |
| `H24API_PROXIES` | `proxies.txt` | grupo de salidas, una línea por proxy — opcional: sin un grupo, la CLI se conecta directamente |
| `H24API_MODEL` | `qwen-plus-latest` | modelo para la verificación en vivo |

## 🛰️ Detrás de un proxy

**Opcional — recomendado a escala (límites por IP, bloqueos).** La herramienta también funciona sin proxy.

- **Con un grupo:** pasa `--pool <file>` (o define `H24API_PROXIES`) y cada solicitud pasa por una salida de esa lista — una línea muerta falla ese intento y la CLI reintenta con la siguiente salida (registro/inicio de sesión tienen límite de velocidad: `login 429` en ráfagas).
- **Sin un grupo:** la CLI imprime `running WITHOUT a proxy (direct)` y se conecta directamente. Nada del registro, check-in, creación de claves o verificación cambia — solo desde dónde salen los paquetes.
- Ejecuta `--screen` en cualquiera de los dos modos: prueba las salidas que tengas (o la ruta directa) contra 24hoursAPI antes de una ejecución real.

## 📄 Archivos

| Ruta | Contenido |
| --- | --- |
| `keys.txt` | una clave verificada en vivo por línea — ignorada por git |
| `accounts.json` | usuario / correo electrónico / quotaStart / quotaAfterCheckin / grupo |
| `proxies.txt` | tus salidas — ignorado por git |
| `outputs/` | registros de ejecución — ignorados por git |

## 🔌 Conectar a 9Router

[9Router](https://9router.com) es un enrutador autoalojado y compatible con OpenAI:

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

Valores predeterminados: `--prefix 24hoursapi`, `--base-url https://24hoursapi.com/v1`, `--model qwen-plus-latest`. Los modelos se sirven como `24hoursapi/<model id>`.

## 🔌 Conectar a OmniRoute

[OmniRoute](https://omniroute.online) es un enrutador autoalojado y compatible con OpenAI:

```bash
node src/connect-omniroute.js --base http://localhost:20128 --password 123456
```

Valores predeterminados: `--prefix 24hoursapi`, `--base-url https://24hoursapi.com/v1`, `--model qwen-plus-latest`. Los modelos se sirven como `24hoursapi/<model id>`.
## 🔑 Qué te da una clave

| | |
| --- | --- |
| 🏢 Proveedor | **24hoursAPI** — [24hoursapi.com](https://24hoursapi.com) |
| 🎁 Qué da el proveedor | **$2 (1.000.000 de unidades)** al registrarte + check-in diario |
| 🔑 Qué te da una clave | una clave en su propia cuenta en `qwen 官直0.8` / `官转 0.8`, verificada por chat |
| 🌐 Forma de la API | Compatible con OpenAI: `POST https://24hoursapi.com/v1/chat/completions` |
| 🔒 Tus datos | solo se contacta con `24hoursapi.com` y tu backend de buzón |

## 🐞 Problemas

Si 24hoursAPI cierra el registro, cambia sus grupos o rota la recompensa del check-in, la herramienta se detiene ahí y lo indica. **[Abre un issue](../../issues)** con el comando y la salida exacta.

## 📄 Licencia

MIT © 2026 — consulta [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 24hoursapi-bulk-apikey contributors

</div>