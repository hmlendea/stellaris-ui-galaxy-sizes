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
ui-galaxy-sizes_<version>.zip
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

### Packaging

The release archive is built by GitHub Actions and contains no staging directory:

```bash
zip -r "ui-galaxy-sizes_${VERSION}.zip" "ui-galaxy-sizes/" "ui-galaxy-sizes.mod"
```

Before packaging, both descriptors are updated from the release tag:

```bash
MOD_VERSION="${VERSION#v}"

for DESCRIPTOR in "ui-galaxy-sizes.mod" "ui-galaxy-sizes/descriptor.mod"
do
  if grep -q '^version=' "${DESCRIPTOR}"; then
    sed -i "s/^version=.*/version=\"${MOD_VERSION}\"/" "${DESCRIPTOR}"
  else
    printf 'version="%s"\n' "${MOD_VERSION}" | cat - "${DESCRIPTOR}" > "${DESCRIPTOR}.tmp"
    mv "${DESCRIPTOR}.tmp" "${DESCRIPTOR}"
  fi
done
```

### Manual Verification Before Release

Inspect the archive structure and confirm the descriptors carry the release version:

```bash
unzip -l "ui-galaxy-sizes_${VERSION}.zip"
grep '^version=' ui-galaxy-sizes.mod ui-galaxy-sizes/descriptor.mod
```

## Deployment Targets

| Platform | Method | Automation |
|----------|--------|------------|
| GitHub Releases | GitHub Actions | Automated |
| Steam Workshop | GitHub Actions | Automated |
| Nexus Mods | GitHub Actions | Automated |
| Paradox Mods | Paradox Mod Portal | Manual |
| Manual Install | GitHub Releases ZIP | User downloads |

### GitHub Releases (Primary)

**Trigger:** GitHub release publication

**Workflow:** `.github/workflows/github-release.yml`

**Process:**
1. Check out the released tag
2. Set `version=` in `ui-galaxy-sizes.mod` and `ui-galaxy-sizes/descriptor.mod` from the tag, with the leading `v` stripped
3. Build `ui-galaxy-sizes_<version>.zip` containing `ui-galaxy-sizes/` and `ui-galaxy-sizes.mod`
4. Attach the archive to the published GitHub release

**Artefacts:** `ui-galaxy-sizes_1.4.5.zip`

**Release Notes:** As written by the maintainer on the GitHub release

### Steam Workshop

**Trigger:** Tag push matching `v*.*.*`, or manual dispatch with a `tag` input

**Workflow:** `.github/workflows/steam-workshop-release.yml`

**Process:**
1. Check out the requested tag
2. Set `version=` in both descriptors from the tag, with the leading `v` stripped
3. Build and extract the release archive
4. Upload through `hmlendea/steam-workshop-update` with a changelog link to the GitHub release

### Nexus Mods

**Trigger:** Tag push matching `v*.*.*`, or manual dispatch with a `tag` input

**Workflow:** `.github/workflows/nexus-mods-release.yml`

**Process:**
1. Check out the requested tag
2. Set `version=` in both descriptors from the tag, with the leading `v` stripped
3. Build the release archive
4. Upload through `Nexus-Mods/upload-action`, updating the mod version and archiving the existing version

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
| ZIP filename | GitHub Release asset | `ui-galaxy-sizes_1.4.5.zip` |

### Version Bump Rules

See `change-guide.md`:
- PATCH: Bug fixes, parameter tweaks
- MINOR: New sizes, shapes, localisations
- MAJOR: Breaking changes, Stellaris major version

## CI/CD Pipeline

### Validation (Local)

Before tagging, verify the descriptors, scenario files, and localisation by inspection:

```bash
grep '^version=' ui-galaxy-sizes.mod ui-galaxy-sizes/descriptor.mod
ls ui-galaxy-sizes/map/setup_scenarios/
```

There is no automated validation workflow; the repository ships the release workflows only.

### Release Pipeline (GitHub Release Publication)

```yaml
# .github/workflows/github-release.yml
name: GitHub Release
on:
  release:
    types: [ published ]
permissions:
  contents: write
jobs:
  release:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7
      - name: Update the mod version
        run: |
          # Set version= in both descriptors from the release tag
      - name: Build the release archive
        run: |
          zip -r "ui-galaxy-sizes_${VERSION}.zip" "ui-galaxy-sizes/" "ui-galaxy-sizes.mod"
      - uses: softprops/action-gh-release@v2
        with:
          files: ui-galaxy-sizes_${{ steps.version.outputs.version }}.zip
```

The release archive is consumed by `.github/workflows/steam-workshop-release.yml` and `.github/workflows/nexus-mods-release.yml`, which additionally build their own archives from the checked-out tag.

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