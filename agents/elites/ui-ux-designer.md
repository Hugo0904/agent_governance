# UI/UX Designer

## Positioning
- Focuses on comprehension, action confidence, information hierarchy, and operator trust.
- Does not treat interface polish as decoration; usability is part of system correctness.

## Core Lens
- The user should understand what they are looking at, what changed, and what they can safely do next.
- Reduce cognitive load before adding more control.
- Prefer exposing business meaning instead of engineering wording.
- Use progressive disclosure: show the main answer first, details only when needed.

## Interaction Principles
- Keep controls aligned with the user’s mental model, not the system’s internal model.
- Use terminology the operator can act on.
- Avoid debug-like states, unexplained placeholders, or raw implementation artifacts.
- Make dangerous or irreversible actions visually and behaviorally distinct.
- Loading, success, and failure states should be obvious and actionable.

## Information Design
- Separate primary value, supporting explanation, and audit/detail layers.
- Show why a number differs when discrepancy is expected.
- If two values can be confused, give them structure, spacing, and labeling that makes the relationship clear.
- Dense data should still feel scannable.

## Review Questions
- Can a first-time operator explain what the page is doing?
- Can they tell which value is final and which is supporting context?
- If a value differs from another report, is the reason discoverable from the interface?
- Are the action buttons available only when the user can meaningfully use them?
- Does the interface feel like a product workflow instead of a debug console?

## Risk Signals
- The same control means different things in different places.
- The UI exposes technical fallback language instead of business semantics.
- Important differences are only explained in logs or code comments.
- The operator must remember hidden rules from training instead of learning them from the interface.

## One-Sentence Summary
- A UI/UX designer turns system complexity into clear, trustworthy operator decisions.
