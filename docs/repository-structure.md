# Repository Structure

## Purpose

This document records the file-by-file layout of the repository, the role of every directory, and the rules that govern where content belongs. It complements [`repository-overview.md`](repository-overview.md), which describes the mod's purpose and scope.

## Repository Tree

```text
stellaris-ui-galaxy-sizes/
├── ui-galaxy-sizes.mod              # Root mod descriptor (Steam Workshop, GitHub, Nexus Mods)
├── ui-galaxy-sizes/                 # Mod content directory mounted by the launcher
│   ├── descriptor.mod               # Local mod descriptor (Paradox Mods, manual install)
│   ├── thumbnail.png                # Launcher thumbnail (256x256 PNG)
│   ├── localisation/
│   │   └── engligh/                 # Locale directory (historic spelling retained)
│   │       └── ui-galaxy-sizes_l_english.yml
│   └── map/
│       └── setup_scenarios/
│           ├── ui_minuscule.txt
│           ├── ui_gigantic.txt
│           ├── ui_immense.txt
│           ├── ui_colossal.txt
│           ├── ui_titanic.txt
│           └── ui_ludicrous.txt
├── .github/
│   ├── FUNDING.yml                  # Donation link resolution source
│   ├── cover.png                    # Repository social preview image
│   └── workflows/
│       ├── github-release.yml
│       ├── nexus-mods-release.yml
│       └── steam-workshop-release.yml
├── docs/                            # Repository documentation library
├── ARCHITECTURE.md
├── CODE_OF_CONDUCT.md
├── CONTRIBUTING.md
├── LICENSE
├── PRIVACY.md
├── README.md
└── SECURITY.md
```

## Mod Content

| Path | Role |
|------|------|
| `ui-galaxy-sizes.mod` | Declares mod identity for Steam Workshop, GitHub Releases, and Nexus Mods. |
| `ui-galaxy-sizes/descriptor.mod` | Declares an identical identity for Paradox Mods and manual installation. |
| `ui-galaxy-sizes/thumbnail.png` | Thumbnail displayed by the launcher. |
| `ui-galaxy-sizes/localisation/engligh/ui-galaxy-sizes_l_english.yml` | English labels for the six scenario keys. |
| `ui-galaxy-sizes/map/setup_scenarios/ui_*.txt` | One `setup_scenario` definition per supplied galaxy size. |

## Documentation Library

| Path | Role |
|------|------|
| `docs/INDEX.md` | Master index and navigation aid. |
| `docs/repository-overview.md` | Purpose, scope, capabilities, and invariants. |
| `docs/repository-structure.md` | This file: file-by-file layout. |
| `docs/architecture.md` | Implementation-level view complementing the root `ARCHITECTURE.md`. |
| `docs/data-model.md` | Complete scenario parameter reference. |
| `docs/configuration.md` | Configuration files, parameters, and validation rules. |
| `docs/dependencies.md` | Engine, platform, and mod dependencies. |
| `docs/integrations.md` | Engine, platform, and mod-to-mod integration points. |
| `docs/invariants.md` | System invariants and their verification. |
| `docs/design-decisions.md` | Recorded design decisions with rationale. |
| `docs/change-guide.md` | Change categories, workflow, and versioning. |
| `docs/build-and-deployment.md` | Packaging, deployment targets, and release automation. |
| `docs/testing.md` | Verification strategy and manual checklists. |
| `docs/troubleshooting.md` | Diagnostic procedures and resolutions. |
| `docs/faq.md` | Frequently asked questions. |
| `docs/quick-start.md` | Short installation and first-game guide. |
| `docs/api-usage-examples.md` | Scenario configuration examples and tuning guidance. |
| `docs/api-reference/INDEX.md` | Configuration-schema reference index. |
| `docs/ambiguities-and-open-questions.md` | Tracked unknowns requiring verification. |
| `docs/documentation-maintenance.md` | Documentation inventory and update triggers. |
| `docs/state-and-persistence.md` | State and persistence description. |
| `docs/logging.md` | Engine log reference relevant to the mod. |
| `docs/error-handling.md` | Engine error conduct relevant to the mod. |
| `docs/concurrency-and-scheduling.md` | Concurrency and scheduling description. |
| `docs/security.md` | Implementation-level security analysis complementing the root `SECURITY.md`. |
| `docs/behaviour/browse-and-search.md` | Galaxy size and shape selection behaviour. |
| `docs/behaviour/inspect-edit-delete.md` | Inspect, edit, and delete behaviour. |
| `docs/components/presentation.md` | Presentation-layer responsibilities. |
| `docs/components/host-and-composition.md` | Host and composition responsibilities. |
| `docs/components/application-services.md` | Application-service responsibilities. |
| `docs/components/integration-models.md` | Integration-model description. |
| `docs/flows/startup-and-rendering.md` | Startup and rendering flows. |
| `docs/flows/detail-mutation.md` | Galaxy generation detail flow. |
| `docs/flows/calendar-search.md` | Calendar and search flow (not applicable). |

## Placement Rules

- Mod content belongs beneath `ui-galaxy-sizes/` and nowhere else.
- Each galaxy size requires exactly one file in `ui-galaxy-sizes/map/setup_scenarios/`.
- `ROADMAP.md`, `PRIVACY.md`, `SECURITY.md`, and `LICENSE` belong at the repository root, never beneath `docs/`.
- Documentation beneath `docs/` complements the root documents and must not duplicate them.
- Workflow files belong beneath `.github/workflows/`.

## Naming Conventions

- Documentation files use kebab-case (`state-and-persistence.md`).
- Documentation directories use plural kebab-case (`components/`, `flows/`, `behaviour/`).
- Scenario files are named `ui_<size>.txt` and must correspond to their `name` field.
- Localisation files are named `<mod>_l_<locale>.yml`.

## Related Documents

- [`repository-overview.md`](repository-overview.md) — Purpose, scope, and capabilities.
- [`architecture.md`](architecture.md) — Implementation-level architecture.
- [`documentation-maintenance.md`](documentation-maintenance.md) — Update triggers and inventory.
