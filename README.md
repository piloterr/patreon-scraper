<p align="center">
  <a href="https://www.piloterr.com"><img src="images/banner.png" alt="Patreon Scraper API by Piloterr – scrape Patreon data as JSON" width="100%"></a>
</p>

# Patreon Scraper API

[![Discord](https://img.shields.io/badge/Discord-Join%20the%20community-5865F2?logo=discord&logoColor=white)](https://discord.gg/CFj3ph8aHU) [![Get API key](https://img.shields.io/badge/Piloterr-Get%20a%20free%20API%20key-111827)](https://app.piloterr.com) [![API docs](https://img.shields.io/badge/Docs-OpenAPI-0EA5E9)](https://app.piloterr.com/openapi) [![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

The **Piloterr Patreon Scraper API** lets you extract public data from Patreon as **clean, structured JSON** — no proxies, headless browsers or HTML parsing to maintain. Endpoints available in production:

- **Patreon Profile Info API** — Retrieve a Patreon creator profile with patron counts, membership tiers, campaign summary, and creator metadata.
- **Patreon Search API** — Search Patreon creators by keyword and retrieve campaign cards with patron counts, post counts, and profile URLs.

Use it for brand monitoring, social listening, influencer research, ad intelligence or content analytics on public data.

| Endpoint | Path | Credits | What you get |
|---|---|---|---|
| [Patreon Profile Info API](#patreon-profile-info-api) | `/v2/patreon/profile/info` | 1 | Retrieve a Patreon creator profile with patron counts, membership tiers, campaign summary, and creator metadata. |
| [Patreon Search API](#patreon-search-api) | `/v2/patreon/search` | 1 | Search Patreon creators by keyword and retrieve campaign cards with patron counts, post counts, and profile URLs. |

## Table of contents

- [How it works](#how-it-works)
- [Patreon Profile Info API](#patreon-profile-info-api)
- [Patreon Search API](#patreon-search-api)
- [Code examples in other languages](#code-examples-in-other-languages)
- [Practical use cases](#practical-use-cases)
- [Why Piloterr?](#why-piloterr)
- [FAQ](#faq)
- [Official SDKs](#official-sdks)
- [Related scrapers](#related-scrapers)
- [Support](#support)

## How it works

1. **Create a free Piloterr account** and copy your API key from the [dashboard](https://app.piloterr.com).
2. **Send a `GET` (or `POST`) request** to `https://api.piloterr.com/v2/...` with the header `x-api-key: YOUR_API_KEY`.
3. **Receive structured JSON** — Piloterr handles rendering, proxies, retries and parsing.

```bash
export PILOTERR_API_KEY="your-api-key"
pip install requests        # Python
# TypeScript: Node.js 18+ (native fetch), run with `npx tsx file.ts`
```

> Every endpoint accepts both `GET` (query string) and `POST` (JSON body with the same fields).

## Patreon Profile Info API

Retrieve a Patreon creator profile with patron counts, membership tiers, campaign summary, and creator metadata.

`GET | POST https://api.piloterr.com/v2/patreon/profile/info` · **1 credit per request** · [Full reference](https://app.piloterr.com/openapi)

### Request parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `query` | string | ✅ | Patreon profile URL or creator vanity name. |

### Python example

```python
import json
import os
import requests

response = requests.get(
    "https://api.piloterr.com/v2/patreon/profile/info",
    headers={"x-api-key": os.environ["PILOTERR_API_KEY"]},
    params={
        "query": "https://www.patreon.com/example-user",
    },
    timeout=120,
)
response.raise_for_status()
data = response.json()

print(json.dumps(data, indent=2, ensure_ascii=False)[:2000])
```

### TypeScript example

```typescript
type PatreonProfileInfo = {
  id?: string;
  url?: string;
  name?: string;
  cover?: string;
  avatar?: string;
  creator?: {
    id?: string;
    url?: string;
    about?: unknown;
    avatar?: string;
    full_name?: string;
    social_connections?: unknown;
  };
  is_nsfw?: boolean;
  rewards?: {
    id?: string;
    url?: unknown;
    title?: unknown;
    published?: unknown;
    description?: string;
    amount_cents?: number;
    patron_count?: unknown;
  }[];
  summary?: string;
  currency?: string;
  one_liner?: unknown;
  created_at?: string;
  is_monthly?: boolean;
  patron_count?: number;
  published_at?: string;
  creation_name?: string;
  creation_count?: number;
  paid_member_count?: number;
};

const params = new URLSearchParams({
  "query": "https://www.patreon.com/example-user",
});

const response = await fetch(`https://api.piloterr.com/v2/patreon/profile/info?${params}`, {
  headers: { "x-api-key": process.env.PILOTERR_API_KEY! },
});
if (!response.ok) throw new Error(`Piloterr error ${response.status}: ${await response.text()}`);

const data = (await response.json()) as PatreonProfileInfo;

console.log(JSON.stringify(data, null, 2).slice(0, 2000));
```

<details>
<summary><b>POST variant</b></summary>

```http
POST https://api.piloterr.com/v2/patreon/profile/info
x-api-key: YOUR_API_KEY
Content-Type: application/json

{"query": "https://www.patreon.com/example-user"}
```
</details>

### Output example

Response shape with placeholder values — field names follow the API reference.

```json
{
  "id": "example",
  "url": "example",
  "name": "example",
  "cover": "example",
  "avatar": "example",
  "creator": {
    "id": "example",
    "url": "example",
    "about": "example",
    "avatar": "example",
    "full_name": "example",
    "social_connections": "example"
  },
  "is_nsfw": false,
  "rewards": [
    {
      "id": "example",
      "url": "example",
      "title": "example",
      "published": "example",
      "description": "example",
      "amount_cents": 0,
      "patron_count": "example"
    }
  ],
  "summary": "example",
  "currency": "example",
  "one_liner": "example",
  "created_at": "example",
  "is_monthly": false,
  "patron_count": 0,
  "published_at": "example",
  "creation_name": "example",
  "creation_count": 0,
  "paid_member_count": 0
}
```

### Response fields

| Field | Type | Description |
|---|---|---|
| `id` | string |  |
| `url` | string |  |
| `name` | string |  |
| `cover` | string |  |
| `avatar` | string |  |
| `creator` | object |  |
| `creator.id` | string |  |
| `creator.url` | string |  |
| `creator.about` | object |  |
| `creator.avatar` | string |  |
| `creator.full_name` | string |  |
| `creator.social_connections` | object |  |
| `is_nsfw` | boolean |  |
| `rewards[]` | object[] |  |
| `rewards[].id` | string |  |
| `rewards[].url` | object |  |
| `rewards[].title` | object |  |
| `rewards[].published` | object |  |
| `rewards[].description` | string |  |
| `rewards[].amount_cents` | integer |  |
| `rewards[].patron_count` | object |  |
| `summary` | string |  |
| `currency` | string |  |
| `one_liner` | object |  |
| `created_at` | string |  |

## Patreon Search API

Search Patreon creators by keyword and retrieve campaign cards with patron counts, post counts, and profile URLs.

`GET | POST https://api.piloterr.com/v2/patreon/search` · **1 credit per request** · [Full reference](https://app.piloterr.com/openapi)

### Request parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `query` | string | ✅ | Search keyword to find Patreon creators. |

### Python example

```python
import json
import os
import requests

response = requests.get(
    "https://api.piloterr.com/v2/patreon/search",
    headers={"x-api-key": os.environ["PILOTERR_API_KEY"]},
    params={
        "query": "creator",
    },
    timeout=120,
)
response.raise_for_status()
data = response.json()

print(json.dumps(data, indent=2, ensure_ascii=False)[:2000])
```

### TypeScript example

```typescript
type PatreonSearch = {
  query?: string;
  results?: {
    id?: string;
    url?: string;
    name?: string;
    thumb?: string;
    avatar?: string;
    is_nsfw?: boolean;
    summary?: unknown;
    post_count?: number;
    creator_name?: string;
    patron_count?: unknown;
    creation_name?: string;
    primary_theme_color?: string;
  }[];
  pagination?: {
    next?: string;
    page?: number;
    prev?: string;
    hits_total?: number;
    pages_total?: number;
    has_next_page?: boolean;
    has_prev_page?: boolean;
  };
};

const params = new URLSearchParams({
  "query": "creator",
});

const response = await fetch(`https://api.piloterr.com/v2/patreon/search?${params}`, {
  headers: { "x-api-key": process.env.PILOTERR_API_KEY! },
});
if (!response.ok) throw new Error(`Piloterr error ${response.status}: ${await response.text()}`);

const data = (await response.json()) as PatreonSearch;

console.log(JSON.stringify(data, null, 2).slice(0, 2000));
```

<details>
<summary><b>POST variant</b></summary>

```http
POST https://api.piloterr.com/v2/patreon/search
x-api-key: YOUR_API_KEY
Content-Type: application/json

{"query": "creator"}
```
</details>

### Output example

Response shape with placeholder values — field names follow the API reference.

```json
{
  "query": "example",
  "results": [
    {
      "id": "example",
      "url": "example",
      "name": "example",
      "thumb": "example",
      "avatar": "example",
      "is_nsfw": false,
      "summary": "example",
      "post_count": 0,
      "creator_name": "example",
      "patron_count": "example",
      "creation_name": "example",
      "primary_theme_color": "example"
    }
  ],
  "pagination": {
    "next": "example",
    "page": 0,
    "prev": "example",
    "hits_total": 0,
    "pages_total": 0,
    "has_next_page": false,
    "has_prev_page": false
  }
}
```

### Response fields

| Field | Type | Description |
|---|---|---|
| `query` | string |  |
| `results[]` | object[] |  |
| `results[].id` | string |  |
| `results[].url` | string |  |
| `results[].name` | string |  |
| `results[].thumb` | string |  |
| `results[].avatar` | string |  |
| `results[].is_nsfw` | boolean |  |
| `results[].summary` | object |  |
| `results[].post_count` | integer |  |
| `results[].creator_name` | string |  |
| `results[].patron_count` | object |  |
| `results[].creation_name` | string |  |
| `results[].primary_theme_color` | string |  |
| `pagination` | object |  |
| `pagination.next` | string |  |
| `pagination.page` | integer |  |
| `pagination.prev` | string |  |
| `pagination.hits_total` | integer |  |
| `pagination.pages_total` | integer |  |
| `pagination.has_next_page` | boolean |  |
| `pagination.has_prev_page` | boolean |  |

## Code examples in other languages

Ready-to-run examples live in the [`examples/`](examples) folder. Each one reads your key from the `PILOTERR_API_KEY` environment variable.

| Language | Profile Info | Search |
|---|---|---|
| cURL (Bash) | [`profile-info`](examples/curl/profile_info.sh) | [`search`](examples/curl/search.sh) |
| PHP | [`profile-info`](examples/php/profile_info.php) | [`search`](examples/php/search.php) |
| C# | [`profile-info`](examples/csharp/ProfileInfo.cs) | [`search`](examples/csharp/Search.cs) |
| Go | [`profile-info`](examples/go/profile_info.go) | [`search`](examples/go/search.go) |
| Java | [`profile-info`](examples/java/ProfileInfoExample.java) | [`search`](examples/java/SearchExample.java) |
| Ruby | [`profile-info`](examples/ruby/profile_info.rb) | [`search`](examples/ruby/search.rb) |
| Node.js | [`profile-info`](examples/nodejs/profile_info.mjs) | [`search`](examples/nodejs/search.mjs) |

## Practical use cases

- **Brand monitoring** — Follow public mentions and engagement around your brand.
- **Social listening** — Analyse public conversations and trends.
- **Influencer research** — Qualify public creator profiles and audience metrics.
- **Ad intelligence** — Study competitors' public ads and creatives.
- **Content analytics** — Benchmark formats and engagement over time.

## Why Piloterr?

- **No infrastructure** — no proxies, headless browsers or CAPTCHA solving to run yourself.
- **Structured JSON** — stable, documented fields instead of brittle HTML selectors.
- **Pay per request** — transparent credit cost per endpoint, failed requests are retried for you.
- **One API key, hundreds of sources** — Patreon plus many other e-commerce, search and company data endpoints in the [Piloterr library](https://www.piloterr.com/library).
- **Any language** — plain HTTPS + JSON, with examples for Python, TypeScript, cURL, PHP, C#, Go, Java, Ruby and Node.js.

## FAQ

### Is it legal to scrape Patreon?

Collecting publicly available data is generally allowed in many jurisdictions, but rules depend on your country, the data you collect and how you use it. Avoid personal data unless you have a lawful basis, respect the website's terms where they apply, and get legal advice for your specific use case. This is not legal advice.

### Do I need proxies or a headless browser?

No. Piloterr handles rendering, proxy rotation and retries on its side. You only send an HTTPS request with your `x-api-key` header and receive JSON.

### How much does a request cost?

Credits per call: `/v2/patreon/profile/info` **1**, `/v2/patreon/search` **1**. See the [Piloterr website](https://www.piloterr.com) for plans.

### What data does the Patreon API return?

Each endpoint returns structured JSON: **Patreon Profile Info** — `id`, `url`, `name`, `cover`, `avatar`, `creator`; **Patreon Search** — `query`, `results[]`, `results[].id`, `results[].url`, `results[].name`, `results[].thumb`.

### Which languages are supported?

Any language that can make an HTTP request. This README shows Python and TypeScript; the [`examples/`](examples) folder contains cURL, PHP, C#, Go, Java, Ruby and Node.js.

## Official SDKs

Use Piloterr from your own language with a typed client:

| SDK | Package | Install |
| --- | --- | --- |
| Node.js | [`@piloterr/sdk`](https://www.npmjs.com/package/@piloterr/sdk) | `npm install @piloterr/sdk` |
| Python | [`piloterr`](https://pypi.org/project/piloterr/) | `pip install piloterr` |
| Go | [`piloterr-go`](https://pkg.go.dev/github.com/piloterr/piloterr-go) | `go get github.com/piloterr/piloterr-go` |
| .NET | [`Piloterr`](https://www.nuget.org/packages/Piloterr) | `dotnet add package Piloterr` |
| Ruby | [`piloterr`](https://rubygems.org/gems/piloterr) | `gem install piloterr` |
| PHP | [`piloterr/sdk`](https://packagist.org/packages/piloterr/sdk) | `composer require piloterr/sdk` |
| Java | [`piloterr-java`](https://github.com/piloterr/piloterr-java) | build from source, see its README |
| CLI | [`@piloterr/cli`](https://www.npmjs.com/package/@piloterr/cli) | `npm install -g @piloterr/cli` |
| MCP server | [`@piloterr/mcp`](https://www.npmjs.com/package/@piloterr/mcp) | `npx -y @piloterr/mcp` |

## Related scrapers

- [Instagram Scraper API](https://github.com/piloterr/instagram-scraper)
- [Kick Scraper API](https://github.com/piloterr/kick-scraper)
- [Pinterest Scraper API](https://github.com/piloterr/pinterest-scraper)
- [TikTok Top Ads Scraper API](https://github.com/piloterr/tiktoktopads-scraper)
- [Weibo Scraper API](https://github.com/piloterr/weibo-scraper)
- [X (Twitter) Scraper API](https://github.com/piloterr/twitter-x-scraper)
- [All Piloterr scrapers on GitHub](https://github.com/piloterr)

## Support

- 💬 Join the [Piloterr Discord](https://discord.gg/CFj3ph8aHU) to ask questions and share what you build.
- 📚 Browse the [API reference (OpenAPI)](https://app.piloterr.com/openapi) and the [Piloterr library](https://www.piloterr.com/library).
- 🔑 [Get your API key](https://app.piloterr.com).

---

<p align="center">
  <a href="https://www.piloterr.com">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="images/logo-dark.svg">
      <img src="images/logo.svg" alt="Piloterr" height="32">
    </picture>
  </a>
  <br>
  <sub>Built by <a href="https://www.piloterr.com">Piloterr</a> — web scraping APIs for e-commerce, search, real estate and more, one API key.</sub>
</p>

<sub>Patreon is a trademark of its respective owner. This repository is not affiliated with or endorsed by Patreon. Examples are released under the MIT license.</sub>
