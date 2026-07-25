---
title: "Weather (.w .wz .wzset)"
weight: 20
---

# Weather (`.w` / `.wz` / `.wzset`)

Xerokewl fetches current conditions and (for non-METAR lookups) a short forecast.
These commands work **both in channel and as a private message** (`/msg Xerokewl <command>`).

```
<you> .wz 94025
<Xerokewl> San Mateo, CA — 62°F (17°C), partly cloudy. …
```

## Commands

| Command | What it does |
| --- | --- |
| `.w <location>` / `.wz <location>` | Get weather for a one-off location. |
| `.w` / `.wz` | Use your **saved default** location (see [Getting registered](#getting-registered)). |
| `.wz --user <nick>` | Use **someone else's** saved default. |
| `.wzset [--metar] [--metric\|--imperial] <location>` | **Save** your default location (and units). |
| `.wzset --metric\|--imperial` | Update only your saved units (no location change). |
| `.wzhelp` | PMs you this command list. |

`.w` and `.wz` are aliases — same command. You can type either.

## Location formats

| Format | Example | Notes |
| --- | --- | --- |
| ZIP | `94025` | US zip code. |
| City, State | `San Mateo, CA` | Spell it out; use the state. |
| IATA airport code | `SFO` | General weather near the airport. |
| ICAO code (with `--metar`) | `KSFO` | **Only** valid with `--metar`. |
| Ambient Weather Network URL | `ambientweather.net/dashboard/…` | A personal weather station dashboard URL. |
| 32-char station slug | `<32-char slug>` | Ambient station slug. |
| CWOP callsign (SSID 13) | `<CALLSIGN-13>` | APRS/CWOP station callsign with SSID 13. |

> [!NOTE]
> ICAO codes (like `KSFO`) **only** work with `--metar`. For ordinary weather
> use the IATA code instead (e.g. `SFO`, not `KSFO`).

## Flags

- `--metar` — raw aviation weather (METAR). **Requires an ICAO code**
  (e.g. `.wz --metar KSFO`). Anything else is rejected: `--metar is only valid
  with ICAO codes (e.g. KSFO). Location not saved.`
- `--metric` — show metric first, imperial second. **(This is the default.)**
- `--imperial` — show imperial first, metric second.
- Use `--metric`/`--imperial` with `.wzset` to change your saved default units.

## Saving a default (`.wzset`)

`.wzset <location>` saves a default so a bare `.w` / `.wz` returns your weather
without typing the location every time. Your default is stored against your
**eggdrop handle** — which is why you have to be registered for it to work.

```
<you> .wzset 94025
<Xerokewl> Preference updated. Default is now 94025 --imperial
<you> .w
<Xerokewl> San Mateo, CA — …   (uses your saved default + units)
```

You can also save a METAR default: `.wzset --metar KSFO`.

## Getting registered

The single most common confusion: you run `.wzset`, or a bare `.w`, and the bot
tells you you're **"not registered"**. Here's what that means and the exact path
to fix it.

The bot doesn't auto-add users — every user record is created by hand by a
botmaster. So to save a default, you need a record. Here's how to get one:

1. **Ask a channel operator (chanop) to add you.** If the op you ask isn't a
   botmaster, they can find one on the bot partyline — see the
   [chanop cheat sheet]({{< relref "/docs/operators/chanop-cheatsheet.md" >}}).
2. **A botmaster creates your record** with `.+user <handle> <hostmask>` — see
   the [botmaster cheat sheet]({{< relref "/docs/operators/botmaster-cheatsheet.md" >}}).
   You'll be non-privileged by default (no flags), and **that's fine** — you
   don't need any flags to save weather preferences, just a record.
3. **The bot should now recognize you** by your hostmask. If it doesn't (your
   IP/host changed — dynamic ISP, VPN, travel), **ident** — that re-matches you
   to your handle and saves your current host for next time — see the
   [eggdrop cheat sheet]({{< relref "/docs/user/eggdrop-cheatsheet.md" >}}).
4. **Now `.wzset <location>` works**, and a bare `.w` / `.wz` uses your saved
   default.

> [!TIP]
> You don't have to wait to be registered to use weather at all —
> `.wz <location>` (with a location) works for **anyone**, no record needed.
> Getting registered is only required for *saving* a default and using the bare
> `.w` / `.wz` form.

## The bot is a pass-through, not the data source

> [!IMPORTANT]
> Xerokewl pulls data from upstream **providers** — WeatherAPI, AVWX (METAR),
> the Ambient Weather Network, and APRS/CWOP — and does **not** generate or
> control any of it. A wrong temperature, a stale METAR, a station that's down,
> or a "location not found" that you *know* exists — that's the upstream
> provider, not the bot. There's nothing to "fix" on the bot side; try a
> different location format (ZIP vs city vs IATA) or wait for the provider to
> update. (Same idea as the [search bot]({{< relref "/docs/user/search.md" >}})
> — blame the source, not the messenger.)

## Error messages

| You see | Why |
| --- | --- |
| `You must be registered with the bot to use a saved default. Try .wz <location>.` | You ran a bare `.w`/`.wz` but have no user record. See [Getting registered](#getting-registered); for now use `.wz <location>`. |
| `You must be a registered bot user to save preferences. Try .wzhelp for usage.` | You ran `.wzset` with no record. See [Getting registered](#getting-registered). |
| `<nick> is not registered with the bot.` | You used `.wz --user <nick>` and that nick has no record. |
| `No default set. Use .wzset <location>.` / `No default location set. Use .wzset <location> to save one.` | You have a record but no saved location. Run `.wzset <location>`. |
| `Unknown location. Try .wzhelp for usage.` / `Unknown location format. Try .wzhelp for usage.` | The bot couldn't parse your location. Try another format. |
| `--metar is only valid with ICAO codes (e.g. KSFO). Location not saved.` | You passed `--metar` with a non-ICAO location. |
| `--metar requires an ICAO code (e.g. KSFO). …` | You passed `--metar` with no location. |
| `<provider error> Location not saved — check the location and try .wzset again.` | The provider rejected the location while saving. |

Run `.wzhelp` in channel or `/msg Xerokewl wzhelp` any time for the command list.
