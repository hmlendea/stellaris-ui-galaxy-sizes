# Integration Models

## Integration Model

**Not Applicable** — This mod has no integration models, DTOs, or data transfer objects.

### Reason

The mod is purely declarative configuration. It contains:
- No data models
- No serialization/deserialization
- No API contracts
- No request/response types
- No event schemas
- No message formats

All data structures are defined by the Stellaris engine's parser for Clausewitz script and YAML.

## Engine Data Models (Context)

### Scenario Model (Clausewitz)

```clausewitz
setup_scenario = {
    name = "string"
    priority = int
    num_stars = int
    radius = int
    num_empires = int
    num_empires_default = int
    num_advanced_empires = int
    num_fallen_empires = int
    num_fallen_empires_default = int
    crisis_strength = float
    num_nebulae = int
    num_systems_per_empire = int
    min_distance_between_empires = int
    guaranteed_habitable = int
    supports_shape = "string"  # repeated 11x
}
```

**Parsed By:** Stellaris engine at galaxy generation

**Validation:** Engine validates types, ranges, required fields

### Localisation Model (YAML)

```yaml
l_english:
  ui_minuscule:0 "Minuscule"
  ui_gigantic:0 "Gigantic"
  ui_immense:0 "Immense"
  ui_colossal:0 "Colossal"
  ui_titanic:0 "Titanic"
  ui_ludicrous:0 "Ludicrous"
```

**Parsed By:** Stellaris engine at UI render

**Validation:** Engine validates YAML syntax, key format

### Descriptor Model (Clausewitz)

```clausewitz
name = "Universum Infinitum: Galaxy Sizes"
version = "1.4.5"
path = "mod/ui-galaxy-sizes"
supported_version = "v4.5.*"
tags = { "Gameplay" "Map" }
```

**Parsed By:** Launcher at startup, Engine at mod load

**Validation:** Launcher validates required fields, version format

## No Mod-Defined Models

The mod does not define:
- Custom data structures
- Validation schemas
- Transformation logic
- Mapping layers
- Adapters
- Facades

## Not Applicable: Traditional Integration Patterns

Since this mod has no integration models, the following are **not applicable**:

- DTOs / ViewModels
- Entity models
- Value objects
- Aggregates
- Domain events
- Commands / Queries
- Event sourcing
- CQRS
- API contracts (OpenAPI, GraphQL, gRPC)
- Schema registry
- Contract testing
- Consumer-driven contracts
- Versioning strategies
- Backward/forward compatibility
- Data transformation pipelines
- ETL processes
- Message serialization (JSON, Protobuf, Avro, etc.)
- API gateways
- BFF (Backend for Frontend)