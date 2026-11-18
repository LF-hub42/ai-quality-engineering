# Case Study 01: Securing Event Ownership in iSYNQ

## Overview

Event participation and event administration require distinct authorization boundaries. I was involved throughout development, independently researched authorization questions, helped establish the technical approach, and contributed directly to implementation and refinement. AI tools and coding agents supported parts of development and testing. I coordinated agent contributions, reviewed generated changes and made technical decisions. This case study reviews the development and available evidence. It does not describe a production incident.

## Security Findings

The QA review focused on whether access controls distinguish ordinary participation from privileged actions, ownership is validated consistently, and unauthorized actions are rejected.

## Implementation & QA

During development, I worked through access restrictions, ownership validation, protection against untrusted identity data, and safe handling of existing records. Test sources include negative cases for unauthenticated access, guest permissions, ownership boundaries, legacy records, and existing event interactions. These scenarios reflect the available test coverage. Their inclusion does not imply that I personally executed every test.

| Scenario | Source test coverage |
|---|---|
| Unauthenticated access | Negative checks for unauthorized actions |
| Guest permissions | Checks that participation does not grant administrative rights |
| Ownership boundaries | Checks authorized and unauthorized management actions |
| Existing data and identity handling | Checks how existing records and identity are treated |
| Existing interactions | Checks that security changes preserve related event interactions |

## Validation Results

**Later evidence review:** I reviewed access-control boundaries, ownership checks, and negative-test coverage in the project materials after the development work.

**Historical results:** Private project documentation records prior database checks and runtime test outcomes. The underlying records are not included here, so readers cannot independently verify those results.

**Outstanding verification:** I did not run tests, apply database changes, or inspect a live environment. No public runtime transcript or independent database snapshot was available. Production readiness remains unverified.

## Key Takeaways

- Treat guest membership and event ownership as separate permissions.
- Validate identity and ownership at trusted boundaries, and restrict access to administrative actions.
- Account for existing records when reviewing ownership and authorization changes.
- Validate assumptions before applying security-sensitive data changes.
- Keep source-level implementation, documented historical results, and independent verification distinct.

## Evidence & References

The source repository and supporting records are private.
