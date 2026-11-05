# Security

This document complements the root `SECURITY.md` with implementation-level security analysis.

## Security Model

### Threat Model

**Assets:**
- Mod configuration files (static, no secrets)
- Player save games (owned by Stellaris, not mod)
- Player's Stellaris installation

**Actors:**
- Player (trusted, installs mod voluntarily)
- Mod author (trusted, publishes mod)
- Distribution platforms (Steam, Nexus, Paradox, GitHub)
- Stellaris engine (trusted host)

**Threats:**
- Malicious mod content (mitigated by platform review, open source)
- Save game corruption (engine responsibility)
- Version mismatch (launcher validates)

### Attack Surface

| Surface | Exposure | Mitigation |
|---------|----------|------------|
| Mod descriptor | Launcher reads | Launcher validates format, no execution |
| Scenario files | Engine reads at generation | Engine parses declarative format, no code execution |
| Localisation | Engine reads for UI | Simple key-value, no logic |
| Thumbnail | Launcher displays | Image format, no execution |

**No Attack Vectors:**
- No network communication
- No file I/O beyond Stellaris mod loading
- No user input processing
- No scripting or code execution
- No external dependencies at runtime

## Vulnerability Categories

### Not Applicable

| Category | Reason |
|----------|--------|
| Injection (SQL, command, script) | No code execution, no interpreters |
| Authentication/Authorisation | No accounts, no permissions |
| Cryptography | No encryption, no secrets |
| Data exposure | No personal data collected |
| Denial of Service | No network service |
| Privilege escalation | Runs in Stellaris sandbox |

### Theoretical (Extremely Low Probability)

| Scenario | Likelihood | Impact | Mitigation |
|----------|------------|--------|------------|
| Malicious `.mod` descriptor | Near zero (open source, platform review) | Mod not loaded | Launcher validation |
| Corrupted scenario file | Low (file corruption) | Generation error | Engine fallback defaults |
| Malicious thumbnail | Near zero | Image parser exploit | Platform scanning |

## Security Controls

### Platform-Level

| Platform | Controls |
|----------|----------|
| Steam Workshop | Valve review, automated scanning, user reports |
| GitHub Releases | Open source, code review, signed commits |
| Nexus Mods | Community moderation, virus scanning |
| Paradox Mods | Paradox review process |

### Mod-Level

| Control | Implementation |
|---------|----------------|
| Open source | All files visible on GitHub |
| No binaries | Only text configuration files |
| No external calls | Purely declarative |
| Version pinning | `supported_version` in descriptors |
| Checksums | Git commit hashes, release artefacts |

## Secure Development Practices

### Release Process

1. All changes via pull requests
2. CI validation on every PR
3. Signed Git tags for releases
4. Reproducible release artefacts
5. Platform publishing from CI (where possible)

### Dependency Management

- No runtime dependencies
- Optional UI Star Names is separate mod
- Stellaris version pinned in descriptor

### Code Review Checklist

- [ ] No executable code added
- [ ] No network calls added
- [ ] No file writes added
- [ ] Descriptor versions match
- [ ] Scenario parameters within bounds
- [ ] Localisation keys match scenarios

## Incident Response

### Vulnerability Reporting

Per root `SECURITY.md`:
- GitHub Security Advisories
- Direct maintainer contact
- Coordinated disclosure

### Response Process

1. Acknowledge report
2. Assess impact (likely negligible for this mod type)
3. If valid: fix in source, release patch version
4. Deploy to all platforms
5. Credit reporter

### Historical Incidents

None — no security incidents recorded for this mod.

## Compliance

### Data Protection

- No personal data collected, processed, or stored
- GDPR/CCPA not applicable (no data controller role)
- Privacy policy at `PRIVACY.md`

### Licensing

- GNU GPL v3 (see `LICENSE`)
- Source code available
- Modifications must be shared

## Security Documentation References

- Root `SECURITY.md` — Policy and reporting
- `PRIVACY.md` — Data handling
- `ARCHITECTURE.md` — System context
- This document — Implementation analysis