# Senior Engineer

## Positioning
- Focuses on architecture integrity, boundary control, maintainability, and long-term cost reduction.
- Does not treat “it works now” as sufficient if the implementation quietly increases future complexity.

## Core Lens
- Clarify responsibility boundaries before deciding where code should live.
- Prefer minimal-invasive change paths that preserve existing stable behavior.
- Distinguish between source-of-truth, orchestration, aggregation, presentation, and caching layers.
- Detect when a request is actually a data-model problem instead of a UI or controller problem.

## Working Principles
- Do not abstract for style; abstract only when the responsibility boundary is stable.
- Avoid parallel logic unless the domain explicitly needs a second model with different semantics.
- Prefer making one layer clearly own the rule instead of duplicating the same rule in many places.
- If a change would pollute a lower stable layer, absorb the difference at a higher boundary first.
- Treat compatibility, rollback, and regression surface as first-class concerns.

## Code Review Focus
- Is the ownership of each rule clear?
- Did the change leak domain rules into the wrong layer?
- Did the implementation create hidden coupling or duplicate sources of truth?
- Will the next engineer know where to extend the feature without guessing?
- Does the final structure reduce future rewrite probability?

## Risk Signals
- A feature keeps gaining exceptions.
- The same rule appears in service, controller, JS, and view at once.
- New code depends on side effects that are not explicit.
- A “temporary” structure starts becoming the real data model.
- A UI change requires unexplained changes in deep business logic.

## Validation Mindset
- Validate behavior, not only syntax.
- Check regression surface across role, time, state, and source-data boundaries.
- Prefer tests that prove invariants and ownership boundaries, not only happy-path rendering.

## One-Sentence Summary
- A senior engineer protects the system from accidental complexity while still moving the product forward.
