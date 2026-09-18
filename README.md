# OriathHub Expedition

Expedition is an OriathHub plugin for visualising and planning Path of Exile 2 Expedition encounters. It is a read-only overlay: it reads the game's exposed state through OriathHub and draws guidance; it does not write to, inject into, or automate the game.

## What it provides

- Grounded world overlays for explosives, Expedition markers, and Runic Remnants that are inside a blast radius.
- A live detonator control window showing available explosives, blast radius, fuse limit, and current Runic Remnant count.
- A route planner that proposes a valid explosive chain, taking terrain clearance, fuse reach, target value, remnant order, and cavern safety into account.
- Named route strategies stored as editable JSON files under `config/strategies/`.
- Optional plan and clear-route hotkeys.
- Runic Remnant price labels in the world, on the Large Map and minimap, and in the Runeshape reward panel.
- Large Map prices draw above Radar's Expedition encounter icon so the exalted value stays readable.

The plugin writes visual settings and hotkeys to `config/settings.json`. Route strategies are separate files, so they can be shared or backed up without replacing the rest of the configuration.

## Using the planner

1. Open Expedition settings, enable the control window, and select a strategy.
2. Press **Plan route** in that window or use the configured hotkey.
3. Follow the numbered points in order. With **Hide route points once placed** enabled, a number disappears only when its corresponding explosive is placed in order.
4. If an explosive is placed away from the current point, re-plan before continuing.

The planner is advisory. It uses observed map data and configurable value estimates, not a guarantee of loot or an instruction to take a particular in-game action.

## Price labels

Runic Remnant prices use the host price service. A price is the best currently compatible reward (or the selected reward once chosen), expressed in exalted orbs. Missing prices can be shown as `-` or hidden. Price availability depends on the active league data and relevant Expedition tables being loaded in the area.

## Tuning route values

See [WEIGHTS.md](WEIGHTS.md) for the scoring model, every priority and base-weight setting, and practical starting profiles.

## Limitations

- Game patches can change layouts and offsets; an unavailable radius, missing price, or empty route is safer than guessing.
- The planner never places a cavern entrance in the same explosion as a remnant because that can break the chain.
- Value settings are estimates in exalted-orb units. They express your preferences, not market predictions.

Use of external overlays may carry account risk under the game's terms. You are responsible for deciding whether to use it.
