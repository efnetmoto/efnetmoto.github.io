---
title: "Joining the partyline"
weight: 10
---

# Joining the partyline

The **partyline** is the bots' private chat network — a direct connection to a
bot (and through it, the other linked bots) that operators use for commands
that have no in-channel equivalent. Both chanops and botmasters connect the same
way; **what you can run once you're on depends on your flags**.

## Why it exists

Some commands manage the bot's internal state — the **user records** (who's
registered, their flags, their hostmasks) and the **ban list** — and have no
channel equivalent. Managing users (`.+user`, `.chattr`, `.+host`, …) and the
ban commands (`.+ban`, `.bans`, `.stick`, …) are **partyline-only**.
So is finding out who else is on (`whom`) and botnet info (`bots`).

In-channel IRC (kicking, `/mode +b`, op/deop) still works for the quick stuff —
see the [chanop cheat sheet]({{< relref "/docs/operators/chanop-cheatsheet.md" >}}) —
but anything the bot should *remember* lives on the partyline.

## Connecting

You need a user record with at least the partyline flag to do anything useful
(operators have `+o` or `+n`/`+m`, both of which grant partyline access).

From your IRC client:

```
/ctcp <bot> chat
```

The bot opens a DCC CHAT with you. Accept it, then log in:

```
<bot> Please enter your handle.
you> yourhandle
<bot> Please enter your password.
you> yourpassword
```

`<bot>` is any of them — **Pompone**, **Xerokewl**, or **Decisis**. They share
user records, so your handle and password work on whichever you connect to.
You set your password with `pass` — see the
[user cheat sheet]({{< relref "/docs/user/eggdrop-cheatsheet.md" >}}) (or a
botmaster set it when creating your record).

## Once you're on

Commands start with a **dot** (`.command`). The essentials:

```
.help                  list the commands you can run
.help <command>        details on one command
.who                   who's on this bot's partyline
.quit                  leave the partyline
```

From here, go to your role's cheat sheet:

- **Chanops** → [chanop cheat sheet]({{< relref "/docs/operators/chanop-cheatsheet.md" >}})
- **Botmasters** → [botmaster cheat sheet]({{< relref "/docs/operators/botmaster-cheatsheet.md" >}})
- **Everyone** → [ban maintenance]({{< relref "/docs/operators/ban-maintenance.md" >}})
  (the deep dive on how the bot's bans work)

## How it differs from the channel

- **Private** — only operators with records can see it; nothing you type on the
  partyline appears in `#motorcycles`.
- **Persistent** — you stay connected across channel activity; the bot keeps
  state (your session, the user records, the ban list) live.
- **Stateful commands** — partyline commands change what the bot *remembers*
  (records, bans, flags), which is exactly why they're not in-channel.
