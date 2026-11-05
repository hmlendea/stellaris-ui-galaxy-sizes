# Change Guide

## Purpose

This guide defines the process for making changes to the mod, from development through release. It ensures consistency, traceability, and quality.

## Change Categories

| Category | Description | Version Bump | Review Required |
|----------|-------------|--------------|-----------------|
| Patch | Bug fixes, parameter tweaks, validation updates | PATCH (1.4.5 → 1.4.6) | Yes |
| Minor | New galaxy size, new shape support, new localisation | MINOR (1.4.5 → 1.5.0) | Yes |
| Major | Breaking descriptor changes, Stellaris version upgrade | MAJOR (1.4.5 → 2.0.0) | Yes + migration doc |

## Development Workflow

### 1. Create Branch

```bash
git checkout -b change/<category>-<description>
# Examples:
# change/patch-fix-ludicrous-priority
# change/minor-add-quantum-size
# change/major-stellaris-4-6-support
```

### 2. Make Changes

Edit files in `ui-galaxy-sizes/` directory:

| File Type | Location | Notes |
|-----------|----------|-------|
| Descriptors | `ui-galaxy-sizes.mod`, `ui-galaxy-sizes/descriptor.mod` | Keep identical |
| Scenarios | `ui-galaxy-sizes/map/setup_scenarios/ui_*.txt` | 6 files |
| Localisation | `ui-galaxy-sizes/localisation/engligh/ui-galaxy-sizes_l_english.yml` | English only |
| Thumbnail | `ui-galaxy-sizes/thumbnail.png` | 256x256 PNG |

### 3. Validate Locally

```bash
# Run all validation scripts
./scripts/validate-all.sh

# Or individually
./scripts/validate-descriptors.sh
./scripts/validate-scenarios.sh
./scripts/validate-localisation.sh
```

### 4. Test In-Game

1. Copy mod to Stellaris mod directory
2. Launch Stellaris (matching `supported_version`)
3. Activate mod
4. Test each changed scenario:
   - Select from dropdown
   - Verify localisation
   - Generate galaxy (at least one shape)
   - Verify star count approximately correct
   - Save/load game

### 5. Update Documentation

| Change Type | Docs to Update |
|-------------|----------------|
| Parameter change | `data-model.md`, `invariants.md` |
| New scenario | `data-model.md`, `repository-structure.md`, `invariants.md`, `testing.md` |
| New shape | `data-model.md`, `invariants.md`, `testing.md` |
| Version bump | `README.md`, `CHANGELOG.md`, `architecture.md` |
| Stellaris version | `architecture.md`, `configuration.md`, `dependencies.md` |
| New validation | `testing.md`, `invariants.md` |

### 6. Update Changelog

Edit `CHANGELOG.md` (create if missing):

```markdown
## [1.4.6] - 2025-01-15

### Fixed
- Ludicrous scenario priority corrected from 10 to 9
- Validation script now checks priority ordering

### Changed
- Updated supported_version to v4.5.2
```

Follow [Keep a Changelog](https://keepachangelog.com/) format.

### 7. Commit and Push

```bash
git add -A
git commit -m "<category>: <description>

<details if needed>

Fixes #<issue-number>"
git push origin change/<category>-<description>
```

### 8. Create Pull Request

- Target: `main`
- Title: `<category>: <description>`
- Description: Link issue, summarise changes, note testing done
- CI must pass

### 9. Review and Merge

- At least one review (can be self for patch)
- CI green
- Merge via squash or rebase

### 10. Release

```bash
# Tag release (signed)
git tag -s v1.4.6 -m "Release v1.4.6"
git push origin v1.4.6
```

CI/CD publishes to:
- GitHub Releases (automatic)
- Steam Workshop (manual or scripted)
- Nexus Mods (manual)
- Paradox Mods (manual)

## Versioning Rules

### Semantic Versioning

`MAJOR.MINOR.PATCH`

- **MAJOR**: Breaking changes (descriptor format, Stellaris major version)
- **MINOR**: New features (sizes, shapes, localisations) — backward compatible
- **PATCH**: Bug fixes, parameter adjustments, validation — backward compatible

### Version in Descriptors

Both `.mod` files must have identical `version` field matching Git tag (without `v` prefix):

```clausewitz
version="1.4.6"
```

### Supported Version

Update `supported_version` when:
- New Stellaris patch tested and working
- New Stellaris minor version supported (MAJOR bump)

Format: `v4.5.*` (wildcard for patches)

## Parameter Change Guidelines

### Safe Changes (PATCH)

| Parameter | Safe Range | Notes |
|-----------|------------|-------|
| `num_stars` | ±10% | Test generation time |
| `radius` | ±10% | Keep proportional to stars |
| `num_empires` | ±2 | Keep within engine limits |
| `crisis_strength` | ±0.25 | Test balance |
| `num_nebulae` | ±2 | Visual only |
| `priority` | Reorder only | Must stay unique |

### Unsafe Changes (Require Testing)

| Parameter | Risk |
|-----------|------|
| `num_stars` > 2000 | Memory, performance |
| `radius` > 500 | Engine limits |
| `num_empires` > 60 | Engine hard limit |
| New `supports_shape` | Engine may not support |
| Removing `supports_shape` | Breaks existing saves |

### New Scenario Checklist

- [ ] Unique name `ui_<name>`
- [ ] Unique priority (integer)
- [ ] All 15 required parameters
- [ ] All 11 `supports_shape` entries
- [ ] Localisation key added
- [ ] Parameter scaling consistent with existing
- [ ] Generation tested on all 11 shapes
- [ ] Documentation updated

## Stellaris Version Upgrade

### Minor Version (4.5 → 4.6)

1. Test mod on new version
2. Update `supported_version` to `v4.6.*`
3. Update `architecture.md` Stellaris version
4. Update `dependencies.md` Stellaris version
5. Update `configuration.md` if new parameters
6. MINOR version bump (1.4.5 → 1.5.0)
7. Document any parameter changes needed

### Major Version (4.x → 5.x)

1. Full re-test all scenarios
2. Check for new/removed parameters
3. Check for new/removed shapes
4. Update `supported_version` to `v5.0.*`
5. MAJOR version bump (1.4.5 → 2.0.0)
6. Create migration guide if saves affected
7. Update all documentation

## Validation Script Changes

When adding validation rules:

1. Add check to appropriate script
2. Document new invariant in `invariants.md`
3. Test against current mod (must pass)
4. Test against known-bad mod (must fail)
5. Update `testing.md` validation section

## Documentation Change Rules

### Required Updates Per Change

| Change | Required Doc Updates |
|--------|---------------------|
| Any parameter | `data-model.md` parameter table |
| New scenario | `repository-structure.md`, `data-model.md`, `invariants.md`, `testing.md` |
| New shape | `data-model.md`, `invariants.md`, `testing.md` |
| Version bump | `README.md`, `CHANGELOG.md`, `architecture.md` |
| Stellaris version | `architecture.md`, `configuration.md`, `dependencies.md` |
| Validation rule | `testing.md`, `invariants.md` |
| New dependency | `dependencies.md`, `architecture.md` |

### Documentation Style

- Follow existing markdown structure
- Use tables for parameter references
- Keep examples current
- Update cross-references

## Rollback Procedure

If release has critical issue:

```bash
# 1. Revert tag
git tag -d v1.4.6
git push origin :refs/tags/v1.4.6

# 2. Revert commit on main
git revert <merge-commit>
git push origin main

# 3. Fix issue on new branch
# 4. Release patch version (1.4.7)
```

## Emergency Hotfix

For critical issues affecting players:

1. Create `hotfix/<description>` branch from `main`
2. Make minimal fix
3. Validate and test
4. PATCH version bump
5. Fast-track PR (single review, CI only)
6. Release immediately

## Change History Template

For each change, record:

```markdown
### Change: <title>
- **Type**: patch|minor|major
- **Version**: 1.4.5 → 1.4.6
- **Files**: list of modified files
- **Validation**: scripts run, in-game test results
- **Documentation**: files updated
- **Release**: tag, platforms published
- **Rollback plan**: if applicable
```

## Checklist Summary

### Pre-Commit
- [ ] Validation scripts pass
- [ ] In-game test (affected scenarios)
- [ ] Documentation updated
- [ ] Changelog updated

### Pre-Merge
- [ ] CI passes
- [ ] Review approved
- [ ] No merge conflicts

### Pre-Release
- [ ] Version bumped in descriptors
- [ ] Changelog finalised
- [ ] Git tag signed
- [ ] Platform publishing ready

### Post-Release
- [ ] Verify GitHub Release created
- [ ] Publish to Steam Workshop
- [ ] Publish to Nexus Mods
- [ ] Publish to Paradox Mods
- [ ] Announce (Discord, Reddit, etc.)