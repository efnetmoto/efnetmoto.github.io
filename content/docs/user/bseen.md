---
title: "Seen tracking (!seen !lastspoke)"
weight: 30
---

# Seen tracking (`!seen`, `!lastspoke`)

Decisis keeps a database of when nicks were last seen in the channel and
answers queries about it. These commands work **in channel** and **by private
message** (`/msg Decisis <command>`).

## Commands

| Command | What it does |
| --- | --- |
| `!seen <query>` | When `<query>` was last seen in channel. Supports nick, mask, and host queries (below). |
| `!seennick <nick>` | Look up an **exact** nick (skips the smart/fuzzy matching `!seen` does). |
| `!seenstats` | Status of the seen database (counts, oldest/newest). |
| `!chanstats <chan>` | Usage statistics for a channel in the seen database. |
| `!lastspoke <nick>` | When `<nick>` last spoke in the channel. **See the caveat below.** |

## `!seen` query formats

`!seen` is flexible about what you hand it:

- **Regular nick:** `!seen lamer` — last time that exact nick was seen.
- **Masked (wildcards):** `!seen *l?mer*`, `!seen *.lame.com`, `!seen *.edu`.
  Glob-style masks match nicks or hosts.
- **With a channel:** `!seen <query> #mychan` — restrict the lookup to a
  specific channel.

A couple of canned responses: asking about yourself gets `go look in a mirror`,
and asking about the bot itself gets `I'm right here. Quit wasting my time!`

## `!seenstats` and `!chanstats`

- `!seenstats` reports the database's overall status — how many records, how
  far back they go, newest and oldest.
- `!chanstats <chan>` reports usage statistics for a given channel from the seen
  database.

## `!lastspoke` — and its caveat

> [!WARNING]
> `!lastspoke <nick>` **only works on nicks that are currently in the channel.**
> It reports recent in-channel activity (when they last spoke, or how long
> they've been idle), using the bot's live view of who's present. If the nick
> **isn't in the channel right now**, `!lastspoke` returns nothing — it can't
> tell you about someone who isn't here.
>
> For someone who **left** the channel (or whose nick you want historically),
> use **`!seen <nick>`** instead — that's the historical query, and works
> whether or not they're currently present.

Typical output when the nick *is* present:

```
<you> !lastspoke alice
<Decisis> alice last uttered a word on #motorcycles 12 minutes ago.
```

or, if they've been quiet the whole time the bot's been in channel:

```
<Decisis> alice hasn't uttered a word since I joined #motorcycles 3 hours ago.
```

## Help on the bot

You can ask Decisis directly for help:

```
/msg Decisis help seen
/msg Decisis help chanstats
/msg Decisis help seenstats
```
