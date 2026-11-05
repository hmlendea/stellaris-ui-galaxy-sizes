# Logging

## Logging Framework

**Not Applicable** — This mod has no runtime logging framework.

### Reason

The mod is purely declarative configuration consumed by the Stellaris engine. It contains:
- No executable code
- No runtime processes
- No application logic
- No error handling code
- No background services

All logging occurs within the Stellaris engine itself, which is outside this mod's scope.

## Stellaris Engine Logging (Context)

For reference, the Stellaris engine logs the following relevant to this mod:

### Mod Loading Logs

```
[mod] Loading mod: Universum Infinitum: Galaxy Sizes (v1.4.5)
[mod] Supported version: v4.5.*
[mod] Mount path: mod/ui-galaxy-sizes
```

### Scenario Parsing Logs

```
[galaxy] Parsing setup_scenario: ui_ludicrous
[galaxy]   num_stars=2000, radius=500
[galaxy]   num_empires=1-60, default=30
[galaxy]   crisis_strength=2.75
[galaxy]   supports_shape: 11 shapes
```

### Generation Logs

```
[galaxy] Generating galaxy: Ludicrous (2000 stars)
[galaxy] Placing 30 AI empires
[galaxy] Placing 9 fallen empires
[galaxy] Building hyperlane network
[galaxy] Placing 20 nebulae
[galaxy] Assigning system names
[galaxy] Generation complete in 45.2s
```

### Error Logs (Engine)

```
[error] Failed to parse setup_scenario: ui_colossal.txt
[error]   Missing required parameter: num_stars
[error] Using default values for scenario
```

```
[warning] Scenario ui_ludicrous: insufficient system names
[warning]   Available: 987, Required: 2000
[warning]   Duplicates will occur
```

## Mod-Related Log Correlation

### Correlation Fields

When debugging mod-related issues, correlate on:

| Field | Source | Purpose |
|-------|--------|---------|
| Mod name | `Universum Infinitum: Galaxy Sizes` | Identify mod |
| Mod version | `1.4.5` | Version tracking |
| Scenario name | `ui_ludicrous` | Which scenario |
| Stellaris version | `4.5.1` | Compatibility |
| Timestamp | Engine log | Temporal correlation |

### Log Locations

| Platform | Log Path |
|----------|----------|
| Windows | `%USERPROFILE%\Documents\Paradox Interactive\Stellaris\logs\` |
| macOS | `~/Documents/Paradox Interactive/Stellaris/logs/` |
| Linux | `~/.local/share/Paradox Interactive/Stellaris/logs/` |

**Primary log file:** `game.log` (or `error.log` for errors)

## Debugging Mod Issues via Logs

### Common Patterns

**Mod not loading:**
```
[mod] Skipping mod: Universum Infinitum: Galaxy Sizes
[mod]   Reason: supported_version mismatch (v4.5.* vs v4.6.0)
```

**Scenario parse error:**
```
[galaxy] Error parsing map/setup_scenarios/ui_titanic.txt
[galaxy]   Line 15: Unexpected token 'priority'
[galaxy]   Scenario ignored
```

**Missing localisation:**
```
[localisation] Key not found: ui_immense
[localisation] Using raw key for display
```

**Generation failure:**
```
[galaxy] Generation failed for scenario ui_gigantic
[galaxy]   Out of memory allocating star positions
[galaxy]   Try smaller galaxy size
```

## Logging Configuration (Engine)

Not configurable by mod. Engine logging controlled by:
- Stellaris launch options (`-debug`, `-logfile`)
- `user_settings.txt` log level settings
- Platform-specific logging (Steam overlay, etc.)

## Recommendations for Mod Users

### Enabling Debug Logs

Launch Stellaris with:
```bash
# Linux/macOS
./stellaris -debug -logfile debug.log

# Windows
stellaris.exe -debug -logfile debug.log
```

### Filtering Mod Logs

```bash
grep -i "universum infinitum" game.log
grep -i "ui_ludicrous" game.log
grep -i "setup_scenario" game.log
```

## Mod Author Debugging

### Validation Without Logs

Since the mod has no runtime, validation is static:

1. **Syntax:** Run validation scripts (CI)
2. **Schema:** Compare against Stellaris game files (if accessible)
3. **Generation:** Manual in-game testing
4. **Logs:** Only engine logs available post-generation

### Adding Diagnostic Information

If needed for debugging, mod author can:
- Add comments to scenario files (ignored by engine)
- Use descriptive parameter names (already done)
- Maintain parameter documentation (data-model.md)

## Summary

| Aspect | Status |
|--------|--------|
| Mod logging framework | None (not applicable) |
| Engine logging | Yes (Stellaris internal) |
| Mod-controllable logging | No |
| Structured logging | Engine-dependent |
| Correlation IDs | Not applicable |
| Log retention | Engine-dependent |
| Performance impact | None (mod has no runtime) |