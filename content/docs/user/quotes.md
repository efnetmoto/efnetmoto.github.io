---
title: "Quotes (!quote !addquote)"
weight: 25
---

# Quotes (`!quote`, `!addquote`, …)

Xerokewl keeps a quote file for the channel — funny lines, memorable moments,
the usual IRC quote-file stuff. Type the commands in channel; `!quote` and
`!getquote` also work via private message (`/msg Xerokewl !quote <title>`).

## Commands

| Command | Who can use it | What it does |
| --- | --- | --- |
| `!quote <title>` | everyone | Fetch the quote with that title. Also via `/msg Xerokewl !quote <title>`. |
| `!getquote <title>` | everyone | Alias for `!quote`. |
| `!randquote [text]` | everyone | A random quote. With optional text, limits to quotes whose title or text matches. |
| `!randauthor [nick]` | everyone | A random quote added by `<nick>`. No nick = any random quote. |
| `!addquote <title> <quote text>` | friends (`+f`) | Add a quote. The first word is the title; the rest is the quote. |
| `!quoteadd <title> <quote text>` | friends (`+f`) | Alias for `!addquote`. |
| `!delquote <title>` | ops (`+o`) | Delete a quote. You can delete your own; a botmaster can delete any. |
| `!deletequote <title>` | ops (`+o`) | Alias for `!delquote`. |
| `!titlesearch <text>` | everyone | Find quote titles containing `<text>`. Results are messaged to you privately. |
| `!searchtitle <text>` / `!searchtitles` | everyone | Aliases for `!titlesearch`. |
| `!quotesearch <text>` | everyone | Find quotes whose text contains `<text>`. Results are messaged to you privately. |
| `!searchquotes <text>` / `!searchquote` | everyone | Aliases for `!quotesearch`. |
| `!quotestats` | everyone | How many quotes are in the database, and how many you've added. |
| `!quoteinfo <title>` | everyone | Who added a quote and when. |
| `!quotever` | everyone | The script version. |
| `!quotehelp` | everyone | PMs you the command list. |

## Fetching a quote

```
<you> !quote squid
<Xerokewl> squid: the squid said hello to the clam
```

Titles are case-insensitive — `!quote Squid`, `!quote SQUID`, and `!quote squid`
all match. If the quote was added in a different channel, the output notes it:
`squid: (#otherchan) the squid said hello to the clam`.

Not sure of the title? Use `!titlesearch <text>` to find it — the bot messages
you the matching titles privately, even though you typed the command in channel.

## Adding a quote

You need the friend flag (`+f`) to add quotes. The first word becomes the title;
everything after is the quote:

```
<you> !addquote squid the squid said hello to the clam
<Xerokewl> Quote squid added.
```

Titles must be unique — a duplicate gets `Quote squid already exists.`

## Searching

Two searches, both sending results to you privately:

- **`!titlesearch <text>`** — matches quote *titles* containing the text.
- **`!quotesearch <text>`** — matches quote *text* (the body) containing the text.

Both list the titles that match, so you can then `!quote <title>` to read one.

## Deleting a quote

`!delquote <title>` is ops-only (`+o`). You can delete your own quotes; a
botmaster (with `+m`) can delete anyone's.

## Stats and info

- **`!quotestats`** — total quotes in the channel, and how many you've personally
  added.
- **`!quoteinfo <title>`** — who added a specific quote and when (date).
