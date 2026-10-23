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

**Crie contas em massa no aizzz.shop (ZZ 中转站) com chaves de API — inferência compatível com OpenAI + conector 9Router.**

[![Node](https://img.shields.io/badge/Node-%E2%89%A518-3DA639?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org)
[![Dependencies](https://img.shields.io/badge/dependencies-0-blue?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-MIT-6ea8fe?style=for-the-badge)](LICENSE)
[![Models](https://img.shields.io/badge/models-undefined%20served-412991?style=for-the-badge)](https://aizzz.shop)
[![9Router](https://img.shields.io/badge/9Router-connector-8B5CF6?style=for-the-badge)](#-connect-to-9router)
[![OmniRoute](https://img.shields.io/badge/OmniRoute-connector-8B5CF6?style=for-the-badge)](#-connect-to-omniroute)
[![X](https://img.shields.io/badge/X-%40bigbullapp-000000?style=for-the-badge&logo=x&logoColor=white)](https://x.com/bigbullapp)

[🛠 Início rápido](#-quick-start) · [🎁 Nível gratuito](#-free-tier--how-much-you-get) · [🤖 Modelos](#-models--all-undefined-of-them) · [🧪 Use a API](#-how-to-use-the-api) · [✅ Verificado](#-verified-working--every-key-is-tested) · [🧭 Como funciona](#-how-it-works) · [🔌 9Router](#-connect-to-9router) · [🔌 OmniRoute](#-connect-to-omniroute) · [⚙️ Opções](#️-cli-options) · [🛰️ Proxies](#️-behind-a-proxy) · [🐞 Problemas](#-issues)

</div>

<div align="center">

**English** · [Türkçe](README.tr.md) · [中文](README.cn.md) · [日本語](README.ja.md) · [한국어](README.ko.md) · [Español](README.es.md) · [Français](README.fr.md) · [Deutsch](README.de.md) · [Русский](README.ru.md) · [Português](README.pt.md)

</div>

---

## ✨ Por que isto existe

aizzz.shop (ZZ 中转站) é um painel **New API** (`aizzz.shop`) com cadastro por código de e-mail, registro por senha aberto, sem captcha e inferência compatível com OpenAI em `https://aizzz.shop/v1`. A unidade útil é *uma conta por chave*, então esta ferramenta transforma isso em um `keys.txt` onde **cada linha sobreviveu a uma chamada de chat real** e a uma sondagem de `tools`. O cadastro está fechado para as caixas de correio públicas que esta ferramenta envia, então a frota é o que sobreviver; a CLI ainda tenta e imprime a resposta do painel literalmente.

## 🎁 Nível gratuito — quanto você recebe

| O quê | Valor |
| --- | --- |
| Crédito de cadastro | ~260 000 de cota ≈ **$0.52** (`quota_per_unit` 500 000) |
| Check-in diário | **Sim** (`POST /api/user/checkin`, medido +11 632 em uma chamada) |
| Cartão exigido | **Não** |
| Captcha | **Desativado** (`turnstile_check=false`) |
| E-mail | código, **lista de domínios permitidos** (gmail.com / outlook.com são recusados) |

Sem cartão, sem captcha: uma chave custa um endereço que o painel aceita.

> 💡 O nível gratuito é definido pelo aizzz.shop (ZZ 中转站) e pode mudar. Confira os números ao vivo em [aizzz.shop](https://aizzz.shop) — e deixe o próprio portão da ferramenta decidir:

```bash
node src/cli.js --verify-only     # every key in keys.txt is re-tested with a real chat call
```

## 🤖 Modelos — todos os undefined deles

O catálogo é servido por grupo de token. IDs verificados ao vivo: `deepseek-v4-flash`, `deepseek-v4.1-flash`, `glm-5.2`. `deepseek-v4-flash` é o padrão da CLI e o modelo usado para toda verificação neste repositório.

## 🧪 Como usar a API

A URL base é **`https://aizzz.shop/v1`** — compatível com OpenAI, então qualquer coisa que fale OpenAI funciona.

```bash
curl https://aizzz.shop/v1/chat/completions \
  -H "Authorization: Bearer <your key>" \
  -H "Content-Type: application/json" \
  -d '{"model":"deepseek-v4-flash","messages":[{"role":"user","content":"hi"}],"max_tokens":8}'
```

## 🛠 Início rápido

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

`keys.txt` é escrito **somente** depois que uma conclusão de chat ao vivo comprova a chave.

### 🖱️ O launcher

O launcher pede a lista de proxies, a contagem e o modelo, memoriza-os em `launcher.config.json` e executa a CLI com os flags corretos.

## ✅ Verificado e funcionando — toda chave é testada

| Verificação | Resultado |
| --- | --- |
| `https://aizzz.shop/v1/models` com a chave | **200** — catálogo real |
| duas chamadas ao vivo de `POST /chat/completions` por chave | **200**, conteúdo não vazio |
| `tools` — uma chamada de ferramenta `get_time`, `tool_choice: auto` | **get_time tool_call retornado** |
| esta execução (`preadd-gate.js --pool`, saídas fail-closed) | **1 ao vivo / 37 chaves tentadas** (34 mortas) |
| uma chave que falha na verificação de chat | nunca escrita em `keys.txt` |

> **Portões de qualidade.** Toda chave aqui passou por um `chat-200` ao vivo antes de ser escrita em `keys.txt`
> (chaves mortas nunca são entregues). As combinações do roteador seguem a mesma regra no nível do modelo: uma entrada é
> adicionada **somente se o modelo responder `chat-200` e puder chamar ferramentas** — um modelo sem suporte a ferramentas,
> uma chave morta ou um provedor morto é descartado em vez de adicionado. Reexecute a etapa de verificação a qualquer momento;
> as falhas são relatadas, nunca escondidas.

## 🧭 Como funciona

```
inbox (one address per account)                                  a fresh address per account
   │
   ├─▶ sign-up: aizzz.shop (ZZ 中转站) page → e-mail address → code / link from the inbox
   ├─▶ session: the account's cookies or access token in hand
   ├─▶ key mint: the account's key endpoint → plaintext key returned once (written to disk immediately)
   └─▶ live proof: POST https://aizzz.shop/v1/chat/completions → real content = the key is live
```

| Etapa | Chamada | Notas |
| --- | --- | --- |
| código | `GET /api/verification?email=<addr>` | o painel responde o veredito da lista de permissões literalmente |
| cadastro | `POST /api/user/register` | O Turnstile está desativado; um código de 6 caracteres chega por e-mail |
| login | `POST /api/user/login` | `data.access_token` (JWT) — este fork **não** usa cookie de sessão |
| crédito | `POST /api/user/checkin` | diário, gratuito |
| token | `POST /api/token/` com `{"group":"default"}` | um token fora de um grupo de serviço responde 403 |
| chave | `POST /api/token/<id>/key` | a lista mascara a chave, o endpoint de revelação não |
| verificação | `POST /v1/chat/completions` e uma chamada de `tools` | 200 ao vivo + tool_call exigidos antes de `keys.txt` |

## ⚙️ Opções da CLI

```bash
node src/cli.js --count N            # mint N accounts (default 1)
node src/cli.js --revive             # re-login accounts.jsonl, check in, mint a fresh key
node src/cli.js --verify             # re-check every key in keys.txt
node src/cli.js --pool proxies.txt   # rotate exits, one exit per account
node src/cli.js --connect-9router    # register the provider on a local 9Router
node src/cli.js --help
```

| Variável de ambiente | Padrão | Significado |
| --- | --- | --- |
| `AIZZZ_BASE` | `https://aizzz.shop` | URL base do painel |
| `AIZZZ_PROXIES` | – | arquivo de pool opcional, uma saída por linha |

## 🛰️ Atrás de um proxy

**Opcional, recomendado em escala** (limites de taxa e banimentos por IP). Passe `--pool <file>` com uma saída por linha (`http://`, `https://` CONNECT ou `socks5://`); cada conta usa a próxima saída, e uma linha morta faz falhar apenas aquela tentativa. Sem um pool, a ferramenta se conecta diretamente.

## 📄 Arquivos

| Caminho | Conteúdo |
| --- | --- |
| `keys.txt` | uma chave verificada ao vivo por linha — ignorada pelo git |
| `accounts.jsonl` | endereço / nome de usuário / senha / chave / status de chat por conta |
| `proxies.txt` | suas saídas, uma por linha — ignorada pelo git |

## 🔌 Conectar ao 9Router

[9Router](https://9router.com) é um roteador auto-hospedado e compatível com OpenAI. Registre este provedor uma vez e cada chave se torna uma conexão sob um nó de provedor:

```bash
node src/connect-9router.js
```

Padrões: `--base http://localhost:20128`, `--password 123456`, `--name "aizzz.shop (

## 🔌 Conectar ao OmniRoute

[OmniRoute](https://omniroute.online) é um roteador auto-hospedado e compatível com OpenAI. Registre este provedor uma vez e cada chave se torna uma conexão sob um nó de provedor:

```bash
node src/connect-omniroute.js
```

Padrões: `--base http://localhost:20128`, `--password 123456`, `--name "aizzz.shop (

ZZ 中转站)"`, `--prefix aizzz`, `--base-url https://aizzz.shop/v1`. Os modelos são então servidos como `aizzz/<model id>` e balanceados entre todas as chaves.

## 🔑 O que uma chave oferece

| | |
| --- | --- |
| 🏢 Provedor | **aizzz.shop (ZZ 中转站)** — [aizzz.shop](https://aizzz.shop) |
| 🎁 O que o provedor oferece | uma conta por chave — toda chave responde a uma chamada de chat ao vivo |
| 🔑 O que uma chave oferece | uma chave de API limitada à sua própria conta: os IDs completos de modelos no endpoint abaixo, nos limites do seu plano. A cota fica na **conta**, que é exatamente por isso que esta ferramenta cria uma conta por chave |
| 🌐 Formato da API | Compatível com OpenAI: `POST https://aizzz.shop/v1/chat/completions` |
| 🔒 Seus dados | apenas aizzz.shop e o backend de caixa de entrada que você configurar são contatados; as chaves ficam na sua máquina |

## 🐞 Problemas

Se o painel mudar o formato do cadastro, os grupos ou o crédito, a ferramenta para nessa etapa e imprime a resposta do painel literalmente. **[Abra uma issue](../../issues)** com o comando que você executou e a saída exata; PRs são bem-vindos.

## 📄 Licença

MIT © 2026 — veja [LICENSE](LICENSE).

<div align="center">

**[MIT](LICENSE)** © 2026 aizzz-bulk-apikey contributors

</div>