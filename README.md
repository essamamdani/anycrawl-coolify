# AnyCrawl — Coolify v4 Template

Deploy **AnyCrawl** on your own server with [Coolify v4](https://coolify.io) in minutes.

AnyCrawl is an open-source, multi-engine web crawler and scraping API supporting Playwright, Puppeteer, and Cheerio.

---

## Quick Deploy

### Option A — One-click via URL

1. Open your Coolify dashboard
2. Go to **New Resource → Docker Compose**
3. Choose **"Load from URL"**
4. Paste the raw URL of `docker-compose.yml`:
   ```
   https://raw.githubusercontent.com/any4ai/AnyCrawl/main/coolify-template/docker-compose.yml
   ```
5. Click **Continue**, review environment variables, then **Deploy**

### Option B — Copy & paste

1. Copy the contents of [`docker-compose.yml`](./docker-compose.yml)
2. In Coolify: **New Resource → Docker Compose → paste the content**
3. Configure environment variables and deploy

---

## Services

| Service | Image | Arch |
|---------|-------|------|
| `api` | `ghcr.io/any4ai/anycrawl-api:latest` | amd64, arm64 |
| `scrape-playwright` | `ghcr.io/any4ai/anycrawl-scrape-playwright:latest` | amd64, arm64 |
| `scrape-cheerio` | `ghcr.io/any4ai/anycrawl-scrape-cheerio:latest` | amd64, arm64 |
| `scrape-puppeteer` | `ghcr.io/any4ai/anycrawl-scrape-puppeteer:latest` | **amd64 only** |
| `redis` | `redis:7-alpine` | amd64, arm64 |

> **arm64 hosts (e.g. Apple Silicon, Ampere):** Remove the `scrape-puppeteer` service from the compose file before deploying, or set `ANYCRAWL_AVAILABLE_ENGINES=playwright,cheerio`.

---

## Environment Variables

Coolify will surface all variables below for you to configure. Defaults work out of the box for a basic deployment.

### Core

| Variable | Default | Description |
|----------|---------|-------------|
| `ANYCRAWL_VERSION` | `latest` | Image tag to deploy (e.g. `v1.0.0-beta.15`) |
| `ANYCRAWL_API_AUTH_ENABLED` | `false` | Enable API key authentication |
| `ANYCRAWL_API_CREDITS_ENABLED` | `false` | Enable usage credit tracking |
| `ANYCRAWL_AVAILABLE_ENGINES` | `playwright,cheerio` | Comma-separated list of enabled engines |

### Scraping

| Variable | Default | Description |
|----------|---------|-------------|
| `ANYCRAWL_HEADLESS` | `true` | Run browsers in headless mode |
| `ANYCRAWL_IGNORE_SSL_ERROR` | `true` | Ignore SSL certificate errors |
| `ANYCRAWL_PROXY_URL` | _(empty)_ | HTTP/SOCKS proxy for all requests |
| `ANYCRAWL_PROXY_STEALTH_URL` | _(empty)_ | Stealth proxy URL |
| `ANYCRAWL_MAX_CONCURRENCY` | `50` | Max concurrent scrape jobs |
| `ANYCRAWL_MIN_CONCURRENCY` | `5` | Min concurrent scrape jobs |

### Caching

| Variable | Default | Description |
|----------|---------|-------------|
| `ANYCRAWL_CACHE_ENABLED` | `true` | Enable response caching |
| `ANYCRAWL_CACHE_DEFAULT_MAX_AGE` | `172800000` | Page cache TTL in ms (2 days) |
| `ANYCRAWL_CACHE_SITEMAP_MAX_AGE` | `604800000` | Sitemap cache TTL in ms (7 days) |

### Storage (S3 — optional)

Leave empty to use local storage. Set `ANYCRAWL_STORAGE=s3` and fill in the fields below to enable S3.

| Variable | Description |
|----------|-------------|
| `ANYCRAWL_STORAGE` | Set to `s3` to enable S3 backend |
| `ANYCRAWL_S3_BUCKET` | S3 bucket name |
| `ANYCRAWL_S3_REGION` | S3 region (default: `us-east-1`) |
| `ANYCRAWL_S3_ENDPOINT` | Custom S3-compatible endpoint |
| `ANYCRAWL_S3_ACCESS_KEY` | S3 access key |
| `ANYCRAWL_S3_SECRET_ACCESS_KEY` | S3 secret key |

### AI / LLM (optional)

| Variable | Description |
|----------|-------------|
| `DEFAULT_LLM_MODEL` | Default model (e.g. `openai/gpt-4o-mini`) |
| `DEFAULT_EXTRACT_MODEL` | Model for structured extraction |
| `DEFAULT_EMBEDDING_MODEL` | Model for embeddings |
| `OPENAI_API_KEY` | OpenAI API key |
| `OPENROUTER_API_KEY` | OpenRouter API key |
| `CUSTOM_BASE_URL` | OpenAI-compatible base URL |
| `CUSTOM_API_KEY` | API key for custom provider |

### OCR (optional)

| Variable | Default | Description |
|----------|---------|-------------|
| `ANYCRAWL_VL_REC_SERVER_URL` | _(empty)_ | OpenAI-compatible vision API URL |
| `ANYCRAWL_VL_REC_API_KEY` | _(empty)_ | API key for vision provider |
| `ANYCRAWL_VL_REC_MODEL` | `PaddlePaddle/PaddleOCR-VL-1.5` | OCR model name |

### Search

| Variable | Default | Description |
|----------|---------|-------------|
| `ANYCRAWL_SEARCH_ENABLED_ENGINES` | `ac-engine` | Enabled search engines |
| `ANYCRAWL_AC_ENGINE_URL` | _(empty)_ | URL of the AnyCrawl search engine |
| `ANYCRAWL_SEARXNG_URL` | _(empty)_ | URL of your SearXNG instance |

### Scheduler & Webhooks

| Variable | Default | Description |
|----------|---------|-------------|
| `ANYCRAWL_SCHEDULER_ENABLED` | `true` | Enable task scheduler |
| `ANYCRAWL_WEBHOOKS_ENABLED` | `true` | Enable webhook delivery |
| `ALLOW_LOCAL_WEBHOOKS` | `false` | Allow webhooks to local addresses (testing only) |

---

## Persistent Data

Coolify manages three named volumes automatically:

| Volume | Contents |
|--------|----------|
| `storage` | Crawl output files and screenshots |
| `db` | SQLite database (`database.db`) |
| `redis-data` | Redis AOF persistence |

---

## Domain & HTTPS

Coolify assigns a domain to the `api` service automatically via `SERVICE_FQDN_API_8080`. HTTPS is managed by Coolify's built-in Traefik proxy — no manual configuration needed.

---

## Pinning a Version

Replace `ANYCRAWL_VERSION=latest` with a specific tag (e.g. `v1.0.0-beta.15`) to lock your deployment to a known release.

Available tags: [ghcr.io/any4ai/anycrawl-api](https://github.com/any4ai/AnyCrawl/pkgs/container/anycrawl-api)

---

## Links

- [AnyCrawl GitHub](https://github.com/any4ai/AnyCrawl)
- [API Documentation](https://github.com/any4ai/AnyCrawl#readme)
- [Coolify Documentation](https://coolify.io/docs)
- [Report Issues](https://github.com/any4ai/AnyCrawl/issues)
