# franklinwh-ha-takeover

Moves a Home Assistant user from an existing integration to a bridge without losing their
entity IDs, automations or history. Phase 1 takes over from an integration that is a client
of a SunSpec Modbus TCP server: no make is assumed and Model 1's manufacturer is not checked.
Shared by the Modbus bridge and the Direct Connect bridge, like `franklinwh-system-view`.

**Status:** planning; no code yet. Design: david2069/franklinwh-modbus-bridge#372 (Phase 1,
SunSpec) and #446 (Phase 2, cloud).

## What it does

1. **Inventory (read only).** Finds the other integration's entities and sorts them:
   active, leftover (nothing serves it any more), disabled, and duplicate `_2` pairs. Shows
   how far back each one's history goes, its value next to the bridge's live reading, and
   which entity IDs the user's automations use.
2. **Turn off** the old integration with one click and a warning, where Home Assistant allows
   it. For one set up in a configuration file, it says plainly what the user has to do.
3. **Backup first.** A required "I have a fresh full backup" tick, logged.
4. **Take over.** Removes the old entity's registry entry, then renames the bridge's entity
   into the freed ID, so no `_2` IDs are created. Keeps the old display unit.
5. **Undo.** Renames the bridge's entities back first. A takeover record lets a reinstall
   re-apply it.

User messages are short, clear and non-technical.

## Sources

| Integration | Kind | Phase |
|---|---|---|
| [hilman2/ha-sunspec2](https://github.com/hilman2/ha-sunspec2) (`sunspec2`) | SunSpec Modbus TCP | 1 |
| [cjne/ha-sunspec](https://github.com/cjne/ha-sunspec) (`sunspec`) | SunSpec Modbus TCP | 1 |
| [richo/homeassistant-franklinwh](https://github.com/richo/homeassistant-franklinwh) (`franklin_wh`) | FranklinWH cloud, YAML | 2 |

Phase 1 covers standard SunSpec points only. Make- and firmware-specific quirks come later,
each in its own vendor profile, completely segregated (david2069/franklinwh-modbus-bridge#372).
Quirks imported from ha-sunspec2's vendor profiles are data, each file stating its source
(repo, path, commit) and keeping ha-sunspec2's MIT copyright lines.

Each bridge declares which entities it provides, under neutral keys such as
`battery_soc`, `home_load_power` and `smart_circuit_1_power`. Anything a bridge doesn't
provide stays with the user's current integration.

## Licence

MIT. See `LICENSE`.
