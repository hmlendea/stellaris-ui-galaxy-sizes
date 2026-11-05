# Application Services

## Service Model

**Not Applicable** — This mod has no application services.

### Reason

The mod is purely declarative configuration. It contains:
- No services
- No business logic
- No domain logic
- No application layer
- No use cases
- No commands/queries
- No transaction management

All "services" are provided by the Stellaris engine.

## Engine Services (Context)

The Stellaris engine provides the following services that consume this mod's configuration:

### Galaxy Generation Service

**Input:** Scenario parameters from `setup_scenario` blocks

**Operations:**
- Star placement
- Hyperlane generation
- Empire placement
- Nebula placement
- Crisis fleet generation
- System naming

**Mod Influence:** Parameters only (no code)

### Localisation Service

**Input:** YAML localisation files

**Operations:**
- Key lookup
- Language fallback
- UI string rendering

**Mod Influence:** Provides 6 keys for scenario names

### Mod Management Service

**Input:** `.mod` descriptor files

**Operations:**
- Mod discovery
- Version validation
- Dependency resolution (none for this mod)
- Load ordering
- Mount point management

**Mod Influence:** Provides valid descriptors

## Not Applicable: Traditional Application Services

Since this mod has no application services, the following are **not applicable**:

- Service layer / business logic layer
- Domain services
- Application services / use cases
- Command/Query handlers
- Event handlers
- Saga orchestrators
- Unit of work / transaction scripts
- Repository pattern
- Service registration / discovery
- Health checks
- Metrics collection
- Distributed tracing
- Circuit breakers
- Retry policies
- Timeouts
- Bulkheads
- Rate limiting
- Caching strategies
- Background job processing
- Message handling
- Stream processing
- Batch processing
- Scheduled tasks