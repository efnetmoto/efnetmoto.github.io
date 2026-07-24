---
title: "Ban maintenance"
weight: 40
---

# Ban maintenance

How the bot's bans actually work, and why a ban you set sometimes "doesn't
stick." Linked from the [chanop cheat sheet]({{< relref "/docs/operators/chanop-cheatsheet.md" >}});
if you just want to kick someone, start there.

## Two kinds of ban

| Kind | How you set it | Does the bot remember it? | Expires? |
| --- | --- | --- | --- |
| **Server ban** | `/mode <chan> +b <hostmask>` (IRC) | **No** — it lives in the channel mode until unset or the channel resets. | depends on the network/channel |
| **Bot ban** | `.+ban <hostmask> [chan] [%Xh] [reason]` (partyline) | **Yes** — stored in the bot's ban list, logged, enforced on rejoin. | yes, unless **sticky** |

Use **`.+ban`** for anything you want the bot to maintain. A raw `/mode +b` is a
one-off the bot doesn't track.

## The commands

```
.+ban <hostmask> [chan] [%Xh] [reason]   add a bot ban (optional duration, e.g. %2h)
.-ban <hostmask|number>                  remove a bot ban (by hostmask or .bans number)
.bans [all]                              list the bot's bans (all = every channel)
.stick <hostmask> [chan]                 make an existing ban permanent (sticky)
.unstick <hostmask> [chan]               let an existing ban expire again
.+exempt <hostmask> [chan] [%Xh] [reason]  whitelist a hostmask so it's never banned
.-exempt <hostmask|number>              remove an exempt
.exempts [all]                           list exempts
```

## Dynamic vs sticky — the "it didn't stick" problem

> [!WARNING]
> **If a ban you set keeps disappearing**, it's because it was **dynamic**, not
> sticky. On this fleet the channel settings are `+dynamicbans` and
> `default-ban-time 300` (= **5 hours**): a non-sticky `.+ban` is **removed when
> the banned user leaves the channel**, and in any case **expires after 5
> hours**. To keep a troll out, add the ban and then **`.stick`** it.

- **Dynamic ban (default):** removed when the matching user leaves the channel,
  and/or after `ban-time` (global default 5 hours; a channel can override it
  with its own `ban-time`). Good for a quick timeout.
- **Sticky ban:** set with `.+ban` then `.stick <hostmask>` (or `.stick` an
  existing ban). It **never expires** and is **never auto-removed** — the bot
  re-applies it. Use this for persistent trolls.

```
.+ban *!*@*.troll.example.com #motorcycles persistent troll
.stick *!*@*.troll.example.com #motorcycles
```

## Existing users aren't auto-kicked (`-enforcebans`)

The channel runs with **`-enforcebans`**: when you set a ban, the bot does
**not** kick people who match it but are **already** in the channel. They're
blocked from **rejoining**. If you need them out now, kick them too:

```
.+ban *!*@*.troll.example.com #motorcycles
.kick #motorcycles troll bye
```

## The bot won't undo your `/mode +b` (`bounce-bans 0`)

`bounce-bans` is `0`, so the bot does **not** bounce (remove) server bans it
didn't set. A `/mode +b` you set stays until you or the channel unset it. The
flip side: the bot also won't *maintain* a server ban for you — for anything
persistent, use `.+ban` + `.stick`.

## Users can set bot bans (`+userbans`), and exempts work (`+userexempts`)

- `+userbans` — operators (`+o`) can set bans the bot stores (your `.+ban`s).
- `+userexempts` / `+dynamicexempts` — exempts work the same way bans do. An
  exempt prevents a hostmask from being banned at all (useful for a regular who
  keeps matching a broad ban). Exempts have their own timer
  (`default-exempt-time 60` = 60 minutes) unless you `.stick` them.

## Per-channel `ban-time`

The global default is `default-ban-time 300` (5 hours), but each channel can set
its own `ban-time` in its channel block, which overrides the global for that
channel. So the exact expiry can differ per channel — check with `.bans` to see
what's set and when it'll expire.

## Quick reference

| You want to… | Command |
| --- | --- |
| Ban someone for up to the default (5h) | `.+ban <host> [chan] [reason]` |
| Ban someone for a specific duration | `.+ban <host> [chan] %2h [reason]` |
| Ban someone permanently | `.+ban <host> [chan] [reason]` then `.stick <host> [chan]` |
| Remove a ban | `.-ban <host\|number>` |
| List bans | `.bans [all]` |
| Make a ban permanent | `.stick <host> [chan]` |
| Let a sticky ban expire again | `.unstick <host> [chan]` |
| Whitelist a host | `.+exempt <host> [chan] [reason]` |
| Kick someone already in channel | `.kick <chan> <nick> [reason]` (a ban alone won't, with `-enforcebans`) |
