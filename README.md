[![Donate](https://img.shields.io/badge/-%E2%99%A5%20Donate-%23ff69b4)](https://hmlendea.go.ro/funding)
[![Latest Release](https://img.shields.io/github/v/release/hmlendea/stellaris-ui-galaxy-sizes)](https://github.com/hmlendea/stellaris-ui-galaxy-sizes/releases/latest)
[![License](https://img.shields.io/github/license/hmlendea/stellaris-ui-galaxy-sizes)](https://github.com/hmlendea/stellaris-ui-galaxy-sizes/blob/master/LICENSE)

# Universum Infinitum - Galaxy Sizes

A Stellaris mod that supplies six galaxy sizes from 100 to 2,000 stars, with generation defaults scaled for every scenario.

## 📑 Table of Contents

- [Capabilities](#-capabilities)
- [Usage](#-usage)
- [Known Limitations](#-known-limitations)
- [Installation](#-installation)
  - [Manual Installation](#manual-installation)
- [Localisation](#-localisation)
- [Compatibility](#-compatibility)
- [Integrations](#-integrations)
- [Privacy and Data](#-privacy-and-data)
- [GitHub Actions](#-github-actions)
- [Project Structure](#-project-structure)
  - [Directories](#directories)
- [Architecture](#-architecture)
- [Documentation](#-documentation)
- [Versioning](#-versioning)
- [Troubleshooting](#-troubleshooting)
- [FAQ](#-faq)
- [Contributing](#-contributing)
- [Code of Conduct](#-code-of-conduct)
- [Related Projects](#-related-projects)
- [Security](#-security)
- [Support](#-support)
- [Project Engagement](#-project-engagement)
- [License](#-license)

## ✨ Capabilities

- Select Ludicrous galaxies with 2,000 stars.
- Select Titanic galaxies with 1,800 stars.
- Select Colossal galaxies with 1,600 stars.
- Select Immense galaxies with 1,400 stars.
- Select Gigantic galaxies with 1,200 stars.
- Select Minuscule galaxies with 100 stars.
- Generate every supplied size with elliptical, ring, bar, starburst, cartwheel, spoked, and 2-, 3-, 4-, or 6-arm spiral galaxy shapes.
- Use size-specific defaults for AI empires, fallen empires, marauders, nomads, advanced empires, nebulae, clusters, hyperlanes, and crisis strength.

## 🚀 Usage

After installing and activating the mod in the Stellaris launcher:
1. Start a **New Game**.
2. Select one of the six supplied galaxy sizes in the **Galaxy Size** selector.
3. Configure the remaining game settings and start the campaign.

## ⚠️ Known Limitations

- The 2,000-star Ludicrous scenario requires [Universum Infinitum - Star Names](https://github.com/hmlendea/stellaris-ui-star-names) to supply sufficient system names.
- Vanilla scripts branch on the five built-in galaxy size names, so the systems that scale by galaxy size — AI habitat caps, cosmic storms, voidworms, starlit links, the Central Crystal, and Eater-of-Worlds thresholds — fall back to their default values on the supplied sizes.
- [Paradox Mods](https://mods.paradoxplaza.com/mods/25431/Any) distributes version 1.3.0 for Stellaris 3.x; version 1.5.0 with Stellaris 4.5.x support is available via Steam Workshop, Nexus Mods, and GitHub.

## 📦 Installation

[![Obtain it from Steam Workshop](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/steam-workshop.png)](https://steamcommunity.com/sharedfiles/filedetails/?id=2047543230)
[![Obtain it from Nexus Mods](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/nexus.png)](https://nexusmods.com/stellaris/mods/76)
[![Obtain it from Paradox Mods](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/paradox-mods.png)](https://mods.paradoxplaza.com/mods/25431/Any)
[![Obtain it from GitHub](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/github.png)](https://github.com/hmlendea/stellaris-ui-galaxy-sizes/releases)

Install the mod via one of the listed platforms, include it in a Stellaris playset, and activate it prior to starting the game.

### Manual Installation

1. Download the archive from the [latest GitHub release](https://github.com/hmlendea/stellaris-ui-galaxy-sizes/releases/latest).
2. Extract `ui-galaxy-sizes.mod` and the `ui-galaxy-sizes/` directory into the Stellaris user mod directory.
3. Include **Universum Infinitum - Galaxy Sizes** in a playset in the Stellaris launcher and activate it.
4. Install [Universum Infinitum - Star Names](https://github.com/hmlendea/stellaris-ui-star-names) when using the Ludicrous galaxy size.

## 🌍 Localisation

English labels are supplied for every galaxy size the mod includes.

| Language | Locale | Status |
|----------|--------|--------|
| English | `l_english` | All scenario names included |

## 🧩 Compatibility

| Component | Supported Versions | Notes |
|-----------|--------------------|-------|
| Stellaris | `4.5.*` | Declared by both mod descriptors for release 1.5.0 |

## 🔌 Integrations

| Integration | Compatibility | Purpose | Required |
|-------------|---------------|---------|----------|
| [Universum Infinitum - Star Names](https://github.com/hmlendea/stellaris-ui-star-names) | Ludicrous scenario | Provides sufficient names for a 2,000-star galaxy | Only for Ludicrous |

## 🛡️ Privacy and Data

Universum Infinitum - Galaxy Sizes does not request, collect, transmit, or store personal data; its files define galaxy-generation scenarios for Stellaris.

For the detailed description of how the application handles privacy and personal data, see [PRIVACY.md](./PRIVACY.md).

The mod does not implement accounts, telemetry, diagnostics, or built-in transfers of personal data.

## ⚙️ GitHub Actions

| Workflow | Purpose | What it does |
|----------|---------|--------------|
| [github-release.yml](https://github.com/hmlendea/stellaris-ui-galaxy-sizes/blob/master/.github/workflows/github-release.yml) | GitHub Release deployment | Updates the mod version in both descriptors, packages the release archive, and attaches it to the published GitHub release |
| [nexus-mods-release.yml](https://github.com/hmlendea/stellaris-ui-galaxy-sizes/blob/master/.github/workflows/nexus-mods-release.yml) | Nexus Mods deployment | Updates the mod version in both descriptors, packages the release archive, and publishes it to Nexus Mods |
| [steam-workshop-release.yml](https://github.com/hmlendea/stellaris-ui-galaxy-sizes/blob/master/.github/workflows/steam-workshop-release.yml) | Steam Workshop deployment | Updates the mod version in both descriptors, packages the release archive, and publishes it to Steam Workshop |

## 🗂️ Project Structure

```
├── ui-galaxy-sizes.mod          # Root descriptor (Steam/GitHub/Nexus)
├── ui-galaxy-sizes/             # Mod content directory
│   ├── descriptor.mod           # Local descriptor (Paradox Mods)
│   ├── localisation/
│   │   └── engligh/
│   │       └── ui-galaxy-sizes_l_english.yml
│   └── map/
│       └── setup_scenarios/
│           ├── ui_minuscule.txt
│           ├── ui_gigantic.txt
│           ├── ui_immense.txt
│           ├── ui_colossal.txt
│           ├── ui_titanic.txt
│           └── ui_ludicrous.txt
├── docs/                        # Comprehensive documentation
├── .github/
│   ├── workflows/
│   ├── FUNDING.yml
│   └── cover.png
├── ARCHITECTURE.md
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
├── PRIVACY.md
├── SECURITY.md
├── LICENSE
└── README.md
```

### Directories

| Directory | Purpose |
|-----------|---------|
| `ui-galaxy-sizes/` | Mod content mounted by the Stellaris launcher at `mod/ui-galaxy-sizes` |
| `ui-galaxy-sizes/map/setup_scenarios/` | One `setup_scenario` definition per supplied galaxy size |
| `ui-galaxy-sizes/localisation/engligh/` | English display names for the scenario keys |
| `docs/` | Repository documentation library |
| `.github/workflows/` | Release deployment workflows |

## 🏗️ Architecture

Universum Infinitum - Galaxy Sizes is a declarative, data-driven Stellaris extension that supplies six `setup_scenario` definitions for the Stellaris galaxy generator.

See the [Architecture Documentation](./ARCHITECTURE.md) for the system context, principal components, runtime flows, ownership boundaries, dependencies, constraints, and extension points.

## 📚 Documentation

| Resource | Description |
|----------|-------------|
| [Documentation Index](docs/INDEX.md) | Master index of the documentation library |
| [Architecture](ARCHITECTURE.md) | System context, components, data flows, interfaces |
| [Quick Start](docs/quick-start.md) | Brief installation and first game setup guide |
| [FAQ](docs/faq.md) | Frequently asked questions |
| [Troubleshooting](docs/troubleshooting.md) | Diagnosis table, issue resolution, log analysis |
| [Testing](docs/testing.md) | Verification strategy, validation scripts, manual checklist |
| [Change Guide](docs/change-guide.md) | Change categories, workflow, versioning, rollback |
| [Build and Deployment](docs/build-and-deployment.md) | Packaging, deployment targets, release automation |

## 🏷️ Versioning

This project follows [Semantic Versioning](https://semver.org/) for mod releases:
- **Major**: Breaking changes to scenario parameters or Stellaris version support
- **Minor**: Additional galaxy sizes, additional localisation, parameter rebalancing
- **Patch**: Defect corrections, typographical corrections, metadata updates

The `supported_version` field in both descriptors declares the supported Stellaris release (currently `v4.5.*`).

## 🩺 Troubleshooting

See the [Troubleshooting Guide](docs/troubleshooting.md) for diagnostic procedures and resolutions to recurring problems.

## ❓ FAQ

See the [Frequently Asked Questions](docs/faq.md) for responses to frequently raised inquiries.

## 🤝 Contributing

You are welcome to submit any suggestion, feedback, or modification to this project.

When doing so, please:
- Maintain cross-platform compatibility
- Submit focused pull requests that conform to the existing code style
- Maintain your branch synchronised with `master`
- Raise a new [issue](https://github.com/hmlendea/stellaris-ui-galaxy-sizes/issues) for problems or suggestions

See the [Contributing Guidelines](./CONTRIBUTING.md) for details on how to report issues, suggest enhancements, and submit changes.

## 📜 Code of Conduct

See the [Code of Conduct](./CODE_OF_CONDUCT.md) for the standards of conduct expected in all project spaces.

## 🔗 Related Projects

- [Universum Infinitum - Character Traits](https://github.com/hmlendea/stellaris-ui-character-traits): Includes character traits
- [Universum Infinitum - Deposit Blockers](https://github.com/hmlendea/stellaris-ui-deposit-blockers): Includes seven planet deposit blockers and one corresponding society technology to clear each of them.
- [Universum Infinitum - Flags](https://github.com/hmlendea/stellaris-ui-flags): Includes hundreds of additional flag designs corresponding to the vanilla flag style.
- [Universum Infinitum - Name Lists](https://github.com/hmlendea/stellaris-ui-name-lists): Provides numerous detailed name lists, including the Human - Extended variant.
- [Universum Infinitum - Planet Modifiers](https://github.com/hmlendea/stellaris-ui-planet-modifiers): Includes four additional planet modifiers with their own icons and English localisation.
- [Universum Infinitum - Prescripted Countries](https://github.com/hmlendea/stellaris-ui-prescripted-countries): Includes prescripted countries for players to select or for the AI to use.
- [Universum Infinitum - Species Names](https://github.com/hmlendea/stellaris-ui-species-names): Includes hundreds of additional species names for the game.
- [Universum Infinitum - Species Traits](https://github.com/hmlendea/stellaris-ui-species-traits): Includes 58 custom species traits and extends several trait families with additional tiers.
- [Universum Infinitum - Star Names](https://github.com/hmlendea/stellaris-ui-star-names): Includes numerous additional names for stars, nebulae, asteroids, and black holes.

## 🔒 Security

Universum Infinitum - Galaxy Sizes defines galaxy-generation scenarios for Stellaris and does not implement accounts, telemetry, diagnostics, or built-in transfers of personal data.

For information on reporting security vulnerabilities, see the [Security Policy](./SECURITY.md).

## 💬 Support

For support, please [open an issue](https://github.com/hmlendea/stellaris-ui-galaxy-sizes/issues).

## 💝 Project Engagement

Discovered a problem with Universum Infinitum - Galaxy Sizes or have a suggestion for it? [Open an issue](https://github.com/hmlendea/stellaris-ui-galaxy-sizes/issues)!

If you find this project useful, consider [funding it](https://hmlendea.go.ro/funding) or starring ⭐️ it on GitHub, or giving it a thumbs up and adding it as a favourite on the [Steam Workshop](https://steamcommunity.com/sharedfiles/filedetails/?id=2047543230)!

[![Donate](https://raw.githubusercontent.com/hmlendea/readme-assets/master/donate_generic.png)](https://hmlendea.go.ro/funding)

## 📄 License

This project is being distributed under the `GNU General Public License version 3`.
See [LICENSE](./LICENSE) for additional information.
