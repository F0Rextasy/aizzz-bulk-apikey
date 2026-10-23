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

**aizzz.shop (ZZ 中转站) hesaplarını API anahtarlarıyla toplu oluşturun — OpenAI uyumlu çıkarım + 9Router bağlayıcısı.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-undefined%20served-412991?style=for-the-badge)](https://aizzz.shop)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![OmniRoute](https://img.shields.io/badge/OmniRoute-connector-8B5CF6?style=for-the-badge)](#-connect-to-omniroute)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Hızlı başlangıç](#-quick-start) · [🎁 Ücretsiz katman](#-free-tier--how-much-you-get) · [🤖 Modeller](#-models--all-undefined-of-them) · [🧪 API'yi kullanın](#-how-to-use-the-api) · [✅ Doğrulandı](#-verified-working--every-key-is-tested) · [🧭 Nasıl çalışır](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [🔌 OmniRoute](#-connect-to-omniroute) · [⚙️ Seçenekler](#️-cli-options) · [🛰️ Proxy'ler](#️-behind-a-proxy) · [🐞 Sorunlar](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Bu neden var

aizzz.shop (ZZ 中转站), e-posta koduyla kayıt, açık şifre kaydı, captcha yok ve OpenAI uyumlu `https://aizzz.shop/v1` çıkarımı sunan bir **New API** panelidir (`aizzz.shop`). Kullanışlı birim *anahtar başına bir hesap*tır, bu yüzden bu araç bunu **her satırı gerçek bir sohbet çağrısından geçmiş** bir `keys.txt` dosyasına ve bir `tools` yoklamasına dönüştürür. Kayıt, bu aracın gönderdiği genel posta kutularına kapalıdır, dolayısıyla filo hayatta kalan kadardır; CLI yine de dener ve panelin yanıtını birebir yazdırır.

## 🎁 Ücretsiz katman — ne kadar alıyorsunuz

| Ne | Değer |
| --- | --- |
| Kayıt kredisi | ~260 000 kota ≈ **$0.52** (`quota_per_unit` 500 000) |
| Günlük giriş | **Evet** (`POST /api/user/checkin`, tek çağrıda +11 632 ölçüldü) |
| Kart gerekli mi | **Hayır** |
| Captcha | **Kapalı** (`turnstile_check=false`) |
| E-posta | kod, **alan adı izin listesi** (gmail.com / outlook.com reddedilir) |

Kart yok, captcha yok: bir anahtar, panelin kabul ettiği bir adrese mal olur.

> 💡 Ücretsiz katman aizzz.shop (ZZ 中转站) tarafından belirlenir ve değişebilir. Güncel sayıları [aizzz.shop](https://aizzz.shop) adresinden kontrol edin — ve kararı aracın kendi kapısına bırakın:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Modeller — hepsi undefined adet

Katalog, token grubu başına sunulur. Canlı doğrulanmış kimlikler: `deepseek-v4-flash`, `deepseek-v4.1-flash`, `glm-5.2`. `deepseek-v4-flash`, CLI'nin varsayılanıdır ve bu depodaki her doğrulamada kullanılan modeldir.

## 🧪 API nasıl kullanılır

Temel URL **`https://aizzz.shop/v1`** — OpenAI uyumlu, yani OpenAI konuşan her şey çalışır.

```bash
curl https://aizzz.shop/v1/chat/completions \
  -H "Authorization: Bearer <your key>" \
  -H "Content-Type: application/json" \
  -d '{"model":"deepseek-v4-flash","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🛠 Hızlı başlangıç

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

`keys.txt` **yalnızca** canlı bir sohbet tamamlaması anahtarı kanıtladıktan sonra yazılır.

### 🖱️ Başlatıcı

Başlatıcı proxy listesini, sayıyı ve modeli sorar, bunları `launcher.config.json` içinde hatırlar ve CLI'yi doğru bayraklarla çalıştırır.

## ✅ Çalıştığı doğrulandı — her anahtar test edilir

| Kontrol | Sonuç |
| --- | --- |
| Anahtarla `https://aizzz.shop/v1/models` | **200** — gerçek katalog |
| Anahtar başına iki canlı `POST /chat/completions` çağrısı | **200**, boş olmayan içerik |
| `tools` — bir `get_time` araç çağrısı, `tool_choice: auto` | **get_time tool_call döndü** |
| bu çalıştırma (`preadd-gate.js --pool`, fail-closed çıkışlar) | **1 canlı / 37 anahtar denendi** (34 ölü) |
| sohbet kontrolünden geçemeyen bir anahtar | asla `keys.txt` dosyasına yazılmaz |

> **Kalite kapıları.** Buradaki her anahtar, `keys.txt` dosyasına yazılmadan önce canlı bir `chat-200` testinden geçti
> (ölü anahtarlar asla gönderilmez). Yönlendirici kombinasyonları model düzeyinde aynı kuralı izler: bir giriş
> **yalnızca model `chat-200` yanıtlıyorsa ve araçları çağırabiliyorsa** eklenir — araç desteği olmayan bir model,
> ölü bir anahtar veya ölü bir sağlayıcı eklenmek yerine çıkarılır. Doğrulama adımını istediğiniz zaman yeniden çalıştırın;
> hatalar bildirilir, asla gizlenmez.

## 🧭 Nasıl çalışır

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: aizzz.shop (ZZ 中转站) page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://aizzz.shop/v1/chat/completions → real content = the key is live
```

| Adım | Çağrı | Notlar |
| --- | --- | --- |
| kod | `GET /api/verification?email=<addr>` | panel, izin listesi kararını birebir yanıtlar |
| kayıt | `POST /api/user/register` | Turnstile kapalı; postayla 6 karakterli bir kod gelir |
| giriş | `POST /api/user/login` | `data.access_token` (JWT) — bu çatal oturum çerezi **kullanmaz** |
| kredi | `POST /api/user/checkin` | günlük, ücretsiz |
| token | `POST /api/token/` ile `{"group":"default"}` | hizmet veren bir grup dışındaki token 403 yanıtlar |
| anahtar | `POST /api/token/<id>/key` | liste anahtarı maskeler, açığa çıkarma uç noktası maskelemez |
| kontrol | `POST /v1/chat/completions` ve bir `tools` çağrısı | `keys.txt` öncesinde canlı 200 + tool_call gerekli |

## ⚙️ CLI seçenekleri

```bash
node src/cli.js --count N            # mint N accounts (default 1)
node src/cli.js --revive             # re-login accounts.jsonl, check in, mint a fresh key
node src/cli.js --verify             # re-check every key in keys.txt
node src/cli.js --pool proxies.txt   # rotate exits, one exit per account
node src/cli.js --connect-9router    # register the provider on a local 9Router
node src/cli.js --help
```

| Ortam değişkeni | Varsayılan | Anlamı |
| --- | --- | --- |
| `AIZZZ_BASE` | `https://aizzz.shop` | panel temel URL'si |
| `AIZZZ_PROXIES` | – | isteğe bağlı havuz dosyası, satır başına bir çıkış |

## 🛰️ Proxy arkasında

**İsteğe bağlı, ölçekte önerilir** (IP başına hız sınırları ve yasaklamalar). Satır başına bir çıkış içeren `--pool <file>` iletin (`http://`, `https://` CONNECT veya `socks5://`); her hesap bir sonraki çıkışı kullanır ve ölü bir satır yalnızca o denemeyi başarısız kılar. Havuz olmadan araç doğrudan bağlanır.

## 📄 Dosyalar

| Yol | İçerik |
| --- | --- |
| `keys.txt` | satır başına bir canlı doğrulanmış anahtar — git tarafından yok sayılır |
| `accounts.jsonl` | hesap başına adres / kullanıcı adı / şifre / anahtar / sohbet durumu |
| `proxies.txt` | çıkışlarınız, satır başına bir tane — git tarafından yok sayılır |

## 🔌 9Router'a bağlan

[9Router](https://9router.com) kendi barındırdığınız, OpenAI uyumlu bir yönlendiricidir. Bu sağlayıcıyı bir kez kaydedin ve her anahtar tek bir sağlayıcı düğümü altında bir bağlantı haline gelsin:

```bash
node src/connect-9router.js
```

Varsayılanlar: `--base http://localhost:20128`, `--password 123456`, `--name "aizzz.shop (

## 🔌 OmniRoute'a bağlan

[OmniRoute](https://omniroute.online) kendi barındırdığınız, OpenAI uyumlu bir yönlendiricidir. Bu sağlayıcıyı bir kez kaydedin ve her anahtar tek bir sağlayıcı düğümü altında bir bağlantı haline gelsin:

```bash
node src/connect-omniroute.js
```

Varsayılanlar: `--base http://localhost:20128`, `--password 123456`, `--name "aizzz.shop (

ZZ 中转站)"`, `--prefix aizzz`, `--base-url https://aizzz.shop/v1`. Modeller daha sonra `aizzz/<model id>` olarak sunulur ve her anahtar arasında yük dengelenir.

## 🔑 Bir anahtar size ne kazandırır

| | |
| --- | --- |
| 🏢 Sağlayıcı | **aizzz.shop (ZZ 中转站)** — [aizzz.shop](https://aizzz.shop) |
| 🎁 Sağlayıcının verdikleri | anahtar başına bir hesap — her anahtar canlı bir sohbet çağrısını yanıtlar |
| 🔑 Bir anahtarın size kazandırdıkları | kendi hesabına kapsanmış bir API anahtarı: aşağıdaki uç noktadaki tam model kimlikleri, planınızın sınırlarında. Kota **hesapta** yaşar, bu aracın anahtar başına bir hesap oluşturmasının nedeni tam da budur |
| 🌐 API şekli | OpenAI uyumlu: `POST https://aizzz.shop/v1/chat/completions` |
| 🔒 Verileriniz | yalnızca aizzz.shop ve yapılandırdığınız gelen kutusu arka ucuyla iletişim kurulur; anahtarlar makinenizde kalır |

## 🐞 Sorunlar

Panel kayıt şeklini, gruplarını veya kredisini değiştirirse, araç o adımda durur ve panelin yanıtını birebir yazdırır. Çalıştırdığınız komut ve tam çıktıyla birlikte **[bir sorun açın](../../issues)**; PR'lar memnuniyetle karşılanır.

## 📄 Lisans

MIT © 2026 — bkz. [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 aizzz-bulk-apikey katkıda bulunanlar

</div>