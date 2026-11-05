# Integrations

## Overview

This mod integrates with the Stellaris engine and several external platforms. It has no direct code-level integrations (no APIs, no SDKs, no network calls).

## Stellaris Engine Integration

### Integration Point: Galaxy Generation

**Mechanism:** Engine reads `setup_scenario` blocks from `map/setup_scenarios/` at galaxy generation time.

**Data Flow:**
```
Mod files → Engine parser → Galaxy generator → Game state
```

**Contract:**
- File format: Clausewitz script
- Location: `mod/ui-galaxy-sizes/map/setup_scenarios/ui_*.txt`
- Required block: `setup_scenario = { ... }`
- Required parameters: 15 core parameters (see data-model.md)

**Version Compatibility:**
- `supported_version="v4.5.*"` in descriptor
- Engine validates version at mod load
- Incompatible = mod hidden in launcher

### Integration Point: Localisation

**Mechanism:** Engine reads YAML localisation files for UI strings.

**Data Flow:**
```
localisation/engligh/ui-galaxy-sizes_l_english.yml → Engine localisation system → UI dropdown
```

**Contract:**
- File format: YAML with `l_english:` root
- Keys: `ui_minuscule`, `ui_gigantic`, `ui_immense`, `ui_colossal`, `ui_titanic`, `ui_ludicrous`
- Format: `key:0 "Label"`

### Integration Point: Mod Descriptor

**Mechanism:** Launcher reads `.mod` files to discover and validate mods.

**Data Flow:**
```
ui-galaxy-sizes.mod / descriptor.mod → Launcher → Playset → Engine
```

**Contract:**
- Format: Clausewitz script
- Required fields: `name`, `version`, `path`, `supported_version`, `tags`
- `path="mod/ui-galaxy-sizes"` (mount point)

## Platform Integrations

### Steam Workshop

**Integration Type:** Distribution platform

**Mechanism:** Steam Workshop hosts mod files, Steam client downloads to mod directory.

**Requirements:**
- `ui-galaxy-sizes.mod` at root of uploaded content
- `ui-galaxy-sizes/` folder with all mod files
- Thumbnail: `thumbnail.png` (256x256)
- App ID: 281990 (Stellaris)

**Update Flow:**
1. Author uploads new version via SteamCMD
2. Steam propagates to subscribers
3. Launcher detects update on next start

### GitHub Releases

**Integration Type:** Distribution + Source hosting

**Mechanism:** Git tags trigger release workflow, ZIP artefact uploaded.

**Artefact:** `ui-galaxy-sizes-v{version}.zip` containing mod files.

**Update Flow:**
1. User downloads ZIP
2. Manual install to mod directory
3. No auto-update

### Nexus Mods

**Integration Type:** Distribution platform

**Mechanism:** Web upload, user downloads via Nexus Mod Manager or manually.

**Requirements:**
- Category: Stellaris → Gameplay
- Version tracking
- Changelog in description

### Paradox Mods

**Integration Type:** Official Paradox platform

**Mechanism:** Upload via Paradox Mod Portal, review process, then available in launcher.

**Requirements:**
- Paradox account
- Mod metadata (version, supported version, tags)
- Review approval (can take days)

## Mod-to-Mod Integrations

### UI Star Names (Soft Dependency)

**Integration Type:** Content dependency

**Mechanism:** UI Star Names mod adds thousands of system names to engine's name pool. Ludicrous scenario (2000 stars) needs this pool.

**Data Flow:**
```
UI Star Names (loads first) → Extends engine name pool
Universum Infinitum (loads second) → Generates 2000 stars
Engine → Draws from combined pool → Unique names
```

**Load Order Requirement:** UI Star Names must load before or alongside this mod.

**Without UI Star Names:** Engine exhausts ~1000 vanilla names, duplicates occur.

**Contract:** None (soft dependency). Mod functions without it.

### Other Galaxy Size Mods (Conflict)

**Integration Type:** Mutual exclusion

**Mechanism:** Multiple mods defining `setup_scenario` with same names or conflicting priorities.

**Behaviour:** Undefined. Engine may:
- Use first loaded
- Merge (unlikely)
- Show duplicates in dropdown
- Crash

**Recommendation:** Use only one galaxy size mod at a time.

### UI Overhaul Mods (Compatible)

**Integration Type:** Independent coexistence

**Mechanism:** UI mods modify interface elements. This mod only adds dropdown entries.

**Compatibility:** Verified compatible with:
- UI Overhaul Dynamic
- Better UI
- Tiny UI

**Contract:** No shared files, no load order dependency.

## Launcher Integration

### Paradox Launcher v1 (Legacy)

**Mechanism:** Reads `.mod` files from `mod/` directory, expects descriptor at repo root.

**Supported:** Yes (via root `ui-galaxy-sizes.mod`)

### Paradox Launcher v2 (Current)

**Mechanism:** Reads `.mod` files from `mod/` directory, expects descriptor in mod folder.

**Supported:** Yes (via `ui-galaxy-sizes/descriptor.mod`)

### Playset Integration

**Mechanism:** User creates playset, enables mod, launcher writes to `launcher-v2.sqlite`.

**Mod Role:** Passive. Provides valid descriptor, launcher handles rest.

## Save Game Integration

### Save Compatibility

**Mechanism:** Save games reference scenario by name (`ui_ludicrous`).

**Breaking Changes:**
- Changing scenario `name` breaks saves
- Changing `num_stars` significantly breaks saves
- Removing scenario breaks saves

**Non-Breaking:**
- Adjusting `crisis_strength`, `num_nebulae` (minor)
- Adding localisation
- Fixing typos

**Version Pinning:** Descriptor `version` helps launcher detect changes.

## Multiplayer Integration

### Host-Generated Galaxy

**Mechanism:** Host generates galaxy, all clients receive generated state.

**Requirements:**
- All players have identical mod version
- All players have mod enabled
- Host selects scenario

**Verification:** Launcher shows mod version in multiplayer lobby.

## Development Tool Integrations

### Validation Scripts

**Integration Type:** Local development

**Scripts:**
- `scripts/validate-descriptors.sh`
- `scripts/validate-scenarios.sh`
- `scripts/validate-localisation.sh`
- `scripts/validate-all.sh`

**CI Integration:** GitHub Actions runs on every PR.

### Packaging Script

**Script:** `scripts/package.sh`

**Output:** Versioned ZIP for distribution.

## Integration Summary Table

| Integration | Type | Direction | Criticality |
|-------------|------|-----------|-------------|
| Stellaris Engine (generation) | Core | Engine → Mod | Critical |
| Stellaris Engine (localisation) | Core | Engine → Mod | High |
| Stellaris Engine (descriptor) | Core | Launcher → Mod | Critical |
| Steam Workshop | Distribution | Platform → User | High |
| GitHub Releases | Distribution | Repo → User | High |
| Nexus Mods | Distribution | Platform → User | Medium |
| Paradox Mods | Distribution | Platform → User | Medium |
| UI Star Names | Content | Mod → Engine | High (Ludicrous) |
| Other size mods | Conflict | Mod ↔ Mod | Avoid |
| UI mods | Independent | Mod ↔ Mod | Compatible |
| Multiplayer | Runtime | Host → Clients | High |
| Save games | Persistence | Engine → Mod | Breaking if changed |
| Validation scripts | Development | Repo → CI | High |
| Packaging script | Release | Repo → Artefact | High |

## Not Applicable: Traditional Integrations

Since this mod has no runtime code, the following are **not applicable**:

- REST API integrations
- GraphQL subscriptions
- gRPC services
- Message queues (Kafka, RabbitMQ, etc.)
- Database connections
- Cache layers (Redis, Memcached)
- Authentication providers (OAuth, OIDC, SAML)
- Payment processors
- Email/SMS providers
- File storage (S3, Azure Blob, GCS)
- CDN providers
- Monitoring/APM (Datadog, New Relic, etc.)
- Logging aggregation (ELK, Loki, etc.)
- Distributed tracing (Jaeger, Zipkin)
- Service mesh (Istio, Linkerd)
- API gateways
- Load balancers
- Circuit breakers
- Retry policies
- Feature flags
- A/B testing frameworks