---
title: "Ban maintenance"
weight: 40
---

# Ban maintenance

How the bot's bans actually work on `#motorcycles`, and why a ban you set
sometimes "doesn't stick." Linked from the
[chanop cheat sheet]({{< relref "/docs/operators/chanop-cheatsheet.md" >}});
if you just want to kick someone, start there.

These bots are single-channel on `#motorcycles`, so the settings below are what's
actually set — not defaults that might vary. Run `.chaninfo #motorcycles` on the
partyline to see them live.

## Two kinds of ban

| Kind | How you set it | Does the bot remember it? | Expires? |
| --- | --- | --- | --- |
| **Server ban** | `/mode #motorcycles +b <hostmask>` (IRC) | **No** — it lives in the channel mode until unset or the channel resets. | depends on the network |
| **Bot ban** | `.+ban <hostmask> [chan] [%Xh] [reason]` (partyline) | **Yes** — stored in the bot's ban list, logged, enforced on rejoin. | yes, unless **sticky** |

Use **`.+ban`** for anything you want the bot to maintain. A raw `/mode +b` is a
one-off the bot doesn't track.

## The commands

```
.+ban <hostmask> [chan] [%Xh] [reason]   add a bot ban (optional duration, e.g. %2h)
.-ban <hostmask|number>                  remove a bot ban (by hostmask or .bans number)
.bans [all]                              list the bot's bans
.stick <hostmask> [chan]                 make an existing ban permanent (sticky)
.unstick <hostmask> [chan]              let a sticky ban expire again
.+exempt <hostmask> [chan] [%Xh] [reason]  whitelist a hostmask so it's never banned
.-exempt <hostmask|number>             remove an exempt
.exempts [all]                           list exempts
```

`<chan>` is optional — the bot knows which channel it's on.

## What's actually set on #motorcycles

- **`+enforcebans`** — when you set a bot ban, the bot **kicks anyone currently
  in the channel who matches it.** You don't need a separate `.kick`; `.+ban`
  removes them too. (Ops are protected — see `+dontkickops` below.)
- **`+dynamicbans`** — the bot only imposes a ban as a channel mode (`+b`)
  *while the matching user is actually on the channel*. When they leave, it
  lifts the channel `+b`, but keeps the ban in its internal list. This is why
  `/mode #motorcycles b` can show nothing even though `.bans` shows a ban — the
  bot is still enforcing it, it's just not currently imposing the channel mode.
- **`+userbans`** — operators (`+o`) can set bans the bot stores (your
  `.+ban`s).
- **`ban-time 300`** — a non-sticky ban expires from the bot's list after
  **5 hours**. Sticky bans don't expire.
- **`ban-type 3`** — the hostmask pattern the bot generates when it bans
  someone by nick. For manual `.+ban` you specify the hostmask yourself, so
  this only affects bot-generated bans (e.g. flood bans).
- **`bounce-bans 0`** — the bot won't remove a server ban (`/mode +b`) it didn't
  set. It also won't *maintain* one for you — for anything persistent, use
  `.+ban` + `.stick`.
- **`+dontkickops`** — the bot won't kick ops. If a ban matches an op who's in
  the channel, the ban is still set, but the bot leaves them in place.

Exempts work the same way bans do:

- **`+dynamicexempts`**, **`+userexempts`** — ops can set exempts; they're
  imposed as a channel mode only while the matching user is present.
- **`exempt-time 60`** — a non-sticky exempt expires after 60 minutes. `.stick`
  an exempt to keep it.
- An exempt prevents a hostmask from being banned at all — handy for a regular
  who keeps matching a broad ban.

## Dynamic vs sticky — the "it didn't stick" problem

> [!WARNING]
> **If a ban you set seems to vanish**, it's because it was **dynamic**, not
> sticky. With `+dynamicbans` and `ban-time 300`, a non-sticky `.+ban` is only
> imposed on the channel while the banned user is present, and **expires from
> the bot's list after 5 hours**. To keep a troll out indefinitely, add the
> ban and then **`.stick`** it — a sticky ban never expires and is always
> imposed on the channel, even when the user isn't around.

- **Dynamic ban (default):** imposed on the channel only while the matching
  user is present; expires from the bot's list after 5 hours. Good for a quick
  timeout.
- **Sticky ban:** `.+ban` then `.stick <hostmask>` (or `.stick` an existing
  ban). It **never expires** and is **always imposed** on the channel — the
  bot re-applies it. Use this for persistent trolls.

```
.+ban *!*@*.troll.example.com #motorcycles persistent troll
.stick *!*@*.troll.example.com #motorcycles
```

## The bot won't undo your `/mode +b`

`bounce-bans` is `0`, so a server ban you set with `/mode +b` stays until you or
the channel unset it. The trade-off: the bot also won't maintain it for you, so
if the channel resets or the ban is cleared, the bot won't put it back. For
anything you want kept, use `.+ban` + `.stick`.

## Quick reference

| You want to… | Command |
| --- | --- |
| Ban someone for up to 5 hours | `.+ban <host> [chan] [reason]` |
| Ban someone for a specific duration | `.+ban <host> [chan] %2h [reason]` |
| Ban someone permanently | `.+ban <host> [chan] [reason]` then `.stick <host> [chan]` |
| Remove a ban | `.-ban <host\|number>` |
| List bans | `.bans [all]` |
| Make a ban permanent | `.stick <host> [chan]` |
| Let a sticky ban expire again | `.unstick <host> [chan]` |
| Whitelist a host | `.+exempt <host> [chan] [reason]` |
| See the channel settings | `.chaninfo #motorcycles` |
