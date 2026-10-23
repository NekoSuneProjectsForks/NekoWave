# NekoWave Radio

Live internet radio for DayZ handheld radios and vehicles, with experimental Radio_Fungi cassette coexistence.

**Based on:** [LootTeamRadio by offmonreal](https://github.com/offmonreal/LootTeamRadio). NekoWave Radio is an independent fork that retains credit to the original project and its contributors. See [LICENSE](LICENSE) for licensing terms.

## Features

- 8–48 configurable internet radio stations, including TruckersFM, NekoSune Community Radio, NCS-themed streams and Capital FM UK.
- In-car radio presets using chat commands, without needing a handheld radio item.
- Existing Personal Radio support.
- Radio_Fungi coexistence: automatically pauses the livestream when a cassette is detected in a vehicle's `Cassette` attachment slot.
- Per-player volume settings. Radio audio plays locally through MPC-HC, **not** as shared in-game vehicle audio.

## Setup

1. Install [MPC-HC](https://github.com/clsid2/mpc-hc/releases) on Windows and keep it running.
2. In **View > Options > Player > Web Interface**, enable **Listen on port** and enter `13579`.
3. Install a matching signed NekoWave Radio client mod and join a server that has integrated its mission-side station configuration.
4. Use a battery-powered **Personal Radio** in your hands, or enter a vehicle with its engine running and select a live station.

## Commands

| Command | Function |
| --- | --- |
| `!radio car 1` to `!radio car 48` | Choose a vehicle livestream station |
| `!radio car` | Select vehicle preset 1 |
| `!radio car off` | Turn off the vehicle livestream |
| `!radio car fungi` | Turn off the livestream before using Radio_Fungi |
| `!radio 50` | Set personal radio volume to 50% (0-100) |
| `!radio help` | Display player help |

Server admins can edit the station catalogue in `$profile:DayZRadio/radio-stations.json`. See [server setup](docs/SERVER.md) and [build instructions](docs/BUILD.md).

## More stations

The server accepts 8–48 ordered station entries in `$profile:DayZRadio/radio-stations.json`. The first eight retain their original physical handheld frequencies. Extra stations are **vehicle-only** and must use sequential slot IDs and unique virtual frequency keys: slot 9 uses `frequency: 200.9`, slot 10 uses `201.0`, and slot 48 uses `204.8` (formula: `(2000 + slotId) / 10`). These are internal identifiers, not actual DayZ radio frequencies.

The included sample now has 12 stations; the four new samples are candidate SomaFM and Radio Paradise endpoints whose availability and permitted usage must be checked. Add/edit entries on the server and run admin `!radio reload` to send the new list to connected players. Clients must use this updated mod version.

**GUI status:** An in-game search/category/favourites interface is still planned, not yet implemented. Until then, select extra stations with `!radio car 9`, `!radio car 10` etc.

## Radio_Fungi coexistence

NekoWave Radio pauses its external livestream when it detects an item attached to the vehicle's `Cassette` slot. Removing the cassette lets the chosen livestream resume when the engine is running. **This checks whether a cassette is inserted, not whether the cassette is currently playing.** It does not control Radio_Fungi or alter its files.

Compatibility with Radio_Fungi and different vehicle mods is **not yet verified in DayZ**. Test both mods together on a staging server before deploying. Stream availability, regional access, and content rights are the server operator's responsibility.

## Development and credits

- **Original mod:** [offmonreal/LootTeamRadio](https://github.com/offmonreal/LootTeamRadio)
- **Enhanced fork:** [NekoSuneProjectsForks/NekoWave](https://github.com/NekoSuneProjectsForks/NekoWave)
- Internal mod and PBO identifiers are now `NekoWave`. Rebuild and reinstall after upgrading; existing player settings remain under `$profile:DayZRadio`.
- DayZ, MPC-HC, and Radio_Fungi belong to their respective owners.
