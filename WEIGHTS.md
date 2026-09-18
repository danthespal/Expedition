# Expedition route weights

This guide explains how the Expedition route planner scores targets. It is for players configuring the plugin, not a statement of an item's actual market value. All values are expressed in **exalted orbs** so they can be compared directly with priced Runic Remnant rewards.

## How a route is scored

Each explosion covers nearby targets. The planner scores their fixed value plus any monster-loot value, then searches for a legal chain that fits the number of explosives, fuse reach, terrain clearance, and cavern rule.

For a target with monster loot, the estimated value is:

```text
monster value × (1 + rarity-per-earlier-remnant × earlier-remnants) × propagation multipliers
```

Only remnants unearthed earlier in the proposed route add juice. This is why the planner may take an Opulent/Power remnant early, then choose a valuable monster target later. The same propagation rule applies once even if multiple remnants match it. Fixed chest and reward values do not receive monster juice.

## Start with priorities

Priorities are multipliers. `1.00x` preserves the base weights, `0` ignores that category, and values above `1` make the planner spend more explosives or travel farther for it.

| Setting | Affects | Raise it when | Lower it when |
|---|---|---|---|
| **Remnant rewards** | Priced/selected Rune Forge reward | You want immediate, known rewards | You prefer monster-heavy chains |
| **Remnant monsters** | Monster waves created by a remnant | You value long, juiced remnant chains | You only care about forge rewards |
| **Chests & caverns** | Faded/bright markers, chest icons, cavern entrances | You want reliable chest/cavern coverage | You want a remnant-first route |
| **Runic monsters** | Monster markers and monster/boss minimap icons | You farm logbooks or runic monster loot | You want faster, less monster-focused paths |

Built-in strategies are starting points:

- **Balanced** — all priorities at `1.00x`.
- **Remnants** — emphasises rewards and remnant monsters; ignores chest/cavern score.
- **Juiced** — favours remnant monsters and de-emphasises chests.
- **Fast** — favours chest/cavern targets and accepts less monster value.
- **Logbooks** — strongly prioritises runic monster targets.

Choose one, plan a few encounters, then change one multiplier at a time. Save a modified strategy under a new name before experimenting further.

## Base weights

Base weights determine what `1.00x` means. The defaults are estimates, not prices.

### Runic Remnants

| Setting | Meaning | Default | Guidance |
|---|---|---:|---|
| **Skip remnants worth less than** | Ignores a remnant below this priced value; `0` disables the filter. Unpriced remnants count as `0`. | `0` | Use it to avoid routing toward low-value forges. A skipped remnant still participates in the cavern-safety rule if an explosion reaches it. |
| **Unpriced remnant reward** | Fixed reward estimate when price data cannot resolve a remnant recipe. | `0 ex` | Raise it only if you deliberately value unpriced rewards. |
| **Remnant monsters at 6 slots** | Base monster-wave estimate for a six-slot remnant before juice. | `150 ex` | This scales by `(slots / 6)²`: 3 slots is `0.25×`, 9 slots is `2.25×`. |
| **Rarity per earlier remnant** | Monster-loot increase from each previously unearthed remnant. | `0.50` | Keep it at `0.50` unless game behaviour changes; it represents 50% increased rarity per prior remnant. |

### Propagating runes

Each propagation rule has a **Match** and a **Multiplier**. If the locked rune name contains `Match`, the multiplier applies to later monster loot.

| Match | Multiplier | Effect |
|---|---:|---|
| `Opulent` | `3.0×` | Strongly favours taking that remnant before later monster targets. |
| `Power` | `2.5×` | Favours using it early in a monster-oriented route. |

Set a multiplier to `1` for no effect. Avoid zero or negative values; invalid values are treated as `1`. Add a rule only when the locked-rune name is stable and its effect genuinely propagates to later monsters.

### Marker and icon weights

Marker weights identify targets by their observed category. Icon weights identify minimap icons; the settings panel lists icons seen in the last plan.

| Setting | Default | Treated as | Notes |
|---|---:|---|---|
| **Runic monster marker** | `25 ex` | Monster loot | Receives remnant rarity and propagation juice. |
| **Faded chest marker** | `5 ex` | Fixed value | Does not receive monster juice. |
| **Bright chest marker** | `50 ex` | Fixed value | Does not receive monster juice. |
| **Monster/boss icon** | Per icon | Monster loot | Receives remnant juice. |
| **Chest icon** | Per icon | Fixed value | Does not receive monster juice. |
| **Cavern entrance** | `50 ex` | Fixed value and safety target | Never shares an explosion with a remnant, even if its weight is zero. |

Set an icon to `0` to remove it from scoring. The plugin still preserves the cavern safety rule.

## Two practical profiles

### Reliable immediate rewards

- Remnant rewards: `1.5–2.0×`
- Remnant monsters: `0.5–1.0×`
- Chests & caverns: `0.5–1.0×`
- Runic monsters: `0.5–1.0×`
- Set a minimum remnant price if you only want high-priced forges.

### Juiced monster chain

- Remnant rewards: `0.75–1.0×`
- Remnant monsters: `1.5–2.0×`
- Chests & caverns: `0–0.5×`
- Runic monsters: `1.5–3.0×`
- Keep rarity and propagation multipliers realistic; inflated inputs make the planner over-commit to long detours.

## Troubleshooting unexpected routes

- **The route ignores an expensive-looking target:** its marker/icon weight may be zero, it may be beyond a legal fuse path, or it may need too many bridge explosives.
- **The route takes a low-value remnant early:** it may be collecting rarity or a propagation rune for later monster targets. Lower **Remnant monsters**, reduce the rune multiplier, or raise the minimum remnant price.
- **The route avoids a cavern:** this is intentional when a remnant would be hit by the same blast.
- **Values seem too large or small:** enter exalted-orb estimates, not divine-orb values. Adjust the relevant base weight before using an extreme priority multiplier.
