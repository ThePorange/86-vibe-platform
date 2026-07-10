ADR-001 – Implement Repository Service Before Validation Framework

Document Control

Item	Value
Document ID	ADR-001
Document Name	Implement Repository Service Before Validation Framework
Platform	86-vibe
Architecture Repository	86-vibe-platform
Status	Approved
Decision Owner	Chief Systems Architect
Approval Authority	Product Owner
Decision Date	2026-07-10
Effective Date	2026-07-10
Depends On	AP-001, AP-002, ARP-001, ARP-002
Affects	IWP-008 – Validation Framework; IWP-009 – Repository Service
Supersedes	Approved implementation roadmap sequencing for IWP-008 and IWP-009 only

⸻

Revision History

Version	Date	Author	Notes
1.0.0	2026-07-10	Chief Systems Architect	Approved decision to implement IWP-009 before IWP-008

⸻

1. Status

Approved

This Architecture Decision Record is effective immediately upon commitment to the authoritative architecture repository.

⸻

2. Decision Summary

The implementation order of the following work packages SHALL be reversed:

Previous Sequence	Revised Sequence
IWP-008 – Validation Framework	IWP-009 – Repository Service
IWP-009 – Repository Service	IWP-008 – Validation Framework

The work package identifiers and scopes SHALL remain unchanged.

The revised implementation sequence is:

1. IWP-009 – Repository Service
2. IWP-008 – Validation Framework
3. IWP-010 – Prompt Management Service
4. IWP-011 – AI Provider Layer
5. IWP-012 – MCP Client Service
6. IWP-013 – Health Monitoring Service
7. IWP-014 – Event Bus Service
8. IWP-015 – Plugin Manager Service
9. IWP-016 – Platform Context Service
10. IWP-017 – End-to-End Platform Integration

This ADR changes implementation sequencing only.

It does not change the approved architecture, service responsibilities, public interfaces, dependency relationships, package structure, or work package scope.

⸻

3. Context

The approved implementation roadmap originally placed:

* IWP-008 – Validation Framework before
* IWP-009 – Repository Service

During preparation of the IWP-008 implementation specification, the governing service contracts were reviewed against the approved implementation sequence.

That review identified a conflict between the roadmap order and the mandatory dependency contract of the Validation Service.

ARP-002-08 – Validation Service Interface Contract defines the Validation Service lifecycle and dependency requirements.

Its lifecycle contract requires the Validation Service to initialize after:

* Configuration Service
* Logging Service
* Repository Service

Its dependency contract requires the Validation Service to depend upon:

* Configuration Service
* Logging Service
* Repository Service

The Repository Service is implemented by IWP-009.

Under the original implementation sequence, IWP-008 would therefore be implemented before one of its mandatory production dependencies existed.

This would prevent IWP-008 from being delivered as a complete, independently testable, architecture-conformant working increment without introducing temporary behavior or implementation structures not authorized by the approved architecture.

⸻

4. Governing Architecture

This decision is governed by the following approved documents:

* AEP-002-10 – Repository Services Specification
* AEP-002-11 – Document Validation Framework Specification
* AEP-002-16 – Platform Bootstrap Sequence Specification
* AEP-002-17 – Service Lifecycle & Dependency Management Specification
* ARP-001-03 – Package Dependency Matrix
* ARP-001-05 – Engineering Coding Standards
* ARP-002-07 – Repository Service Interface Contract
* ARP-002-08 – Validation Service Interface Contract
* ARP-002-16 – Platform Service Contract Compliance Matrix

Where the original roadmap sequence conflicts with the service dependency contracts, the approved dependency and lifecycle contracts take precedence.

⸻

5. Problem Statement

Implementing IWP-008 before IWP-009 would require at least one of the following:

1. Implementing the Validation Service without its mandatory Repository Service dependency.
2. Introducing a temporary production repository implementation.
3. Implementing part of the Repository Service inside IWP-008.
4. Creating an interim adapter or abstraction not authorized by the approved architecture.
5. Deferring required Validation Service behavior until a later work package.
6. Treating test doubles as production dependencies.
7. Revisiting and modifying IWP-008 after IWP-009 is completed.

Each option would violate one or more approved implementation principles.

In particular, the original sequence would conflict with the requirements that every implementation work package:

* conform to the approved architecture
* satisfy applicable ARP-002 service contracts
* be independently implementable
* be independently testable
* produce a working increment
* be committed independently
* avoid introducing architecture not present in the approved baseline

A sequencing correction is therefore required before implementation continues.

⸻

6. Decision Drivers

The decision is based on the following drivers:

6.1 Mandatory Service Dependency

The Repository Service is a required dependency of the Validation Service.

It is not an optional integration.

6.2 Lifecycle Ordering

The approved lifecycle contract requires Repository Service initialization before Validation Service initialization.

Implementation order should support the same dependency direction as runtime lifecycle order.

6.3 Independent Testability

IWP-008 must be testable against the actual approved Repository Service public contract rather than an invented interim implementation.

6.4 Independently Mergeable Work Packages

IWP-009 can be implemented and merged independently because its required dependencies have already been implemented.

After IWP-009 is merged, IWP-008 can also be implemented and merged independently against a complete baseline.

6.5 Avoidance of Temporary Architecture

Reversing the implementation order avoids temporary adapters, duplicate implementations, deferred production behavior, and later rework.

6.6 Preservation of Approved Scope

The sequencing correction can be made without changing either work package’s identifier, responsibilities, interfaces, or acceptance boundary.

6.7 Architectural Traceability

The revised order directly reflects the approved dependency direction:

Validation Service
        |
        v
Repository Service
        |
        v
Configuration Service and Logging Service

⸻

7. Considered Options

7.1 Option A – Retain the Original Sequence

Implement IWP-008 before IWP-009.

Consequences

* The mandatory Repository Service dependency would not exist.
* Production behavior would need to be incomplete or temporary.
* Tests could validate only a substitute rather than the completed dependency.
* IWP-008 would likely require modification after IWP-009.
* The implementation would not satisfy the approved service lifecycle contract.

Decision

Rejected.

⸻

7.2 Option B – Implement a Temporary Repository Adapter in IWP-008

Create a temporary adapter, direct filesystem implementation, or minimal repository abstraction for use by the Validation Service.

Consequences

* Introduces an implementation component not defined by the approved architecture.
* Risks creating two repository execution paths.
* Could cause behavioral divergence when IWP-009 replaces the temporary implementation.
* Weakens deterministic testing and traceability.
* Requires later removal or migration work.

Decision

Rejected.

⸻

7.3 Option C – Subdivide IWP-009

Implement a minimum Repository Service foundation before IWP-008 and complete the remaining Repository Service behavior afterward.

Consequences

* Could preserve the original top-level sequence only through artificial subdivision.
* Adds package-management and traceability complexity.
* Risks separating behavior that belongs to one coherent service increment.
* Is unnecessary because the complete Repository Service can be implemented before the Validation Framework.

Decision

Rejected.

Subdivision remains permissible under the general implementation rules if the complete IWP-009 is later found to be too large, but no subdivision is authorized by this ADR.

⸻

7.4 Option D – Reverse IWP-008 and IWP-009

Implement the complete Repository Service before implementing the Validation Framework.

Consequences

* Satisfies the approved dependency contract.
* Satisfies the approved lifecycle order.
* Allows both work packages to remain independently implementable and testable.
* Avoids temporary production components.
* Preserves the existing package scopes and identifiers.
* Requires only a controlled sequencing amendment.

Decision

Accepted.

⸻

8. Decision

IWP-009 – Repository Service SHALL be implemented, approved, and merged before implementation begins on IWP-008 – Validation Framework.

The identifiers SHALL NOT be renumbered.

The work packages SHALL continue to be named:

* IWP-008 – Validation Framework
* IWP-009 – Repository Service

Their implementation order SHALL be:

IWP-001 – Repository Foundation
IWP-002 – Configuration Service
IWP-003 – Logging Service
IWP-004 – Bootstrap Service
IWP-005 – Service Registry
IWP-006 – Service Lifecycle Manager
IWP-007 – CLI Framework
IWP-009 – Repository Service
IWP-008 – Validation Framework
IWP-010 – Prompt Management Service
IWP-011 – AI Provider Layer
IWP-012 – MCP Client Service
IWP-013 – Health Monitoring Service
IWP-014 – Event Bus Service
IWP-015 – Plugin Manager Service
IWP-016 – Platform Context Service
IWP-017 – End-to-End Platform Integration

IWP-009 SHALL treat IWP-001 through IWP-007 as its implementation baseline.

IWP-008 SHALL treat IWP-001 through IWP-007 and IWP-009 as its implementation baseline.

⸻

9. Scope of Change

This ADR changes only the implementation order of IWP-008 and IWP-009.

It SHALL NOT:

* modify the scope of IWP-008
* modify the scope of IWP-009
* renumber either work package
* change any public service interface
* change any package name
* change any dependency relationship
* change the runtime bootstrap sequence
* change the service lifecycle model
* introduce a new service
* introduce a temporary repository implementation
* permit Validation Service access to the filesystem outside the Repository Service
* authorize modifications to approved AP or ARP documents
* alter the order of IWP-010 through IWP-017

All approved architecture documents remain immutable.

This ADR acts as the authoritative amendment to the implementation roadmap where the roadmap specifies the relative order of IWP-008 and IWP-009.

⸻

10. Implementation Requirements

10.1 IWP-009 – Repository Service

The next implementation package SHALL be IWP-009 – Repository Service.

Its Cursor implementation specification SHALL:

* reference this ADR
* use IWP-001 through IWP-007 as the implementation baseline
* implement only the Repository Service scope
* conform to AEP-002-10 and ARP-002-07
* preserve all previously implemented public interfaces
* remain independently testable
* avoid implementing Validation Framework behavior
* expose the approved public Repository Service interface required by later services
* include the lifecycle, dependency, health, error, logging, and testing requirements defined by the governing architecture

10.2 IWP-008 – Validation Framework

IWP-008 SHALL begin only after IWP-009 has been:

1. implemented
2. tested
3. reviewed
4. approved
5. committed
6. merged into the 86-vibe-cli implementation repository

Its Cursor implementation specification SHALL:

* reference this ADR
* treat IWP-009 as part of the immutable implementation baseline
* use the public Repository Service interface
* avoid direct repository filesystem access
* avoid duplicate repository behavior
* avoid temporary repository adapters in production code
* conform to AEP-002-11 and ARP-002-08
* satisfy the approved Validation Service lifecycle and dependency contracts

⸻

11. Consequences

11.1 Positive Consequences

The decision:

* aligns implementation order with architectural dependency order
* enables complete implementation of the Repository Service before dependent services
* enables the Validation Service to use its real production dependency
* preserves independent testing and mergeability
* removes the need for temporary production components
* reduces implementation rework
* improves architectural traceability
* preserves all approved service scopes and interfaces
* keeps the remainder of the implementation roadmap unchanged

11.2 Negative Consequences

The decision:

* causes work package numbers to be implemented out of numerical order
* requires implementation records and completion reports to state the revised sequence clearly
* requires future prompts and status reports to recognize that IWP-009 was completed before IWP-008
* may require roadmap displays or status trackers to include an ADR reference to prevent confusion

These consequences are administrative and do not affect runtime architecture.

11.3 Neutral Consequences

The numeric work package identifiers remain unchanged.

This means repository history will show IWP-009 as completed before IWP-008.

That is intentional and SHALL NOT be treated as an error.

⸻

12. Compliance Requirements

Implementation and review activities SHALL verify that:

1. IWP-009 is completed before IWP-008 begins.
2. IWP-009 does not implement Validation Framework behavior.
3. IWP-008 consumes the approved Repository Service public interface.
4. IWP-008 does not introduce direct repository-management behavior.
5. No temporary production repository implementation is introduced.
6. No approved service contract is changed.
7. No work package is renumbered.
8. IWP-010 does not begin until both IWP-009 and IWP-008 have been completed.
9. Completion reports reference this ADR where sequencing is relevant.
10. The Platform Service Contract Compliance Matrix remains satisfied.

⸻

13. Traceability

Decision Element	Governing Source
Repository Service responsibilities	AEP-002-10
Validation Framework responsibilities	AEP-002-11
Platform bootstrap ordering	AEP-002-16
Service dependency and lifecycle management	AEP-002-17
Package dependency direction	ARP-001-03
Repository Service public contract	ARP-002-07
Validation Service public contract	ARP-002-08
Service contract compliance	ARP-002-16
Authority to amend immutable architecture	Architecture Decision Record process

⸻

14. Validation of the Decision

This ADR shall be considered correctly applied when:

* it is committed to the authoritative architecture repository
* IWP-009 is produced as the next implementation prompt
* IWP-009 references ADR-001
* IWP-009 is implemented and merged before IWP-008 begins
* IWP-008 subsequently uses the implemented Repository Service
* no temporary repository service or direct filesystem workaround is introduced
* IWP-010 begins only after both IWP-009 and IWP-008 are complete

⸻

15. Rollback

This decision SHALL remain in force unless superseded by a later approved ADR.

Rollback to the original sequence is not operationally meaningful after IWP-009 implementation begins.

A future ADR may modify the implementation sequence only if it:

* identifies a new architectural conflict
* preserves approved service contracts
* documents migration and implementation consequences
* is approved before affected implementation begins

⸻

16. Approval

By approving and committing this ADR, the Product Owner confirms that:

* the sequencing conflict has been reviewed
* IWP-009 is authorized to proceed before IWP-008
* the work package identifiers will remain unchanged
* the architecture and service contracts remain unchanged
* this ADR is the authoritative amendment to the approved implementation roadmap

⸻

17. Final Decision Statement

IWP-009 – Repository Service SHALL be implemented before IWP-008 – Validation Framework.

This reversal is required because the approved Validation Service contract mandates the Repository Service as an initialization and runtime dependency.

The change is limited to implementation sequencing and introduces no change to the approved platform architecture.

