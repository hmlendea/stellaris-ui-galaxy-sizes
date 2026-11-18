# Documentation Index

Complete documentation for **Universum Infinitum - Galaxy Sizes** — a declarative Stellaris mod including six galaxy sizes (Minuscule, Gigantic, Immense, Colossal, Titanic, Ludicrous).

## Repository Summary

Universum Infinitum - Galaxy Sizes extends the Stellaris galaxy generator with six `setup_scenario` definitions ranging from 100 to 2,000 stars. The mod contains no executable code: it supplies generation parameters and English display names that the engine reads at galaxy generation time.

## Root Document Map

| Document | Purpose |
|----------|---------|
| [`README.md`](../README.md) | Project overview, installation, and usage |
| [`ARCHITECTURE.md`](../ARCHITECTURE.md) | System context, components, flows, and constraints |
| [`SECURITY.md`](../SECURITY.md) | Vulnerability reporting policy |
| [`PRIVACY.md`](../PRIVACY.md) | Personal-data handling description |
| [`CONTRIBUTING.md`](../CONTRIBUTING.md) | Contribution guidelines |
| [`CODE_OF_CONDUCT.md`](../CODE_OF_CONDUCT.md) | Community standards |
| [`LICENSE`](../LICENSE) | GNU General Public License version 3 |

## Navigation

| Audience | Start Here |
|----------|------------|
| Newcomers | [`quick-start.md`](quick-start.md), [`repository-overview.md`](repository-overview.md) |
| Component owners | [`components/presentation.md`](components/presentation.md), [`components/host-and-composition.md`](components/host-and-composition.md), [`components/application-services.md`](components/application-services.md), [`components/integration-models.md`](components/integration-models.md) |
| Debuggers | [`flows/startup-and-rendering.md`](flows/startup-and-rendering.md), [`flows/detail-mutation.md`](flows/detail-mutation.md), [`behaviour/browse-and-search.md`](behaviour/browse-and-search.md) |
| Maintainers | [`change-guide.md`](change-guide.md), [`documentation-maintenance.md`](documentation-maintenance.md), [`design-decisions.md`](design-decisions.md) |

## Getting Started

| Document | Description |
|----------|-------------|
| [`quick-start.md`](quick-start.md) | 5-minute installation and first game setup |
| [`repository-overview.md`](repository-overview.md) | What the mod is, what it does, key facts |
| [`repository-structure.md`](repository-structure.md) | File-by-file layout of the repository |
| [`faq.md`](faq.md) | Frequently asked questions |

## Architecture and Design

| Document | Description |
|----------|-------------|
| [`architecture.md`](architecture.md) | System architecture, engine integration model |
| [`design-decisions.md`](design-decisions.md) | Recorded design decisions with rationale |
| [`data-model.md`](data-model.md) | Complete scenario parameter reference |
| [`invariants.md`](invariants.md) | Invariants throughout 8 categories |
| [`dependencies.md`](dependencies.md) | Engine, platform, and mod dependencies |
| [`configuration.md`](configuration.md) | Configuration files and parameters |
| [`ambiguities-and-open-questions.md`](ambiguities-and-open-questions.md) | Tracked ambiguities and unknowns |

## Components

| Document | Description |
|----------|-------------|
| [`components/presentation.md`](components/presentation.md) | UI presentation layer (engine context) |
| [`components/host-and-composition.md`](components/host-and-composition.md) | Host and composition (engine context) |
| [`components/application-services.md`](components/application-services.md) | Application services (engine context) |
| [`components/integration-models.md`](components/integration-models.md) | Integration models (engine context) |

## Flows

| Document | Description |
|----------|-------------|
| [`flows/startup-and-rendering.md`](flows/startup-and-rendering.md) | Startup and rendering flows (engine context) |
| [`flows/detail-mutation.md`](flows/detail-mutation.md) | Detail mutation flows (engine context) |
| [`flows/calendar-search.md`](flows/calendar-search.md) | Calendar and search flows (not applicable) |

## Behaviour

| Document | Description |
|----------|-------------|
| [`behaviour/browse-and-search.md`](behaviour/browse-and-search.md) | Browse and select behaviour (dropdown) |
| [`behaviour/inspect-edit-delete.md`](behaviour/inspect-edit-delete.md) | Inspect, edit, and delete behaviour (not applicable) |

## API Reference

| Document | Description |
|----------|-------------|
| [`api-reference/INDEX.md`](api-reference/INDEX.md) | API reference index (configuration schema) |
| [`api-usage-examples.md`](api-usage-examples.md) | Scenario configuration examples and tuning |

## Operations

| Document | Description |
|----------|-------------|
| [`build-and-deployment.md`](build-and-deployment.md) | Packaging, deployment targets, continuous delivery |
| [`testing.md`](testing.md) | Verification strategy, validation scripts, manual checklist |
| [`change-guide.md`](change-guide.md) | Change categories, workflow, versioning, rollback |
| [`troubleshooting.md`](troubleshooting.md) | Diagnosis table, issue resolution, log analysis |
| [`error-handling.md`](error-handling.md) | Error handling (engine context) |
| [`logging.md`](logging.md) | Logging (engine log patterns) |
| [`security.md`](security.md) | Threat model, attack surface, platform controls |
| [`state-and-persistence.md`](state-and-persistence.md) | State and persistence (engine context) |
| [`concurrency-and-scheduling.md`](concurrency-and-scheduling.md) | Concurrency (engine context) |
| [`integrations.md`](integrations.md) | Engine, platform, and mod-to-mod integrations |

## Maintenance

| Document | Description |
|----------|-------------|
| [`documentation-maintenance.md`](documentation-maintenance.md) | Documentation inventory, update triggers, style guide |

## Maintenance Metadata

| Field | Value |
|-------|-------|
| Owner | Repository maintainer |
| Last reviewed | 2026-11-18 |
| Coverage | Every tracked file and directory is documented |

## Documentation Conventions

- **"Not Applicable" pattern:** Declarative mod aspects (no runtime, no code, no build system) are marked **Not Applicable** with a reason and engine context.
- **Engine context:** Flows and behaviours possessed by the Stellaris engine are documented as context.
- **Cross-references:** Documents link to related documents rather than duplicating content.
