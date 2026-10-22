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

**Crie contas 24hoursAPI em massa com chaves de API geradas automaticamente — inferência compatível com OpenAI + conector 9Router.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-61%20served-412991?style=for-the-badge)](https://24hoursapi.com)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![OmniRoute](https://img.shields.io/badge/OmniRoute-connector-8B5CF6?style=for-the-badge)](#-connect-to-omniroute)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Início rápido](#-quick-start) · [🎁 Camada gratuita](#-free-tier--how-much-you-get) · [🤖 Modelos](#-models--all-61-of-them) · [🧪 Use a API](#-how-to-use-the-api) · [✅ Verificado](#-verified-working--every-key-is-tested) · [🧭 Como funciona](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ Opções](#️-cli-options) · [🛰️ Proxies](#️-behind-a-proxy) · [🐞 Problemas](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Por que isso existe

O 24hoursAPI distribui **1.000.000 de unidades de cota (≈ $2 com qpu=500.000) para cada nova conta** mais um check-in diário (+2.000…+9.752 medidos). O grupo `default` do painel está **obsoleto** (`403 分组 default 已被弃用`) — seus canais ativos ficam em **`qwen 官直0.8`** (`qwen-plus-latest`) e **`官转 0.8`** (`deepseek-v4.1-flash`, modelo de raciocínio). O cadastro é e-mail + código, sem captcha; o painel fecha os cadastros periodicamente pelo lado do administrador, e esta ferramenta avisa quando isso acontece.

## 🎁 Camada gratuita — quanto você recebe

| O quê | Valor |
| --- | --- |
| Crédito de cadastro | **1.000.000 de unidades ≈ $2** por conta (medido em todas as 52 contas; qpu=500.000) |
| Check-in diário | **+2.000 … +9.752 unidades/dia** (pago em 50 de 52 contas na faixa) |
| Grupos ativos | **`qwen 官直0.8`** → `qwen-plus-latest` · **`官转 0.8`** → `deepseek-v4.1-flash` |
| Grupo `default` | **obsoleto neste fork** (`403 分组 default 已被弃用`) — as chaves devem ser criadas em um grupo ativo |
| Cadastro | e-mail + código de 6 caracteres, **sem captcha**; o cadastro pode ser fechado pelo administrador (`New user registration has been disabled by administrator`) — a CLI informa o corpo exato |
| Catálogo | 61 entradas de modelos na tabela de preços ativa |

> 💡 A camada gratuita é definida pelo 24hoursAPI e pode mudar. Confira os números ativos em [24hoursapi.com](https://24hoursapi.com) — e deixe o próprio gate da ferramenta decidir:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Modelos — todos os 61

O catálogo ativo contém **61 entradas de modelos**. Ambas as linhas verificadas, em seus próprios grupos:

| ID do modelo | Grupo | Verificado |
| --- | --- | --- |
| `qwen-plus-latest` | `qwen 官直0.8` | **49 chaves** chat-200 durante a criação; reverificação independente **3/3** |
| `deepseek-v4.1-flash` | `官转 0.8` | **3 chaves** chat-200 (modelo de raciocínio — o conteúdo pode ficar em `reasoning_content`) |

```bash
curl https://24hoursapi.com/v1/chat/completions \
  -H "Authorization: Bearer JnSI3L…" -H "Content-Type: application/json" \
  -d '{"model":"qwen-plus-latest","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 Como usar a API

URL base **`https://24hoursapi.com/v1`** — compatível com OpenAI.

```python
from openai import OpenAI
c = OpenAI(base_url="https://24hoursapi.com/v1", api_key="JnSI3L…")
print(c.chat.completions.create(model="qwen-plus-latest",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 Início rápido

```bash
cd 24hoursapi-bulk-apikey
printf 'socks5://host:port\n' > proxies.txt   # optional — without it the tool runs direct
node src/launcher.js        # interactive: mint / verify / screen / settings

# or: node src/cli.js --count 30    (check-in runs before the chat gate)
```

Uma chave é gravada em `keys.txt` **somente** depois que `/v1/chat/completions` responde 200 com conteúdo real; `accounts.json` mantém username / e-mail / quotaStart → quotaAfterCheckin / group.

### 🖱️ O launcher

O launcher solicita a lista de proxies (opcional — sem uma, a CLI roda direto) e a quantidade, memoriza-os e executa a CLI — mint, verify, screen exits, settings, help, Connect to 9Router.

## ✅ Verificado e funcionando — toda chave é testada

| Verificação | Resultado |
| --- | --- |
| `GET /api/status` | **200** — verificação por e-mail ativada, check-in ativado, captcha desativado (este fork não publica `register_enabled`; o corpo da recusa é o sinal) |
| `POST /api/user/checkin` | **200** — a cota vai de 1.000.000 → até 1.009.752 (50/52 contas) |
| `POST /api/token/` em `qwen 官直0.8` / `官转 0.8` | **200** — chaves retornadas uma vez |
| `POST /v1/chat/completions` por chave antes de ser gravada | **200** — 49× `qwen-plus-latest` + 3× `deepseek-v4.1-flash` |
| reverificação independente (saída nova, amostra) | **3/3** chaves `GET /v1/models` 200 + conclusão `qwen-plus-latest` 200 |
| indisponibilidade do painel (observada durante a reconstrução) | chamadas de chat responderam `500 Database error, please contact the administrator` — uma falha **do lado do painel**; execute novamente `--verify-only` quando o painel estiver saudável |

```bash
node src/cli.js --verify-only
node src/cli.js --screen
```

## 🧭 Como funciona

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: 24hoursAPI page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://24hoursapi.com/v1/chat/completions → real content = the key is live
```

| Etapa | Chamada | Observações |
| --- | --- | --- |
| status | `GET /api/status` | gates de mail + check-in; flag de registro não publicada neste fork |
| mailbox | Emailnator `POST /api/generate-email` | após o limite de Mailtrap da frota, o gmail é o trilho |
| enviar código | `GET /api/verification?email=…` | código de 6 caracteres |
| registrar | `POST /api/user/register` | quando o administrador fecha os cadastros, o corpo diz isso literalmente |
| login | `POST /api/user/login` | token de sessão |
| **check-in** | `POST /api/user/checkin` | bônus diário além do crédito de cadastro de $2 |
| chave | `POST /api/token/` em `qwen 官直0.8` (fallback `官转 0.8`) | a varredura de grupos captura sorteios vazios do grupo primário |
| prova | `POST /v1/chat/completions` | 200 com conteúdo = a chave está ativa |

## ⚙️ Opções da CLI

```bash
node src/cli.js --count N        # mint N keys (check-in + chat gate per account)
node src/cli.js --group "qwen 官直0.8" --alt-groups "官转 0.8"
node src/cli.js --verify-only    # re-verify every key in keys.txt
node src/cli.js --screen         # exit pool self-test
node src/cli.js --connect-9router
node src/cli.js --help
```

| Env | Padrão | Significado |
| --- | --- | --- |
| `H24API_PROXIES` | `proxies.txt` | pool de saída, uma linha por proxy — opcional: sem um pool, a CLI se conecta diretamente |
| `H24API_MODEL` | `qwen-plus-latest` | modelo para a verificação ativa |

## 🛰️ Atrás de um proxy

**Opcional — recomendado em escala (limites por IP, banimentos).** A ferramenta funciona sem proxy também.

- **Com um pool:** passe `--pool <file>` (ou defina `H24API_PROXIES`) e cada requisição passa por uma saída dessa lista — uma linha morta falha naquela tentativa e a CLI tenta novamente com a próxima saída (register/login têm limite de taxa: `login 429` em rajadas).
- **Sem um pool:** a CLI imprime `running WITHOUT a proxy (direct)` e se conecta diretamente. Nada sobre cadastro, check-in, criação de chaves ou verificação muda — apenas de onde os pacotes saem.
- Execute `--screen` em qualquer um dos modos: ele testa as saídas que você tem (ou a rota direta) contra o 24hoursAPI antes de uma execução real.

## 📄 Arquivos

| Caminho | Conteúdo |
| --- | --- |
| `keys.txt` | uma chave verificada em produção por linha — ignorada pelo git |
| `accounts.json` | username / e-mail / quotaStart / quotaAfterCheckin / group |
| `proxies.txt` | suas saídas — ignorado pelo git |
| `outputs/` | logs de execução — ignorados pelo git |

## 🔌 Conectar ao 9Router

[9Router](https://9router.com) é um roteador auto-hospedado e compatível com OpenAI:

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

Padrões: `--prefix 24hoursapi`, `--base-url https://24hoursapi.com/v1`, `--model qwen-plus-latest`. Os modelos são servidos como `24hoursapi/<model id>`.

## 🔌 Conectar ao OmniRoute

[OmniRoute](https://omniroute.online) é um roteador auto-hospedado e compatível com OpenAI:

```bash
node src/connect-omniroute.js --base http://localhost:20128 --password 123456
```

Padrões: `--prefix 24hoursapi`, `--base-url https://24hoursapi.com/v1`, `--model qwen-plus-latest`. Os modelos são servidos como `24hoursapi/<model id>`.
## 🔑 O que uma chave oferece

| | |
| --- | --- |
| 🏢 Provedor | **24hoursAPI** — [24hoursapi.com](https://24hoursapi.com) |
| 🎁 O que o provedor oferece | **$2 (1.000.000 de unidades)** no cadastro + check-in diário |
| 🔑 O que uma chave oferece | uma chave em sua própria conta em `qwen 官直0.8` / `官转 0.8`, verificada por chat |
| 🌐 Formato da API | Compatível com OpenAI: `POST https://24hoursapi.com/v1/chat/completions` |
| 🔒 Seus dados | apenas `24hoursapi.com` e seu backend de inbox são contatados |

## 🐞 Problemas

Se o 24hoursAPI fechar o cadastro, mudar seus grupos ou rotacionar a recompensa de check-in, a ferramenta para ali e avisa. **[Abra uma issue](../../issues)** com o comando e a saída exata.

## 📄 Licença

MIT © 2026 — veja [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 24hoursapi-bulk-apikey contributors

</div>