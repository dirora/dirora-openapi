<div align="center">

# Dirora Developer Platform

**Build on [Dirora](https://dirora.com) — the modern, multi-language commerce platform.**

REST API · Webhooks · OAuth apps · Themes · CLI

[![Website](https://img.shields.io/badge/website-dirora.com-2563eb)](https://dirora.com)
[![API status](https://img.shields.io/badge/API-live-brightgreen)](https://dirora.com)
[![Docs](https://img.shields.io/badge/docs-developers-8b5cf6)](https://dirora.com)
[![License](https://img.shields.io/badge/license-MIT-black)](./LICENSE)

</div>

---

## Why build on Dirora?

Dirora is a hosted, multi-tenant commerce platform — think a modern storefront
engine you don't have to run. Merchants launch beautiful, multi-language stores;
you build the apps, integrations and automations on top.

- 🏬 **Multi-tenant storefronts** on `{store}.dirora.com` or custom domains
- 🎨 **Visual theme editor + layout engine** — no rebuilds to restyle a store
- 💳 **Payments, orders, subscriptions, quotes, bookings, gift cards, discounts**
- 🌍 **26 languages out of the box** — every resource is localisable
- 🧩 **App marketplace** — publish once, install across thousands of stores
- 🔌 **Provider-neutral API** — `provider` + `external_id` pairs, never leaky vendor fields
- 🪝 **Webhooks** for every meaningful event, with signed, idempotent delivery

> Dirora is a fully managed SaaS. There's nothing to host — you get an API key
> and start building.

## What's in this repo

| Path | What it is |
|------|------------|
| `openapi/` | The Dirora OpenAPI 3 specification (source of truth for the REST API) |
| `postman/` | A ready-to-import Postman collection |
| `examples/` | Runnable request/response examples per resource |
| `guides/` | Quickstarts: first call, OAuth app, webhooks, pagination, errors |

## Quickstart

```bash
# 1. Create a store — free, no card — at https://dirora.com
# 2. In Admin → Developers, create an API key (or an OAuth app for the marketplace)
# 3. Make your first call:

curl https://api.dirora.com/api/products/a \
  -H "Authorization: Bearer $DIRORA_API_KEY" \
  -H "X-Tenant-ID: $YOUR_STORE_ID"
```

## Authentication

- **API keys** — server-to-server, scoped per store. Send as `Authorization: Bearer <key>`.
- **OAuth apps** — for marketplace apps installed by merchants; you receive a
  per-install token scoped to that store.

All requests are made to `https://api.dirora.com/api`.

## Webhooks

Subscribe to events (orders, payments, customers, products, app installs, …) and
Dirora POSTs a **signed, provider-neutral** payload to your endpoint. Verify the
signature, return `2xx`, and handle retries idempotently.

```jsonc
{
  "event": "order.paid",
  "provider": "stripe",          // never raw vendor fields
  "external_id": "evt_...",
  "data": { "order_id": "...", "amount": 4200, "currency": "GBP" }
}
```

## Conventions worth knowing

- **Money is integer minor units** (`4200` = £42.00) everywhere.
- **Pagination** via `?page` + `?per_page` (max 100); responses carry
  `meta.total` / `meta.total_pages`.
- **Locale-aware reads** — pass `?locale=de-DE`; untranslated fields fall back to English.
- **Errors** are neutral + machine-readable; never parse human copy.

## SDKs & tooling

Generate a typed client from the OpenAPI spec in any language, or use the
official SDKs (see `guides/sdks.md`). A CLI for scaffolding and publishing apps
is available in the [`dirora-integrations`](https://github.com/dirora/dirora-integrations) repo.

## Get involved

- 🚀 **Start free:** <https://dirora.com>
- 📚 **API reference & guides:** <https://dirora.com> → Developers
- 🧩 **Build an app / connector:** [`dirora-integrations`](https://github.com/dirora/dirora-integrations)
- 💬 **Questions:** open an issue, or email `support@dirora.com`

## License

MIT — see [`LICENSE`](./LICENSE). (The Dirora platform itself is a hosted service;
this repo covers the public API surface, specs and examples.)
