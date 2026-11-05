# Documentation Maintenance

## Purpose

This document defines processes for keeping documentation accurate, current, and aligned with the mod. It complements the root `CONTRIBUTING.md` documentation section.

## Documentation Inventory

### Root Documents (Repository Root)

| File | Purpose | Update Trigger |
|------|---------|----------------|
| `README.md` | Project overview, install, usage | Every release |
| `LICENSE` | GNU GPL v3 | Never (unless licence change) |
| `CHANGELOG.md` | Version history | Every release |
| `CONTRIBUTING.md` | Contribution guidelines | Process changes |
| `SECURITY.md` | Vulnerability reporting | Policy changes |
| `PRIVACY.md` | Data handling | Data practice changes |
| `CODE_OF_CONDUCT.md` | Community standards | Community changes |
| `ROADMAP.md` | Future plans | Planning updates |
| `ARCHITECTURE.md` | System architecture | Architecture changes |

### Documentation Directory (`docs/`)

| File | Purpose | Update Trigger |
|------|---------|----------------|
| `repository-overview.md` | High-level repo description | Structural changes |
| `repository-structure.md` | File tree with descriptions | File add/remove/move |
| `architecture.md` | Technical architecture | Architecture changes |
| `data-model.md` | Parameter reference | Parameter changes |
| `configuration.md` | Configuration guide | Config changes |
| `dependencies.md` | Dependency inventory | Dependency changes |
| `testing.md` | Test strategy | Test changes |
| `security.md` | Security analysis | Security changes |
| `logging.md` | Logging reference | Logging changes |
| `error-handling.md` | Error handling reference | Error handling changes |
| `invariants.md` | System invariants | Invariant changes |
| `change-guide.md` | Change process | Process changes |
| `documentation-maintenance.md` | This file | Process changes |
| `faq.md` | Frequently asked questions | New common questions |
| `troubleshooting.md` | Problem resolution | New issues/solutions |
| `ambiguities-and-open-questions.md` | Unresolved items | New ambiguities |
| `quick-start.md` | 5-minute start guide | Install/usage changes |
| `api-usage-examples.md` | Usage examples | New use cases |
| `design-decisions.md` | Architectural decisions | New decisions |
| `integrations.md` | Integration points | New integrations |
| `state-and-persistence.md` | State management | State changes |
| `concurrency-and-scheduling.md` | Concurrency model | Concurrency changes |
| `components/presentation.md` | UI components | UI changes |
| `components/host-and-composition.md` | Host/composition | Host changes |
| `components/application-services.md` | App services | Service changes |
| `components/integration-models.md` | Integration models | Model changes |
| `flows/startup-and-rendering.md` | Startup flow | Flow changes |
| `flows/detail-mutation.md` | Detail mutation flow | Flow changes |
| `flows/calendar-search.md` | Calendar search flow | Flow changes |
| `behaviour/browse-and-search.md` | Browse/search behaviour | Behaviour changes |
| `behaviour/inspect-edit-delete.md` | Inspect/edit/delete | Behaviour changes |
| `api-reference/INDEX.md` | API reference index | API changes |
| `INDEX.md` | Master documentation index | Any doc change |

## Maintenance Principles

### Single Source of Truth

- Mod files (`ui-galaxy-sizes/`) are the source of truth
- Documentation reflects mod files, not vice versa
- Validation scripts enforce consistency

### Version Alignment

- Documentation version = mod version (in `CHANGELOG.md`)
- `README.md` version badge matches latest release
- `ARCHITECTURE.md` `supported_version` matches descriptors

### Completeness Over Perfection

- Document "not applicable" explicitly for declarative mod
- Stub sections preferred over missing sections
- Cross-reference instead of duplicating

## Update Workflow

### Per-Change Updates

When making a mod change (per `change-guide.md`):

1. Identify affected documentation (see change-guide.md table)
2. Update each affected file
3. Verify cross-references still valid
4. Run documentation validation (if available)

### Per-Release Updates

At each release:

1. Update `CHANGELOG.md` with release notes
2. Update `README.md` version badge and any changed instructions
3. Update `ARCHITECTURE.md` if Stellaris version changed
4. Verify `INDEX.md` links all current docs
5. Check for broken internal links

### Periodic Reviews

| Frequency | Scope | Owner |
|-----------|-------|-------|
| Per release | Changed files only | Release author |
| Quarterly | Full inventory | Maintainer |
| Major version | Complete audit | Maintainer |

## Validation

### Automated Checks (CI)

```bash
# Link validation
./scripts/validate-doc-links.sh

# Structure validation
./scripts/validate-doc-structure.sh

# Cross-reference validation
./scripts/validate-doc-xrefs.sh
```

**Link Validation:**
- All internal links resolve
- No broken relative paths
- Repository-relative links preferred

**Structure Validation:**
- All inventory files exist
- Required sections present per template
- Markdown syntax valid

**Cross-Reference Validation:**
- Parameter names match `data-model.md`
- File paths match `repository-structure.md`
- Version numbers consistent

### Manual Checks

- Read-through for clarity
- Verify examples work
- Check "not applicable" sections still accurate
- Confirm external links accessible

## Documentation Standards

### Formatting

- Markdown (CommonMark + GitHub Flavored)
- Tables for parameter references
- Code blocks for Clausewitz examples
- Relative links for internal references

### Templates

Each doc type follows a template (see `github/template-fidelity` instructions):

| Doc Type | Template Source |
|----------|-----------------|
| README | `github.readme.template` |
| ARCHITECTURE | `github.architecture.template` |
| CONTRIBUTING | `github.contributing.template` |
| SECURITY | `github.security.template` |
| PRIVACY | `github.privacy.template` |
| CODE_OF_CONDUCT | `github.code-of-conduct.template` |
| ROADMAP | `github.roadmap.template` |
| Others | Ad-hoc, follow existing patterns |

### Style Guide

| Element | Convention |
|---------|------------|
| Headings | ATX style (`#`, `##`, `###`) |
| Lists | Dashes for unordered, numbers for ordered |
| Code | Backticks for inline, fenced blocks for multi-line |
| Emphasis | `**bold**`, `*italic*` |
| Links | `[text](relative/path.md)` |
| Images | `![alt](relative/path.png)` |
| Notes | `> **Note:** ...` blockquotes |
| Warnings | `> **Warning:** ...` blockquotes |

### "Not Applicable" Pattern

For sections not relevant to this declarative mod:

```markdown
## Section Title

**Not Applicable** — This mod has no [runtime/component/feature].

### Reason

[One sentence explaining why.]

### Context

[Optional: what the engine does instead, or why it doesn't apply.]
```

## Cross-Reference Management

### Reference Types

| Reference | Format | Validation |
|-----------|--------|------------|
| Internal doc | `[text](file.md)` | Link check |
| Internal section | `[text](file.md#section)` | Link + anchor check |
| Mod file | `ui-galaxy-sizes/map/setup_scenarios/ui_ludicrous.txt` | Path existence |
| Parameter | `num_stars` (backticks) | data-model.md presence |
| External | `[text](https://...)` | Periodic manual check |

### Anchor Standards

Use GitHub-style anchors from headings:
- `#repository-overview` → `#repository-overview`
- `#parameter-reference` → `#parameter-reference`
- Lowercase, hyphens, no punctuation

## Deprecation and Removal

### Deprecating Documentation

1. Add deprecation notice at top of file
2. Update `INDEX.md` to mark deprecated
3. Keep content for historical reference
4. Remove after 2 major versions

### Removing Documentation

1. Ensure no internal links point to it
2. Remove from `INDEX.md`
3. Remove from inventory table
4. Delete file
5. Update validation scripts

## Tooling

### Required Tools

| Tool | Purpose | Installation |
|------|---------|--------------|
| `markdownlint` | Lint markdown | `npm install -g markdownlint-cli` |
| `linkcheck` | Validate links | `npm install -g linkcheck` |
| `vale` | Prose linting | `brew install vale` |

### Validation Scripts

```bash
# docs/validate-all.sh
#!/bin/bash
set -e

echo "Validating markdown syntax..."
markdownlint docs/**/*.md README.md

echo "Validating internal links..."
linkcheck docs/**/*.md README.md

echo "Validating structure..."
./scripts/validate-doc-structure.sh

echo "All validations passed."
```

## Metrics and Quality

### Quality Indicators

| Metric | Target | Measurement |
|--------|--------|-------------|
| Broken links | 0 | CI link check |
| Missing files | 0 | Inventory check |
| Outdated version refs | 0 | Release checklist |
| "Not applicable" accuracy | 100% | Quarterly review |
| Cross-reference consistency | 100% | CI xref check |

### Documentation Debt

Track in `ambiguities-and-open-questions.md`:
- Sections needing expansion
- Assumptions needing verification
- Engine behaviours needing documentation

## Onboarding New Maintainers

### Required Reading Order

1. `README.md`
2. `ARCHITECTURE.md`
3. `data-model.md`
4. `change-guide.md`
5. `documentation-maintenance.md` (this file)
6. `INDEX.md`

### First Contribution

1. Pick item from `ambiguities-and-open-questions.md`
2. Follow `change-guide.md` workflow
3. Update documentation per this guide
4. Submit PR

## Emergency Documentation Updates

For critical issues (security, data loss, breaking changes):

1. Create hotfix branch
2. Update only critical docs (README, SECURITY, CHANGELOG)
3. Fast-track review
4. Release patch
5. Full documentation update in next minor release

## Archive Policy

### Versioned Documentation

- Each release tags documentation state
- Git history preserves all versions
- No separate versioned doc site needed

### Historical References

- Keep superseded designs in `design-decisions.md`
- Mark with version and status
- Link from current decisions

## Checklist

### Per Change
- [ ] Affected docs identified
- [ ] Each affected doc updated
- [ ] Cross-references verified
- [ ] No broken links introduced

### Per Release
- [ ] CHANGELOG.md updated
- [ ] README.md version/badge updated
- [ ] ARCHITECTURE.md version updated
- [ ] INDEX.md complete
- [ ] CI validation passes

### Quarterly
- [ ] Full inventory review
- [ ] External link check
- [ ] "Not applicable" accuracy check
- [ ] Ambiguities list reviewed
- [ ] Style consistency check