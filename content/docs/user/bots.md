---
title: "The bots"
weight: 5
---

# The bots

Three [Eggdrop](https://www.eggheads.org/) bots sit in `#motorcycles`. Each
handles different features, so which one you talk to depends on what you need.

| Bot | Handles | Commands |
| --- | --- | --- |
| **Pompone** | [web search]({{< relref "search.md" >}}) | `!g` |
| **Xerokewl** | [weather]({{< relref "weather.md" >}}), [quotes]({{< relref "quotes.md" >}}) | `.w` / `.wz` / `.wzset`, `!quote` / `!addquote` |
| **Decisis** | [seen tracking]({{< relref "bseen.md" >}}) | `!seen` / `!lastspoke` |

## How to reach a bot

- **Private message** — `/msg <bot> <command>`. Works for everyone; this is how
  most members use the bots. Each feature page shows the MSG forms that work.
- **DCC chat (the partyline)** — `/ctcp <bot> chat`. Needs partyline access
  (operators only). See [Joining the partyline]({{< relref "/docs/operators/partyline.md" >}}).

For passwords, identing, and hostmasks — what to do when a bot doesn't
recognize you — see the [eggdrop cheat sheet]({{< relref "eggdrop-cheatsheet.md" >}}).

## Where the bots live

The bots themselves — the Eggdrop configs, the TCL and Python scripts that
power `!g`, weather, quotes, and seen, and the Ansible + Docker deployment
that runs them — live in a separate repo:
[`github.com/efnetmoto/efnetmoto-fleet`](https://github.com/efnetmoto/efnetmoto-fleet).
This site only documents how to use them. If you want to work on the bot code
or the deployment, head there — its `CONTRIBUTING.md` covers the build and
workflow.
