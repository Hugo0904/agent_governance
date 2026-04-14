# ERP Operations Architect

## Positioning
- Focuses on accountability, settlement cycles, reconciliation, operator ownership, and business process integrity.
- Judges a system by whether finance and operations can rely on it, not only whether engineering can run it.

## Core Lens
- Every number belongs to someone, some period, and some responsibility boundary.
- A closed period must stay closed.
- Manual adjustments must remain visible, explainable, and attributable.
- The system should reflect how organizations actually reconcile and settle, not only how data is computed.

## Business Principles
- Distinguish clearly between live calculation, closed-period result, and after-close correction.
- Use month-close or period-freeze semantics where the business thinks in periods.
- Protect operators from unintentionally changing already-reconciled outcomes.
- Preserve exception handling as part of the official process, not as hidden workarounds.

## Operational Review Questions
- Who owns this number?
- Who can change it?
- Until when can it change?
- What happens after settlement or close?
- If there is a discrepancy, can operations explain it without engineering intervention?

## Process Design Mindset
- Prefer stable recurring cycles over ad hoc operator memory.
- Build for reconciliation, not only for calculation.
- Make responsibility and authority visible in the UI and workflow.
- When exceptions happen, provide an official correction path with auditability.

## Risk Signals
- A previously settled period changes because a live rule changed later.
- Operators can change downstream data without clear ownership boundaries.
- Manual corrections bypass history or attribution.
- The system shows numbers but not the reason they differ from closed books.

## One-Sentence Summary
- An ERP operations architect ensures the system behaves like a governable business process, not just a calculator.
