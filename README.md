# efnetmoto.com

Source for **efnetmoto.com** — the documentation site for the EFNet `#motorcycles`
IRC channel. It's a docs site, not a blog: no posts, feeds, or marketing pages.
Built with [Hugo](https://gohugo.io) and the [Hugo Book](https://github.com/alex-shpak/hugo-book)
theme, deployed to GitHub Pages via GitHub Actions.

The site covers three things:

- **[User guide](https://efnetmoto.com/docs/user/)** — web search, weather, seen
  tracking, quotes, and the eggdrop cheat sheet for channel members.
- **[Operator guide](https://efnetmoto.com/docs/operators/)** — partyline,
  kick/ban, user records, and ban maintenance for chanops and botmasters.
- **[Resources](https://efnetmoto.com/resources/)** — a small library of
  motorcycle reference files hosted directly on the site.

The bots themselves (Eggdrop, deployed with Ansible + Docker) live in a separate
repo: [`github.com/efnetmoto/efnetmoto-fleet`](https://github.com/efnetmoto/efnetmoto-fleet).
This repo only documents how to use them.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to make a change — local preview,
the content-style rules, and the deploy flow. The detailed contributor and agent
guide lives in [AGENTS.md](AGENTS.md).

## Licensing

[MIT](LICENSE).
