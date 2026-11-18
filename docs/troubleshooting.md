# Troubleshooting

## Rapid Diagnosis

| Symptom | Probable Cause | First Step |
|---------|--------------|------------|
| Mod not in launcher | Incorrect install path | Verify mod directory |
| Sizes absent from dropdown | Scenario parse error | Check `game.log` |
| Generation crashes | Out of memory | Try a smaller size |
| Duplicate system names | Ludicrous without UI Star Names | Install UI Star Names |
| "Mod incompatible" | Stellaris version mismatch | Check version |
| Save won't load | Parameter change broke save | Start a new game |

## Installation Issues

### Mod Not Appearing in Launcher

**Symptoms:** Mod not listed in Playsets, not in dropdown

**Checks:**
1. **File location:**
   ```
   Windows: %USERPROFILE%\Documents\Paradox Interactive\Stellaris\mod\
   macOS: ~/Documents/Paradox Interactive/Stellaris/mod/
   Linux: ~/.local/share/Paradox Interactive/Stellaris/mod/
   ```
   Must contain: `ui-galaxy-sizes.mod` + `ui-galaxy-sizes/` directory

2. **File permissions:** Readable by user running Stellaris

3. **Descriptor syntax:** Validate `ui-galaxy-sizes.mod`:
   ```clausewitz
   name="Universum Infinitum - Galaxy Sizes"
   version="1.5.0"
   path="mod/ui-galaxy-sizes"
   supported_version="v4.5.*"
   tags={"Gameplay" "Map"}
   ```

4. **Launcher cache:** Restart launcher, or delete `launcher-v2.sqlite` (Paradox Launcher v2)

### "Mod Incompatible" in Launcher

**Cause:** Stellaris version not matching `supported_version`

**Fix:**
- Update Stellaris to 4.5.x
- Or wait for mod update supporting your version
- Check `supported_version` in descriptor matches your Stellaris version

### Manual Install Not Working

**Prevalent mistakes:**
- Extracting the archive but not placing both the `.mod` file AND the directory
- Placing files in a `mod/` subdirectory instead of directly in the mod directory
- Using an obsolete Paradox Launcher v1 path with the v2 launcher

## Galaxy Generation Issues

### Generation Hangs / Takes Very Long

**Symptoms:** "Generating galaxy..." spinner for minutes

**Causes & Fixes:**

| Size | Expected Time | If Exceeds | Action |
|------|---------------|------------|--------|
| Minuscule | < 5s | 30s | Restart, try again |
| Gigantic | 10-20s | 60s | Check RAM, close apps |
| Immense | 15-30s | 90s | Check RAM, close apps |
| Colossal | 20-40s | 120s | Require 16GB+ RAM |
| Titanic | 30-60s | 180s | Require 16GB+ RAM |
| Ludicrous | 60-120s | 300s | Require 32GB+ RAM |

**General fixes:**
- Close the browser, other games, and demanding applications
- Verify the available memory (Task Manager / `free -h`)
- Try a smaller size first to verify that the mod functions
- Check `game.log` for repeated warnings

### Generation Crashes / "Out of Memory"

**Symptoms:** Game closes, or "Out of memory" error

**Fixes:**
1. Reduce the galaxy size
2. Install additional memory
3. Increase the swap or page file
4. Close all other applications
5. Launch with `-lowmem` (if supported)

**Minimum RAM estimates:**
- Minuscule: 4GB
- Gigantic: 8GB
- Immense: 12GB
- Colossal: 16GB
- Titanic: 16-24GB
- Ludicrous: 24-32GB

### Galaxy Generates But Appears Incorrect

**Symptoms:** Incorrect shape, disconnected systems, absent empires

**Checks:**
1. **Shape support:** Gigantic through Ludicrous support all 10 shapes; Minuscule supports the 4 vanilla tiny shapes. If one fails, try another.
2. **Parameter bounds:** The engine clamps extreme values. Check `game.log` for warnings.
3. **Mod conflicts:** Deactivate other galaxy and map mods.

### "Failed to Generate Galaxy" Error Dialogue

**Symptoms:** Explicit error dialogue after generation attempt

**Prevalent causes:**
- `num_stars` exceeds the engine firm limit (~2000-2500)
- `radius` too large for the engine
- Corrupted scenario file
- Mod conflict

**Debug:**
1. Check `game.log` for `[galaxy]` and `[error]` records
2. Try Minuscule (100 stars) — if it functions, the mod loads correctly
3. Deactivate other mods
4. Verify the scenario file syntax

## UI Issues

### Sizes Display as Raw Keys (ui_gigantic, etc.)

**Cause:** Localisation file absent or not loading

**Fixes:**
1. Verify that `ui-galaxy-sizes/localisation/engligh/ui-galaxy-sizes_l_english.yml` exists
2. Check the YAML syntax (no tabs, correct indentation)
3. Restart the game (localisation caches)
4. Verify the file encoding (UTF-8)

### Absent Thumbnail in Launcher

**Cause:** `thumbnail.png` absent or of incorrect format

**Fix:**
- Ensure `ui-galaxy-sizes/thumbnail.png` exists
- 256x256 pixels, PNG format
- Not required for functionality

## Save Game Issues

### Save Won't Load After Mod Update

**Cause:** Galaxy generation parameters changed, and the save references preceding values

**Fix:** Start a new game. Galaxy size mods are not save-compatible throughout parameter changes.

### Save Won't Load After Mod Extraction

**Cause:** Save references the mod's scenario, but the mod is not loaded

**Fix:** Re-activate the mod, or start a new game.

### Multiplayer Desync

**Cause:** Players have different mod versions or different mods active

**Fix:**
- All players have identical mod versions
- All players have identical mod sets
- Host generates the galaxy

## Log Analysis

### Finding Relevant Logs

**Log location:**
```
Windows: %USERPROFILE%\Documents\Paradox Interactive\Stellaris\logs\game.log
macOS: ~/Documents/Paradox Interactive/Stellaris/logs/game.log
Linux: ~/.local/share/Paradox Interactive/Stellaris/logs/game.log
```

**Filter for mod:**
```bash
grep -i "universum infinitum" game.log
grep -i "ui_" game.log
grep -i "setup_scenario" game.log
```

### Key Log Patterns

**Mod loading:**
```
[mod] Loading mod: Universum Infinitum - Galaxy Sizes (v1.5.0)
[mod]   path: mod/ui-galaxy-sizes
[mod]   supported_version: v4.5.*
```

**Scenario parsing:**
```
[galaxy] Parsing setup_scenario: ui_ludicrous
[galaxy]   num_stars=2000
[galaxy]   supports_shape: 10 entries
```

**Warnings (non-fatal):**
```
[warning] Scenario ui_ludicrous: crisis_strength 2.75 clamped to 2.5
[warning] Insufficient system names for 2000 stars, duplicates will occur
```

**Errors (fatal to scenario):**
```
[error] Failed to parse map/setup_scenarios/ui_colossal.txt
[error]   Line 12: Expected '=' found 'priority'
[error] Scenario ui_colossal ignored
```

## Mod Conflicts

### Known Compatible Mods

| Mod Type | Examples | Notes |
|----------|----------|-------|
| UI | UI Star Names, UI Overhaul Dynamic | Ludicrous requires UI Star Names |
| Species | More Species, Species Pack | No conflict |
| Graphics | Better Stars, Coloured Stars | No conflict |
| Gameplay | Guilli's Planet Modifiers | Usually compatible |

### Known Incompatible Mods

| Mod Type | Conflict |
|----------|----------|
| Other galaxy size mods | Duplicate scenarios, undefined behaviour |
| Map generation overhauls | May supersede scenarios |
| Total conversions | May not support custom scenarios |

### Debugging Conflicts

1. Deactivate all other mods
2. Test each size
3. Re-activate mods one by one
4. Identify the conflicting mod
5. Report to both mod authors

## Platform-Specific Issues

### Steam Workshop

- **Mod not updating:** Restart Steam, verify subscription
- **Old version loaded:** Unsubscribe, resubscribe
- **Workshop sync issues:** Verify game files in Steam

### GitHub Releases / Manual

- **Incorrect version:** Re-download and verify the checksum
- **Absent files:** Extract the entire archive, not merely the directory

### Nexus Mods / Paradox Mods

- **Version mismatch:** Check that the mod version matches the Stellaris version
- **Installer issues:** Use manual installation as a fallback

## Reporting Issues

### Before Reporting

1. Consult this troubleshooting guide
2. Check GitHub Issues (open and closed)
3. Try a minimal reproduction (with only this mod active)
4. Note the exact Stellaris version (bottom right of the main menu)
5. Note the mod version (launcher or descriptor)

### Issue Template

```markdown
**Stellaris Version:** 4.5.2
**Mod Version:** 1.5.0
**Scenario:** Ludicrous (2000 stars)
**Shape:** Spiral 4-arm
**Other Mods:** UI Star Names v2.1, [list others]

**Issue:** Generation crashes at 45% with "Out of memory"

**Log Excerpt:**
```
[galaxy] Generating galaxy: Ludicrous (2000 stars)
[galaxy] Placing 30 AI empires
[error] Out of memory allocating star positions
```

**Steps to Reproduce:**
1. New game
2. Select Ludicrous
3. Select Spiral 4-arm
4. Generate

**System:** Windows 11, 16GB RAM, RTX 3070
```

### Log Excerpt Guidelines

- Include 20 lines before and after the error
- Redact nothing (no personal data in logs)
- Use code blocks

## Emergency Workarounds

### Requirement to Play Immediately, Mod Broken

1. **Use a smaller size** — Minuscule and Gigantic usually work
2. **Deactivate the mod** — Play vanilla sizes
3. **Restore a preceding mod version** — Download a previous release from GitHub

### Mod Author Unavailable

- Fork repository
- Fix locally
- Install manually
- Submit PR when possible

## Escalation

| Severity | Channel | Response Target |
|----------|---------|-----------------|
| Crash/data loss | GitHub Issue (bug label) | 48 hours |
| Generation failure | GitHub Issue | 1 week |
| UI/minor | GitHub Discussion | Best effort |
| Security | SECURITY.md process | 24 hours |

## Related Documents

- `faq.md` — Prevalent questions
- `error-handling.md` — Engine error conduct
- `logging.md` — Log reference
- `invariants.md` — Configuration constraints
- `change-guide.md` — Fix release process
