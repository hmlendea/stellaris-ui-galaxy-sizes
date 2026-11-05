# Startup and Rendering Flow

## Flow Overview

**Not Applicable** — This mod has no startup or rendering flow.

### Reason

The mod is purely declarative configuration. It contains:
- No startup sequence
- No initialization code
- No rendering pipeline
- No UI components
- No client-side code

All flows occur within the Stellaris engine and launcher.

## Engine Flows (Context)

### Launcher Startup Flow

```
1. Launcher starts
2. Scans mod directories for .mod files
3. Parses each descriptor
4. Validates supported_version
5. Builds mod list for UI
6. User selects playset
7. Launcher writes enabled mods to launcher-v2.sqlite
8. User clicks Play
9. Launcher starts Stellaris with mod mount points
```

**Mod Role:** Provides valid `.mod` files at expected paths.

### Engine Mod Load Flow

```
1. Stellaris starts
2. Reads mount points from launcher
3. For each mod:
   a. Reads descriptor.mod from mod folder
   b. Validates version, path
   c. Mounts mod directory at path
4. Mods available for game systems
```

**Mod Role:** Provides valid `descriptor.mod` in `ui-galaxy-sizes/`.

### Galaxy Generation Flow

```
1. Player clicks New Game
2. Player selects Galaxy Size (scenario)
3. Player selects Galaxy Shape
4. Player adjusts settings (empires, crisis, etc.)
5. Player clicks Generate
6. Engine reads selected setup_scenario
7. Engine validates parameters
8. Engine generates galaxy:
   a. Places stars (num_stars, radius, shape)
   b. Builds hyperlanes
   c. Places empires (num_empires, distances)
   d. Places fallen empires
   e. Places nebulae (num_nebulae)
   f. Generates crisis fleets (crisis_strength)
   g. Assigns system names
9. Game starts with generated galaxy
```

**Mod Role:** Provides `setup_scenario` parameters in `map/setup_scenarios/ui_*.txt`.

### UI Rendering Flow (Dropdown)

```
1. Player opens Galaxy Size dropdown
2. Engine queries loaded scenarios
3. For each scenario:
   a. Reads name from setup_scenario
   b. Looks up localisation key
   c. Displays localised label (or raw key)
4. Player selects scenario
```

**Mod Role:** Provides localisation keys in `localisation/engligh/ui-galaxy-sizes_l_english.yml`.

## Mod's Passive Role

The mod participates in flows **passively**:
- Files exist at known paths
- Engine reads files when needed
- No callbacks, no events, no hooks
- No mod code executes

## Not Applicable: Traditional Startup/Rendering

Since this mod has no startup or rendering flow, the following are **not applicable**:

- Application bootstrap
- Dependency injection container initialization
- Configuration loading
- Plugin loading
- Middleware pipeline
- Routing initialization
- Rendering engine initialization
- Virtual DOM mounting
- Component hydration
- State hydration
- Route guards
- Authentication initialization
- Theme loading
- Locale loading
- Asset preloading
- Service worker registration
- Background sync
- Push notification setup