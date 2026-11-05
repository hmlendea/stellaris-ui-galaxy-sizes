# Flows

## Galaxy Generation Detail Flow

### Parameter Application Sequence

```mermaid
sequenceDiagram
    participant Generator
    participant StarMap
    participant EmpirePlacer
    participant HyperlaneBuilder
    participant NebulaPlacer
    participant NameAssigner
    Generator->>StarMap: Create 2000 star positions (radius 500)
    Generator->>EmpirePlacer: Place 30 AI empires (max 60)
    Generator->>EmpirePlacer: Place 9 fallen empires (max 10)
    Generator->>EmpirePlacer: Place 5 marauders (max 6)
    Generator->>EmpirePlacer: Place 9 advanced empires
    Generator->>StarMap: Apply cluster_count (max 6, value 1)
    Generator->>StarMap: Apply cluster_radius 200
    Generator->>StarMap: Apply cluster_distance_from_core 300
    Generator->>HyperlaneBuilder: Build network (max_dist 50, density 0.5-5)
    Generator->>NebulaPlacer: Place 20 nebulae (size 60, min_dist 200)
    Generator->>StarMap: Add wormhole pairs (0-5, default 1)
    Generator->>StarMap: Add gateways (0-5, default 1)
    Generator->>NameAssigner: Assign names from pool
    Generator-->>Player: Complete galaxy
```

### Empire Placement Detail

```mermaid
flowchart TD
    A[Generator starts empire placement] --> B[Read num_empires range]
    B --> C[Read num_empire_default]
    C --> D{Player customised count?}
    D -->|Yes| E[Use player value within range]
    D -->|No| F[Use num_empire_default]
    E --> G[Place empires using home_system_partitions]
    F --> G
    G --> H[Apply breadth_first partition method]
    H --> I[Ensure min_bridges 2, max_bridges 4]
    I --> J[Place fallen empires (fallen_empire_default)]
    J --> K[Place marauders (marauder_empire_default)]
    K --> L[Place advanced empires (advanced_empire_default)]
    L --> M[Validate all empires have valid home systems]
```

### Hyperlane Network Generation

```mermaid
flowchart TD
    A[HyperlaneBuilder starts] --> B[Read max_hyperlane_distance=50]
    B --> C[Read num_hyperlanes range 0.5-5]
    C --> D{Player customised density?}
    D -->|Yes| E[Use player value]
    D -->|No| F[Use num_hyperlanes_default=1]
    E --> G[Generate connections within distance limit]
    F --> G
    G --> H[Apply partition algorithms]
    H --> I[home_system_partitions: breadth_first]
    H --> J[open_space_partitions: depth_first]
    I --> K[Ensure connectivity]
    J --> K
    K --> L[Validate no isolated systems]
```

### Crisis Strength Application

```mermaid
flowchart TD
    A[Generator applies crisis settings] --> B[Read crisis_strength=2.75]
    B --> C[Read extra_crisis_strength={10,25}]
    C --> D[Apply base multiplier to crisis fleets]
    D --> E[Apply extra scaling at crisis milestones]
    E --> F[Scale crisis spawn rates and fleet power]
```

## Shape Selection Flow

### Player Shape Choice

```mermaid
sequenceDiagram
    participant Player
    participant UI
    participant Engine
    Player->>UI: Opens Galaxy Shape dropdown
    UI->>Engine: Queries supports_shape for selected scenario
    Engine-->>UI: Returns [elliptical, spiral_2, spiral_3, spiral_4, spiral_6, ring, bar, starburst, cartwheel, spoked]
    UI->>Player: Shows 11 shape options
    Player->>UI: Selects "spiral_4"
    UI->>Engine: Passes shape=spiral_4
    Engine->>Generator: Generates 4-arm spiral galaxy
```

**Shape Support Invariants:**
- All six scenarios declare identical `supports_shape` list
- Engine filters UI to only show supported shapes
- No scenario restricts shapes differently

## Save Game Flow

### Galaxy Persistence

```mermaid
sequenceDiagram
    participant Generator
    participant SaveSystem
    participant SaveFile
    Generator->>SaveSystem: Serialises generated galaxy
    SaveSystem->>SaveFile: Writes star systems, empires, hyperlanes, nebulae
    SaveSystem->>SaveFile: Writes scenario parameters used
    SaveSystem->>SaveFile: Writes mod version (1.4.5)
    SaveFile-->>Player: Save game created
```

**Save Game Contents:**
- Complete galaxy state (stars, empires, fleets, etc.)
- Scenario parameters that generated it
- Mod version for compatibility checking
- No mod files embedded (mod must be active to load)

### Save Game Loading

```mermaid
sequenceDiagram
    participant Player
    participant Launcher
    participant Engine
    participant SaveFile
    Player->>Launcher: Activates mod v1.4.5
    Player->>Engine: Loads save game
    Engine->>SaveFile: Reads mod version
    Engine->>Engine: Validates mod active and version matches
    alt Mod active, version matches
        Engine->>Engine: Loads galaxy state
        Engine-->>Player: Resumes game
    else Mod missing or version mismatch
        Engine-->>Player: Error: missing/incompatible mod
    end
```

**Compatibility Rules:**
- Save requires exact mod version that created it
- New mod version cannot load old saves (parameters may differ)
- Vanilla saves unaffected by mod presence