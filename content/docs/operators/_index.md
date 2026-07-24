---
title: "Operator guide"
weight: 20
---

# Operator guide

Operator docs cover two distinct privilege levels. You may hold one or both.

- **Chanop** — has channel ops (`+o`); kicks, bans, and modes, plus the ability
  to look up user records and find a botmaster.
- **Botmaster** — has bot master/owner flags (`+n`/`+m`); creates and maintains
  the bot's userfile records, including adding new members so their feature
  prefs (like a saved weather default) work.

Both roles need partyline access, so start with **[Joining the partyline]({{< relref "/docs/operators/partyline.md" >}})**.

- [Partyline]({{< relref "/docs/operators/partyline.md" >}}) — how to connect and
  why some commands live only there.
- [Chanop cheat sheet]({{< relref "/docs/operators/chanop-cheatsheet.md" >}}) —
  kick, ban, modes, finding a user record, finding a botmaster.
- [Botmaster cheat sheet]({{< relref "/docs/operators/botmaster-cheatsheet.md" >}}) —
  user CRUD: adding, finding, modifying, and removing userfile records.
- [Ban maintenance]({{< relref "/docs/operators/ban-maintenance.md" >}}) — the
  deep dive on dynamic vs sticky bans, expiry, and exemptions.
