# Host and Composition

## Component Model

**Not Applicable** — This mod has no component architecture, host, or composition model.

### Reason

The mod is purely declarative configuration files consumed by the Stellaris engine. It contains:
- No components
- No host application
- No composition root
- No dependency injection
- No plugin system
- No module system

## Stellaris Engine as Host (Context)

The Stellaris engine acts as the host that consumes this mod's configuration:

### Engine Responsibilities

| Responsibility | Mod Involvement |
|----------------|-----------------|
| Mod discovery | Reads `.mod` descriptors |
| Mod validation | Validates `supported_version` |
| Mod loading | Mounts `mod/ui-galaxy-sizes` |
| Scenario parsing | Reads `setup_scenario` blocks |
| Galaxy generation | Uses scenario parameters |
| UI rendering | Reads localisation for dropdown |

### Mod Responsibilities

| Responsibility | Implementation |
|----------------|----------------|
| Provide valid descriptors | `ui-galaxy-sizes.mod`, `descriptor.mod` |
| Provide valid scenarios | 6 files in `map/setup_scenarios/` |
| Provide localisation | `localisation/engligh/ui-galaxy-sizes_l_english.yml` |
| Provide thumbnail | `thumbnail.png` |

## Composition

**No Composition** — The mod does not compose other mods, nor is it composed by other mods (except soft dependency on UI Star Names).

### Soft Dependency: UI Star Names

```
┌─────────────────┐     ┌──────────────────────┐
│ UI Star Names   │────▶│ Stellaris Engine     │
│ (loads first)   │     │ (name pool extended) │
└─────────────────┘     └──────────┬───────────┘
                                   │
                                   ▼
                          ┌──────────────────────┐
                          │ Universum Infinitum  │
                          │ (generates 2000 stars)│
                          └──────────────────────┘
```

**Composition Type:** Sequential load order, shared engine resource (name pool)

**No Direct Interaction:** Mods don't reference each other.

## Not Applicable: Traditional Component Patterns

Since this mod has no component architecture, the following are **not applicable**:

- Component lifecycle (mount, update, unmount)
- Component composition (parent/child, slots, props)
- Host environment (browser, Node.js, Electron, etc.)
- Dependency injection containers
- Service locators
- Plugin architectures
- Module federation
- Micro-frontends
- Web Components
- React/Vue/Svelte/Angular components
- Dependency graphs
- Circular dependency detection
- Lazy loading / code splitting
- Tree shaking
- Hot module replacement
- Component testing
- Storybook / component documentation