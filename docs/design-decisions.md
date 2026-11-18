# Design Decisions

## Purpose

This document records significant architectural and design decisions for the mod, their rationale, and alternatives considered. It serves as institutional memory for maintainers.

## Decision Record Format

Each decision follows:
- **Decision**: What was decided
- **Context**: Why it was required
- **Alternatives**: What else was considered
- **Consequences**: Trade-offs accepted
- **Status**: Accepted / Superseded / Deprecated

---

## 1. Declarative Configuration Only

**Decision:** The mod contains only static configuration files (Clausewitz script, YAML, PNG). No executable code, scripts, or runtime logic.

**Context:** Stellaris modding supports both scripted mods (Lua and Clausewitz events) and pure data mods. Data mods are simpler, more stable, and possess no security surface.

**Alternatives:**
- Scripted mod with dynamic galaxy generation — Rejected: complexity, maintenance, breakage risk
- Hybrid with minimal scripts — Rejected: no requirement; the engine handles generation

**Consequences:**
- No runtime defects are possible
- No performance overhead
- Limited to the engine's built-in generation parameters
- Cannot include custom galaxy shapes or generation algorithms

**Status:** Accepted (v1.0.0)

---

## 2. Six Fixed Galaxy Sizes

**Decision:** Exactly six galaxy sizes (100, 1200, 1400, 1600, 1800, 2000 stars) with fixed parameters.

**Context:** Players requested sizes beyond vanilla Huge (1000). Each size requires a complete scenario file with more than 40 tuned parameters.

**Alternatives:**
- Configurable slider (100-2000) — Rejected: the engine does not support dynamic scenario parameters
- More granular sizes (every 100 stars) — Rejected: 19 scenario files, maintenance burden
- Fewer sizes (just 2000) — Rejected: no intermediate options

**Consequences:**
- Discrete choices only
- Each size tuned manually for balance
- Including sizes = linear maintenance cost
- Parameters do not scale automatically

**Status:** Accepted (v1.0.0)

---

## 3. Logarithmic Size Progression

**Decision:** Star counts follow roughly logarithmic progression: 100 → 1200 → 1400 → 1600 → 1800 → 2000.

**Context:** Linear progression (100, 500, 900, 1300, 1700, 2000) would cluster sizes at the low end. A logarithmic progression provides meaningful differentiation at the high end where players desire it.

**Alternatives:**
- Linear progression — Rejected: poor differentiation at the high end
- Exponential progression — Rejected: too extreme, 100 → 2000 gap
- Vanilla-matching progression — Rejected: vanilla stops at 1000

**Consequences:**
- Substantial jump from Minuscule (100) to Gigantic (1200)
- Smaller increments at the high end (200 stars each)
- Corresponds to the player desire for "larger than Huge" options

**Status:** Accepted (v1.0.0)

---

## 4. Priority Values: -1, 5, 6, 7, 8, 9

**Decision:** Scenario priorities use non-sequential values with negative for smallest.

**Context:** The engine uses priority for UI ordering. Vanilla sizes use priorities 0-4. Mod sizes must integrate without collision.

**Alternatives:**
- Sequential 10-15 — Rejected: no reason to avoid negative values
- Match the vanilla scale (0-4) — Rejected: would collide
- Large gaps (100, 200...) — Rejected: unnecessary

**Consequences:**
- Minuscule (-1) appears before vanilla Tiny (0)
- Gigantic (5) appears after vanilla Huge (4)
- Clear visual separation in the dropdown
- The gap permits future vanilla sizes

**Status:** Accepted (v1.0.0)

---

## 5. Vanilla Shape Coverage per Size

**Decision:** Gigantic through Ludicrous declare all 10 vanilla shapes. Minuscule declares only the four shapes vanilla permits at 100 stars (elliptical, spiral_2, ring, bar).

**Context:** The engine supports 10 galaxy shapes. Vanilla itself gates shapes by size because multi-arm shapes degenerate below approximately 400 stars.

**Alternatives:**
- Restrict every large size to elliptical and spiral — Rejected: no technical reason, player freedom
- Declare all 10 shapes at every size — Rejected: spiral_6, cartwheel, and spoked produce degenerate layouts at 100 stars

**Consequences:**
- 54 scenario/shape combinations to test (4 + 10×5)
- No maintenance burden (declaration only)

**Status:** Accepted (2026)

---

## 6. Ludicrous Requires UI Star Names

**Decision:** Ludicrous (2000 stars) documents dependency on UI Star Names mod for unique names.

**Context:** Vanilla contains approximately 1000 unique system names. 2000 stars guarantees duplicates without additional names.

**Alternatives:**
- Bundle names in the mod — Rejected: licensing, maintenance, duplication
- Generate procedural names — Rejected: the engine does not support them
- Accept duplicates — Rejected: poor user experience, confusing

**Consequences:**
- Soft dependency (the mod functions without it, but duplicates occur)
- Documentation burden
- The player must install two mods
- Clear communication in the README and FAQ

**Status:** Accepted (v1.2.0)

---

## 7. English-Only Localisation

**Decision:** Only English localisation is supplied (`l_english`).

**Context:** The mod author speaks only English. Community translations are welcome but not maintained.

**Alternatives:**
- Machine translate to major languages — Rejected: poor quality, misleading
- Crowdsource translations — Rejected: no infrastructure, quality control
- Ship with placeholder keys — Rejected: displays raw keys, worse user experience

**Consequences:**
- Non-English players observe raw keys (`ui_gigantic`)
- The community can contribute translations through pull requests
- Low maintenance

**Status:** Accepted (v1.0.0)

---

## 8. Typo in Localisation Directory: "engligh"

**Decision:** Preserve the historic typo `localisation/engligh/` (missing 's').

**Context:** The directory was created with the typo in v1.0.0. The engine reads it correctly. Correcting it would break existing manual installations.

**Alternatives:**
- Correct the typo and include a migration — Rejected: breaks manual installs, no benefit
- Support both spellings — Rejected: complexity, confusion

**Consequences:**
- A permanent typo in the file path
- Documented in the FAQ
- No functional impact

**Status:** Accepted (v1.0.0)

---

## 9. Dual Descriptor Files (Root + Local)

**Decision:** Maintain identical `ui-galaxy-sizes.mod` (repository root) and `ui-galaxy-sizes/descriptor.mod` (mod directory).

**Context:** Steam Workshop expects the descriptor at the mod root. Paradox Launcher v1 expected it at the repository root. Paradox Launcher v2 expects it in the mod directory. Both must exist for compatibility.

**Alternatives:**
- Single descriptor with a symbolic link — Rejected: Windows symbolic link issues; Steam Workshop does not follow links
- Only the root descriptor — Rejected: Launcher v2 may not locate it
- Only the local descriptor — Rejected: Steam Workshop may not locate it

**Consequences:**
- Both files must remain identical (validated in continuous integration)
- Slight duplication
- Maximum compatibility

**Status:** Accepted (v1.0.0)

---

## 10. No Build System / CI Only Validation

**Decision:** No build step. Continuous integration executes validation scripts only. Release artefacts are created by the packaging script.

**Context:** The mod consists of static files. Build systems introduce complexity with no benefit.

**Alternatives:**
- Node.js build with npm scripts — Rejected: unnecessary dependency
- Python build — Rejected: unnecessary dependency
- Makefile — Rejected: excessive for archive creation

**Consequences:**
- Simple repository
- Validation scripts in bash (portable)
- Manual packaging for release
- No compiled artefacts to verify

**Status:** Accepted (v1.0.0)

---

## 11. GNU GPL v3 Licence

**Decision:** Licensed under the GNU GPL v3.

**Context:** Paradox modding community convention. Ensures derivatives remain open. Compatible with the Stellaris modding ecosystem.

**Alternatives:**
- MIT — Rejected: permits proprietary derivatives
- CC BY-NC-SA — Rejected: the non-commercial restriction is problematic for mods
- Paradox-specific — Rejected: no standard licence

**Consequences:**
- Derivatives must remain GPL v3
- The source must be supplied
- Compatible with Steam Workshop terms
- Commercial use permitted (with source)

**Status:** Accepted (v1.0.0)

---

## 12. Parameter Scaling Methodology

**Decision:** Parameters are tuned manually per size, not derived algorithmically.

**Context:** Galaxy generation balance is non-linear. Engine conduct changes at scale. Manual tuning produces superior gameplay.

**Alternatives:**
- Mathematical scaling formulas — Rejected: does not correspond to engine conduct
- Copy vanilla Huge and multiply — Rejected: breaks at the extremes
- Community voting — Rejected: subjective, slow

**Consequences:**
- Author judgement required for each new size
- No automated scaling
- Balance validated by playtesting
- Documented in data-model.md

**Status:** Accepted (v1.0.0)

---

## 13. Crisis Strength Scaling

**Decision:** Crisis strength follows the vanilla curve `0.25 + 0.00125 × num_stars` (0.375 → 2.75).

**Context:** Larger galaxies contain more empires and more resources, so they require stronger crises to remain menacing. Vanilla derives crisis strength arithmetically from the star count, so following that curve keeps crisis difficulty consistent throughout sizes.

**Alternatives:**
- Fixed crisis strength — Rejected: trivial in large galaxies
- Linear with stars — Adopted: this is precisely the vanilla formula
- Player-adjustable only — Rejected: the default should be balanced

**Consequences:**
- Corresponds exactly to the vanilla scaling
- Players can supersede it in game setup
- May require adjustment per Stellaris patch

**Status:** Accepted (v1.0.0)

---

## 14. Empire Count Scaling

**Decision:** Empire counts scale roughly linearly with stars (2→30 default, 3→60 max).

**Context:** More stars means more room for empires. However, too many empires causes performance issues and diplomatic clutter. Vanilla derives its default as `num_stars / 66.67`; the mod follows that ratio precisely.

**Alternatives:**
- Square root scaling — Rejected: too few at the high end
- Fixed at the vanilla maximum (60) — Rejected: too many for small sizes
- Engine maximum only — Rejected: no guidance for players

**Consequences:**
- Conservative defaults (30 at 2000 stars)
- Maximums at the engine limits
- Players can adjust downwards

**Status:** Accepted (v1.0.0)

---

## 15. No Runtime Version Detection

**Decision:** Mod declares `supported_version="v4.5.*"` statically. No runtime version adaptation.

**Context:** Stellaris mods are pinned to a version. The engine handles compatibility. Runtime detection is impossible in a declarative mod.

**Alternatives:**
- Multiple descriptor versions — Rejected: Steam Workshop permits a single upload
- Scripted version check — Rejected: no scripts

**Consequences:**
- The mod is hidden on unsupported versions
- Manual update required per Stellaris version
- Clear compatibility boundary

**Status:** Accepted (v1.0.0)

---

## 16. Vanilla `galaxy_size` Branches Do Not Recognise Custom Sizes

**Decision:** Accept the limitation and document it rather than shipping supersede scripts.

**Context:** Vanilla scripts branch on the literal scenario names `tiny`, `small`, `medium`, `large`, and `huge`. Six of them never correspond to a supplied size, so they resolve to their base value:

| Script | Effect on supplied sizes |
|--------|--------------------------|
| `ai_habitat_cap` | Falls to `min = 1` instead of 10 |
| `storm_size_multiplier` | 1.0 (medium-equivalent) instead of 1.4 |
| `voidworms_*_spawn_chance` | Large and huge voidworms never spawn |
| `voidworm_nest_spawn_minimum` | Minimum not applied |
| `starlit_links_count` | 0 starlit links |
| `eotc_lvl1/2/3_end` | 0 (no `base`) |
| `central_crystal_spawn_effect` | No `else` branch — the crystal never spawns |

**Alternatives:**
- Ship supersede versions of the six vanilla script files — Rejected: fragile throughout patches, conflicts with other mods, extensive maintenance surface
- Ship `scripted_trigger` aliases — Rejected: the vanilla files reference `galaxy_size = tiny` directly, not a trigger

**Consequences:**
- AI habitats and several DLC systems under-scale on supplied sizes
- The Overlord origin degrades gracefully (spawns all 7 systems)
- Revisit if Paradox exposes a star-count trigger

**Status:** Accepted (2026)

---

## Decision Index

| # | Decision | Date | Status |
|---|----------|------|--------|
| 1 | Declarative Configuration Only | 2024 | Accepted |
| 2 | Six Fixed Galaxy Sizes | 2024 | Accepted |
| 3 | Logarithmic Size Progression | 2024 | Accepted |
| 4 | Priority Values: -1, 5, 6, 7, 8, 9 | 2024 | Accepted |
| 5 | Vanilla Shape Coverage per Size | 2024 | Accepted |
| 6 | Ludicrous Requires UI Star Names | 2024 | Accepted |
| 7 | English-Only Localisation | 2024 | Accepted |
| 8 | Typo in "engligh" Directory | 2024 | Accepted |
| 9 | Dual Descriptor Files | 2024 | Accepted |
| 10 | No Build System | 2024 | Accepted |
| 11 | GNU GPL v3 Licence | 2024 | Accepted |
| 12 | Manual Parameter Scaling | 2024 | Accepted |
| 16 | Vanilla `galaxy_size` Branch Limitation | 2026 | Accepted |
| 13 | Crisis Strength Scaling | 2024 | Accepted |
| 14 | Empire Count Scaling | 2024 | Accepted |
| 15 | Static Version Pinning | 2024 | Accepted |

---

## Adding New Decisions

When making a significant design decision:
1. Include an entry in this document
2. Use the format above
3. Link from the relevant documentation
4. Announce it in the pull request and changelog
