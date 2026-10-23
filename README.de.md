<div align="center">

```
 █████╗ ██╗███████╗███████╗███████╗
██╔══██╗██║╚══███╔╝╚══███╔╝╚══███╔╝
███████║██║  ███╔╝   ███╔╝   ███╔╝
██╔══██║██║ ███╔╝   ███╔╝   ███╔╝
██║  ██║██║███████╗███████╗███████╗
╚═╝  ╚═╝╚═╝╚══════╝╚══════╝╚══════╝
```

# aizzz-bulk-apikey

**Massenhaft aizzz.shop (ZZ 中转站)-Konten mit API-Schlüsseln erstellen — OpenAI-kompatible Inferenz + 9Router-Connector.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-undefined%20served-412991?style=for-the-badge)](https://aizzz.shop)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![OmniRoute](https://img.shields.io/badge/OmniRoute-connector-8B5CF6?style=for-the-badge)](#-connect-to-omniroute)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Schnellstart](#-quick-start) · [🎁 Kostenlose Stufe](#-free-tier--how-much-you-get) · [🤖 Modelle](#-models--all-undefined-of-them) · [🧪 Die API verwenden](#-how-to-use-the-api) · [✅ Verifiziert](#-verified-working--every-key-is-tested) · [🧭 So funktioniert es](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [🔌 OmniRoute](#-connect-to-omniroute) · [⚙️ Optionen](#️-cli-options) · [🛰️ Proxys](#️-behind-a-proxy) · [🐞 Probleme](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Warum es das gibt

aizzz.shop (ZZ 中转站) ist ein **New API**-Panel (`aizzz.shop`) mit E-Mail-Code-Registrierung, offener Passwortregistrierung, keinem Captcha und OpenAI-kompatibler Inferenz unter `https://aizzz.shop/v1`. Die nützliche Einheit ist *ein Konto pro Schlüssel*, daher macht dieses Tool daraus eine `keys.txt`, in der **jede Zeile einen echten Chat-Aufruf** und eine `tools`-Prüfung überstanden hat. Die Registrierung ist für die öffentlichen Mailboxen, die dieses Tool mitliefert, geschlossen, daher ist der Bestand das, was übrig bleibt; die CLI versucht es trotzdem und gibt die Antwort des Panels wörtlich aus.

## 🎁 Kostenlose Stufe — wie viel du bekommst

| Was | Wert |
| --- | --- |
| Anmeldeguthaben | ~260 000 Quota ≈ **$0.52** (`quota_per_unit` 500 000) |
| Täglicher Check-in | **Ja** (`POST /api/user/checkin`, gemessen +11 632 in einem Aufruf) |
| Karte erforderlich | **Nein** |
| Captcha | **Aus** (`turnstile_check=false`) |
| E-Mail | Code, **Domain-Allow-List** (gmail.com / outlook.com werden abgelehnt) |

Keine Karte, kein Captcha: ein Schlüssel kostet eine Adresse, die das Panel akzeptiert.

> 💡 Die kostenlose Stufe wird von aizzz.shop (ZZ 中转站) festgelegt und kann sich ändern. Prüfe die aktuellen Zahlen auf [aizzz.shop](https://aizzz.shop) — und lass das Gate des Tools selbst entscheiden:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Modelle — alle undefined davon

Der Katalog wird pro Token-Gruppe bereitgestellt. Verifizierte Live-IDs: `deepseek-v4-flash`, `deepseek-v4.1-flash`, `glm-5.2`. `deepseek-v4-flash` ist der Standard der CLI und das Modell, das für jede Verifizierung in diesem Repo verwendet wird.

## 🧪 So verwendest du die API

Die Basis-URL ist **`https://aizzz.shop/v1`** — OpenAI-kompatibel, also funktioniert alles, was OpenAI spricht.

```bash
curl https://aizzz.shop/v1/chat/completions \
  -H "Authorization: Bearer <your key>" \
  -H "Content-Type: application/json" \
  -d '{"model":"deepseek-v4-flash","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🛠 Schnellstart

```bash

# clone or fork this repository, then:
cd aizzz-bulk-apikey

# 1) your proxy list, one per line (http://… or socks5://…) — optional
printf 'http://127.0.0.1:8080\n' > proxies.txt

# 2) go
node src/launcher.js                 # interactive
# or directly:
node src/cli.js --count 10 --pool proxies.txt
node src/cli.js --revive --pool proxies.txt   # re-login stored accounts, check in, re-mint
node src/cli.js --verify                       # re-test every key in keys.txt
```

`keys.txt` wird **nur** geschrieben, nachdem eine Live-Chat-Vervollständigung den Schlüssel bewiesen hat.

### 🖱️ Der Launcher

Der Launcher fragt nach der Proxy-Liste, Anzahl und Modell, merkt sie sich in `launcher.config.json` und führt die CLI mit den richtigen Flags aus.

## ✅ Verifiziert funktionierend — jeder Schlüssel wird getestet

| Prüfung | Ergebnis |
| --- | --- |
| `https://aizzz.shop/v1/models` mit dem Schlüssel | **200** — echter Katalog |
| zwei Live-`POST /chat/completions`-Aufrufe pro Schlüssel | **200**, nicht-leerer Inhalt |
| `tools` — ein `get_time`-Tool-Aufruf, `tool_choice: auto` | **get_time tool_call zurückgegeben** |
| dieser Lauf (`preadd-gate.js --pool`, fail-closed exits) | **1 live / 37 Schlüssel versucht** (34 tot) |
| ein Schlüssel, der die Chat-Prüfung nicht besteht | wird nie in `keys.txt` geschrieben |

> **Qualitätsgates.** Jeder Schlüssel hier hat ein Live-`chat-200` bestanden, bevor er in `keys.txt` geschrieben wurde
> (tote Schlüssel werden nie ausgeliefert). Router-Kombinationen folgen derselben Regel auf Modellebene: ein Eintrag wird
> **nur hinzugefügt, wenn das Modell `chat-200` antwortet und Tools aufrufen kann** — ein Modell ohne Tool-Unterstützung,
> ein toter Schlüssel oder ein toter Anbieter wird verworfen statt hinzugefügt. Führe den Verifizierungsschritt jederzeit erneut aus;
> Fehler werden gemeldet, nie versteckt.

## 🧭 So funktioniert es

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: aizzz.shop (ZZ 中转站) page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://aizzz.shop/v1/chat/completions → real content = the key is live
```

| Schritt | Aufruf | Hinweise |
| --- | --- | --- |
| Code | `GET /api/verification?email=<addr>` | das Panel gibt das Allow-List-Urteil wörtlich zurück |
| Registrierung | `POST /api/user/register` | Turnstile ist aus; ein 6-stelliger Code kommt per Mail |
| Anmeldung | `POST /api/user/login` | `data.access_token` (JWT) — dieser Fork verwendet **kein** Session-Cookie |
| Guthaben | `POST /api/user/checkin` | täglich, kostenlos |
| Token | `POST /api/token/` mit `{"group":"default"}` | ein Token außerhalb einer bedienenden Gruppe antwortet mit 403 |
| Schlüssel | `POST /api/token/<id>/key` | die Liste maskiert den Schlüssel, der Reveal-Endpunkt nicht |
| Prüfung | `POST /v1/chat/completions` und ein `tools`-Aufruf | Live-200 + tool_call erforderlich vor `keys.txt` |

## ⚙️ CLI-Optionen

```bash
node src/cli.js --count N            # mint N accounts (default 1)
node src/cli.js --revive             # re-login accounts.jsonl, check in, mint a fresh key
node src/cli.js --verify             # re-check every key in keys.txt
node src/cli.js --pool proxies.txt   # rotate exits, one exit per account
node src/cli.js --connect-9router    # register the provider on a local 9Router
node src/cli.js --help
```

| Umgebungsvariable | Standard | Bedeutung |
| --- | --- | --- |
| `AIZZZ_BASE` | `https://aizzz.shop` | Basis-URL des Panels |
| `AIZZZ_PROXIES` | – | optionale Pool-Datei, ein Exit pro Zeile |

## 🛰️ Hinter einem Proxy

**Optional, bei Skalierung empfohlen** (Rate-Limits und Bans pro IP). Übergib `--pool <file>` mit einem Exit pro Zeile (`http://`, `https://` CONNECT oder `socks5://`); jedes Konto verwendet den nächsten Exit, und eine tote Zeile lässt nur diesen Versuch fehlschlagen. Ohne Pool verbindet sich das Tool direkt.

## 📄 Dateien

| Pfad | Inhalt |
| --- | --- |
| `keys.txt` | ein live-verifizierter Schlüssel pro Zeile — von Git ignoriert |
| `accounts.jsonl` | Adresse / Benutzername / Passwort / Schlüssel / Chat-Status pro Konto |
| `proxies.txt` | deine Exits, einer pro Zeile — von Git ignoriert |

## 🔌 Mit 9Router verbinden

[9Router](https://9router.com) ist ein selbst gehosteter, OpenAI-kompatibler Router. Registriere diesen Anbieter einmal und jeder Schlüssel wird zu einer Verbindung unter einem Anbieter-Node:

```bash
node src/connect-9router.js
```

Standardwerte: `--base http://localhost:20128`, `--password 123456`, `--name "aizzz.shop (

## 🔌 Mit OmniRoute verbinden

[OmniRoute](https://omniroute.online) ist ein selbst gehosteter, OpenAI-kompatibler Router. Registriere diesen Anbieter einmal und jeder Schlüssel wird zu einer Verbindung unter einem Anbieter-Node:

```bash
node src/connect-omniroute.js
```

Standardwerte: `--base http://localhost:20128`, `--password 123456`, `--name "aizzz.shop (

ZZ 中转站)"`, `--prefix aizzz`, `--base-url https://aizzz.shop/v1`. Modelle werden dann als `aizzz/<model id>` bereitgestellt und über jeden Schlüssel lastverteilt.

## 🔑 Was ein Schlüssel dir bringt

| | |
| --- | --- |
| 🏢 Anbieter | **aizzz.shop (ZZ 中转站)** — [aizzz.shop](https://aizzz.shop) |
| 🎁 Was der Anbieter gibt | ein Konto pro Schlüssel — jeder Schlüssel beantwortet einen Live-Chat-Aufruf |
| 🔑 Was ein Schlüssel dir bringt | ein API-Schlüssel, der auf sein eigenes Konto beschränkt ist: die vollständigen Modell-IDs am unten stehenden Endpunkt, im Rahmen der Limits deines Tarifs. Die Quota liegt auf dem **Konto**, genau deshalb erstellt dieses Tool ein Konto pro Schlüssel |
| 🌐 API-Form | OpenAI-kompatibel: `POST https://aizzz.shop/v1/chat/completions` |
| 🔒 Deine Daten | nur aizzz.shop und das von dir konfigurierte Inbox-Backend werden kontaktiert; Schlüssel bleiben auf deinem Rechner |

## 🐞 Probleme

Wenn das Panel die Form seiner Registrierung, Gruppen oder sein Guthaben ändert, stoppt das Tool an diesem Schritt und gibt die Antwort des Panels wörtlich aus. **[Öffne ein Issue](../../issues)** mit dem ausgeführten Befehl und der exakten Ausgabe; PRs sind willkommen.

## 📄 Lizenz

MIT © 2026 — siehe [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 aizzz-bulk-apikey contributors

</div>