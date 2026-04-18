# deathcount.pk3

A co-op death tracking mod for UZDoom 4.14.3.

Tracks per-player deaths, friendly fire deaths, and suicides across a co-op Doom session and displays them across multiple screens.

## Features

- **Death tracking** — counts every player death per map (deaths include FF and suicide)
- **Suicide detection** — separately tracks self-inflicted deaths
- **Friendly fire tracking** — tracks deaths caused by other players
- **Murder callout** — when you die to friendly fire, the respawn scoreboard shows who killed you
- **Counters reset on map change** — clean slate for each level
- **Co-op intermission scoreboard** — adds DEATHS, FF, and SUICIDE columns to the end-of-level stats screen
- **SP intermission screen** — adds DEATHS and SUICIDES rows to the single-player stats screen
- **Automap HUD overlay** — shows running totals while the automap is open, scaled to match the engine's own stat display
- **Respawn scoreboard** — when a player dies in co-op, a full scoreboard overlay is displayed while waiting to respawn, showing NAME, KILLS, DEATHS, FF, and SUICIDE for all players

## Requirements

- UZDoom 4.14.3 or later
- Any Doom IWAD
- Designed for co-op multiplayer, but SP death/suicide tracking works too

## Installation

Drop `deathcount.pk3` into your UZDoom directory and load it alongside your WAD:
```
uzdoom -iwad doom2.wad -file deathcount.pk3
```

Or add it via the UZDoom launcher.

## File Structure
```
deathcount.pk3/
  zscript.zsc     — all scripting logic
  CVARINFO        — server-replicated death counter CVars
  MAPINFO         — registers event handler, stat screens, and status bar
```

## CVars

All CVars are `server int` and replicate to all players in a netgame. Stats are tracked for players 0–3.

| CVar | Description |
|---|---|
| `dc_p0_deaths` | Total deaths for player 0 (includes FF and suicide) |
| `dc_p0_ffdeaths` | Friendly fire deaths for player 0 |
| `dc_p0_suicides` | Suicides for player 0 |
| `dc_p0_last_killer` | Player index of the last player to FF-kill player 0 (-1 if none) |
| `dc_p1_*` | Same for player 1 |
| `dc_p2_*` | Same for player 2 |
| `dc_p3_*` | Same for player 3 |

## Known Limitations

### Player cap

Stats are tracked for players 0–3 only. Players with index 4 or higher (5+ player sessions) will not have their deaths recorded. The mod fails silently for those players — no console spam, just no tracking.

### Respawn Scoreboard

UZDoom 4.14.3's `DrawScoreboard` virtual function is version-gated to ZScript 4.15.1, which is not yet available in a released build. As a result the respawn scoreboard is implemented as a `Draw()` override rather than a proper `DrawScoreboard()` override. The practical difference for a 2–4 player co-op session is negligible.

When UZDoom ships a release with ZScript 4.15.1 support, the fix is:
1. Change `version "4.14.3"` to `version "4.15.1"` in `zscript.zsc`
2. Replace the `Draw()` override with the proper `DrawScoreboard()` override

## Tested With

- UZDoom 4.14.3

## Notes

- Deaths are a rollup — a suicide or FF kill increments both `deaths` and the specific sub-counter
- Counters reset when a new map is loaded, not on player respawn
- Suicide is detected when the killer and victim are the same actor
- Friendly fire is detected when the killer is a different player
- The automap overlay shows per-player breakdown in multiplayer, and only shows deaths/suicides in SP when the count is greater than zero
- The murder callout persists until the next death or map change
