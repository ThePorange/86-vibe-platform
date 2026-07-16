# IWP-016-00 — Platform Context Service Master Implementation Specification

---

# Document Control

| Item | Value |
|------|-------|
| Document ID | IWP-016-00 |
| Document Name | Platform Context Service Master Implementation Specification |
| Work Package | IWP-016 – Platform Context Service |
| Status | Implementation Ready |
| Repository | 86-vibe-cli |
| Governing Architecture | AP-001, AP-002, ARP-001, ARP-002 |
| Sequencing Authority | ADR-001 and all subsequently approved ADRs |
| Depends On | IWP-001 through IWP-015 |
| Implementation Agent | Cursor |

---

# 1. Purpose

This document is the master implementation specification for **IWP-016 – Platform Context Service**.

The Platform Context Service implementation consists of multiple implementation specifications.

This document governs the implementation package.

Cursor SHALL treat every companion document as part of a single implementation work package.

No companion document may be omitted.

---

# 2. Governing Architecture

Implementation SHALL conform to:

- AP-001 – Product Inception & Foundation
- AP-002 – Core Platform Implementation
- ARP-001 – Architecture Readiness Package
- ARP-002 – Service Interface Contracts
- ARP-002-15 – Platform Context Service Interface Contract
- ARP-002-17 – Platform Context Service Architecture Completion Addendum
- ADR-001 – Reorder IWP-008 and IWP-009
- Any additional approved ADRs contained within the architecture baseline

Architecture SHALL NOT be modified.

Implementation SHALL NOT redesign architecture.

Implementation SHALL NOT reinterpret architecture.

Implementation SHALL conform to every approved service contract.

Implementation SHALL NOT introduce architectural changes.

---

# 3. Implementation Baseline

The following implementation work packages SHALL be considered complete.

- IWP-001 – Repository Foundation
- IWP-002 – Configuration Service
- IWP-003 – Logging Service
- IWP-004 – Bootstrap Service
- IWP-005 – Service Registry
- IWP-006 – Service Lifecycle Manager
- IWP-007 – CLI Framework
- IWP-008 – Validation Framework
- IWP-009 – Repository Service
- IWP-010 – Document Parser Framework
- IWP-011 – AI Provider Layer
- IWP-012 – MCP Client Service
- IWP-013 – Health Monitoring Service
- IWP-014 – Event Bus Service
- IWP-015 – Plugin Manager Service

Previous implementation work packages SHALL NOT be modified except for approved defect corrections.

---

# 4. Scope

Platform Context Service SHALL implement:

- the approved Platform Context Service defined by ARP-002-15;
- the implementation architecture defined by ARP-002-17;
- immutable execution context creation;
- execution context acquisition;
- execution context cloning;
- execution context propagation;
- execution context lifecycle management;
- correlation identifier management;
- request identifier management;
- originating service identity management;
- platform metadata ownership;
- execution metadata ownership;
- deterministic runtime context propagation;
- context isolation;
- lifecycle-controlled resource cleanup;
- configuration defined by the approved architecture;
- package structure defined by the approved architecture;
- logging, diagnostics and acceptance requirements defined by the governing architecture.

Implementation SHALL remain within the approved architecture.

---

# 5. Out of Scope

The following capabilities are outside the scope of this work package.

- redesign of Platform Context architecture;
- modification of approved service contracts;
- mutable shared runtime state;
- general-purpose application storage;
- event bus redesign;
- bootstrap redesign;
- service registry redesign;
- lifecycle manager redesign;
- logging service redesign;
- AI provider functionality;
- MCP functionality;
- plugin management functionality;
- repository management functionality;
- any capability not explicitly defined by AP, ARP or ADR documentation.

Implementation SHALL NOT introduce additional functionality.

---

# 6. Companion Specifications

The following implementation specifications constitute the complete implementation package.

- IWP-016-00 — Platform Context Service Master Implementation Specification
- IWP-016-01 — Platform Context Service
- IWP-016-02 — Platform Context Engine
- IWP-016-03 — Context Models
- IWP-016-04 — Context Propagation
- IWP-016-05 — CLI Integration
- IWP-016-06 — Testing & Acceptance

Every companion document SHALL be implemented.

---

# 7. Cursor Requirements

Before implementation Cursor SHALL:

- read all governing AP, ARP and ADR documents;
- read DOCUMENT_GENERATION_RULES.md;
- read TEMPLATE_VARIABLES.md;
- implement only approved architecture;
- preserve all public service interfaces;
- preserve dependency sequencing;
- preserve lifecycle ordering;
- preserve deterministic behaviour;
- preserve package structure;
- preserve engineering coding standards;
- preserve companion document sequencing;
- avoid introducing undocumented behaviour.

Cursor SHALL NOT infer functionality not defined by the approved architecture.

---

# 8. Implementation Sequence

Implementation SHALL follow the approved sequence.

1. Master implementation specification.
2. Platform Context Service implementation.
3. Platform Context Engine implementation.
4. Context Models implementation.
5. Context Propagation implementation.
6. CLI Integration.
7. Testing & Acceptance.

Implementation order SHALL NOT modify the approved architecture.

---

# 9. Coding Standards

Implementation SHALL:

- conform to AP-002 engineering standards;
- conform to ARP-001 engineering coding standards;
- implement deterministic behaviour;
- maintain immutable execution contexts;
- preserve dependency injection patterns;
- preserve package boundaries;
- provide structured logging;
- provide comprehensive type annotations;
- implement approved error handling;
- implement unit and integration tests required by the architecture.

Implementation SHALL remain consistent with previous implementation work packages.

---

# 10. Dependency Graph

The Platform Context Service SHALL depend upon:

- Configuration Service
- Logging Service
- Bootstrap Service
- Service Registry
- Service Lifecycle Manager
- Health Monitoring Service
- approved platform models
- approved platform interfaces

The Platform Context Service SHALL NOT depend upon:

- Plugin implementations
- AI provider implementations
- MCP implementations
- repository-specific business logic
- mutable shared global state
- circular service dependencies

No circular dependencies may be introduced.

---

# 11. Deliverables

Implementation SHALL produce:

- complete Platform Context Service implementation;
- complete companion specifications;
- production implementation;
- unit tests;
- integration tests;
- acceptance tests;
- CLI integration;
- documentation required by the implementation package.

---

# 12. Acceptance Criteria

Implementation SHALL satisfy:

- all acceptance criteria defined by ARP-002-15;
- all architectural completion requirements defined by ARP-002-17;
- successful implementation of every companion specification;
- successful platform integration;
- successful automated test execution;
- deterministic runtime behaviour;
- full architectural compliance.

Acceptance SHALL require successful completion of every companion specification.

---

# End of Part 1A
