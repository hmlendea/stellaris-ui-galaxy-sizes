# Integrations

## Overview

This mod integrates with the Stellaris engine and several external platforms. It has no direct code-level integrations (no APIs, no SDKs, no network calls).

## Stellaris Engine Integration

### Integration Point: Galaxy Generation

**Mechanism:** The engine reads `setup_scenario` blocks from `map/setup_scenarios/` at galaxy generation time.

**Data Flow:**
```
Mod files → Engine parser → Galaxy generator → Game state
```

**Contract:**
- File format: Clausewitz script
- Location: `mod/ui-galaxy-sizes/map/setup_scenarios/ui_*.txt`
- Required block: `setup_scenario = { ... }`
- Required parameters: the 20 core parameters (see data-model.md)

**Version Compatibility:**
- `supported_version="v4.5.*"` in the descriptor
- The engine validates the version at mod load
- Incompatible = mod hidden in the launcher

### Integration Point: Localisation

**Mechanism:** The engine reads YAML localisation files for UI strings.

**Data Flow:**
```
localisation/engligh/ui-galaxy-sizes_l_english.yml → Engine localisation system → UI dropdown
```

**Contract:**
- File format: YAML with an `l_english:` root
- Keys: `ui_minuscule`, `ui_gigantic`, `ui_immense`, `ui_colossal`, `ui_titanic`, `ui_ludicrous`
- Format: `key:0 "Label"`

### Integration Point: Mod Descriptor

**Mechanism:** The launcher reads `.mod` files to discover and validate mods.

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

**Mechanism:** The Steam Workshop hosts the mod files; the Steam client downloads them to the mod directory.

**Requirements:**
- `ui-galaxy-sizes.mod` at the root of the uploaded content
- The `ui-galaxy-sizes/` directory containing every mod file
- Thumbnail: `thumbnail.png` (256x256)
- App ID: 281990 (Stellaris)

**Update Flow:**
1. The author uploads the new version through SteamCMD
2. Steam propagates it to subscribers
3. The launcher detects the update at the next start

### GitHub Releases

**Integration Type:** Distribution + Source hosting

**Mechanism:** Publishing a GitHub release initiates `.github/workflows/github-release.yml`, which updates the descriptor versions and uploads the archive asset. Tag pushes additionally initiate `.github/workflows/steam-workshop-release.yml` and `.github/workflows/nexus-mods-release.yml`.

**Artefact:** `ui-galaxy-sizes_{version}.zip` containing the mod descriptor and mod directory.

**Update Flow:**
1. The user downloads the archive
2. Manual installation to the mod directory
3. No automatic update

### Nexus Mods

**Integration Type:** Distribution platform

**Mechanism:** Web upload, user downloads via Nexus Mod Manager or manually.

**Requirements:**
- Category: Stellaris → Gameplay
- Version tracking
- Changelog in description

### Paradox Mods

**Integration Type:** Official Paradox platform

**Mechanism:** Upload through the Paradox Mod Portal, then a review process, after which the mod is available in the launcher.

**Requirements:**
- A Paradox account
- Mod metadata (version, supported version, tags)
- Review approval (may require several days)

## Mod-to-Mod Integrations

### UI Star Names (Soft Dependency)

**Integration Type:** Content dependency

**Mechanism:** The UI Star Names mod includes thousands of system names in the engine name pool. The Ludicrous scenario (2000 stars) requires this pool.

**Data Flow:**
```
UI Star Names (loads first) → Extends the engine name pool
Universum Infinitum (loads second) → Generates 2000 stars
Engine → Draws from the combined pool → Unique names
```

**Load Order Requirement:** UI Star Names must load before or alongside this mod.

**Without UI Star Names:** The engine exhausts the ~1000 vanilla names, and duplicates occur.

**Contract:** None (soft dependency). The mod functions without it.

### Other Galaxy Size Mods (Conflict)

**Integration Type:** Mutual exclusion

**Mechanism:** Multiple mods defining `setup_scenario` with identical names or conflicting priorities.

**Conduct:** Undefined. The engine may:
- Use the first loaded
- Merge (improbable)
- Display duplicates in the dropdown
- Terminate

**Recommendation:** Use only one galaxy size mod at a time.

### UI Overhaul Mods (Compatible)

**Integration Type:** Independent coexistence

**Mechanism:** UI mods modify interface elements. This mod only includes dropdown records.

**Compatibility:** Verified compatible with:
- UI Overhaul Dynamic
- Better UI
- Tiny UI

**Contract:** No shared files and no load order dependency.

## Launcher Integration

### Paradox Launcher v1 (Legacy)

**Mechanism:** Reads `.mod` files from `mod/` directory, expects descriptor at repo root.

**Supported:** Yes (via root `ui-galaxy-sizes.mod`)

### Paradox Launcher v2 (Current)

**Mechanism:** Reads `.mod` files from the `mod/` directory and expects the descriptor in the mod directory.

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

**Version Pinning:** The descriptor `version` assists the launcher in detecting changes.

## Multiplayer Integration

### Host-Generated Galaxy

**Mechanism:** Host generates galaxy, all clients receive generated state.

**Requirements:**
- All players have identical mod version
- Every player has the mod active
- Host selects scenario

**Verification:** The launcher displays the mod version in the multiplayer lobby.

## Development Tool Integrations

### Release Workflows

**Integration Type:** CI/CD

**Workflows:**
- `.github/workflows/github-release.yml`
- `.github/workflows/steam-workshop-release.yml`
- `.github/workflows/nexus-mods-release.yml`

**Trigger:** GitHub release publication (`github-release.yml`), or a `v*.*.*` tag push and manual dispatch (`steam-workshop-release.yml`, `nexus-mods-release.yml`).

**Output:** Versioned archive for distribution.

## Integration Summary Table

| Integration | Type | Direction | Criticality |
|-------------|------|-----------|-------------|
| Stellaris Engine (generation) | Central | Engine → Mod | Critical |
| Stellaris Engine (localisation) | Central | Engine → Mod | High |
| Stellaris Engine (descriptor) | Central | Launcher → Mod | Critical |
| Steam Workshop | Distribution | Platform → User | High |
| GitHub Releases | Distribution | Repository → User | High |
| Nexus Mods | Distribution | Platform → User | Medium |
| Paradox Mods | Distribution | Platform → User | Medium |
| UI Star Names | Content | Mod → Engine | High (Ludicrous) |
| Other size mods | Conflict | Mod ↔ Mod | Avoid |
| UI mods | Independent | Mod ↔ Mod | Compatible |
| Multiplayer | Runtime | Host → Clients | High |
| Save games | Persistence | Engine → Mod | Breaking if changed |
| Validation scripts | Development | Repository → Continuous integration | High |
| Packaging script | Release | Repository → Artefact | High |

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
