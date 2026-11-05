# Build and Deployment

## Build Process

**Not Applicable** — This mod has no build process.

### Reason

The mod consists entirely of static configuration files (Clausewitz script, YAML, PNG) that are consumed directly by the Stellaris engine. There is no:
- Compilation
- Transpilation
- Bundling
- Minification
- Code generation
- Artifact creation beyond packaging

### Source = Distribution

Files in `ui-galaxy-sizes/` are the exact files the engine reads. No transformation occurs.

## Packaging

### Release Artefact

A release is a ZIP archive containing:

```
ui-galaxy-sizes.zip
├── ui-galaxy-sizes.mod          # Root descriptor
└── ui-galaxy-sizes/             # Mod directory
    ├── descriptor.mod           # Local descriptor (identical to root)
    ├── thumbnail.png            # 256x256 PNG
    ├── localisation/
    │   └── engligh/
    │       └── ui-galaxy-sizes_l_english.yml
    └── map/
        └── setup_scenarios/
            ├── ui_minuscule.txt
            ├── ui_gigantic.txt
            ├── ui_immense.txt
            ├── ui_colossal.txt
            ├── ui_titanic.txt
            └── ui_ludicrous.txt
```

### Packaging Script

```bash
# scripts/package.sh
#!/bin/bash
set -e

VERSION=$(grep 'version=' ui-galaxy-sizes.mod | sed 's/.*version="\([^"]*\)".*/\1/')
NAME="ui-galaxy-sizes-v${VERSION}"

# Create clean staging directory
rm -rf "dist/${NAME}"
mkdir -p "dist/${NAME}"

# Copy mod files
cp ui-galaxy-sizes.mod "dist/${NAME}/"
cp -r ui-galaxy-sizes "dist/${NAME}/"

# Verify structure
./scripts/validate-all.sh "dist/${NAME}"

# Create ZIP
cd dist
zip -r "${NAME}.zip" "${NAME}"
cd ..

echo "Created dist/${NAME}.zip"
```

### Validation Before Packaging

```bash
# Run all validations on staged files
./scripts/validate-all.sh dist/ui-galaxy-sizes-v1.4.5
```

## Deployment Targets

| Platform | Method | Automation |
|----------|--------|------------|
| GitHub Releases | Manual upload / GitHub Actions | Semi-automated |
| Steam Workshop | SteamCMD / Workshop Uploader | Manual |
| Nexus Mods | Web upload / Nexus API | Manual |
| Paradox Mods | Paradox Mod Portal | Manual |
| Manual Install | GitHub Releases ZIP | User downloads |

### GitHub Releases (Primary)

**Trigger:** Git tag push (`v1.4.5`)

**Workflow:** `.github/workflows/release.yml`

```yaml
name: Release
on:
  push:
    tags:
      - 'v*'
jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Package
        run: ./scripts/package.sh
      - name: Create Release
        uses: softprops/action-gh-release@v1
        with:
          files: dist/*.zip
          generate_release_notes: true
```

**Artefacts:** `ui-galaxy-sizes-v1.4.5.zip`

**Release Notes:** Auto-generated from `CHANGELOG.md`

### Steam Workshop

**Process:**
1. Install SteamCMD
2. Login: `steamcmd +login <username> +workshop_build_item <app_id> <vdf_file> +quit`
3. VDF file references `ui-galaxy-sizes/` directory
4. Update description/changelog in Workshop UI

**Frequency:** Manual per release

### Nexus Mods

**Process:**
1. Login to Nexus Mods
2. Upload `ui-galaxy-sizes-v1.4.5.zip`
3. Set version, Stellaris 4.5.x category
4. Update changelog

**Frequency:** Manual per release

### Paradox Mods

**Process:**
1. Login to Paradox Mod Portal
2. Upload mod files
3. Set metadata (version, supported version, tags)
4. Submit for review

**Frequency:** Manual per release

## Versioning

### Version Sources (Must Match)

| Source | Location | Format |
|--------|----------|--------|
| Git tag | `git tag` | `v1.4.5` |
| Descriptor | `ui-galaxy-sizes.mod` | `version="1.4.5"` |
| Local descriptor | `ui-galaxy-sizes/descriptor.mod` | `version="1.4.5"` |
| Changelog | `CHANGELOG.md` | `## [1.4.5]` |
| Release name | GitHub Release | `v1.4.5` |
| ZIP filename | `dist/` | `ui-galaxy-sizes-v1.4.5.zip` |

### Version Bump Rules

See `change-guide.md`:
- PATCH: Bug fixes, parameter tweaks
- MINOR: New sizes, shapes, localisations
- MAJOR: Breaking changes, Stellaris major version

## CI/CD Pipeline

### Validation Pipeline (Every PR)

```yaml
# .github/workflows/validate.yml
name: Validate
on: [pull_request, push]
jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Validate Descriptors
        run: ./scripts/validate-descriptors.sh
      - name: Validate Scenarios
        run: ./scripts/validate-scenarios.sh
      - name: Validate Localisation
        run: ./scripts/validate-localisation.sh
```

### Release Pipeline (Tag Push)

```yaml
# .github/workflows/release.yml
name: Release
on:
  push:
    tags:
      - 'v*'
jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Validate
        run: ./scripts/validate-all.sh
      - name: Package
        run: ./scripts/package.sh
      - name: Create GitHub Release
        uses: softprops/action-gh-release@v1
        with:
          files: dist/*.zip
```

## Environment Requirements

### Packaging Environment

- Linux/macOS/Windows (bash compatible)
- `zip` command
- Validation scripts (bash, grep, sed, awk, yq for YAML)

### Steam Workshop Environment

- Windows (SteamCMD primary)
- Steam account with mod ownership
- Stellaris App ID: 281990

### No Runtime Environment

- No Node.js, Python, .NET, Java required
- No database, no server, no container

## Rollback Procedure

### GitHub Release

```bash
# Delete release and tag
gh release delete v1.4.5 --yes
git tag -d v1.4.5
git push origin :refs/tags/v1.4.5
```

### Steam Workshop

- Revert to previous version via Workshop UI
- Or upload previous ZIP

### Nexus Mods / Paradox Mods

- Upload previous version
- Or mark current as hidden

## Deployment Checklist

### Pre-Deployment

- [ ] All validation scripts pass
- [ ] Manual in-game test (all 6 sizes, spot-check shapes)
- [ ] Version bumped in both descriptors
- [ ] CHANGELOG.md updated
- [ ] Git tag created and signed
- [ ] CHANGELOG.md matches tag version

### Deployment

- [ ] GitHub Release created with ZIP
- [ ] Steam Workshop updated
- [ ] Nexus Mods updated
- [ ] Paradox Mods updated
- [ ] Community announcement (Discord, Reddit, etc.)

### Post-Deployment

- [ ] Verify GitHub Release downloads work
- [ ] Verify Steam Workshop shows new version
- [ ] Monitor for issue reports (24-48 hours)
- [ ] Update `ROADMAP.md` if needed

## Not Applicable: Traditional Build/Deploy

Since this mod has no build process, the following are **not applicable**:

- Compilation (C#, TypeScript, Rust, Go, etc.)
- Transpilation (Babel, tsc, etc.)
- Bundling (Webpack, Vite, Rollup, etc.)
- Minification (Terser, cssnano, etc.)
- Testing frameworks (Jest, NUnit, pytest, etc.)
- Code coverage (Istanbul, coverlet, etc.)
- Static analysis (ESLint, SonarQube, etc.)
- Dependency installation (npm, nuget, cargo, etc.)
- Docker images
- Kubernetes manifests
- Helm charts
- Terraform
- Ansible
- CI runners with specific language runtimes
- Artifact repositories (npm, NuGet, Maven, PyPI, etc.)
- Blue/green deployment
- Canary releases
- Feature flags
- Database migrations
- Health checks
- Load balancers
- CDN invalidation
- SSL certificates
- Environment variables / secrets management
- Service discovery
- Circuit breakers
- Retry policies
- Observability (metrics, traces, logs)
- Auto-scaling
- Rolling updates

## Related Documents

- `change-guide.md` — Versioning and release process
- `testing.md` — Validation and testing
- `repository-structure.md` — File layout
- `architecture.md` — System context