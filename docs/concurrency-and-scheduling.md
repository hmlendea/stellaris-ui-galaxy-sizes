# Concurrency and Scheduling

## Concurrency Model

**Not Applicable** — This mod has no runtime concurrency.

### Reason

The mod is purely declarative configuration. It contains:
- No threads
- No processes
- No async operations
- No parallel execution
- No background jobs
- No event loop
- No scheduler

All execution occurs within the Stellaris engine's single-threaded galaxy generation process.

## Engine Concurrency (Context)

### Galaxy Generation Threading

Stellaris galaxy generation runs on the main thread (or a dedicated generation thread). The mod's scenario parameters are read once at generation start.

**No Mod Influence On:**
- Thread creation
- Thread pooling
- Task scheduling
- Lock contention
- Race conditions
- Deadlocks

### Multiplayer Synchronisation

In multiplayer, host generates galaxy, clients receive result. No concurrent generation.

## Scheduling

**Not Applicable** — No scheduled tasks, cron jobs, or recurring operations.

### Mod Load Timing

Launcher loads mod descriptors at startup. Engine reads scenario files at galaxy generation. No periodic re-reading.

## Resource Contention

**Not Applicable** — Mod holds no resources at runtime.

### File Access

- Launcher reads `.mod` files at startup (read-only)
- Engine reads scenario files at generation (read-only)
- No write access ever
- No file locking needed

## Parallelism Opportunities

**None** — Mod is static data. No computation to parallelise.

## Related Documents

- `architecture.md` — System context
- `integrations.md` — Engine integration
- `state-and-persistence.md` — No runtime state