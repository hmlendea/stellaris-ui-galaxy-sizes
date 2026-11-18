# Ambiguities and Open Questions

## Purpose

This document tracks unresolved design questions, engine behaviours that require verification, and premises that may require correction. Items here represent known unknowns — not defects, but areas where the mod's conduct depends on undocumented or unclear engine mechanics.

## Engine Behaviour Ambiguities

### 1. Priority Ordering Semantics

**Question:** How does the engine use `priority` when multiple scenarios are valid?

**Current Premise:** Higher priority = preferred or default in the UI dropdown. Values: -1, 5, 6, 7, 8, 9.

**Unknowns:**
- Does negative priority have special meaning?
- Is priority only for UI ordering, or does it affect generation?
- What occurs if two scenarios possess an identical priority?

**Verification Required:** Test with duplicate priorities, then observe the UI order.

### 2. Crisis Strength Scaling Formula

**Question:** What is the exact formula mapping `crisis_strength` to genuine crisis fleet power?

**Current Premise:** A linear multiplier on vanilla crisis scaling.

**Unknowns:**
- Is 1.0 = vanilla Huge (1000 stars) crisis?
- Does it scale with `num_stars` automatically?
- Are there caps or diminishing returns?

**Verification Required:** Compare crisis fleet power throughout sizes in-game.

### 3. Empire Placement Algorithm

**Question:** How does the engine distribute `num_empires` throughout the galaxy?

**Current Premise:** A roughly even distribution governed by `home_system_partitions` and `open_space_partitions`.

**Unknowns:**
- Does `advanced_empire_default` affect placement?
- How does each empire's home system partition interact with nebula and cluster placement?
- What happens when empires can't fit (clamping behaviour)?

**Verification Required:** Map the empire positions for different sizes.

### 4. Shape Support Declaration

**Question:** Does `supports_shape` restrict player choice, or just declare compatibility?

**Current Premise:** The player can select only the declared shapes for that scenario.

**Unknowns:**
- What if player selects undeclared shape via console/mod?
- Does engine validate at generation time?
- Are there shapes the engine supports but mod doesn't declare?

**Verification Required:** Test the selection of an undeclared shape.

### 5. System Name Pool Exhaustion

**Question:** Exactly how does the engine handle insufficient system names?

**Current Premise:** Duplicates permitted, warning logged.

**Unknowns:**
- At what threshold does the warning initiate?
- Are duplicates sequential or random?
- Does UI Star Names mod integrate at engine level or mod level?

**Verification Required:** Generate Ludicrous with and without UI Star Names, then count the duplicates.

## Parameter Interaction Ambiguities

### 6. Radius vs Num_Stars Relationship

**Question:** How do `radius` and `num_stars` interact for density?

**Current Premise:** `radius` establishes the galaxy bounds and `num_stars` fills them. Density = stars / area.

**Unknowns:**
- Does engine adjust radius to fit stars?
- Minimum distance between stars enforced?
- How does shape affect effective radius?

**Verification Required:** Measure genuine star density per size and shape.

### 7. Nebulae Placement Rules

**Question:** How does `num_nebulas` translate to genuine nebula count and placement?

**Current Premise:** The engine places approximately that many nebulae.

**Unknowns:**
- Exact count or approximate?
- Placement algorithm (random, clustered, avoided)?
- Interaction with shape (e.g., spiral arms)?
- Visual vs gameplay nebulae distinction?

**Verification Required:** Count the nebulae in generated galaxies.

### 8. Fallen Empire Spawn Logic

**Question:** How do `fallen_empire_max` and `fallen_empire_default` interact?

**Current Premise:** Default = UI default, maximum = cap for player adjustment. Vanilla defines exactly 6 fallen empire types, so any `fallen_empire_max` above 6 cannot be honoured.

**Unknowns:**
- Does engine guarantee fallen empire spawns?
- Placement rules (edges, specific systems)?
- Interaction with regular empire placement?

**Verification Required:** Count fallen empires throughout multiple generations.

## Cross-Mod Interaction Ambiguities

### 9. UI Star Names Integration Point

**Question:** At what point does UI Star Names provide names to the engine?

**Current Premise:** The companion mod loads first, extends the name pool, and the engine draws from that pool.

**Unknowns:**
- Load order dependency?
- Does it patch vanilla name files or include new ones?
- Conflict if multiple name mods active?

**Verification Required:** Test load order variations.

### 10. Other Galaxy Mod Compatibility

**Question:** What occurs when two mods define a `setup_scenario` with an identical name?

**Current Premise:** The first mod loaded prevails, or the definitions merge, or the engine reports an error.

**Unknowns:**
- Launcher load order deterministic?
- Can scenarios be merged additively?
- Namespace collision handling?

**Verification Required:** Install two size mods and observe the conduct.

## Performance Ambiguities

### 11. Generation Time Complexity

**Question:** What is the algorithmic complexity of galaxy generation?

**Current Premise:** Approximately O(n2) for hyperlane generation and O(n) for star placement.

**Unknowns:**
- Exact complexity class?
- Which parameters affect which phase?
- Memory allocation pattern (single large allocation or incremental)?

**Verification Required:** Profile generation at different sizes.

### 12. Save Game Size Scaling

**Question:** How does save file size scale with `num_stars`?

**Current Premise:** Linear with the star count, plus empire, ship, and fleet data.

**Unknowns:**
- Base overhead per star?
- Compression effectiveness?
- Impact on load times?

**Verification Required:** Measure save sizes throughout sizes.

## Documentation Ambiguities

### 13. Vanilla Parameter Defaults

**Question:** What are the vanilla values for all 15 parameters for each vanilla size?

**Current Premise:** The mod parameters are scaled from vanilla Huge (1000 stars).

**Unknowns:**
- Exact vanilla Tiny, Small, Medium, Large, and Huge values?
- Are any parameters derived rather than explicit?
- Engine defaults for omitted parameters?

**Verification Required:** Extract from game files or reverse-engineer.

### 14. Supported Version Wildcard Semantics

**Question:** Does `v4.5.*` match 4.5.0, 4.5.1, 4.5.2, 4.5.10?

**Current Premise:** Standard wildcard semantics, matching any 4.5.x release.

**Unknowns:**
- Launcher parsing exact rules?
- Does 4.6.0 match `v4.*`?
- Pre-release versions (4.5.0-beta)?

**Verification Required:** Test the launcher version correspondence.

## Process Ambiguities

### 15. Steam Workshop Update Propagation

**Question:** How long for Steam Workshop updates to reach all users?

**Current Premise:** Minutes to hours, automatically.

**Unknowns:**
- Forced update on launch?
- Offline mode behaviour?
- Version pinning in playsets?

**Verification Required:** Monitor the update rollout.

### 16. Paradox Mods vs Steam Workshop Parity

**Question:** Are there differences in mod loading between platforms?

**Current Premise:** Identical mod files function identically.

**Unknowns:**
- Different mount paths?
- Different validation?
- Platform-specific metadata?

**Verification Required:** Test an identical mod on both platforms.

## Tracking Template

For each ambiguity, track:

```markdown
### N. Short Title

**Status:** Open | Investigating | Resolved | Won't Fix

**Question:** [Clear question]

**Current Premise:** [What is presumed]

**Impact:** [If incorrect, what breaks]

**Verification Plan:** [How to test]

**Resolution:** [When resolved, what was found]

**Related:** [Links to issues, docs, code]
```

## Priority Classification

| Priority | Criteria | Examples |
|----------|----------|----------|
| Critical | An erroneous premise breaks the mod | Priority semantics, crisis formula |
| High | Affects balance or user experience | Empire placement, shape support |
| Medium | Affects edge cases | Name pool, nebula placement |
| Low | Documentation only | Vanilla defaults, version wildcard |

## Resolution Process

1. **Identify** — Include in this document
2. **Prioritise** — Assign priority
3. **Investigate** — Test, research, reverse-engineer
4. **Document** — Revise the premise or include a workaround
5. **Close** — Mark resolved with the discovery
6. **Propagate** — Revise affected docs (data-model.md, invariants.md, etc.)

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
1. Revise this document with the discovery
2. Revise affected documentation
3. Include validation if testable
4. Close the related GitHub issue
