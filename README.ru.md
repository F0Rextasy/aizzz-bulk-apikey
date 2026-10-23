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

**Массовое создание аккаунтов aizzz.shop (ZZ 中转站) с API-ключами — вывод, совместимый с OpenAI, + коннектор 9Router.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-undefined%20served-412991?style=for-the-badge)](https://aizzz.shop)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![OmniRoute](https://img.shields.io/badge/OmniRoute-connector-8B5CF6?style=for-the-badge)](#-connect-to-omniroute)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Быстрый старт](#-quick-start) · [🎁 Бесплатный тариф](#-free-tier--how-much-you-get) · [🤖 Модели](#-models--all-undefined-of-them) · [🧪 Использование API](#-how-to-use-the-api) · [✅ Проверено](#-verified-working--every-key-is-tested) · [🧭 Как это работает](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [🔌 OmniRoute](#-connect-to-omniroute) · [⚙️ Опции](#️-cli-options) · [🛰️ Прокси](#️-behind-a-proxy) · [🐞 Проблемы](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Зачем это нужно

aizzz.shop (ZZ 中转站) — это панель **New API** (`aizzz.shop`) с регистрацией по коду из письма, открытой регистрацией по паролю, без капчи и с выводом, совместимым с OpenAI, по адресу `https://aizzz.shop/v1`. Полезная единица — *один аккаунт на ключ*, поэтому этот инструмент превращает это в `keys.txt`, где **каждая строка прошла реальный чат-запрос** и проверку `tools`. Регистрация закрыта для публичных почтовых ящиков, которые поставляет этот инструмент, поэтому парк — это то, что выживет; CLI всё равно пытается и печатает ответ панели дословно.

## 🎁 Бесплатный тариф — сколько вы получаете

| Что | Значение |
| --- | --- |
| Кредит при регистрации | ~260 000 квоты ≈ **$0.52** (`quota_per_unit` 500 000) |
| Ежедневная отметка | **Да** (`POST /api/user/checkin`, замерено +11 632 за один вызов) |
| Требуется карта | **Нет** |
| Капча | **Отключена** (`turnstile_check=false`) |
| E-mail | код, **белый список доменов** (gmail.com / outlook.com отклоняются) |

Без карты, без капчи: ключ стоит один адрес, который принимает панель.

> 💡 Бесплатный тариф задаётся aizzz.shop (ZZ 中转站) и может измениться. Проверяйте актуальные цифры на [aizzz.shop](https://aizzz.shop) — и пусть собственный шлюз инструмента решает:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Модели — все undefined из них

Каталог выдаётся по группам токенов. Проверенные живые id: `deepseek-v4-flash`, `deepseek-v4.1-flash`, `glm-5.2`. `deepseek-v4-flash` — это модель по умолчанию в CLI и модель, используемая для каждой проверки в этом репозитории.

## 🧪 Как использовать API

Базовый URL — **`https://aizzz.shop/v1`** — совместим с OpenAI, поэтому работает всё, что говорит на языке OpenAI.

```bash
curl https://aizzz.shop/v1/chat/completions \
  -H "Authorization: Bearer <your key>" \
  -H "Content-Type: application/json" \
  -d '{"model":"deepseek-v4-flash","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🛠 Быстрый старт

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

`keys.txt` записывается **только** после того, как живое завершение чата подтвердит ключ.

### 🖱️ Лаунчер

Лаунчер запрашивает список прокси, количество и модель, запоминает их в `launcher.config.json` и запускает CLI с нужными флагами.

## ✅ Проверено в работе — каждый ключ протестирован

| Проверка | Результат |
| --- | --- |
| `https://aizzz.shop/v1/models` с ключом | **200** — реальный каталог |
| два живых вызова `POST /chat/completions` на ключ | **200**, непустое содержимое |
| `tools` — один вызов инструмента `get_time`, `tool_choice: auto` | **get_time tool_call returned** |
| этот запуск (`preadd-gate.js --pool`, fail-closed exits) | **1 live / 37 keys tried** (34 dead) |
| ключ, не прошедший чат-проверку | никогда не записывается в `keys.txt` |

> **Контроль качества.** Каждый ключ здесь прошёл живой `chat-200` до того, как был записан в `keys.txt`
> (мёртвые ключи никогда не поставляются). Комбинации роутера следуют тому же правилу на уровне модели: запись
> добавляется **только если модель отвечает `chat-200` и умеет вызывать инструменты** — модель без поддержки инструментов,
> мёртвый ключ или мёртвый провайдер отбрасывается, а не добавляется. Перезапускайте шаг проверки в любое время;
> сбои сообщаются, а не скрываются.

## 🧭 Как это работает

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: aizzz.shop (ZZ 中转站) page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://aizzz.shop/v1/chat/completions → real content = the key is live
```

| Шаг | Вызов | Примечания |
| --- | --- | --- |
| код | `GET /api/verification?email=<addr>` | панель отвечает вердиктом по белому списку дословно |
| регистрация | `POST /api/user/register` | Turnstile отключён; 6-символьный код приходит по почте |
| вход | `POST /api/user/login` | `data.access_token` (JWT) — этот форк **не** использует сессионную cookie |
| кредит | `POST /api/user/checkin` | ежедневно, бесплатно |
| токен | `POST /api/token/` с `{"group":"default"}` | токен вне обслуживающей группы отвечает 403 |
| ключ | `POST /api/token/<id>/key` | список маскирует ключ, а endpoint раскрытия — нет |
| проверка | `POST /v1/chat/completions` и вызов `tools` | живой 200 + tool_call обязательны до `keys.txt` |

## ⚙️ Опции CLI

```bash
node src/cli.js --count N            # mint N accounts (default 1)
node src/cli.js --revive             # re-login accounts.jsonl, check in, mint a fresh key
node src/cli.js --verify             # re-check every key in keys.txt
node src/cli.js --pool proxies.txt   # rotate exits, one exit per account
node src/cli.js --connect-9router    # register the provider on a local 9Router
node src/cli.js --help
```

| Переменная окружения | По умолчанию | Значение |
| --- | --- | --- |
| `AIZZZ_BASE` | `https://aizzz.shop` | базовый URL панели |
| `AIZZZ_PROXIES` | – | необязательный файл пула, один выход на строку |

## 🛰️ За прокси

**Необязательно, рекомендуется при масштабе** (ограничения по IP и блокировки). Передайте `--pool <file>` с одним выходом на строку (`http://`, `https://` CONNECT или `socks5://`); каждый аккаунт использует следующий выход, а мёртвая строка проваливает только эту попытку. Без пула инструмент подключается напрямую.

## 📄 Файлы

| Путь | Содержимое |
| --- | --- |
| `keys.txt` | один проверенный в работе ключ на строку — игнорируется git |
| `accounts.jsonl` | адрес / имя пользователя / пароль / ключ / статус чата для каждого аккаунта |
| `proxies.txt` | ваши выходы, один на строку — игнорируется git |

## 🔌 Подключение к 9Router

[9Router](https://9router.com) — это самостоятельно размещаемый роутер, совместимый с OpenAI. Зарегистрируйте этого провайдера один раз, и каждый ключ станет соединением под одним узлом-провайдером:

```bash
node src/connect-9router.js
```

По умолчанию: `--base http://localhost:20128`, `--password 123456`, `--name "aizzz.shop (

## 🔌 Подключение к OmniRoute

[OmniRoute](https://omniroute.online) — это самостоятельно размещаемый роутер, совместимый с OpenAI. Зарегистрируйте этого провайдера один раз, и каждый ключ станет соединением под одним узлом-провайдером:

```bash
node src/connect-omniroute.js
```

По умолчанию: `--base http://localhost:20128`, `--password 123456`, `--name "aizzz.shop (

ZZ 中转站)"`, `--prefix aizzz`, `--base-url https://aizzz.shop/v1`. Затем модели выдаются как `aizzz/<model id>` и балансируются между всеми ключами.

## 🔑 Что даёт один ключ

| | |
| --- | --- |
| 🏢 Провайдер | **aizzz.shop (ZZ 中转站)** — [aizzz.shop](https://aizzz.shop) |
| 🎁 Что даёт провайдер | один аккаунт на ключ — каждый ключ отвечает на живой чат-запрос |
| 🔑 Что даёт один ключ | один API-ключ, привязанный к своему аккаунту: полные id моделей на endpoint ниже, в пределах лимитов вашего тарифа. Квота живёт на **аккаунте**, и именно поэтому этот инструмент создаёт один аккаунт на ключ |
| 🌐 Форма API | Совместим с OpenAI: `POST https://aizzz.shop/v1/chat/completions` |
| 🔒 Ваши данные | связь только с aizzz.shop и настроенным вами бэкендом почтового ящика; ключи остаются на вашей машине |

## 🐞 Проблемы

Если панель изменит форму регистрации, группы или кредит, инструмент останавливается на этом шаге и печатает ответ панели дословно. **[Откройте issue](../../issues)** с командой, которую вы запустили, и точным выводом; PR приветствуются.

## 📄 Лицензия

MIT © 2026 — см. [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 aizzz-bulk-apikey contributors

</div>