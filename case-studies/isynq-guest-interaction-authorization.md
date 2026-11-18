# Case Study 02: Securing Guest Identity and Event Interactions in iSYNQ

## Overview

Guest interactions need consistent identity and event-specific authorization across user actions, persistence and realtime updates. I was involved throughout development, independently worked through identity and authorization questions, helped establish the technical approach, and contributed directly to implementation and refinement. AI tools and coding agents supported parts of development and testing. I coordinated agent contributions, reviewed generated changes and made technical decisions. This case study also reviews the available evidence. It complements Case Study 01, which covers event-administration controls.

## Security Findings

The review assessed whether guest identity is established and validated consistently, and whether authorization is checked for relevant event interactions.

## Implementation & QA

During development, I worked through identity and authorization flows, persistence integrity, and negative test cases. I contributed to implementing and refining improvements. The scenarios below describe source-defined coverage, not tests run for this later case study.

Realtime delivery was assessed separately from access control. Successful persistence alone does not establish that an update reached the intended audience.

## Validation Results

The matrix separates test coverage in source, historical outcomes, and verification for this later case study.

| Area | Reviewed Coverage | Verification Status |
|---|---|---|
| Identity and access | Identity boundaries, event-specific authorization and negative cases | Source reviewed; tests not rerun for this case study |
| Persistence | Integrity of stored interactions and ownership association | Historical outcomes are documented in private records and were not independently verified here |
| Realtime | Event delivery and isolation | Delivery remains unverified |
| Negative coverage | Unauthorized access and invalid identity or request scenarios | Test source reviewed; not run for this case study |

For this later evidence review, I examined project source and records but did not run database tests or inspect a live environment. Realtime delivery remains unverified.

## Key Takeaways

- Authentication establishes identity. Authorization determines which event actions are permitted.
- Derive authorization from trusted identity and enforce it consistently across the application and data store.
- Existing records need separate review; their presence alone does not establish correct ownership or authorization.
- Successful persistence does not prove that an update reached its intended audience.
- Keep source-defined tests, recorded outcomes, and independently repeated checks distinct.

## Evidence & References

The source repository and supporting records are private.
