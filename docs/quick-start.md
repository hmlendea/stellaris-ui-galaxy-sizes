# Quick Start

## 5-Minute Setup

### 1. Install the Mod

**Steam Workshop (Recommended):**
1. Open Steam → Stellaris Workshop
2. Search for "Universum Infinitum - Galaxy Sizes"
3. Click **Subscribe**
4. Launch Stellaris → Playsets → Activate the mod

**Manual (GitHub Releases):**
1. Download the latest `ui-galaxy-sizes.zip` from [GitHub Releases](https://github.com/hmlendea/stellaris-ui-galaxy-sizes/releases)
2. Extract it to the Stellaris mod directory:
   - **Windows:** `%USERPROFILE%\Documents\Paradox Interactive\Stellaris\mod\`
   - **macOS:** `~/Documents/Paradox Interactive/Stellaris/mod/`
   - **Linux:** `~/.local/share/Paradox Interactive/Stellaris/mod/`
3. Launch Stellaris → Playsets → Activate the mod

### 2. Start a New Game

1. **New Game** → **Galaxy Size** dropdown
2. Select one of the six novel sizes:
   - **Minuscule** (100 stars) — Rapid tests
   - **Gigantic** (1,200 stars) — Large
   - **Immense** (1,400 stars) — Very large
   - **Colossal** (1,600 stars) — Huge
   - **Titanic** (1,800 stars) — Massive
   - **Ludicrous** (2,000 stars) — Maximum
3. Select the **Galaxy Shape** (11 options)
4. Adjust the **Empire Settings** (defaults scale with size)
5. **Generate Galaxy** → Play

### 3. Ludicrous Size Note

**Requires the [UI Star Names](https://github.com/hmlendea/stellaris-ui-star-names) mod** for unique system names. Without it, duplicate names occur (the engine contains approximately 1000 vanilla names).

## Size Selection Guide

| Size | Stars | Time | Memory | Best For |
|------|-------|------|-----|----------|
| Minuscule | 100 | <5s | 4GB | Testing, rapid games |
| Gigantic | 1,200 | 10-20s | 8GB | Standard large |
| Immense | 1,400 | 15-30s | 12GB | Long campaigns |
| Colossal | 1,600 | 20-40s | 16GB | Epic scale |
| Titanic | 1,800 | 30-60s | 16-24GB | Maximum vanilla-like |
| Ludicrous | 2,000 | 60-120s | 24-32GB | Absolute maximum |

## Frequent Issues

| Problem | Resolution |
|---------|----------|
| Mod absent from the launcher | Verify that the files are in the correct mod directory, then restart the launcher |
| "Mod incompatible" | Update Stellaris to 4.5.x |
| Generation crashes | Try a smaller size, close other applications, and check the available memory |
| Duplicate names (Ludicrous) | Install the UI Star Names mod |
| Sizes display as `ui_gigantic` | Reinstall the mod and verify the localisation file |

## Uninstallation

1. Deactivate the mod in Playsets
2. Delete `ui-galaxy-sizes.mod` and `ui-galaxy-sizes/` from the mod directory
3. Or unsubscribe in the Steam Workshop

## Links

- **GitHub:** [Repository](https://github.com/hmlendea/stellaris-ui-galaxy-sizes)
- **Steam Workshop:** [Mod Page](https://steamcommunity.com/sharedfiles/filedetails/?id=2047543230)
- **Issues:** [GitHub Issues](https://github.com/hmlendea/stellaris-ui-galaxy-sizes/issues)
- **Full Documentation:** [INDEX.md](INDEX.md)
