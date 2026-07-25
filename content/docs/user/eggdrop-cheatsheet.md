---
title: "Eggdrop cheat sheet (users)"
weight: 40
---

# Eggdrop cheat sheet (users)

The bots are [Eggdrop](https://www.eggheads.org/). This page is the quick
reference for **members** who've been added to a bot so feature preferences
(like your [saved weather default]({{< relref "/docs/user/weather.md" >}})) stick.

## Two ways to talk to a bot

There are two interfaces, and which one you can use depends on whether you have
flags on the bot:

- **MSG** — a normal private message: `/msg <bot> <command>`. **Available to
  everyone**, even with zero flags. **This is the form most members use.**
- **Partyline** — a DCC chat connection to the bot, where commands start with a
  dot (`.pass`, `.ident`, …). **Only available if you have the partyline flag.**
  On this fleet, newly added users get **no flags** by default, so **most
  members have no partyline access** — use MSG.

Where a command exists in both forms, both are shown below; **MSG is listed
first** because that's what you almost certainly have.

> [!NOTE]
> `<bot>` is whichever bot you have a record on — **Xerokewl** for weather
> defaults, for example. Substitute the bot's nick.

## Set a password

After a botmaster adds your record, set a password so you can ident later.

```
/msg <bot> pass <newpass>          (MSG — primary)
```

If you happen to have partyline access:

```
.pass <newpass>                    (partyline)
```

To change it later, the partyline form takes the old then the new
(`.pass <old> <new>`); the MSG form just sets it.

## Ident when your hostmask changed

The bot recognizes you by your **hostmask** (your `nick!user@host`). If your
IP or hostname changes — dynamic ISP, VPN, travel, a different nick — the bot
won't match you to your record until you **ident**.

```
/msg <bot> ident <password>              (MSG — primary)
/msg <bot> ident <password> <nickname>   (if you're on a different nick)
```

Partyline form (if you have access):

```
.ident <password>
.ident <password> <nickname>
```

Identing re-matches your *current* host to your record **and adds it
permanently** — you won't need to ident again from that host. You only need a
botmaster's `.+host` to add a host you're *not* connecting from right now (a
wildcarded mask, or a host for when you're offline) — see the
[botmaster cheat sheet]({{< relref "/docs/operators/botmaster-cheatsheet.md" >}}).

> [!IMPORTANT]
> **If the bot isn't recognizing you** — e.g. `.wzset` says you have no default
> even though you set one, or the bot treats you like a stranger — it's almost
> always a **hostmask mismatch**. Ident now (above): that adds your current
> host to your record, so the bot recognizes you now **and** next time. No
> botmaster needed unless you want to add a host you're not connecting from.

## You need a record first

Saved defaults and idents are tied to a bot record, and records are created by
a botmaster — see [Getting registered]({{< relref "/docs/user/weather.md" >}}#getting-registered)
for the exact path. If you've never been added, identing alone won't create
one — ask an operator to get you added.

## Quick reference

| Task | MSG (primary) | Partyline (if you have access) |
| --- | --- | --- |
| Set password | `/msg <bot> pass <newpass>` | `.pass <newpass>` |
| Ident (same nick) | `/msg <bot> ident <password>` | `.ident <password>` |
| Ident (different nick) | `/msg <bot> ident <password> <nickname>` | `.ident <password> <nickname>` |
