# Behaviours

## Browse and Select Galaxy Size

### User Story
As a player, I want to choose from six additional galaxy sizes so that I can play on smaller or larger galaxies than vanilla provides.

### Flow
1. Player opens Stellaris Launcher
2. Player activates "Universum Infinitum: Galaxy Sizes" in a playset
3. Player clicks "New Game"
4. Player opens Galaxy Size dropdown
5. Player sees 11 options (5 vanilla + 6 mod):
   - Minuscule (100 stars) — new, smallest
   - Tiny (200 stars) — vanilla
   - Small (400 stars) — vanilla
   - Medium (600 stars) — vanilla
   - Large (800 stars) — vanilla
   - Huge (1000 stars) — vanilla
   - Gigantic (1200 stars) — new
   - Immense (1400 stars) — new
   - Colossal (1600 stars) — new
   - Titanic (1800 stars) — new
   - Ludicrous (2000 stars) — new, largest
6. Player selects desired size
7. Player configures remaining settings
8. Player starts game

### Acceptance Criteria
- All six mod sizes appear in dropdown
- Names display correctly in English (localised)
- Sort order: Minuscule first, Ludicrous last
- Selection persists through game start

### Edge Cases
- Mod not activated → Sizes not shown
- Localisation missing → Raw keys shown (ui_ludicrous, etc.)
- Unsupported game version → Mod hidden in launcher

## Generate Galaxy with Selected Size

### User Story
As a player, I want the galaxy generator to create a galaxy matching my selected size with appropriate defaults so that gameplay scales correctly.

### Flow
1. Player selects galaxy size (e.g., Colossal)
2. Engine loads corresponding `setup_scenario` (ui_colossal.txt)
3. Generator applies all parameters:
   - 1600 star systems
   - Radius 480
   - 24 default AI empires (max 48)
   - 7 fallen empires (max 8)
   - 4 marauders (max 5)
   - 7 advanced empires
   - Crisis strength 2.25
   - 16 nebulae
   - Hyperlane density 0.5-5
4. Generator creates star map
5. Generator places empires using partition algorithms
6. Generator builds hyperlane network
7. Generator places nebulae, wormholes, gateways
8. Generator assigns system names
9. Galaxy displayed to player

### Acceptance Criteria
- Star count matches scenario (1600 for Colossal)
- Empire counts respect min/max and defaults
- All 11 shapes available for selection
- Generation completes without error
- Save game records scenario parameters

### Edge Cases
- Ludicrous without UI Star Names → Duplicate system names
- Very large galaxies → Longer generation time, higher memory
- Invalid parameter → Engine uses default, logs warning

## Select Galaxy Shape

### User Story
As a player, I want to choose from 11 galaxy shapes for any mod size so that I can customise the galaxy topology.

### Flow
1. Player selects galaxy size (any of the 6 mod sizes)
2. Player opens Galaxy Shape dropdown
3. Engine queries `supports_shape` for selected scenario
4. All 11 shapes shown (identical for all mod sizes):
   - Elliptical
   - Spiral (2-arm)
   - Spiral (3-arm)
   - Spiral (4-arm)
   - Spiral (6-arm)
   - Ring
   - Bar
   - Starburst
   - Cartwheel
   - Spoked
5. Player selects shape
6. Generator creates galaxy with chosen topology

### Acceptance Criteria
- All 11 shapes available for all 6 mod sizes
- Shape selection independent of size selection
- Generated galaxy matches selected topology

### Edge Cases
- Engine doesn't recognise shape → Shape filtered from UI
- Shape generation fails → Fallback to elliptical

## Configure Empire Settings

### User Story
As a player, I want to customise empire counts within scenario limits so that I can control game density.

### Flow
1. Player selects galaxy size (e.g., Gigantic)
2. Engine reads `num_empires` range (1-36) and default (18)
3. Player adjusts "AI Empires" slider
4. Slider constrained to scenario min/max
5. Player adjusts "Fallen Empires" slider (0-6, default 5)
6. Player adjusts "Marauders" slider (0-4, default 3)
7. Player adjusts "Advanced AI" slider (0-5, default 5)
8. Generator uses player values within scenario bounds

### Acceptance Criteria
- Sliders respect scenario min/max
- Defaults match scenario `*_default` values
- Player cannot exceed scenario maximums
- Changes reflected in generation

### Edge Cases
- Player sets 0 AI empires → Valid (min=1 for all scenarios)
- Player sets max fallen empires → Respected
- Mod update changes bounds → New games use new bounds

## Crisis Strength Scaling

### User Story
As a player, I want crisis difficulty to scale with galaxy size so that larger galaxies present appropriate challenges.

### Flow
1. Player selects galaxy size
2. Engine reads `crisis_strength` and `extra_crisis_strength`
3. Generator applies base multiplier to crisis fleets
4. Generator applies extra scaling at crisis milestones
5. Crisis events spawn with scaled power

### Parameter Scaling

| Size | crisis_strength | extra_crisis_strength |
|------|-----------------|----------------------|
| Minuscule | 0.25 | {10, 25} |
| Gigantic | 1.75 | {10, 25} |
| Immense | 2.0 | {10, 25} |
| Colossal | 2.25 | {10, 25} |
| Titanic | 2.5 | {10, 25} |
| Ludicrous | 2.75 | {10, 25} |

### Acceptance Criteria
- Crisis strength increases with galaxy size
- Extra crisis strength constant across sizes
- Crisis fleets appropriately challenging for galaxy scale

### Edge Cases
- Minuscule crisis very weak (0.25x)
- Ludicrous crisis very strong (2.75x)
- Player can further adjust via game settings

## Nebula Density Scaling

### User Story
As a player, I want nebula count to scale with galaxy size so that exploration feels consistent across sizes.

### Flow
1. Player selects galaxy size
2. Engine reads `num_nebulas`, `nebula_size`, `nebula_min_dist`
3. Generator places nebulae across galaxy
4. Nebulae provide strategic terrain

### Parameter Scaling

| Size | num_nebulas | nebula_size | nebula_min_dist |
|------|-------------|-------------|-----------------|
| Minuscule | 1 | 60 | 50 |
| Gigantic | 12 | 60 | 200 |
| Immense | 14 | 60 | 200 |
| Colossal | 16 | 60 | 200 |
| Titanic | 18 | 60 | 200 |
| Ludicrous | 20 | 60 | 200 |

### Acceptance Criteria
- Nebula count scales with star count
- Nebula size constant (60)
- Minimum distance larger for bigger galaxies
- Nebulae distributed across galaxy

## Hyperlane Density Configuration

### User Story
As a player, I want hyperlane connectivity to scale with galaxy size so that travel time feels appropriate.

### Flow
1. Player selects galaxy size
2. Engine reads `num_hyperlanes` range and default
3. Player can adjust "Hyperlane Density" slider
4. Generator builds network within distance limit (50)
5. Partition algorithms applied (breadth_first for home, depth_first for open)

### Parameter Scaling

| Size | num_hyperlanes.min | num_hyperlanes.max | num_hyperlanes_default |
|------|-------------------|-------------------|----------------------|
| Minuscule | 0.5 | 3 | 1 |
| Gigantic | 0.5 | 5 | 1 |
| Immense | 0.5 | 5 | 1 |
| Colossal | 0.5 | 5 | 1 |
| Titanic | 0.5 | 5 | 1 |
| Ludicrous | 0.5 | 5 | 1 |

### Acceptance Criteria
- Minuscule has lower max density (3 vs 5)
- Default density same (1) for all sizes
- Max hyperlane distance constant (50)
- Network connectivity maintained

## Wormhole and Gateway Placement

### User Story
As a player, I want wormholes and gateways to provide strategic shortcuts scaled to galaxy size.

### Flow
1. Player selects galaxy size
2. Engine reads wormhole/gateway ranges (0-5, default 1 for all sizes)
3. Player can adjust counts
4. Generator places pairs across galaxy
5. Wormholes connect distant regions
6. Gateways enable late-game fast travel

### Acceptance Criteria
- Identical ranges across all sizes (0-5)
- Default 1 pair each
- Placement respects galaxy topology
- Strategic value maintained at all scales

## Cluster Configuration

### User Story
As a player, I want empire clusters to create interesting regional politics at all galaxy sizes.

### Flow
1. Player selects galaxy size
2. Engine reads `cluster_count`, `cluster_radius`, `cluster_distance_from_core`
3. Generator groups empires into clusters
4. Clusters create natural alliance/conflict zones

### Parameter Scaling

| Size | cluster_count.max | cluster_radius | cluster_distance_from_core |
|------|------------------|----------------|---------------------------|
| Minuscule | 2 | 70 | 70 |
| Gigantic | 6 | 160 | 300 |
| Immense | 6 | 170 | 300 |
| Colossal | 6 | 180 | 300 |
| Titanic | 6 | 190 | 300 |
| Ludicrous | 6 | 200 | 300 |

### Acceptance Criteria
- Minuscule limited to 2 clusters (small galaxy)
- Larger galaxies support up to 6 clusters
- Cluster radius scales with galaxy radius
- Core distance constant for large galaxies (300)
- Cluster method: one_every_x_empire (value=1)