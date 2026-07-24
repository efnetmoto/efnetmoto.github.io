# efnetmoto.github.io

Source for **efnetmoto.com** — the EFNet `#motorcycles` docs site. Built with
[Hugo](https://gohugo.io) and the [Hugo Book](https://github.com/alex-shpak/hugo-book)
theme, deployed to GitHub Pages via GitHub Actions.

This is a **docs site, not a blog**: no news/posts, pagination, or feeds. Two
content areas — `/docs/` (user + operator bot docs, nested sidebar) and
`/resources/` (a flat file-library table).

## Local development

```sh
hugo server --buildDrafts --disableFastRender
# site:  http://localhost:1313/
```

The Hugo Book theme is vendored as a git submodule under `themes/hugo-book`.
After cloning, initialise it:

```sh
git submodule update --init --recursive
```

## Layout

```
content/
  _index.md            homepage
  docs/user/           user-facing bot docs (search, weather, bseen, eggdrop cheat sheet)
  docs/operators/      operator docs (partyline, chanop + botmaster cheat sheets, bans)
  resources/_index.md  flat file-library table (data-driven; see data/resources.toml)
data/resources.toml    intended resource-file list (name, filename, description)
layouts/resources/     resources-section list layout (hides rows whose file isn't in static/files/)
static/files/          recovered assets served at /files/<asset>
```

## Asset recovery (Phase 1)

`static/files/` currently holds only a placeholder. The five original documents
need to be restored from the tarball backup of the old `static.efnetmoto.com`
bucket, using **exactly** these filenames (the resources page matches on them):

| File                                   | Served at                |
| -------------------------------------- | ------------------------ |
| `Bill_of_Sale.doc`                     | `/files/Bill_of_Sale.doc` |
| `MSF_ParkingLotExercises.pdf`          | `/files/MSF_ParkingLotExercises.pdf` |
| `fault-finding-diagram.pdf`            | `/files/fault-finding-diagram.pdf` |
| `gearing.xls`                          | `/files/gearing.xls` |
| `suspension.pdf`                       | `/files/suspension.pdf` |

Drop them into `static/files/`; the resources table fills in automatically.
Rows whose file isn't present stay hidden — no broken links ship.

## Custom domain / CNAME

The apex `efnetmoto.com` is the canonical domain; `www.efnetmoto.com`
redirects to it. `static/CNAME` (containing `efnetmoto.com`) is committed —
Hugo copies it to the site root, where GitHub Pages reads it to know which
apex to serve.

DNS points the apex to the GitHub Pages A/AAAA records and `www` CNAMEs to
`efnetmoto.github.io`. The custom domain is set in Settings → Pages (this
provisions the TLS certificate and enables the `www` → apex redirect).

While the apex cert is provisioning, the `efnetmoto.github.io` URL keeps
serving the site at the same content.
