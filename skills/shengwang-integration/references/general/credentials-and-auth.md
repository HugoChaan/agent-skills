# Agora Credentials & Authentication

Cross-product knowledge shared by all Agora products.

## Credentials

| Credential | Where to find | Used for |
|------------|--------------|---------|
| `AGORA_APP_ID` | Console → Project Overview | All API calls and SDK init |
| `AGORA_CUSTOMER_KEY` | Console → Settings → RESTful API | REST API Basic Auth username |
| `AGORA_CUSTOMER_SECRET` | Console → Settings → RESTful API | REST API Basic Auth password |
| `AGORA_APP_CERTIFICATE` | Console → Project Overview | Token generation (only if enabled) |

Console: https://console.shengwang.cn/

### Environment Variables

```bash
AGORA_APP_ID=your_app_id
AGORA_CUSTOMER_KEY=your_customer_key
AGORA_CUSTOMER_SECRET=your_customer_secret
AGORA_APP_CERTIFICATE=your_app_certificate   # only if App Certificate enabled
```

- ALWAYS read from env vars — never hardcode
- NEVER put secrets in client-side code
- NEVER commit `.env` with real values

### Service Activation

Some products require extra activation in Console beyond having credentials:

| Product | Extra requirement |
|---------|------------------|
| ConvoAI | Enable ConvoAI service (403 if not done) |
| Cloud Recording | Enable Cloud Recording service |
| RTC / RTM | No extra activation needed |

## REST API Authentication

Agora REST APIs 支持以下两种鉴权方式（任选其一）：

### 方式一：RTC Token（ConvoAI 等支持的产品）

使用声网对话式 AI 引擎项目的 RTC Token 进行鉴权：

```
Authorization: agora token="007abcxxxxxxx123"
```

**curl example:**
```bash
curl -H "Authorization: agora token=\"$RTC_TOKEN\"" \
     -H "Content-Type: application/json" \
     https://api.agora.io/...
```

Token 获取方式：
- 测试环境：从[声网控制台](https://console.shengwang.cn/)生成临时 Token（有效期 24 小时）
- 生产环境：部署 [token-server](../token-server/README.md) 生成 Token

### 方式二：Basic Auth

```
Authorization: Basic base64("{AGORA_CUSTOMER_KEY}:{AGORA_CUSTOMER_SECRET}")
```

**curl example:**
```bash
AUTH=$(echo -n "$AGORA_CUSTOMER_KEY:$AGORA_CUSTOMER_SECRET" | base64)
curl -H "Authorization: Basic $AUTH" \
     -H "Content-Type: application/json" \
     https://api.agora.io/...
```

> **注意：** 并非所有产品都支持 Token 鉴权，Cloud Recording 等产品仍仅支持 Basic Auth。具体支持情况请参考各产品模块文档。

For language-specific auth patterns (Go, Java, Python, Node.js), fetch the quick start docs for each product (see URLs in product module READMEs).

## RTC / RTM Token

Token generation is separate from REST auth. See [token-server](../token-server/README.md).

## Docs

Fetch docs using the doc fetching script (see [doc-fetching.md](../doc-fetching.md)):

| Topic | Command |
|-------|---------|
| Token authentication overview | `bash skills/shengwang-integration/scripts/fetch-doc-content.sh "docs://default/rtc/android/basic-features/token-authentication"` |

## Docs Fallback

If fetch fails: https://doc.shengwang.cn/doc/rtc/android/basic-features/token-authentication
