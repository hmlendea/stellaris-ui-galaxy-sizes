# Documentation Maintenance

## Purpose

This document defines processes for keeping documentation accurate, current, and aligned with the mod. It complements the root `CONTRIBUTING.md` documentation section.

## Documentation Inventory

### Root Documents (Repository Root)

| File | Purpose | Update Trigger |
|------|---------|----------------|
| `README.md` | Project overview, installation, usage | Every release |
| `LICENSE` | GNU GPL v3 | Never (unless a licence change) |
| `CONTRIBUTING.md` | Contribution guidelines | Process changes |
| `SECURITY.md` | Vulnerability reporting | Policy changes |
| `PRIVACY.md` | Data handling | Data practice changes |
| `CODE_OF_CONDUCT.md` | Community standards | Community changes |
| `ARCHITECTURE.md` | System architecture | Architecture changes |

### Documentation Directory (`docs/`)

| File | Purpose | Update Trigger |
|------|---------|----------------|
| `repository-overview.md` | High-level repository description | Structural changes |
| `repository-structure.md` | File tree with descriptions | File inclusion, extraction, or move |
| `architecture.md` | Technical architecture | Architecture changes |
| `data-model.md` | Parameter reference | Parameter changes |
| `configuration.md` | Configuration guide | Configuration changes |
| `dependencies.md` | Dependency inventory | Dependency changes |
| `testing.md` | Test strategy | Test changes |
| `security.md` | Security analysis | Security changes |
| `logging.md` | Logging reference | Logging changes |
| `error-handling.md` | Error handling reference | Error handling changes |
| `invariants.md` | System invariants | Invariant changes |
| `change-guide.md` | Change process | Process changes |
| `documentation-maintenance.md` | This file | Process changes |
| `faq.md` | Frequently asked questions | New frequent questions |
| `troubleshooting.md` | Problem resolution | New issues and solutions |
| `ambiguities-and-open-questions.md` | Unresolved items | New ambiguities |
| `quick-start.md` | 5-minute start guide | Installation and usage changes |
| `api-usage-examples.md` | Usage examples | New use cases |
| `design-decisions.md` | Architectural decisions | New decisions |
| `integrations.md` | Integration points | New integrations |
| `state-and-persistence.md` | State and persistence | State changes |
| `concurrency-and-scheduling.md` | Concurrency model | Concurrency changes |
| `components/presentation.md` | UI components | UI changes |
| `components/host-and-composition.md` | Host and composition | Host changes |
| `components/application-services.md` | Application services | Service changes |
| `components/integration-models.md` | Integration models | Model changes |
| `flows/startup-and-rendering.md` | Startup flow | Flow changes |
| `flows/detail-mutation.md` | Detail mutation flow | Flow changes |
| `flows/calendar-search.md` | Calendar search flow | Flow changes |
| `behaviour/browse-and-search.md` | Browse and select behaviour | Behaviour changes |
| `behaviour/inspect-edit-delete.md` | Inspect, edit, and delete | Behaviour changes |
| `api-reference/INDEX.md` | API reference index | API changes |
| `INDEX.md` | Master documentation index | Any documentation change |

## Maintenance Principles

### Single Source of Truth

- Mod files beneath `ui-galaxy-sizes/` constitute the source of truth
- Documentation reflects mod files, never vice versa
- Validation scripts enforce consistency

### Version Alignment

- Documentation version = mod version (in `CHANGELOG.md`)
- The `README.md` version badge corresponds to the latest release
- The `ARCHITECTURE.md` `supported_version` corresponds to the descriptors

### Completeness Over Perfection

- Document "not applicable" explicitly for this declarative mod
- Stub sections are preferred over absent sections
- Cross-reference instead of duplicating

## Update Workflow

### Per-Change Updates

When making a mod change (per `change-guide.md`):

1. Identify affected documentation (see the change-guide.md table)
2. Update each affected file
3. Verify that the cross-references remain valid
4. Run documentation validation (if available)

### Per-Release Updates

At each release:

1. Update `CHANGELOG.md` with the release notes
2. Update the `README.md` version badge and any changed instructions
3. Update `ARCHITECTURE.md` if the Stellaris version changed
4. Verify that `INDEX.md` links all current documentation
5. Check for broken internal links

### Periodic Reviews

| Frequency | Scope | Owner |
|-----------|-------|-------|
| Per release | Changed files only | Release author |
| Quarterly | Full inventory | Maintainer |
| Major version | Complete audit | Maintainer |

## Validation

### Automated Checks (Continuous Integration)

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
- Every inventory file exists
- Required sections present per template
- Valid Markdown syntax

**Cross-Reference Validation:**
- Parameter names correspond to `data-model.md`
- File paths correspond to `repository-structure.md`
- Version numbers correspond

### Manual Checks

- Read-through for clarity
- Verify that examples function
- Confirm "not applicable" sections remain accurate
- Confirm that external links remain accessible

## Documentation Standards

### Formatting

- Markdown (CommonMark and GitHub Flavored)
- Tables for parameter references
- Code blocks for Clausewitz examples
- Repository-relative links for internal references

### Templates

### Templates

Each document type follows a template (see the `github-md-template-fidelity` instructions):

| Document Type | Template Source |
|---------------|-----------------|
| README | `github.readme.template` |
| ARCHITECTURE | `github.architecture.template` |
| CONTRIBUTING | `github.contributing.template` |
| SECURITY | `github.security.template` |
| PRIVACY | `github.privacy.template` |
| CODE_OF_CONDUCT | `github.code-of-conduct.template` |
| ROADMAP | `github.roadmap.template` |
| Others | Ad hoc, following existing patterns |

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

**Not Applicable** — This mod has no runtime code, no component model, or no [feature].

### Reason

[One sentence explaining why.]

### Context

[Optional: what the engine does instead, or why it does not apply.]
```

## Cross-Reference Management

### Reference Types

| Reference | Format | Validation |
|-----------|--------|------------|
| Internal document | `[text](file.md)` | Link check |
| Internal section | `[text](file.md#section)` | Link and anchor check |
| Mod file | `ui-galaxy-sizes/map/setup_scenarios/ui_ludicrous.txt` | Path existence |
| Parameter | `num_stars` (backticks) | Presence in `data-model.md` |
| External | `[text](https://...)` | Periodic manual check |

### Anchor Standards

Use GitHub-style anchors derived from headings:
- `#repository-overview` → `#repository-overview`
- `#parameter-reference` → `#parameter-reference`
- Lowercase, hyphens, no punctuation

## Deprecation and Removal

### Deprecating Documentation

1. Include a deprecation notice at the top of the file
2. Mark the document as deprecated in `INDEX.md`
3. Keep the content for historical reference
4. Extract it after two major versions

### Removing Documentation

1. Confirm that no internal links point to it
2. Removal from `INDEX.md`
3. Removal from the inventory table
4. Removing the file
5. Updating the validation scripts

## Tooling

### Required Tools

| Tool | Purpose | Installation |
|------|---------|--------------|
| `markdownlint` | Lint Markdown | `npm install -g markdownlint-cli` |
| `linkcheck` | Link validation | `npm install -g linkcheck` |
| `vale` | Prose linting | `brew install vale` |

### Validation Scripts

```bash
# docs/validate-all.sh
#!/bin/bash
set -e

echo "Validating Markdown syntax..."
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
| Broken links | 0 | Continuous integration link check |
| Absent files | 0 | Inventory check |
| Outdated version references | 0 | Release checklist |
| "Not applicable" accuracy | 100% | Quarterly review |
| Cross-reference consistency | 100% | Continuous integration cross-reference check |

### Documentation Debt

Track in `ambiguities-and-open-questions.md`:
- Sections requiring expansion
- Premises requiring verification
- Engine behaviours requiring documentation

## Onboarding New Maintainers

### Required Reading Order

1. `README.md`
2. `ARCHITECTURE.md`
3. `data-model.md`
4. `change-guide.md`
5. `documentation-maintenance.md` (this file)
6. `INDEX.md`

### First Contribution

1. Select an item from `ambiguities-and-open-questions.md`
2. Follow the `change-guide.md` workflow
3. Update the documentation per this guide
4. Submit a pull request

## Emergency Documentation Updates

For critical issues (security, data loss, breaking changes):

1. Create a hotfix branch
2. Revise only critical documentation (`README.md`, `SECURITY.md`, `CHANGELOG.md`)
3. Expedite the review
4. Release a patch
5. Complete the documentation update in the next minor release

## Archive Policy

### Versioned Documentation

- Each release tags its documentation state
- Git history preserves every version
- A separate versioned documentation site is unnecessary

### Historical References

- Keep superseded designs in `design-decisions.md`
- Mark with version and status
- Link from current decisions

## Checklist

### Per Change
- [ ] Affected documents identified
- [ ] Each affected document updated
- [ ] Cross-references verified
- [ ] No broken links introduced

### Per Release
- [ ] `CHANGELOG.md` updated
- [ ] Version and badge updated in `README.md`
- [ ] Version updated in `ARCHITECTURE.md`
- [ ] `INDEX.md` complete
- [ ] Continuous integration validation passes

### Quarterly
- [ ] Full inventory review
- [ ] External link check
- [ ] "Not applicable" accuracy check
- [ ] Ambiguities list reviewed
- [ ] Style consistency check
