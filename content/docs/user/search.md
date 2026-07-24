---
title: "Search (!g)"
weight: 10
---

# Search (`!g`)

Type `!g <query>` in channel and Pompone looks it up on the web and posts the
top results with short links.

```
<you> !g best chain lube for road bikes
<Pompone> [motorcyclist.com] **Best Chain Lubes 2026** → https://go.efnetmoto.com/a1b2
<Pompone> [revzilla.com] **Chain Lube 101** → https://go.efnetmoto.com/c3d4
```

## What it actually searches

`!g` queries the **Brave Search API** and hands the results back to IRC. The bot
is a **pass-through, not the search engine**.

> [!IMPORTANT]
> The bot does **not** control Brave's results. If a result is wrong, outdated,
> spammy, or just bizarre, that's what Brave returned — not a bot bug. There's
> nothing to "fix" on the bot side.

## Result format

Each result is one line:

```
[domain] **title** → shorturl
```

- The `domain` in brackets shows which site the result is from.
- The **title** is bolded (that's the page title Brave returned).
- The `→` points to a **short URL** on `go.efnetmoto.com` that redirects to the
  real page. If the shortener is down, the bot falls back to the full original
  URL — you still get a working link.

## Limits

- **Up to 2 results** per query. (There's no setting to ask for more.)
- **Query length: 256 characters max.** Longer and you'll get
  `Query too long (max 256 chars).`
- **Per-user cooldown: 5 seconds.** A channel-wide cooldown of 1 second also
  applies. If you hit it: `Rate limited — try again shortly.`
- **Cache: 1 hour** (up to ~1024 queries). A repeat query hits the cache and
  comes back instantly — and a cache hit does **not** cost you a rate-limit
  slot, so popular lookups stay free.

## No `!g N` to expand a result

Results aren't numbered, and there's **no** `!g 2` to "open the second result."
Each line already includes the link; click it. The bot intentionally doesn't
add a numeric index — it'd be noise next to an already-bolded title.

## Error messages

| You see | Why |
| --- | --- |
| `Usage: !g <search query>` | You typed `!g` with nothing after it. |
| `Query too long (max 256 chars).` | Your query exceeded 256 characters. |
| `Rate limited — try again shortly.` | You searched again within the cooldown. |
| `No results found for: <query>` | Brave returned nothing for that query. |
| `An unexpected error occurred. Please try again later.` | Transient failure (network/upstream). Retry shortly. |

## There's no `!ghelp`

`!g` is deliberately a one-trigger interface — there's no help command on the
bot. This page *is* the help. (A botmaster can bust the result cache from the
partyline with `.searchcache`, but that's an operator concern.)
