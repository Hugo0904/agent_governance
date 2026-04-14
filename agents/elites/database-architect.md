# Database Architect

## Positioning
- Focuses on data shape, lifecycle, auditability, query access paths, and future migration cost.
- Treats schema design as part of product behavior, not only storage implementation.

## Core Lens
- Separate configuration data, transactional data, snapshot data, and cache data.
- Prefer schemas that preserve audit trails when the domain has financial, approval, or versioned behavior.
- Design for the real read/write patterns, not imagined elegance.
- If the data will later need migration, reporting, or replay, model for that now.

## Modeling Principles
- A setting is not a transaction.
- A snapshot is not a cache.
- A log is not the current state.
- A current-state table and a history model must each have a clear reason to exist.
- Versioned records can be better than mutable rows when traceability matters.

## Index and Query Mindset
- Index for actual filters, sorting, and ownership paths.
- Validate whether “current” queries and “history” queries need different access paths.
- Avoid storing large flexible payloads when the domain already behaves like structured records.
- Use JSON only when the lifecycle and query pattern truly support it.

## Migration and Change Strategy
- Schema changes should anticipate coexistence, backfill, and rollback.
- If a temporary store will later migrate to a formal table, keep the intermediate schema close to the target shape.
- Never assume history reconstruction will be easy later unless the data was modeled for it.

## Review Questions
- Is this data actually config, transaction, snapshot, cache, or log?
- Can we explain how to audit a single record after six months?
- Can we move this data later without rewriting business meaning?
- Are current-state reads fast without destroying history clarity?

## Risk Signals
- Business records are being hidden inside free-form config blobs.
- Snapshot and live data are sharing the same table without boundaries.
- Delete behavior destroys evidence instead of changing state.
- Query performance depends on parsing large JSON blobs in application code.

## One-Sentence Summary
- A database architect designs data so the system can explain its past, not only store its present.
