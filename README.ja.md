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

**aizzz.shop (ZZ 中转站) のアカウントを API キー付きで一括作成 — OpenAI 互換の推論 + 9Router コネクタ。**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-undefined%20served-412991?style=for-the-badge)](https://aizzz.shop)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![OmniRoute](https://img.shields.io/badge/OmniRoute-connector-8B5CF6?style=for-the-badge)](#-connect-to-omniroute)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 クイックスタート](#-quick-start) · [🎁 無料枠](#-free-tier--how-much-you-get) · [🤖 モデル](#-models--all-undefined-of-them) · [🧪 API の使い方](#-how-to-use-the-api) · [✅ 検証済み](#-verified-working--every-key-is-tested) · [🧭 仕組み](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [🔌 OmniRoute](#-connect-to-omniroute) · [⚙️ オプション](#️-cli-options) · [🛰️ プロキシ](#️-behind-a-proxy) · [🐞 イシュー](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ これが存在する理由

aizzz.shop (ZZ 中转站) は **New API** パネル (`aizzz.shop`) で、メールコードによるサインアップ、パスワード登録が開放されており、キャプチャはなく、OpenAI 互換の `https://aizzz.shop/v1` 推論を提供しています。有用な単位は *キーごとに 1 アカウント* なので、このツールはそれを `keys.txt` に変換します。そこでは **すべての行が実際のチャット呼び出しと `tools` プローブを通過した** ものです。登録はこのツールが同梱するパブリックメールボックスには閉じられているため、生き残ったものがそのままフリートとなります。CLI はそれでも試行し、パネルの応答をそのまま出力します。

## 🎁 無料枠 — どれだけ得られるか

| 項目 | 値 |
| --- | --- |
| サインアップクレジット | 約 260 000 クォータ ≈ **$0.52** (`quota_per_unit` 500 000) |
| デイリーチェックイン | **あり** (`POST /api/user/checkin`、1 回の呼び出しで +11 632 を計測) |
| カード必須 | **いいえ** |
| キャプチャ | **オフ** (`turnstile_check=false`) |
| メール | コード、**ドメイン許可リスト** (gmail.com / outlook.com は拒否される) |

カード不要、キャプチャ不要: キーのコストはパネルが受け入れるアドレス 1 つ分です。

> 💡 無料枠は aizzz.shop (ZZ 中转站) によって設定されており、変更される可能性があります。最新の数値は [aizzz.shop](https://aizzz.shop) で確認してください — そしてツール自身のゲートに判断を委ねてください:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 モデル — そのすべてが undefined

カタログはトークングループごとに提供されます。検証済みのライブ ID: `deepseek-v4-flash`、`deepseek-v4.1-flash`、`glm-5.2`。`deepseek-v4-flash` は CLI のデフォルトであり、このリポジトリのすべての検証で使用されるモデルです。

## 🧪 API の使い方

ベース URL は **`https://aizzz.shop/v1`** です — OpenAI 互換なので、OpenAI を扱えるものなら何でも動作します。

```bash
curl https://aizzz.shop/v1/chat/completions \
  -H "Authorization: Bearer <your key>" \
  -H "Content-Type: application/json" \
  -d '{"model":"deepseek-v4-flash","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🛠 クイックスタート

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

`keys.txt` は、実際のチャット補完がキーを証明した **後にのみ** 書き込まれます。

### 🖱️ ランチャー

ランチャーはプロキシリスト、件数、モデルを尋ね、それらを `launcher.config.json` に記憶し、適切なフラグを付けて CLI を実行します。

## ✅ 動作検証済み — すべてのキーがテストされています

| チェック | 結果 |
| --- | --- |
| キーを使った `https://aizzz.shop/v1/models` | **200** — 実際のカタログ |
| キーごとに 2 回のライブ `POST /chat/completions` 呼び出し | **200**、空でないコンテンツ |
| `tools` — 1 回の `get_time` ツール呼び出し、`tool_choice: auto` | **get_time tool_call が返された** |
| この実行 (`preadd-gate.js --pool`、fail-closed で終了) | **1 ライブ / 37 キー試行** (34 デッド) |
| チャットチェックに失敗したキー | `keys.txt` に書き込まれることはない |

> **品質ゲート。** ここにあるすべてのキーは、`keys.txt` に書き込まれる前にライブの `chat-200` を通過しています
> (デッドキーは決して出荷されません)。ルーターのコンボもモデルレベルで同じルールに従います: エントリは
> **モデルが `chat-200` に応答し、ツールを呼び出せる場合にのみ** 追加されます — ツールサポートのないモデル、
> デッドキー、デッドプロバイダは追加されずに除外されます。検証ステップはいつでも再実行できます;
> 失敗は報告され、決して隠されません。

## 🧭 仕組み

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: aizzz.shop (ZZ 中转站) page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://aizzz.shop/v1/chat/completions → real content = the key is live
```

| ステップ | 呼び出し | 備考 |
| --- | --- | --- |
| code | `GET /api/verification?email=<addr>` | パネルは許可リストの判定をそのまま返す |
| signup | `POST /api/user/register` | Turnstile はオフ; 6 文字のコードがメールで届く |
| sign-in | `POST /api/user/login` | `data.access_token` (JWT) — このフォークはセッションクッキーを **使用しない** |
| credit | `POST /api/user/checkin` | 毎日、無料 |
| token | `POST /api/token/` with `{"group":"default"}` | 提供グループ外のトークンは 403 を返す |
| key | `POST /api/token/<id>/key` | リストはキーをマスクするが、reveal エンドポイントはマスクしない |
| check | `POST /v1/chat/completions` and a `tools` call | `keys.txt` の前にライブ 200 + tool_call が必須 |

## ⚙️ CLI オプション

```bash
node src/cli.js --count N            # mint N accounts (default 1)
node src/cli.js --revive             # re-login accounts.jsonl, check in, mint a fresh key
node src/cli.js --verify             # re-check every key in keys.txt
node src/cli.js --pool proxies.txt   # rotate exits, one exit per account
node src/cli.js --connect-9router    # register the provider on a local 9Router
node src/cli.js --help
```

| 環境変数 | デフォルト | 意味 |
| --- | --- | --- |
| `AIZZZ_BASE` | `https://aizzz.shop` | パネルのベース URL |
| `AIZZZ_PROXIES` | – | オプションのプールファイル、1 行に 1 出口 |

## 🛰️ プロキシの背後で

**オプション、大規模運用では推奨** (IP ごとのレート制限と BAN)。`--pool <file>` に 1 行 1 出口 (`http://`、`https://` CONNECT、または `socks5://`) を渡します; 各アカウントは次の出口を使用し、デッドな行はその試行のみを失敗させます。プールなしの場合、ツールは直接接続します。

## 📄 ファイル

| パス | 内容 |
| --- | --- |
| `keys.txt` | 1 行に 1 つのライブ検証済みキー — git で無視される |
| `accounts.jsonl` | アカウントごとのアドレス / ユーザー名 / パスワード / キー / チャットステータス |
| `proxies.txt` | あなたの出口、1 行に 1 つ — git で無視される |

## 🔌 9Router に接続する

[9Router](https://9router.com) はセルフホスト型の OpenAI 互換ルーターです。このプロバイダを一度登録すれば、すべてのキーが 1 つのプロバイダノードの下の接続になります:

```bash
node src/connect-9router.js
```

デフォルト: `--base http://localhost:20128`、`--password 123456`、`--name "aizzz.shop (

## 🔌 OmniRoute に接続する

[OmniRoute](https://omniroute.online) はセルフホスト型の OpenAI 互換ルーターです。このプロバイダを一度登録すれば、すべてのキーが 1 つのプロバイダノードの下の接続になります:

```bash
node src/connect-omniroute.js
```

デフォルト: `--base http://localhost:20128`、`--password 123456`、`--name "aizzz.shop (

ZZ 中转站)"`、`--prefix aizzz`、`--base-url https://aizzz.shop/v1`。モデルはその後 `aizzz/<model id>` として提供され、すべてのキーにわたって負荷分散されます。

## 🔑 1 つのキーで得られるもの

| | |
| --- | --- |
| 🏢 プロバイダ | **aizzz.shop (ZZ 中转站)** — [aizzz.shop](https://aizzz.shop) |
| 🎁 プロバイダが提供するもの | キーごとに 1 アカウント — すべてのキーがライブチャット呼び出しに応答する |
| 🔑 1 つのキーで得られるもの | 自身のアカウントにスコープされた 1 つの API キー: 下記エンドポイントの完全なモデル ID を、あなたのプランの制限で利用可能。クォータは **アカウント** に存在し、まさにそれがこのツールがキーごとに 1 アカウントを作成する理由です |
| 🌐 API の形 | OpenAI 互換: `POST https://aizzz.shop/v1/chat/completions` |
| 🔒 あなたのデータ | 接触するのは aizzz.shop と、あなたが設定した inbox バックエンドのみ; キーはあなたのマシン上に留まります |

## 🐞 イシュー

パネルがサインアップの形、グループ、クレジットを変更した場合、ツールはそのステップで停止し、パネルの応答をそのまま出力します。実行したコマンドと正確な出力を添えて **[イシューを開いてください](../../issues)**; PR も歓迎します。

## 📄 ライセンス

MIT © 2026 — [LICENSE](LICENSE) を参照してください。

<div align="center">

**[MIT](LICENSE)** © 2026 aizzz-bulk-apikey contributors

</div>