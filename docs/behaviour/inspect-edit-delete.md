# Inspect, Edit, Delete Behaviour

## Behaviour Overview

**Not Applicable** — This mod has no inspect/edit/delete behaviour.

### Reason

The mod is purely declarative configuration. It contains:
- No CRUD operations
- No user-editable data at runtime
- No entity management
- No delete/restore flows

Players cannot inspect, edit, or delete mod data from within the game. All data is static files.

## Closest Analogous Behaviours (Context)

### Inspect (View Galaxy Size Details)

**Player Action:** Hover/select a galaxy size in the New Game dropdown.

**Engine Behaviour:**
1. Engine displays localised name (e.g., "Ludicrous")
2. Engine may display star count or description (vanilla UI behaviour)
3. Player sees available shapes for that size

**Mod Role:** Provides `name` and `supports_shape` parameters.

### Edit (Adjust Galaxy Settings)

**Player Action:** Adjust sliders in New Game setup (AI empires, fallen empires, crisis strength, etc.).

**Engine Behaviour:**
1. Engine reads scenario min/max bounds
2. Slider constrained to scenario range
3. Default value from scenario `*_default` parameters
4. Player adjusts within bounds

**Mod Role:** Provides bounds and defaults via scenario parameters:

| Setting | Parameter | Example (Ludicrous) |
|---------|-----------|---------------------|
| AI Empires | `num_empires` | 1-60, default 30 |
| Advanced AI | `advanced_empire_default` | 9 |
| Fallen Empires | `fallen_empire_range` | 0-10, default 9 |
| Marauders | `marauder_empire_range` | 0-6, default 5 |
| Crisis Strength | `crisis_strength` | 2.75 |

**Note:** Players edit *game settings*, not mod data. Mod parameters define the bounds.

### Delete (Remove Mod)

**Player Action:** Unsubscribe (Steam) or delete files (manual install).

**Effect:**
1. Mod sizes disappear from dropdown
2. Existing saves referencing mod scenarios may fail to load
3. Vanilla sizes unaffected

**Mod Role:** None (passive removal).

## Mod Author Edit Flow (Development)

For mod authors, "edit" means modifying scenario files:

1. Edit `ui-galaxy-sizes/map/setup_scenarios/ui_<size>.txt`
2. Run validation scripts
3. Test in-game
4. Follow `change-guide.md` for release

See `change-guide.md` and `api-usage-examples.md` for details.

## Not Applicable: Traditional CRUD Patterns

Since this mod has no CRUD behaviour, the following are **not applicable**:

- Detail views / edit forms
- Validation on save
- Optimistic updates
- Undo/redo
- Soft delete / trash
- Bulk operations
- Confirmation dialogs
- Audit trails
- Change tracking
- Conflict resolution (concurrent edits)
- Draft/publish workflows
- Permissions on edit/delete
- Cascade deletes
- Referential integrity checks