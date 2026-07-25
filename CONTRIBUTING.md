# Contributing to efnetmoto.com

This is the source for **efnetmoto.com**, the docs site for the EFNet
`#motorcycles` IRC channel. It's built with [Hugo](https://gohugo.io) and the
[Hugo Book](https://github.com/alex-shpak/hugo-book) theme, deployed to GitHub
Pages via GitHub Actions. Thanks for helping improve it — this guide covers
everything you need to make a change.

The detailed contributor and agent guide (architecture, content-style rules,
theme overrides) lives in [AGENTS.md](AGENTS.md). Read it before larger edits.

## Prerequisites

- [Hugo](https://gohugo.io/installation/) **extended** edition, `v0.163.2` or
  compatible — the site uses SCSS, so the extended build is required
- `git`
- `gh` (GitHub CLI) — optional, for PRs from the command line

Check your local toolchain:

```sh
hugo version    # must print "extended"
git --version
```

## Run locally

```sh
git clone https://github.com/efnetmoto/efnetmoto.github.io.git
cd efnetmoto.github.io
git submodule update --init --recursive   # themes/hugo-book
hugo server --buildDrafts --disableFastRender
```

The site is at <http://localhost:1313/>. Hugo rebuilds on every save.

## Repo layout

```
config/_default/hugo.toml    site config (theme, menus, params)
content/
  _index.md                  homepage — channel intro, no bots
  docs/
    user/                    user-facing bot docs (search, weather, quotes, bseen, cheat sheet)
    operators/               operator docs (partyline, cheat sheets, ban maintenance)
  resources/_index.md        flat file-library table
data/resources.toml          resource-file list — the source of truth for the resources table
layouts/
  resources/list.html        renders a resources row only when the file exists in static/files/
  _partials/docs/footer.html overrides the Book theme footer (GitHub edit link)
assets/icons/github.svg      GitHub mark for the footer edit link
static/files/                files served at /files/<asset>
themes/hugo-book/            git submodule — DO NOT edit; override via layouts/ or assets/
```

## Making a change

Most changes are edits to Markdown under `content/`:

1. Edit the page (for example, `content/docs/user/weather.md`).
2. Preview with `hugo server` — live reload shows the change immediately.
3. Link to other pages with `{{< relref "path/to/file.md" >}}`. This is
   **strict**: the build fails if the target is missing, so broken internal
   links can't ship. For an anchor, append it after the shortcode:
   `{{< relref "/docs/user/weather.md" >}}#getting-registered`.
4. Don't edit `themes/hugo-book/` — it's a submodule. Override the theme by
   dropping a copy of a partial in `layouts/_partials/docs/`, or an asset in
   `assets/`. See the "Theme overrides" section of AGENTS.md.

### Adding a doc page

Create `content/docs/<section>/<name>.md` with front matter and a heading:

```toml
---
title: "Page title"
weight: 10
---

# Page title
```

`weight` orders pages within their subsection in the sidebar. The two
subsections are `user/` and `operators/`.

### Adding a resource file

The resources page is data-driven — it only renders a row when the file
actually exists, so missing files never ship as broken links.

1. Drop the file in `static/files/` (exact filename, including extension).
2. Add an entry to `data/resources.toml` with `name`, `filename`,
   `description`, and `weight`. The `filename` must match the file in
   `static/files/` exactly.
3. Rebuild — the table fills in automatically.

## Build & deploy

CI (`.github/workflows/hugo.yml`) builds on every push and pull request to
`main` and deploys to GitHub Pages on push. There's nothing to run by hand —
push to `main` (or merge a PR) and the site updates.

Pages build source must be set to **GitHub Actions**
(Settings → Pages → "Build and deployment" → Source: GitHub Actions). If Pages
ever resets to "Deploy from a branch", flip it back:

```sh
gh api -X PUT repos/efnetmoto/efnetmoto.github.io/pages -f build_type=workflow
```

## Markdown linting

Markdown is linted in CI (and locally) with
[markdownlint-cli](https://github.com/igorshubov/markdownlint-cli) `0.48.0`:

```sh
npm install -g markdownlint-cli@0.48.0
markdownlint --config .markdownlint.yml '**/*.md'
```

The config (`.markdownlint.yml`) disables a few rules that conflict with Hugo
Book conventions or IRC/partyline command examples; `.markdownlintignore` skips
the theme submodule, build output, and the agent guide. Structural rules (blank
lines around headings, lists, and code fences) stay on — keep those tidy in new
content.

## Content style

The site has deliberate content conventions — the bot docs show both partyline
and MSG command forms, lead with the user-visible problem, frame the bots as
pass-throughs, and never document absent features. The full rules live in the
"Content conventions" section of [AGENTS.md](AGENTS.md); follow them for
bot-related edits.

## Git workflow

- Branch off `main`: `docs/description`, `fix/description`, `content/description`.
- Open a PR against `main`; CI (build + markdown lint) must pass.
- Commit messages in the present tense: "Add weather `#getting-registered`
  section", not "Added ...".
- Direct pushes to `main` deploy — keep large or risky changes behind a PR.

## Licensing

This repo is [MIT licensed](LICENSE). By contributing, you agree your
contributions are licensed accordingly.
