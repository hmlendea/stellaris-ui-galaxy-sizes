# Concurrency and Scheduling

## Concurrency Model

**Not Applicable** — This mod has no runtime concurrency.

### Reason

The mod is purely declarative configuration. It contains:
- No threads
- No processes
- No asynchronous operations
- No parallel execution
- No background assignments
- No event loop
- No scheduler

All execution occurs within the Stellaris engine's single-threaded galaxy generation process.

## Engine Concurrency (Context)

### Galaxy Generation Threading

Stellaris galaxy generation executes on the main thread (or a dedicated generation thread). The mod's scenario parameters are read once at the start of generation.

**No Mod Influence On:**
- Thread creation
- Thread pooling
- Assignment scheduling
- Lock contention
- Race conditions
- Deadlocks

### Multiplayer Synchronisation

In multiplayer, the host generates the galaxy and clients receive the result. No concurrent generation occurs.

## Scheduling

**Not Applicable** — No scheduled assignments, cron jobs, or recurring operations.

### Mod Load Timing

The launcher reads mod descriptors at startup. The engine reads scenario files at galaxy generation. No periodic re-reading occurs.

## Resource Contention

**Not Applicable** — The mod holds no resources at runtime.

### File Access

- The launcher reads `.mod` files at startup (read-only)
- The engine reads scenario files at generation (read-only)
- No write access ever occurs
- No file locking required

## Parallelism Opportunities

**None** — The mod consists of static data. There is no computation to parallelise.

## Related Documents

- `architecture.md` — System context
- `integrations.md` — Engine integration
- `state-and-persistence.md` — No runtime state
