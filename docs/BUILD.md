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
