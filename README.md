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

## Custom domain / CNAME (Phase 4 — NOT done yet)

The apex `efnetmoto.com` canonical and the `www` → apex redirect are configured
in **Phase 4** of the migration plan. To keep the `efnetmoto.github.io` preview
live until then, `static/CNAME` is **not** committed yet — adding it would make
GitHub Pages redirect the `*.github.io` URL to the apex before DNS points there.

To cut over: add `static/CNAME` containing `efnetmoto.com`, set the custom domain
in Settings → Pages, and point DNS (apex A records + `www` CNAME to
`efnetmoto.github.io`).
