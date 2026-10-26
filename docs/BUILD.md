# Building and signing NekoWave Radio

The mod's internal script paths, PBO prefix and config identifiers now use **NekoWave**.
Rebuild and resign the mod after this change.

The PBO root is `mod-src/NekoWave/`, containing `$PBOPREFIX$` set to
`NekoWave` and a matching `config.cpp`. PBO names, folder and `CfgMods`
must agree. From the project directory, on a machine with armake2:

```sh
armake2 build mod-src/NekoWave NekoWave.pbo
armake2 inspect NekoWave.pbo
```

Publish this fork as its **own** Workshop item. Do not reuse the original
LootTeamRadio Workshop ID; `server/meta.cpp` uses `publishedid = 0` until
an authorized new ID is assigned. Update that value only after publishing.

Use **DayZ Tools / Publisher** in the owner's authorized Windows Steam session
to generate/sign with a DayZ-compatible key and upload to Workshop. Keep
`.biprivatekey` **outside Git and release artifacts**. Distribute only the
signed `.pbo` plus matching `.bisign` to clients and the `.bikey` to the
server's keys directory. Never accept packaging success as proof of DayZ
client script compatibility: launch DayZ with the exact PBO on Windows and
Proton and inspect both client/server RPT after each change.

Before enabling `verifySignatures=2`, verify all *other* server mods' keys
are installed too. Test a fresh player's download, station tuning, radio off,
player exit, MPC-HC closed (must not freeze), and reconnect after MPC restart.

## F7 menu release preflight (important)

A GitHub commit **does not** update Workshop item 3825145417. Build the *feature/vehicle-radio-multi-stations* branch, not an older checkout of `main`, and republish the signed PBO.

Before packing, verify these files exist **inside the build input root** `mod-src/NekoWave`:

- `config.cpp` — `CfgMods.NekoWave.inputs = "NekoWave/inputs.xml"`
- `inputs.xml` — action `UANekoWaveToggle`, sortable "NekoWave Radio" controls entry, default `kF7`
- `stringtable.csv` — localized keybind labels
- `gui/layouts/nekowave.layout` — UI widget tree
- `5_Mission/NekoWave/NekoWaveMenu.c` — UI handler
- `5_Mission/NekoWave/missionGameplay.c` — context-sensitive toggle

Pack **the contents of the build root** into the PBO, including XML, CSV and layout assets. It is not sufficient to pack only `.c` scripts. Preserve the PBO prefix `NekoWave` and inspect the output PBO to make sure these paths are present. A common failure mode is packaging the `mod-src` directory rather than `mod-src/NekoWave`, which shifts all virtual paths.

On the playing Windows PC, shut down DayZ before updating. In DayZ Launcher, verify that the Workshop item has downloaded its latest update; start the game with that item enabled. The Linux server cannot test F7: keypresses and GUI loading occur **only on the client**.

The client script log at `%LOCALAPPDATA%\\DayZ` now prints one of:

- `[NekoWave] UANekoWaveToggle registered` — engine registered the custom input.
- `[NekoWave] ERROR: UANekoWaveToggle not registered` — inspect the *published client* PBO config and `inputs.xml`.
- `[NekoWave] ERROR: menu widget creation failed` — inspect the layout path in the *published client* PBO.

If **none** appears after joining a game, the new mission script may be missing or the player is running a previous Workshop build. The current menu is experimental and still needs an actual DayZ client compile and play test.
