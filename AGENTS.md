# AGENTS.md

Guidance for coding agents working on this repo. Read this before making changes.

## What this is

The source for **efnetmoto.com** — the EFNet `#motorcycles` docs site. Built
with [Hugo](https://gohugo.io) and the [Hugo Book](https://github.com/alex-shpak/hugo-book)
theme, deployed to GitHub Pages via GitHub Actions. This is a **docs site, not a
blog** — no news/posts, pagination, feeds, or marketing homepage.

The bot fleet itself lives in a **separate repo**: `github.com/efnetmoto/efnetmoto-fleet`
(cloned at `~/src/efnetmoto-fleet`). The bots are Eggdrop; the fleet repo holds
the eggdrop config template (`templates/eggdrop.conf.j2`), the bot scripts
(`shared/python-scripts/`, `shared/tcl-scripts/`), and the Ansible deployment.
**When documenting bot behavior, read the actual source there** — don't guess
from eggdrop upstream defaults, because the fleet overrides several.

## Build & deploy

```sh
# local dev
hugo server --buildDrafts --disableFastRender     # http://localhost:1313/

# production build (CI does this; baseURL is injected per the Pages URL)
hugo --gc --minify --baseURL "https://efnetmoto.github.io/"
```

The Hugo Book theme is a **git submodule** under `themes/hugo-book`. After
cloning: `git submodule update --init --recursive`. Local toolchain: Hugo
`v0.163.2+extended`, git, `gh`.

CI (`.github/workflows/hugo.yml`) builds on push to `main` and deploys to
GitHub Pages via `actions/deploy-pages`. Pages build source must be set to
**GitHub Actions** (not "Deploy from a branch"). The `build_type` was flipped
from the auto-provisioned `legacy`/Jekyll to `workflow` — if Pages ever resets,
re-run: `gh api -X PUT repos/efnetmoto/efnetmoto.github.io/pages -f build_type=workflow`.

The site is live at `https://efnetmoto.github.io/`. The apex `efnetmoto.com`
canonical is **not yet wired** (Phase 4 — DNS cutover). `static/CNAME` is
deliberately absent until then; adding it now would redirect the preview to the
apex before DNS points there.

## Repo layout

```
config/_default/hugo.toml    site config (BookSection, BookRepo, menus, params)
content/
  _index.md                  homepage — channel intro, NO bots
  docs/                      nested sidebar nav (user + operators)
    user/                    search, weather, bseen, eggdrop-cheatsheet
    operators/               partyline, chanop-cheatsheet, botmaster-cheatsheet, ban-maintenance
  resources/_index.md        flat file-library table
data/resources.toml          intended resource-file list (name, filename, description)
layouts/resources/list.html  resources list layout — hides rows whose file isn't in static/files/
layouts/_partials/docs/footer.html  overrides the Book theme footer (GitHub icon + "Edit this page on GitHub")
assets/icons/github.svg      the GitHub mark for the footer edit link
static/files/                recovered assets served at /files/<asset> (currently only .gitkeep)
themes/hugo-book/            git submodule — DO NOT edit; override via project layouts/assets instead
```

## Content conventions

### Two sections, not one tree
- **`/docs/`** — nested sidebar nav, built from `content/docs/` page weights.
  Two subsections: `user/` and `operators/`.
- **`/resources/`** — standalone flat page, **not** under `/docs`. Surfaced via
  the Book top-nav menu. Flat table, no categorization.

### Eggdrop docs: show both command forms
Where the bot supports them, show **both** partyline (`.command`) and MSG
(`/msg <bot> command`) forms:
- **User docs lead with MSG** — most members have no partyline access
  (`default-flags ""` → no flags → no partyline login).
- **Operator docs lead with partyline** — chanops have `+o`, botmasters have
  `+n`/`+m`, both get partyline. Show MSG where available.

Note: `/msg`-bound commands are **unprefixed** (no `!`), since `/msg` already
names the bot. In-channel `pub`-bound commands use the `!` prefix. (`!lastspoke`
is pub-only — no msg bind — so it's channel-only.)

### Anchor on the fleet config, not upstream
When stating a default behavior, cite the value in
`templates/eggdrop.conf.j2`, since the fleet overrides several eggdrop defaults.
Key overrides: `default-flags ""`, `learn-users 0`, `unbind msg - addhost`,
`bounce-bans 0`, `ban-time`/`default-ban-time` (300 = 5 hours, in minutes).

### Symptom-driven framing
Lead with the user-visible problem ("the bot isn't recognizing you"), then the
fix, so a frustrated member can self-serve without reading eggdrop internals.

### Don't document absent features
Never write "there's no `!g N`" or "the bot doesn't do X" or explain the design
rationale for an omission. Documenting what's *missing* invites feature requests.
Lead with what to do, not what's not there. (This was a hard-won rule — see git
history if tempted to re-add a "this feature doesn't exist" section.)

### Pass-through / anti-blame framing
The search (`!g` → Brave Search API) and weather (→ WeatherAPI/AVWX/Ambient/APRS)
docs state the bot is a pass-through and doesn't control upstream data — bad
results are the source, not a bot bug. Framed for naive users who blame the bot.

### `#getting-registered`
The weather doc has a `## Getting registered` section (anchor `#getting-registered`)
walking the path: ask an op → op finds a botmaster → botmaster `.+user`s you →
ident if needed → `.wzset` works. This is the landing page the IRC "not
registered" messages will link to (Phase 5 wiring). Cross-linked from the user
and botmaster cheat sheets.

### No bots on the homepage
The homepage introduces the **channel**, not the bots. Bots live in the docs.
The homepage is a few short paragraphs in a collective "we" voice — no CTAs, no
"where to go next" section (the leftnav handles navigation). Soft phrasing over
exact figures ("late 90s" not "1998", "a small crowd" not "15-25").

### Single-channel bots
The bots are single-channel on `#motorcycles`. Channel settings don't vary —
state the actual values (run `.chaninfo #motorcycles` on the partyline to see
them live), don't hedge with "can differ per channel."

## Resources page (data-driven)

`layouts/resources/list.html` renders a row **only when the matching file exists
in `static/files/`** (checked via `os.ReadDir`). This keeps the page free of
broken links until assets are restored. The file list lives in
`data/resources.toml`. To populate: drop the files into `static/files/` with
the exact filenames in the data file; the table fills in on the next build.

**Asset recovery (pending):** 5 files need to be restored from a tarball backup
of the old `static.efnetmoto.com` bucket. Exact filenames: `Bill_of_Sale.doc`,
`MSF_ParkingLotExercises.pdf`, `fault-finding-diagram.pdf`, `gearing.xls`,
`suspension.pdf`. See README.md.

## Theme overrides

The Book theme is a submodule — **never edit it directly**. Override via the
project:
- **Partials** — drop a copy in `layouts/_partials/docs/` (e.g. `footer.html`).
- **Icons** — add to `assets/icons/` (resolved by the theme's `docs/icon`
  partial; project `assets/` overrides theme `assets/`).

GitHub footer: the edit link uses `assets/icons/github.svg` (`fill="currentColor"`
to inherit link color) and literal "Edit this page on GitHub" text.

## Nav menu

Configured in `hugo.toml`:
- **`menu.before`** (above the docs tree): Home, Resources
- **`menu.after`** (below the docs tree): Stats (`https://stats.efnetmoto.com`),
  GitHub (`https://github.com/efnetmoto` — the **org**, not this repo)

`BookRepo` stays on **this repo** (`efnetmoto.github.io`) — it drives the
per-page "Edit this page" footer links, which need the actual page source.

## Cross-linking

Use `{{< relref "path/to/file.md" >}}` for internal links (strict — errors at
build time if the target is missing). For section anchors:
`{{< relref "/docs/user/weather.md" >}}#getting-registered`. `BookPortableLinks`
is **off** — `relref` already enforces broken-link checking, and portable-links
emits spurious warnings on the relref placeholder.

## What's done / not done

- **Done (Phases 0-3):** repo scaffolded, CI pipeline, homepage, resources page,
  all user docs, all operator docs.
- **Pending (Phase 4):** DNS cutover to GitHub Pages (apex `efnetmoto.com`
  canonical, www redirect), commit `static/CNAME`, decommission GitLab Pages.
- **Pending (Phase 5):** fleet PR wiring weather/quote/bseen help output to the
  site URLs, including augmenting `weather.py`'s "not registered" messages to
  link `#getting-registered`. Xerokewl + Decisis redeploy. Pompone untouched.
- **Pending:** asset recovery into `static/files/`.

The full implementation plan lives in the Obsidian Research folder:
`~/obsidian/01_Projects/EFNetMoto Website Modernization/Research/Implementation Plan.md`.
