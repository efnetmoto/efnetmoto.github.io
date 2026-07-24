---
title: "Chanop cheat sheet"
weight: 20
---

# Chanop cheat sheet

For channel operators (`+o`). You have partyline access, so **partyline
(`.command`) forms lead**; the in-channel IRC equivalents are shown alongside.
(See [Joining the partyline]({{< relref "/docs/operators/partyline.md" >}}) if
you're not connected yet.)

`<chan>` is `#motorcycles` unless you're acting on another channel the bot sits
on.

## Kick

```
.kick <chan> <nick> [reason]        (partyline — goes through the bot)
```

In channel directly:

```
/kick <chan> <nick> [reason]        (IRC — your client op, immediate)
```

## Ban

Use the **bot's** `.+ban` for any ban you want the bot to remember (with expiry,
logging, and sticky/dynamic behavior — see
[ban maintenance]({{< relref "/docs/operators/ban-maintenance.md" >}})):

```
.+ban <hostmask> [chan] [%Xh] [reason]   (partyline — bot-managed)
.-ban <hostmask|number>                  (remove a bot ban)
.bans [all]                              (list the bot's bans)
```

For a one-off you don't need the bot to track, a raw server ban works too:

```
/mode <chan> +b <hostmask>          (IRC — not remembered by the bot)
```

> [!TIP]
> **If a ban isn't sticking** — it keeps disappearing, or comes back — read
> [ban maintenance]({{< relref "/docs/operators/ban-maintenance.md" >}}). It's
> the **dynamic vs sticky** distinction: dynamic bans expire (default 5 hours
> on this fleet!) and get cleaned up; sticky bans persist. You almost
> always want `.+ban` + `.stick` for a troll, not `/mode +b`.

## Op / deop / voice

In channel (IRC):

```
/mode <chan> +o <nick>     op someone
/mode <chan> -o <nick>     deop
/mode <chan> +v <nick>     voice
```

Via the bot (partyline):

```
.op <nick> <chan>
.deop <nick> <chan>
```

## Channel modes

Set with `/mode`:

```
/mode <chan> +m            moderated (only +v/+o can speak)
/mode <chan> +s            secret (hidden from /list)
/mode <chan> +l 50         limit
/mode <chan> +R            registered nicks only (if the network supports it)
```

## Finding a user's record

To see if someone has a bot record, their flags, and their hostmasks:

```
.whois <nick>              (partyline — looks up the nick's record)
.match <handle>            (partyline — find a record by handle)
```

`.whois` is the quick check; `.match` finds records by handle or pattern.

## Finding a botmaster

You can't add or fix a user record yourself (that's a
[botmaster]({{< relref "/docs/operators/botmaster-cheatsheet.md" >}}) job), but
you can find one:

```
.whom *                    who's on the partyline + their flags
.bots                      linked bots on the botnet
```

`.whom *` lists everyone currently on the partyline with their flags — look for
someone carrying `+n` (owner) or `+m` (master). That's a botmaster; ask them
(via partyline or in channel) to add or fix a member's record.

## Quick reference

| Task | Partyline (lead) | In-channel IRC |
| --- | --- | --- |
| Kick | `.kick <chan> <nick> [reason]` | `/kick <chan> <nick> [reason]` |
| Ban (persistent) | `.+ban <hostmask> [chan] [%Xh] [reason]` | — (use `.+ban`) |
| Ban (one-off) | — | `/mode <chan> +b <hostmask>` |
| Remove a bot ban | `.-ban <hostmask\|number>` | — |
| List bans | `.bans [all]` | `/mode <chan> +b` (server bans only) |
| Op / deop | `.op <nick> <chan>` / `.deop <nick> <chan>` | `/mode <chan> ±o <nick>` |
| Find a record | `.whois <nick>` / `.match <handle>` | — |
| Find a botmaster | `.whom *` / `.bots` | — |
