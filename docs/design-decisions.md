# Design Decisions

## Purpose

This document records significant architectural and design decisions for the mod, their rationale, and alternatives considered. It serves as institutional memory for maintainers.

## Decision Record Format

Each decision follows:
- **Decision**: What was decided
- **Context**: Why it was needed
- **Alternatives**: What else was considered
- **Consequences**: Trade-offs accepted
- **Status**: Accepted / Superseded / Deprecated

---

## 1. Declarative Configuration Only

**Decision:** Mod contains only static configuration files (Clausewitz script, YAML, PNG). No executable code, scripts, or runtime logic.

**Context:** Stellaris modding supports both scripted mods (Lua/Clausewitz events) and pure data mods. Data mods are simpler, more stable, and have zero security surface.

**Alternatives:**
- Scripted mod with dynamic galaxy generation — Rejected: complexity, maintenance, breakage risk
- Hybrid with minimal scripts — Rejected: no need, engine handles all generation

**Consequences:**
- Zero runtime bugs possible
- No performance overhead
- Limited to engine's built-in generation parameters
- Cannot add custom galaxy shapes or generation algorithms

**Status:** Accepted (v1.0.0)

---

## 2. Six Fixed Galaxy Sizes

**Decision:** Exactly six galaxy sizes (100, 1200, 1400, 1600, 1800, 2000 stars) with fixed parameters.

**Context:** Players requested sizes beyond vanilla Huge (1000). Each size requires a complete scenario file with 40+ tuned parameters.

**Alternatives:**
- Configurable slider (100-2000) — Rejected: engine doesn't support dynamic scenario parameters
- More granular sizes (every 100 stars) — Rejected: 19 scenario files, maintenance burden
- Fewer sizes (just 2000) — Rejected: no intermediate options

**Consequences:**
- Discrete choices only
- Each size hand-tuned for balance
- Adding sizes = linear maintenance cost
- Parameters don't auto-scale

**Status:** Accepted (v1.0.0)

---

## 3. Logarithmic Size Progression

**Decision:** Star counts follow roughly logarithmic progression: 100 → 1200 → 1400 → 1600 → 1800 → 2000.

**Context:** Linear progression (100, 500, 900, 1300, 1700, 2000) would cluster sizes at low end. Logarithmic gives meaningful differentiation at high end where players want it.

**Alternatives:**
- Linear progression — Rejected: poor differentiation at high end
- Exponential progression — Rejected: too extreme, 100 → 2000 gap
- Vanilla-matching progression — Rejected: vanilla stops at 1000

**Consequences:**
- Large jump from Minuscule (100) to Gigantic (1200)
- Smaller increments at high end (200 stars each)
- Matches player desire for "bigger than huge" options

**Status:** Accepted (v1.0.0)

---

## 4. Priority Values: -1, 5, 6, 7, 8, 9

**Decision:** Scenario priorities use non-sequential values with negative for smallest.

**Context:** Engine uses priority for UI ordering. Vanilla sizes use priorities 0-4. Mod sizes must integrate without collision.

**Alternatives:**
- Sequential 10-15 — Rejected: no reason to avoid negative
- Match vanilla scale (0-4) — Rejected: would collide
- Large gaps (100, 200...) — Rejected: unnecessary

**Consequences:**
- Minuscule (-1) appears before vanilla Tiny (0)
- Gigantic (5) appears after vanilla Huge (4)
- Clear visual separation in dropdown
- Gap allows future vanilla sizes

**Status:** Accepted (v1.0.0)

---

## 5. All 11 Shapes Supported for All Sizes

**Decision:** Every scenario declares `supports_shape` for all 11 vanilla shapes.

**Context:** Engine supports 11 galaxy shapes. Restricting shapes would limit player choice without benefit.

**Alternatives:**
- Restrict large sizes to elliptical/spiral — Rejected: no technical reason, player freedom
- Only support shapes tested — Rejected: all shapes work at all sizes

**Consequences:**
- 66 scenario/shape combinations to test
- Some combinations may look odd (Ring at 2000 stars)
- No maintenance burden (declaration only)

**Status:** Accepted (v1.0.0)

---

## 6. Ludicrous Requires UI Star Names

**Decision:** Ludicrous (2000 stars) documents dependency on UI Star Names mod for unique names.

**Context:** Vanilla has ~1000 unique system names. 2000 stars guarantees duplicates without additional names.

**Alternatives:**
- Bundle names in mod — Rejected: licensing, maintenance, duplication
- Generate procedural names — Rejected: engine doesn't support
- Accept duplicates — Rejected: poor UX, confusing

**Consequences:**
- Soft dependency (mod works without, but duplicates)
- Documentation burden
- Player must install two mods
- Clear communication in README/FAQ

**Status:** Accepted (v1.2.0)

---

## 7. English-Only Localisation

**Decision:** Only English localisation provided (`l_english`).

**Context:** Mod author only speaks English. Community translations welcome but not maintained.

**Alternatives:**
- Machine translate to major languages — Rejected: poor quality, misleading
- Crowdsource translations — Rejected: no infrastructure, quality control
- Ship with placeholder keys — Rejected: shows raw keys, worse UX

**Consequences:**
- Non-English players see raw keys (`ui_gigantic`)
- Community can contribute translations via PR
- Low maintenance

**Status:** Accepted (v1.0.0)

---

## 8. Typo in Localisation Folder: "engligh"

**Decision:** Preserve historical typo `localisation/engligh/` (missing 's').

**Context:** Folder created with typo in v1.0.0. Engine reads it correctly. Fixing would break existing manual installations.

**Alternatives:**
- Fix typo, add migration — Rejected: breaks manual installs, no benefit
- Support both — Rejected: complexity, confusion

**Consequences:**
- Permanent typo in file path
- Documented in FAQ
- No functional impact

**Status:** Accepted (v1.0.0)

---

## 9. Dual Descriptor Files (Root + Local)

**Decision:** Maintain identical `ui-galaxy-sizes.mod` (repo root) and `ui-galaxy-sizes/descriptor.mod` (mod folder).

**Context:** Steam Workshop expects descriptor at mod root. Paradox Launcher v1 expected at repo root. Paradox Launcher v2 expects in mod folder. Both must exist for compatibility.

**Alternatives:**
- Single descriptor, symlink — Rejected: Windows symlink issues, Steam Workshop doesn't follow
- Only root — Rejected: Launcher v2 may not find
- Only local — Rejected: Steam Workshop may not find

**Consequences:**
- Must keep identical (validated in CI)
- Slight duplication
- Maximum compatibility

**Status:** Accepted (v1.0.0)

---

## 10. No Build System / CI Only Validation

**Decision:** No build step. CI runs validation scripts only. Release artefacts created by packaging script.

**Context:** Mod is static files. Build systems add complexity with zero benefit.

**Alternatives:**
- Node.js build with npm scripts — Rejected: unnecessary dependency
- Python build — Rejected: unnecessary dependency
- Makefile — Rejected: overkill for zip creation

**Consequences:**
- Simple repository
- Validation scripts in bash (portable)
- Manual packaging for release
- No compiled artefacts to verify

**Status:** Accepted (v1.0.0)

---

## 11. GNU GPL v3 License

**Decision:** Licensed under GNU GPL v3.

**Context:** Paradox modding community norm. Ensures derivatives remain open. Compatible with Stellaris modding ecosystem.

**Alternatives:**
- MIT — Rejected: allows proprietary derivatives
- CC BY-NC-SA — Rejected: non-commercial restriction problematic for mods
- Paradox-specific — Rejected: no standard licence

**Consequences:**
- Derivatives must be GPL v3
- Source must be provided
- Compatible with Steam Workshop terms
- Commercial use allowed (with source)

**Status:** Accepted (v1.0.0)

---

## 12. Parameter Scaling Methodology

**Decision:** Parameters hand-tuned per size, not algorithmically derived.

**Context:** Galaxy generation balance is non-linear. Engine behaviour changes at scale. Hand-tuning produces better gameplay.

**Alternatives:**
- Mathematical scaling formulas — Rejected: doesn't match engine behaviour
- Copy vanilla Huge and multiply — Rejected: breaks at extremes
- Community voting — Rejected: subjective, slow

**Consequences:**
- Author judgement required for new sizes
- No automated scaling
- Balance validated by playtesting
- Documented in data-model.md

**Status:** Accepted (v1.0.0)

---

## 13. Crisis Strength Scaling

**Decision:** Crisis strength increases with galaxy size (0.25 → 2.75).

**Context:** Larger galaxies have more empires, more resources, need stronger crises to be threatening.

**Alternatives:**
- Fixed crisis strength — Rejected: trivial in large galaxies
- Linear with stars — Rejected: too strong at 2000
- Player-adjustable only — Rejected: default should be balanced

**Consequences:**
- Approximate balance, not mathematically derived
- Players can override in game setup
- May need adjustment per Stellaris patch

**Status:** Accepted (v1.0.0)

---

## 14. Empire Count Scaling

**Decision:** Empire counts scale roughly linearly with stars (3→30 default, 3→60 max).

**Context:** More stars = more room for empires. But too many empires = performance issues, diplomacy clutter.

**Alternatives:**
- Square root scaling — Rejected: too few at high end
- Fixed at vanilla max (60) — Rejected: too many for small sizes
- Engine maximum only — Rejected: no guidance for players

**Consequences:**
- Defaults conservative (30 at 2000 stars)
- Maximums at engine limits
- Players can adjust down

**Status:** Accepted (v1.0.0)

---

## 15. No Runtime Version Detection

**Decision:** Mod declares `supported_version="v4.5.*"` statically. No runtime version adaptation.

**Context:** Stellaris mods are version-pinned. Engine handles compatibility. Runtime detection impossible in declarative mod.

**Alternatives:**
- Multiple descriptor versions — Rejected: Steam Workshop single upload
- Scripted version check — Rejected: no scripts

**Consequences:**
- Mod hidden on unsupported versions
- Manual update needed per Stellaris version
- Clear compatibility boundary

**Status:** Accepted (v1.0.0)

---

## Decision Index

| # | Decision | Date | Status |
|---|----------|------|--------|
| 1 | Declarative Configuration Only | 2024 | Accepted |
| 2 | Six Fixed Galaxy Sizes | 2024 | Accepted |
| 3 | Logarithmic Size Progression | 2024 | Accepted |
| 4 | Priority Values: -1, 5, 6, 7, 8, 9 | 2024 | Accepted |
| 5 | All 11 Shapes Supported | 2024 | Accepted |
| 6 | Ludicrous Requires UI Star Names | 2024 | Accepted |
| 7 | English-Only Localisation | 2024 | Accepted |
| 8 | Typo in "engligh" Folder | 2024 | Accepted |
| 9 | Dual Descriptor Files | 2024 | Accepted |
| 10 | No Build System | 2024 | Accepted |
| 11 | GNU GPL v3 License | 2024 | Accepted |
| 12 | Hand-Tuned Parameter Scaling | 2024 | Accepted |
| 13 | Crisis Strength Scaling | 2024 | Accepted |
| 14 | Empire Count Scaling | 2024 | Accepted |
| 15 | Static Version Pinning | 2024 | Accepted |

---

## Adding New Decisions

When making a significant design decision:
1. Add entry to this document
2. Use the format above
3. Link from relevant documentation
4. Announce in PR/changelog