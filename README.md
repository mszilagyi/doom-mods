# doom-mods

A collection of mods for [UZDoom](https://github.com/UZDoom/UZDoom), written in ZScript.

## Mods

| Mod | File | Description |
|---|---|---|
| [deathcount](deathcount/) | `deathcount.pk3` | Co-op death, friendly fire, and suicide tracker with HUD overlays and scoreboard |

## Releases

Each mod is released independently. Releases are tagged as `{mod-name}/v{version}` (e.g. `death-counter/v1.0.0`) and the corresponding `.pk3` is attached to the GitHub release.

To download a mod, go to the [Releases](../../releases) page and grab the `.pk3` for the version you want.

## Requirements

- UZDoom 5.0.0 or later
- Any Doom IWAD

## Usage

Load a `.pk3` alongside your WAD:

```
uzdoom -iwad doom2.wad -file yourwad.wad deathcount.pk3
```

See each mod's README for specific features, CVars, and known limitations.
