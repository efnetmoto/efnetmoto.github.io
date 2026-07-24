---
title: "Botmaster cheat sheet"
weight: 30
---

# Botmaster cheat sheet

For **botmasters** (`+n`/`+m`). Your job is **user CRUD** — creating, finding,
modifying, and removing the bot's userfile records, including adding new
members so their feature prefs (like a [saved weather default]({{< relref "/docs/user/weather.md" >}}#getting-registered))
stick (see [Adding a non-privileged member](#adding-a-non-privileged-member)).

You have partyline access, so **partyline (`.command`) forms lead** here. If
you're not on yet, see [Joining the partyline]({{< relref "/docs/operators/partyline.md" >}}).

> [!IMPORTANT]
> The bot does **not** auto-add users (`learn-users 0`). **Every** record is
> botmaster-created — there is no self-registration path for members. If someone
> needs a record, you create it.

## Create — `.+user`

```
.+user <handle> <hostmask>
```

- `<handle>` is the bot record name (a short, stable identifier; often the
  member's usual nick).
- `<hostmask>` is how the bot will recognize them — see
  [Hostmask formats](#hostmask-formats).

Example:

```
.+user alice *!*@alice-dsl.example.com
```

After creating, set them a password (or tell them to set one via MSG `pass` —
see the [user cheat sheet]({{< relref "/docs/user/eggdrop-cheatsheet.md" >}})):

```
.chpass alice <temp-password>
```

## Finding a member's hostmask

If they're in channel, the quickest way to get their current hostmask:

```
.whois <nick>
```

The output shows their `nick!user@host` — use that (wildcarded) as the hostmask
when you `.+user` or `.+host`.

## Hostmask formats

Eggdrop matches on `nick!user@host` with wildcards. Typical safe choices:

| Pattern | Matches |
| --- | --- |
| `*!*@*.example.com` | anyone from that domain |
| `*!ident@*.example.com` | a specific ident from that domain |
| `alice!*@*` | the nick `alice` from anywhere (fragile — nicks are cheap) |
| `*!*@192.168.0.*` | a subnet |

Prefer host/ident-based masks over nick-based ones — a nick can be taken by
anyone.

## Read — find and inspect records

```
.whois <nick>          look up by current nick (shows handle, flags, hosts, last seen)
.match <handle>        find a record by handle
.match *               list everyone
.userinfo <handle>     extra stored info for a handle
```

## Update — flags and hostmasks

### Flags — `.chattr`

```
.chattr <handle> <flags>          global flags
.chattr <handle> <flags> #chan    channel-specific flags
```

Common flags:

| Flag | Meaning |
| --- | --- |
| `+o` | op (channel op). Global `+o` = op on every channel the bot is on. |
| `+m` | master (can manage the bot's channel settings) |
| `+n` | owner (full bot control — hand these out very sparingly) |
| `+p` | partyline access |
| `+f` | friend (protected from some bans/limits) |
| `+v` | voice (channel) |

Examples:

```
.chattr alice +o #motorcycles     give alice op on #motorcycles only
.chattr alice +o                  give alice global op
.chattr alice -o                  remove alice's global op
```

### Hostmasks — `.+host` / `.-host`

```
.+host <handle> <hostmask>     add a hostmask to an existing record
.-host <handle> <hostmask>     remove a hostmask
```

> [!IMPORTANT]
> **`.+host` is the path members can't self-serve.** The `addhost` MSG command
> is disabled on this fleet (the fleet unbinds it), so a member whose hostmask
> changed can **not** add the new one themselves. They ident to get recognized
> for the session, then come to **you** to make it permanent with `.+host`. This
> cheat sheet is where that handoff lands. Point them at the
> [user cheat sheet]({{< relref "/docs/user/eggdrop-cheatsheet.md" >}}) to ident
> first.

## Adding a non-privileged member

Most members just need a record so their feature prefs (like a
[weather default]({{< relref "/docs/user/weather.md" >}}#getting-registered))
work — no flags, no special privileges. On this fleet `default-flags` is
empty, so a freshly `.+user`'d member is **non-privileged by default**: no
`+o`, no `+m`, no `+n`, no partyline. You don't have to do anything special;
just **don't** add flags.

Verify with `.whois` — confirm the flags line is empty (no `+m`/`+o`/`+n`):

```
.+user alice *!*@*.example.com
.whois alice
# → check: no +o/+m/+n in the flags output
```

Only add flags deliberately, when someone actually needs them.

## Delete — `.-user`

```
.-user <handle>
```

Removes the record and all its hostmasks/flags. Use with care.

## Save your work

Changes to the userfile are saved on a timer, but you can force it:

```
.save
```

## Symptom: "the bot isn't recognizing me"

> [!NOTE]
> If a member says the bot isn't recognizing them — `.wzset` claims they have no
> default, or the bot ignores them — it's almost always a **hostmask mismatch**.
> First `.whois <nick>` and check their hostmasks against their *current* host
> (shown in the `.whois` output). If the new host isn't on the record, you have
> two ways to fix it — pick one, you don't need both:
> - **`.+host <handle> <new-hostmask>`** (permanent) — the bot recognizes them
>   by the new hostmask from now on. This is the fix for a hostmask that's
>   changed for good.
> - **Have the member ident** (this session only) — point them at the
>   [user cheat sheet]({{< relref "/docs/user/eggdrop-cheatsheet.md" >}}).
>   Identing re-matches them for the current session but doesn't save the
>   hostmask, so they'd need to ident again next time. Use this when the change
>   is temporary (VPN, travel) or while you're about to `.+host` it anyway.
>
> If `whois` finds **no record at all**, it's not a hostmask issue — create one
> with `.+user` (see [Create](#create--user)).

## Out of scope here

This cheat sheet covers **userfile CRUD only**. It does **not** cover:

- Botnet linking / partyline plumbing — that's a separate concern.
- Fleet management (Ansible/Compose, bot deployment) — a separate concern.
- Ban maintenance — see [ban maintenance]({{< relref "/docs/operators/ban-maintenance.md" >}}).
