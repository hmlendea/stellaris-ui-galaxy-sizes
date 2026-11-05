# Ambiguities and Open Questions

## Purpose

This document tracks unresolved design questions, engine behaviours that need verification, and assumptions that may need correction. Items here represent known unknowns — not bugs, but areas where the mod's behaviour depends on undocumented or unclear engine mechanics.

## Engine Behaviour Ambiguities

### 1. Priority Ordering Semantics

**Question:** How does the engine use `priority` when multiple scenarios are valid?

**Current Assumption:** Higher priority = preferred/default in UI dropdown. Values: -1, 5, 6, 7, 8, 9.

**Unknowns:**
- Does negative priority have special meaning?
- Is priority only for UI ordering, or does it affect generation?
- What happens if two scenarios have same priority?

**Verification Needed:** Test with duplicate priorities, observe UI order.

### 2. Crisis Strength Scaling Formula

**Question:** What is the exact formula mapping `crisis_strength` to actual crisis fleet power?

**Current Assumption:** Linear multiplier on vanilla crisis scaling.

**Unknowns:**
- Is 1.0 = vanilla Huge (1000 stars) crisis?
- Does it scale with `num_stars` automatically?
- Are there caps or diminishing returns?

**Verification Needed:** Compare crisis fleet power across sizes in-game.

### 3. Empire Placement Algorithm

**Question:** How does the engine distribute `num_empires` across the galaxy?

**Current Assumption:** Roughly even distribution with `min_distance_between_empires` constraint.

**Unknowns:**
- Does `num_advanced_empires` affect placement?
- How does `guaranteed_habitable` interact with empire spawns?
- What happens when empires can't fit (clamping behaviour)?

**Verification Needed:** Map empire positions for different sizes.

### 4. Shape Support Declaration

**Question:** Does `supports_shape` restrict player choice, or just declare compatibility?

**Current Assumption:** Player can only select declared shapes for that scenario.

**Unknowns:**
- What if player selects undeclared shape via console/mod?
- Does engine validate at generation time?
- Are there shapes the engine supports but mod doesn't declare?

**Verification Needed:** Test undeclared shape selection.

### 5. System Name Pool Exhaustion

**Question:** Exactly how does the engine handle insufficient system names?

**Current Assumption:** Duplicates allowed, warning logged.

**Unknowns:**
- At what threshold does warning trigger?
- Are duplicates sequential or random?
- Does UI Star Names mod integrate at engine level or mod level?

**Verification Needed:** Generate Ludicrous with/without UI Star Names, count duplicates.

## Parameter Interaction Ambiguities

### 6. Radius vs Num_Stars Relationship

**Question:** How do `radius` and `num_stars` interact for density?

**Current Assumption:** `radius` sets galaxy bounds, `num_stars` fills within. Density = stars/area.

**Unknowns:**
- Does engine adjust radius to fit stars?
- Minimum distance between stars enforced?
- How does shape affect effective radius?

**Verification Needed:** Measure actual star density per size/shape.

### 7. Nebulae Placement Rules

**Question:** How does `num_nebulae` translate to actual nebula count and placement?

**Current Assumption:** Engine places approximately that many nebulae.

**Unknowns:**
- Exact count or approximate?
- Placement algorithm (random, clustered, avoided)?
- Interaction with shape (e.g., spiral arms)?
- Visual vs gameplay nebulae distinction?

**Verification Needed:** Count nebulae in generated galaxies.

### 8. Fallen Empire Spawn Logic

**Question:** How do `num_fallen_empires` and `num_fallen_empires_default` interact?

**Current Assumption:** Default = UI default, max = cap for player adjustment.

**Unknowns:**
- Does engine guarantee fallen empire spawns?
- Placement rules (edges, specific systems)?
- Interaction with regular empire placement?

**Verification Needed:** Count fallen empires across multiple generations.

## Cross-Mod Interaction Ambiguities

### 9. UI Star Names Integration Point

**Question:** At what point does UI Star Names provide names to the engine?

**Current Assumption:** Mod loads first, extends name pool, engine draws from pool.

**Unknowns:**
- Load order dependency?
- Does it patch vanilla name files or add new?
- Conflict if multiple name mods active?

**Verification Needed:** Test load order variations.

### 10. Other Galaxy Mod Compatibility

**Question:** What happens when two mods define `setup_scenario` with same name?

**Current Assumption:** First loaded wins, or merge, or error.

**Unknowns:**
- Launcher load order deterministic?
- Can scenarios be merged additively?
- Namespace collision handling?

**Verification Needed:** Install two size mods, observe behaviour.

## Performance Ambiguities

### 11. Generation Time Complexity

**Question:** What is the algorithmic complexity of galaxy generation?

**Current Assumption:** Roughly O(n²) for hyperlane generation, O(n) for star placement.

**Unknowns:**
- Exact complexity class?
- Which parameters affect which phase?
- Memory allocation pattern (single large, incremental)?

**Verification Needed:** Profile generation at different sizes.

### 12. Save Game Size Scaling

**Question:** How does save file size scale with `num_stars`?

**Current Assumption:** Linear with stars, plus empire/ship/fleet data.

**Unknowns:**
- Base overhead per star?
- Compression effectiveness?
- Impact on load times?

**Verification Needed:** Measure save sizes across sizes.

## Documentation Ambiguities

### 13. Vanilla Parameter Defaults

**Question:** What are the vanilla values for all 15 parameters for each vanilla size?

**Current Assumption:** Mod parameters are scaled from vanilla Huge (1000 stars).

**Unknowns:**
- Exact vanilla Tiny/Small/Medium/Large/Huge values?
- Are any parameters derived rather than explicit?
- Engine defaults for omitted parameters?

**Verification Needed:** Extract from game files or reverse-engineer.

### 14. Supported Version Wildcard Semantics

**Question:** Does `v4.5.*` match 4.5.0, 4.5.1, 4.5.2, 4.5.10?

**Current Assumption:** Standard wildcard, matches any 4.5.x.

**Unknowns:**
- Launcher parsing exact rules?
- Does 4.6.0 match `v4.*`?
- Pre-release versions (4.5.0-beta)?

**Verification Needed:** Test launcher version matching.

## Process Ambiguities

### 15. Steam Workshop Update Propagation

**Question:** How long for Steam Workshop updates to reach all users?

**Current Assumption:** Minutes to hours, automatic.

**Unknowns:**
- Forced update on launch?
- Offline mode behaviour?
- Version pinning in playsets?

**Verification Needed:** Monitor update rollout.

### 16. Paradox Mods vs Steam Workshop Parity

**Question:** Are there differences in mod loading between platforms?

**Current Assumption:** Same mod files work identically.

**Unknowns:**
- Different mount paths?
- Different validation?
- Platform-specific metadata?

**Verification Needed:** Test same mod on both platforms.

## Tracking Template

For each ambiguity, track:

```markdown
### N. Short Title

**Status:** Open | Investigating | Resolved | Won't Fix

**Question:** [Clear question]

**Current Assumption:** [What we assume]

**Impact:** [If wrong, what breaks]

**Verification Plan:** [How to test]

**Resolution:** [When resolved, what was found]

**Related:** [Links to issues, docs, code]
```

## Priority Classification

| Priority | Criteria | Examples |
|----------|----------|----------|
| Critical | Wrong assumption breaks mod | Priority semantics, crisis formula |
| High | Affects balance/UX | Empire placement, shape support |
| Medium | Affects edge cases | Name pool, nebula placement |
| Low | Documentation only | Vanilla defaults, version wildcard |

## Resolution Process

1. **Identify** — Add to this document
2. **Prioritise** — Assign priority
3. **Investigate** — Test, research, reverse-engineer
4. **Document** — Update assumption or add workaround
5. **Close** — Mark resolved with finding
6. **Propagate** — Update affected docs (data-model.md, invariants.md, etc.)

## Current Status Summary

| # | Title | Priority | Status |
|---|-------|----------|--------|
| 1 | Priority Ordering Semantics | Critical | Open |
| 2 | Crisis Strength Scaling Formula | High | Open |
| 3 | Empire Placement Algorithm | High | Open |
| 4 | Shape Support Declaration | Medium | Open |
| 5 | System Name Pool Exhaustion | High | Open |
| 6 | Radius vs Num_Stars Relationship | Medium | Open |
| 7 | Nebulae Placement Rules | Low | Open |
| 8 | Fallen Empire Spawn Logic | Medium | Open |
| 9 | UI Star Names Integration Point | High | Open |
| 10 | Other Galaxy Mod Compatibility | Low | Open |
| 11 | Generation Time Complexity | Low | Open |
| 12 | Save Game Size Scaling | Low | Open |
| 13 | Vanilla Parameter Defaults | Medium | Open |
| 14 | Supported Version Wildcard Semantics | Low | Open |
| 15 | Steam Workshop Update Propagation | Low | Open |
| 16 | Paradox Mods vs Steam Workshop Parity | Low | Open |

## Contributing Resolutions

If you resolve an ambiguity:
1. Update this document with finding
2. Update affected documentation
3. Add validation if testable
4. Close related GitHub issue