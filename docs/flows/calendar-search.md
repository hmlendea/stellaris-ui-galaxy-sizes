# Calendar Search Flow

## Flow Overview

**Not Applicable** — This mod has no calendar or search flow.

### Reason

The mod is purely declarative configuration consumed by the Stellaris engine. It contains:
- No calendar functionality
- No search functionality
- No date/time handling
- No query/filter logic
- No UI components

The "calendar search" pattern (browsing time-based data) does not apply to a static galaxy-size configuration mod.

## Closest Analogous Flow (Context)

If "calendar search" is interpreted as "browsing a list of options and selecting one," the analogous flow is the **Galaxy Size dropdown selection**:

```
1. Player opens New Game screen
2. Player opens Galaxy Size dropdown
3. Engine lists all loaded scenarios (vanilla + mod)
4. Player browses options (sorted by priority)
5. Player selects a size
6. Engine loads corresponding setup_scenario
```

This is documented in detail in:
- `behaviour/browse-and-search.md` — Browse and select behaviour
- `flows/startup-and-rendering.md` — Dropdown rendering flow

## Not Applicable: Calendar/Search Patterns

Since this mod has no calendar or search flow, the following are **not applicable**:

- Date range queries
- Calendar views (month, week, day)
- Event scheduling
- Recurring events
- Timezone handling
- Search indexing
- Full-text search
- Faceted search
- Filter/pagination
- Sorting strategies
- Search-as-you-type
- Autocomplete
- Search result ranking
- Search analytics
- Saved searches
- Search history
- Date pickers
- Time slot selection
- Availability checking
- Booking flows