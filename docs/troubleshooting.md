# Troubleshooting

## Quick Diagnosis

| Symptom | Likely Cause | First Step |
|---------|--------------|------------|
| Mod not in launcher | Wrong install path | Verify mod directory |
| Sizes missing from dropdown | Scenario parse error | Check `game.log` |
| Generation crashes | Out of memory | Try smaller size |
| Duplicate system names | Ludicrous without UI Star Names | Install UI Star Names |
| "Mod incompatible" | Stellaris version mismatch | Check version |
| Save won't load | Parameter change broke save | Start new game |

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
   Must contain: `ui-galaxy-sizes.mod` + `ui-galaxy-sizes/` folder

2. **File permissions:** Readable by user running Stellaris

3. **Descriptor syntax:** Validate `ui-galaxy-sizes.mod`:
   ```clausewitz
   name="Universum Infinitum: Galaxy Sizes"
   version="1.4.5"
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

**Common mistakes:**
- Extracting zip but not placing both `.mod` file AND folder
- Placing in `mod/` subfolder instead of directly in mod directory
- Using old Paradox Launcher v1 path with v2 launcher

## Galaxy Generation Issues

### Generation Hangs / Takes Very Long

**Symptoms:** "Generating galaxy..." spinner for minutes

**Causes & Fixes:**

| Size | Expected Time | If Exceeds | Action |
|------|---------------|------------|--------|
| Minuscule | < 5s | 30s | Restart, try again |
| Gigantic | 10-20s | 60s | Check RAM, close apps |
| Immense | 15-30s | 90s | Check RAM, close apps |
| Colossal | 20-40s | 120s | Need 16GB+ RAM |
| Titanic | 30-60s | 180s | Need 16GB+ RAM |
| Ludicrous | 60-120s | 300s | Need 32GB+ RAM |

**General fixes:**
- Close browser, other games, heavy apps
- Verify RAM available (Task Manager / `free -h`)
- Try smaller size first to verify mod works
- Check `game.log` for repeated warnings

### Generation Crashes / "Out of Memory"

**Symptoms:** Game closes, or "Out of memory" error

**Fixes:**
1. Reduce galaxy size
2. Add more RAM
3. Increase swap/page file
4. Close all other applications
5. Launch with `-lowmem` (if supported)

**Minimum RAM estimates:**
- Minuscule: 4GB
- Gigantic: 8GB
- Immense: 12GB
- Colossal: 16GB
- Titanic: 16-24GB
- Ludicrous: 24-32GB

### Galaxy Generates But Looks Wrong

**Symptoms:** Wrong shape, disconnected systems, missing empires

**Checks:**
1. **Shape support:** All 11 shapes should work. If one fails, try another.
2. **Parameter bounds:** Engine clamps extreme values. Check `game.log` for warnings.
3. **Mod conflicts:** Disable other galaxy/map mods.

### "Failed to Generate Galaxy" Error Dialog

**Symptoms:** Explicit error dialog after generation attempt

**Common causes:**
- `num_stars` exceeds engine hard limit (~2000-2500)
- `radius` too large for engine
- Corrupted scenario file
- Mod conflict

**Debug:**
1. Check `game.log` for `[galaxy]` and `[error]` entries
2. Try Minuscule (100 stars) — if works, mod loads correctly
3. Disable other mods
4. Verify scenario file syntax

## UI Issues

### Sizes Show as Raw Keys (ui_gigantic, etc.)

**Cause:** Localisation file missing or not loading

**Fixes:**
1. Verify `ui-galaxy-sizes/localisation/engligh/ui-galaxy-sizes_l_english.yml` exists
2. Check YAML syntax (no tabs, proper indentation)
3. Restart game (localisation caches)
4. Verify file encoding (UTF-8)

### Missing Thumbnail in Launcher

**Cause:** `thumbnail.png` missing or wrong format

**Fix:**
- Ensure `ui-galaxy-sizes/thumbnail.png` exists
- 256x256 pixels, PNG format
- Not required for functionality

## Save Game Issues

### Save Won't Load After Mod Update

**Cause:** Galaxy generation parameters changed, save references old values

**Fix:** Start new game. Galaxy size mods are not save-compatible across parameter changes.

### Save Won't Load After Mod Removal

**Cause:** Save references mod's scenario, mod not loaded

**Fix:** Re-enable mod, or start new game.

### Multiplayer Desync

**Cause:** Players have different mod versions or different mods active

**Fix:**
- All players same mod version
- All players same mod set
- Host generates galaxy

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
[mod] Loading mod: Universum Infinitum: Galaxy Sizes (v1.4.5)
[mod]   path: mod/ui-galaxy-sizes
[mod]   supported_version: v4.5.*
```

**Scenario parsing:**
```
[galaxy] Parsing setup_scenario: ui_ludicrous
[galaxy]   num_stars=2000
[galaxy]   supports_shape: 11 entries
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
| UI | UI Star Names, UI Overhaul Dynamic | Ludicrous needs UI Star Names |
| Species | More Species, Species Pack | No conflict |
| Graphics | Better Stars, Colored Stars | No conflict |
| Gameplay | Guilli's Planet Modifiers | Usually compatible |

### Known Incompatible Mods

| Mod Type | Conflict |
|----------|----------|
| Other galaxy size mods | Duplicate scenarios, undefined behaviour |
| Map generation overhauls | May override scenarios |
| Total conversions | May not support custom scenarios |

### Debugging Conflicts

1. Disable all other mods
2. Test each size
3. Re-enable mods one by one
4. Identify conflicting mod
5. Report to both mod authors

## Platform-Specific Issues

### Steam Workshop

- **Mod not updating:** Restart Steam, verify subscription
- **Old version loaded:** Unsubscribe, resubscribe
- **Workshop sync issues:** Verify game files in Steam

### GitHub Releases / Manual

- **Wrong version:** Re-download, verify checksum
- **Missing files:** Extract entire archive, not just folder

### Nexus Mods / Paradox Mods

- **Version mismatch:** Check mod version matches Stellaris version
- **Installer issues:** Use manual install as fallback

## Reporting Issues

### Before Reporting

1. Check this troubleshooting guide
2. Check GitHub Issues (open/closed)
3. Try minimal reproduction (only this mod active)
4. Note exact Stellaris version (main menu bottom right)
5. Note mod version (launcher or descriptor)

### Issue Template

```markdown
**Stellaris Version:** 4.5.2
**Mod Version:** 1.4.5
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

- Include 20 lines before and after error
- Redact nothing (no personal data in logs)
- Use code blocks

## Emergency Workarounds

### Need to Play Now, Mod Broken

1. **Use smaller size** — Minuscule/Gigantic usually work
2. **Disable mod** — Play vanilla sizes
3. **Rollback mod version** — Download previous release from GitHub

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

- `faq.md` — Common questions
- `error-handling.md` — Engine error behaviour
- `logging.md` — Log reference
- `invariants.md` — Configuration constraints
- `change-guide.md` — Fix release process