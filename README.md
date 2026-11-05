[![Donate](https://img.shields.io/badge/-%E2%99%A5%20Donate-%23ff69b4)](https://hmlendea.go.ro/funding)
[![Latest Release](https://img.shields.io/github/v/release/hmlendea/stellaris-ui-galaxy-sizes)](https://github.com/hmlendea/stellaris-ui-galaxy-sizes/releases/latest)
[![License](https://img.shields.io/github/license/hmlendea/stellaris-ui-galaxy-sizes)](https://github.com/hmlendea/stellaris-ui-galaxy-sizes/blob/master/LICENSE)

# Universum Infinitum: Galaxy Sizes

A Stellaris mod that adds six galaxy sizes ranging from 100 to 2,000 stars, with scaled generation defaults for each scenario.

## 📑 Table of Contents

- [Table of Contents](#-table-of-contents)
- [Capabilities](#-capabilities)
- [Usage](#-usage)
- [Known Limitations](#-known-limitations)
- [Installation](#-installation)
	- [Manual Installation](#manual-installation)
- [Localisation](#-localisation)
- [Compatibility](#-compatibility)
- [Integrations](#-integrations)
- [Privacy and Data](#-privacy-and-data)
- [Architecture](#-architecture)
- [Contributing](#-contributing)
- [Code of Conduct](#-code-of-conduct)
- [Security](#-security)
- [Related Projects](#-related-projects)
- [GitHub Actions](#-github-actions)
- [Project Structure](#-project-structure)
- [Documentation](#-documentation)
- [Versioning](#-versioning)
- [FAQ](#-faq)
- [Troubleshooting](#-troubleshooting)
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
- Generate every additional size with elliptical, spiral, ring, bar, starburst, cartwheel, or spoked galaxy shapes.
- Use size-specific defaults for AI empires, fallen empires, marauders, advanced empires, nebulae, and crisis strength.

## 🚀 Usage

After installing and activating the mod in the Stellaris launcher:
1. Start a **New Game**.
2. Select one of the six additional options from the **Galaxy Size** selector.
3. Configure the remaining game settings and start the campaign.

## ⚠️ Known Limitations

- The 2,000-star Ludicrous scenario requires [Universum Infinitum: Star Names](https://github.com/hmlendea/stellaris-ui-star-names) to provide sufficient system names.
- [Paradox Mods](https://mods.paradoxplaza.com/mods/25431/Any) currently provides mod version 1.3.0 for Stellaris 3.x. Use Steam Workshop, Nexus Mods, or GitHub for version 1.4.5 with Stellaris 4.5.x.

## 📦 Installation

[![Obtain it from Steam Workshop](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/steam-workshop.png)](https://steamcommunity.com/sharedfiles/filedetails/?id=2047543230)
[![Obtain it from Nexus Mods](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/nexus.png)](https://nexusmods.com/stellaris/mods/76)
[![Obtain it from Paradox Mods](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/paradox-mods.png)](https://mods.paradoxplaza.com/mods/25431/Any)
[![Obtain it from GitHub](https://raw.githubusercontent.com/hmlendea/readme-assets/master/badges/stores/github.png)](https://github.com/hmlendea/stellaris-ui-galaxy-sizes/releases)

Install the mod through one of the listed platforms, add it to a Stellaris playset, and activate it before starting the game.

### Manual Installation

1. Download the archive from the [latest GitHub release](https://github.com/hmlendea/stellaris-ui-galaxy-sizes/releases/latest).
2. Extract `ui-galaxy-sizes.mod` and the `ui-galaxy-sizes/` directory into the Stellaris user mod directory.
3. Add **Universum Infinitum: Galaxy Sizes** to a playset in the Stellaris launcher and activate it.
4. Install [Universum Infinitum: Star Names](https://github.com/hmlendea/stellaris-ui-star-names) when using the Ludicrous galaxy size.

## 🌍 Localisation

English labels are included for all six additional galaxy sizes.

| Language | Locale | Status |
|----------|--------|--------|
| English | `l_english` | All scenario names included |

## 🧩 Compatibility

| Component | Supported Versions | Notes |
|-----------|--------------------|-------|
| Stellaris | `4.5.*` | Declared by both mod descriptors for release 1.4.5 |

## 🔌 Integrations

| Integration | Compatibility | Purpose | Required |
|-------------|---------------|---------|----------|
| [Universum Infinitum: Star Names](https://github.com/hmlendea/stellaris-ui-star-names) | Ludicrous scenario | Provides sufficient names for a 2,000-star galaxy | Only for Ludicrous |

## 🛡️ Privacy and Data

Universum Infinitum: Galaxy Sizes does not request, collect, transmit, or store personal data; its files define galaxy-generation scenarios for Stellaris.

For the detailed description of how the application handles privacy and personal data, see [PRIVACY.md](./PRIVACY.md).

The mod does not implement accounts, telemetry, diagnostics, or built-in transfers of personal data.

## 🤝 Contributing

You are welcome to submit any suggestion, feedback, or modification to this project.

When doing so, please:
- Maintain cross-platform compatibility
- Submit focused pull requests that conform to the existing code style
- Maintain your branch synchronised with `master`
- Revise the documentation when functionality changes
- Raise a new [issue](https://github.com/hmlendea/stellaris-ui-galaxy-sizes/issues) for problems or suggestions

See the [contributing guidelines](./CONTRIBUTING.md) for details on how to report issues, suggest enhancements, and submit changes.

## 📜 Code of Conduct

See the [Code of Conduct](./CODE_OF_CONDUCT.md) for the standards of conduct expected in all project spaces.

## 🔒 Security

Universum Infinitum: Galaxy Sizes defines galaxy-generation scenarios for Stellaris and does not implement accounts, telemetry, diagnostics, or built-in transfers of personal data.

For information on reporting security vulnerabilities, see [SECURITY.md](./SECURITY.md).

## 🏗️ Architecture

The mod is a declarative, data-driven Stellaris extension. It supplies six `setup_scenario` definitions consumed by the Stellaris galaxy generator. See the [architecture documentation](./ARCHITECTURE.md) for the system context, component catalogue, data flows, interfaces, deployment model, and compatibility contracts.

## 🔗 Related Projects

- [Universum Infinitum: Flags](https://github.com/hmlendea/stellaris-ui-flags): Companion Stellaris mod from the Universum Infinitum collection.
- [Universum Infinitum: Name Lists](https://github.com/hmlendea/stellaris-ui-name-lists): Companion Stellaris mod from the Universum Infinitum collection.
- [Universum Infinitum: Prescripted Countries](https://github.com/hmlendea/stellaris-ui-prescripted-countries): Companion Stellaris mod from the Universum Infinitum collection.
- [Universum Infinitum: Species Names](https://github.com/hmlendea/stellaris-ui-species-names): Companion Stellaris mod from the Universum Infinitum collection.
- [Universum Infinitum: Star Names](https://github.com/hmlendea/stellaris-ui-star-names): Companion mod required by the Ludicrous galaxy size.

## ⚙️ GitHub Actions

| Workflow | Purpose | What it does |
|----------|---------|--------------|
| [nexus-mods.yml](https://github.com/hmlendea/stellaris-ui-galaxy-sizes/blob/master/.github/workflows/nexus-mods.yml) | Nexus Mods deployment | Packages and publishes releases to Nexus Mods |
| [steam-workshop.yml](https://github.com/hmlendea/stellaris-ui-galaxy-sizes/blob/master/.github/workflows/steam-workshop.yml) | Steam Workshop deployment | Packages and publishes releases to Steam Workshop |

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
├── docs/                        # Comprehensive documentation (33 files)
├── .github/
│   ├── workflows/               # CI/CD pipelines
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

## 📚 Documentation

| Resource | Description |
|----------|-------------|
| [Documentation Index](docs/INDEX.md) | Master index of all 33 documentation files |
| [Architecture](ARCHITECTURE.md) | System context, components, data flows, interfaces |
| [Quick Start](docs/quick-start.md) | 5-minute installation and first game setup |
| [FAQ](docs/faq.md) | Frequently asked questions |
| [Troubleshooting](docs/troubleshooting.md) | Diagnosis table, issue resolution, log analysis |
| [Testing](docs/testing.md) | Test strategy, validation scripts, manual checklist |
| [Change Guide](docs/change-guide.md) | Change categories, workflow, versioning, rollback |
| [Build & Deployment](docs/build-and-deployment.md) | Packaging, deployment targets, CI/CD |

## 🏷️ Versioning

This project follows [Semantic Versioning](https://semver.org/) for mod releases:
- **Major**: Breaking changes to scenario parameters or Stellaris version support
- **Minor**: New galaxy sizes, new localisation, parameter rebalancing
- **Patch**: Bug fixes, typo corrections, metadata updates

The `supported_version` in descriptors declares Stellaris compatibility (currently `v4.5.*`).

## ❓ FAQ

See the [Frequently Asked Questions](docs/faq.md) for responses to frequently raised inquiries.

## 🩺 Troubleshooting

See the [Troubleshooting Guide](docs/troubleshooting.md) for diagnostic procedures and resolutions to recurring problems.

## 💬 Support

Use the [Steam Workshop](https://steamcommunity.com/sharedfiles/filedetails/?id=2047543230) or [Nexus Mods](https://www.nexusmods.com/stellaris/mods/76) pages for general support and discussion. Submit reproducible problems and project suggestions through [GitHub Issues](https://github.com/hmlendea/stellaris-ui-galaxy-sizes/issues).

## 💝 Project Engagement

Discovered a problem or have a suggestion? [Open an issue](https://github.com/hmlendea/stellaris-ui-galaxy-sizes/issues)!

If you find this project useful, consider [funding it](https://hmlendea.go.ro/funding) or starring ⭐️ it on GitHub!

[![Donate](https://raw.githubusercontent.com/hmlendea/readme-assets/master/donate_generic.png)](https://hmlendea.go.ro/funding)

## 📄 License

This project is being distributed under the `GNU General Public License version 3`.
See [LICENSE](./LICENSE) for further information.
