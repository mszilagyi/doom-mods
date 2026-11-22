# deathcount.pk3

A co-op death tracking mod for UZDoom 5.0+.

Tracks per-player deaths, friendly fire kills (teammates killed), and suicides across a co-op Doom session and displays them on the status bar, intermission screens, and automap.

## Features

- **Death tracking** — counts every player death per map (deaths include suicides and deaths caused by teammates)
- **Suicide detection** — separately tracks self-inflicted deaths
- **Friendly fire tracking** — counts how many teammates each player has killed
- **Murder callout** — when you die to friendly fire, the respawn scoreboard shows who killed you
- **Counters reset on map change** — clean slate for each level
- **Persistent status bar overlay** — live D/FF/S counters always visible on the standard HUD (see below)
- **Co-op intermission scoreboard** — adds DEATHS, FF, SUICIDE, and SESSION columns to the end-of-level stats screen and the "Entering" screen that follows, with ready-up checkmarks for players who are ready to continue
- **SP intermission screen** — adds DEATHS and SUICIDES rows to the single-player stats screen
- **Automap HUD overlay** — shows running totals while the automap is open, scaled to match the engine's own stat display
- **Co-op scoreboard** — replaces UZDoom's co-op scoreboard, both while you wait to respawn and while the scoreboard key (Tab by default) is held, showing NAME, KILLS, DEATHS, FF, SUICIDE, and PING for all players. Deathmatch keeps the engine's scoreboard

## Status Bar Overlay

When the standard status bar is visible (not the fullscreen/alternative HUD), death stats are drawn directly on the stone background panels on either side of the bar.

**Left panel — your stats (always visible):**
```
D:n    ← total deaths (red)
FF:n   ← teammates you killed (orange)
S:n    ← suicides (yellow)
```
Right-justified against the left edge of the AMMO box.

**Right panel — other players (multiplayer only):**

One column per other player, starting just right of the ammo type counters. Each column shows:
```
Name   ← truncated player name (white, small)
D:n    ← deaths (red)
FF:n   ← teammates they killed (orange)
S:n    ← suicides (yellow)
```
On a 3440×1440 ultrawide display, approximately 3 other players are visible before the panels extend past the stone area. More panels become visible by reducing HUD scale.

Both overlays are gated by the `dc_show_hud_stats` CVar and are hidden when the alternative/fullscreen HUD is active.

## Requirements

- UZDoom 5.0.0 or later (UZDoom 4.14.3 is no longer supported)
- Any Doom IWAD
- Designed for co-op multiplayer; SP death/suicide tracking works too

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
  CVARINFO        — display toggle CVar
  MAPINFO         — registers event handler, stat screens, and status bar
```

## CVars

| CVar | Type | Default | Description |
|---|---|---|---|
| `dc_show_hud_stats` | `user bool` | `true` | Show/hide the persistent status bar overlay |

`user` CVars are local per-player settings and are not synced to the server. Toggle in-game:
```
dc_show_hud_stats false
```

Death counters are stored in memory by the `DeathCountHandler` event handler and are not exposed as CVars.

## Known Limitations

### Status bar overlay on non-widescreen displays

The left-panel stats render in the widescreen stone extension to the left of the AMMO box (at negative virtual x). On 4:3 displays with no widescreen extension this area is not visible and the overlay will not appear. The alternative/fullscreen HUD is unaffected.

## Tested With

- UZDoom 5.0.3

## Notes

- Deaths are a rollup — a suicide increments both `deaths` and `suicides` for that player. A friendly fire kill increments the victim's `deaths` and the killer's `ffkills`, so FF is not part of a player's own deaths
- Counters reset when a new map is loaded, not on player respawn
- Suicide is detected when the killer and victim are the same actor
- Friendly fire is detected when the killer is a different player, and is credited to the killer
- The automap overlay shows per-player breakdown in multiplayer, and only shows deaths/suicides in SP when the count is greater than zero
- The murder callout persists until the next death or map change
