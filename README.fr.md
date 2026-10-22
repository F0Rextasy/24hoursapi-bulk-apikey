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

**Créer en masse des comptes 24hoursAPI avec des clés API générées automatiquement — inférence compatible OpenAI + connecteur 9Router.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-61%20served-412991?style=for-the-badge)](https://24hoursapi.com)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Démarrage rapide](#-quick-start) · [🎁 Offre gratuite](#-free-tier--how-much-you-get) · [🤖 Modèles](#-models--all-61-of-them) · [🧪 Utiliser l'API](#-how-to-use-the-api) · [✅ Vérifié](#-verified-working--every-key-is-tested) · [🧭 Comment ça marche](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [⚙️ Options](#️-cli-options) · [🛰️ Proxys](#️-behind-a-proxy) · [🐞 Problèmes](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Pourquoi cet outil existe

24hoursAPI offre **1 000 000 d'unités de quota (≈ 2 $ à qpu=500 000) à chaque nouveau compte**, plus un check-in quotidien (+2 000…+9 752 mesurés). Le groupe `default` du panneau est **déprécié** (`403 分组 default 已被弃用`) — ses canaux actifs se trouvent dans **`qwen 官直0.8`** (`qwen-plus-latest`) et **`官转 0.8`** (`deepseek-v4.1-flash`, modèle de raisonnement). L'inscription se fait par e-mail + code, sans captcha ; le panneau ferme périodiquement les inscriptions côté admin, et cet outil le signale le cas échéant.

## 🎁 Offre gratuite — ce que vous obtenez

| Élément | Valeur |
| --- | --- |
| Crédit d'inscription | **1 000 000 d'unités ≈ 2 $** par compte (mesuré sur les 52 comptes ; qpu=500 000) |
| Check-in quotidien | **+2 000 … +9 752 unités/jour** (versé sur 50 des 52 comptes de la voie) |
| Groupes fonctionnels | **`qwen 官直0.8`** → `qwen-plus-latest` · **`官转 0.8`** → `deepseek-v4.1-flash` |
| Groupe `default` | **déprécié sur ce fork** (`403 分组 default 已被弃用`) — les clés doivent être créées dans un groupe actif |
| Inscription | e-mail + code à 6 caractères, **sans captcha** ; l'inscription peut être fermée par l'admin (`New user registration has been disabled by administrator`) — le CLI rapporte le corps exact |
| Catalogue | 61 entrées de modèles sur la grille tarifaire en ligne |

> 💡 L'offre gratuite est définie par 24hoursAPI et peut changer. Consultez les chiffres en direct sur [24hoursapi.com](https://24hoursapi.com) — et laissez le contrôle de l'outil décider :

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Modèles — les 61

Le catalogue en ligne contient **61 entrées de modèles**. Les deux lignes vérifiées, sur leurs propres groupes :

| ID du modèle | Groupe | Vérifié |
| --- | --- | --- |
| `qwen-plus-latest` | `qwen 官直0.8` | **49 clés** chat-200 pendant la génération ; re-vérification indépendante **3/3** |
| `deepseek-v4.1-flash` | `官转 0.8` | **3 clés** chat-200 (modèle de raisonnement — le contenu peut se trouver dans `reasoning_content`) |

```bash
curl https://24hoursapi.com/v1/chat/completions \
  -H "Authorization: Bearer JnSI3L…" -H "Content-Type: application/json" \
  -d '{"model":"qwen-plus-latest","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🧪 Comment utiliser l'API

URL de base **`https://24hoursapi.com/v1`** — compatible OpenAI.

```python
from openai import OpenAI
c = OpenAI(base_url="https://24hoursapi.com/v1", api_key="JnSI3L…")
print(c.chat.completions.create(model="qwen-plus-latest",
      messages=[{"role":"user","content":"hi"}]).choices[0].message.content)
```

## 🛠 Démarrage rapide

```bash
cd 24hoursapi-bulk-apikey
printf 'socks5://host:port\n' > proxies.txt   # optional — without it the tool runs direct
node src/launcher.js        # interactive: mint / verify / screen / settings

# or: node src/cli.js --count 30    (check-in runs before the chat gate)
```

Une clé n'est écrite dans `keys.txt` **qu'après** que `/v1/chat/completions` a répondu 200 avec du contenu réel ; `accounts.json` conserve username / e-mail / quotaStart → quotaAfterCheckin / groupe.

### 🖱️ Le lanceur

Le lanceur demande la liste de proxys (facultative — sans elle, le CLI fonctionne en direct) et le nombre, les mémorise, puis exécute le CLI — mint, verify, screen exits, settings, help, Connect to 9Router.

## ✅ Vérifié fonctionnel — chaque clé est testée

| Vérification | Résultat |
| --- | --- |
| `GET /api/status` | **200** — vérification e-mail activée, check-in activé, captcha désactivé (ce fork ne publie pas `register_enabled` ; le corps de refus est le signal) |
| `POST /api/user/checkin` | **200** — le quota passe de 1 000 000 → jusqu'à 1 009 752 (50/52 comptes) |
| `POST /api/token/` dans `qwen 官直0.8` / `官转 0.8` | **200** — clés renvoyées une seule fois |
| `POST /v1/chat/completions` par clé avant écriture | **200** — 49× `qwen-plus-latest` + 3× `deepseek-v4.1-flash` |
| re-vérification indépendante (sortie fraîche, échantillon) | **3/3** clés `GET /v1/models` 200 + complétion `qwen-plus-latest` 200 |
| panne côté panneau (observée pendant la reconstruction) | les appels chat ont répondu `500 Database error, please contact the administrator` — une défaillance **côté panneau** ; relancez `--verify-only` quand le panneau est sain |

```bash
node src/cli.js --verify-only
node src/cli.js --screen
```

## 🧭 Comment ça marche

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: 24hoursAPI page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://24hoursapi.com/v1/chat/completions → real content = the key is live
```

| Étape | Appel | Remarques |
| --- | --- | --- |
| status | `GET /api/status` | portes mail + check-in ; indicateur register non publié sur ce fork |
| boîte mail | Emailnator `POST /api/generate-email` | après la limite Mailtrap de la flotte, gmail est le rail |
| envoi du code | `GET /api/verification?email=…` | code à 6 caractères |
| inscription | `POST /api/user/register` | quand l'admin ferme les inscriptions, le corps le dit textuellement |
| connexion | `POST /api/user/login` | jeton de session |
| **check-in** | `POST /api/user/checkin` | bonus quotidien en plus du crédit d'inscription de 2 $ |
| clé | `POST /api/token/` dans `qwen 官直0.8` (repli `官转 0.8`) | le parcours des groupes rattrape les tirages vides du groupe principal |
| preuve | `POST /v1/chat/completions` | 200 avec contenu = la clé est active |

## ⚙️ Options du CLI

```bash
node src/cli.js --count N        # mint N keys (check-in + chat gate per account)
node src/cli.js --group "qwen 官直0.8" --alt-groups "官转 0.8"
node src/cli.js --verify-only    # re-verify every key in keys.txt
node src/cli.js --screen         # exit pool self-test
node src/cli.js --connect-9router
node src/cli.js --help
```

| Env | Défaut | Signification |
| --- | --- | --- |
| `H24API_PROXIES` | `proxies.txt` | pool de sorties, une ligne par proxy — facultatif : sans pool, le CLI se connecte directement |
| `H24API_MODEL` | `qwen-plus-latest` | modèle pour la vérification en direct |

## 🛰️ Derrière un proxy

**Facultatif — recommandé à grande échelle (limites par IP, bannissements).** L'outil fonctionne aussi sans proxy.

- **Avec un pool :** passez `--pool <file>` (ou définissez `H24API_PROXIES`) et chaque requête passe par une sortie de cette liste — une ligne morte fait échouer cette tentative et le CLI réessaie avec la sortie suivante (register/login sont limités en débit : `login 429` en rafale).
- **Sans pool :** le CLI affiche `running WITHOUT a proxy (direct)` et se connecte directement. Rien ne change pour l'inscription, le check-in, la génération de clés ou la vérification — seul le point de sortie des paquets diffère.
- Lancez `--screen` dans l'un ou l'autre mode : il teste les sorties dont vous disposez (ou la route directe) contre 24hoursAPI avant une exécution réelle.

## 📄 Fichiers

| Chemin | Contenu |
| --- | --- |
| `keys.txt` | une clé vérifiée en direct par ligne — ignoré par git |
| `accounts.json` | username / e-mail / quotaStart / quotaAfterCheckin / groupe |
| `proxies.txt` | vos sorties — ignoré par git |
| `outputs/` | journaux d'exécution — ignorés par git |

## 🔌 Connexion à 9Router

[9Router](https://9router.com) est un routeur auto-hébergé compatible OpenAI :

```bash
node src/connect-9router.js --base http://localhost:20128 --password 123456
```

Valeurs par défaut : `--prefix 24hoursapi`, `--base-url https://24hoursapi.com/v1`, `--model qwen-plus-latest`. Les modèles sont servis sous `24hoursapi/<model id>`.

## 🔑 Ce qu'une clé vous apporte

| | |
| --- | --- |
| 🏢 Fournisseur | **24hoursAPI** — [24hoursapi.com](https://24hoursapi.com) |
| 🎁 Ce que le fournisseur offre | **2 $ (1 000 000 d'unités)** à l'inscription + check-in quotidien |
| 🔑 Ce qu'une clé vous apporte | une clé sur son propre compte dans `qwen 官直0.8` / `官转 0.8`, vérifiée par chat |
| 🌐 Forme de l'API | compatible OpenAI : `POST https://24hoursapi.com/v1/chat/completions` |
| 🔒 Vos données | seuls `24hoursapi.com` et votre backend de boîte mail sont contactés |

## 🐞 Problèmes

Si 24hoursAPI ferme les inscriptions, change ses groupes ou modifie la récompense de check-in, l'outil s'arrête et le signale. **[Ouvrez un ticket](../../issues)** avec la commande et la sortie exacte.

## 📄 Licence

MIT © 2026 — voir [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 24hoursapi-bulk-apikey contributors

</div>