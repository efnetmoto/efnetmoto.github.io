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
  docs/user/           user-facing bot docs (search, weather, quotes, bseen, eggdrop cheat sheet)
  docs/operators/      operator docs (partyline, chanop + botmaster cheat sheets, bans)
  resources/_index.md  flat file-library table (data-driven; see data/resources.toml)
data/resources.toml    resource-file list (name, filename, description) — the source of truth for the table
layouts/resources/     resources-section list layout — renders a row only when the file exists in static/files/ (no broken links)
static/files/          recovered assets served at /files/<asset>
```

To add a resource: drop the file in `static/files/` and add an entry to
`data/resources.toml` with the exact filename. The table fills in on the next
build; the layout hides rows whose file isn't present, so nothing ships broken.

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
