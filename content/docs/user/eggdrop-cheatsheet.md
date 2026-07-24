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
> `<bot>` is whichever bot you have a record on — **XeroKewl** for weather
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

Identing re-matches your *current* host to your record for this session. It
doesn't permanently save the new hostmask — see below.

## You can't add your own hostmask

On this fleet the `addhost` MSG command is **disabled** (the fleet unbinds it),
so there is **no** `/msg <bot> addhost …` — it won't work. To make a new
hostmask **permanent**, you have to ask a botmaster to add it to your record
(`.+host`). See the [botmaster cheat sheet]({{< relref "/docs/operators/botmaster-cheatsheet.md" >}}).

> [!IMPORTANT]
> **If the bot isn't recognizing you** — e.g. `.wzset` says you have no default
> even though you set one, or the bot treats you like a stranger — it's almost
> always a **hostmask mismatch**. Ident now (above) to get recognized for this
> session; if the new hostmask is permanent, ask a botmaster to add it.

## You need a record first

The bot does **not** auto-add users from the channel — every record is created
by hand by a botmaster. So if you've **never been added**, no amount of identing
will help: there's no record to match you to. Ask an operator to get you added —
see [Getting registered]({{< relref "/docs/user/weather.md" >}}#getting-registered)
for the exact path.

## Quick reference

| Task | MSG (primary) | Partyline (if you have access) |
| --- | --- | --- |
| Set password | `/msg <bot> pass <newpass>` | `.pass <newpass>` |
| Ident (same nick) | `/msg <bot> ident <password>` | `.ident <password>` |
| Ident (different nick) | `/msg <bot> ident <password> <nickname>` | `.ident <password> <nickname>` |
| Add a hostmask | **not available** — ask a botmaster | `.+host` is a botmaster command (see operator docs) |
