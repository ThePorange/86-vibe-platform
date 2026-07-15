# {{IWP_ID}} — {{MASTER_DOCUMENT_TITLE}}

---

# Document Control

| Item | Value |
|------|-------|
| Document ID | {{IWP_ID}} |
| Document Name | {{MASTER_DOCUMENT_TITLE}} |
| Work Package | {{WORK_PACKAGE}} |
| Status | Implementation Ready |
| Repository | {{REPOSITORY_NAME}} |
| Governing Architecture | AP-001, AP-002, ARP-001, ARP-002 |
| Sequencing Authority | {{ADR_LIST}} |
| Depends On | {{DEPENDENCY_LIST}} |
| Implementation Agent | Cursor |

---

# 1. Purpose

This document is the master implementation specification for **{{WORK_PACKAGE}}**.

The {{SERVICE_NAME}} implementation consists of multiple implementation specifications.

This document governs the implementation package.

Cursor SHALL treat every companion document as part of a single implementation work package.

No companion document may be omitted.

---

# 2. Governing Architecture

Implementation SHALL conform to:

{{ARCHITECTURE_DOCUMENT_LIST}}

Architecture SHALL NOT be modified.

Implementation SHALL NOT redesign architecture.

Implementation SHALL NOT reinterpret architecture.

Implementation SHALL conform to every approved service contract.

Implementation SHALL NOT introduce architectural changes.

---

# 3. Implementation Baseline

The following implementation work packages SHALL be considered complete.

{{IMPLEMENTATION_BASELINE}}

Previous implementation work packages SHALL NOT be modified except for approved defect corrections.

---

# 4. Scope

{{SERVICE_NAME}} SHALL implement:

{{IMPLEMENTATION_SCOPE}}

Implementation SHALL remain within the approved architecture.

---

# 5. Out of Scope

The following capabilities are outside the scope of this work package.

{{OUT_OF_SCOPE}}

Implementation SHALL NOT introduce additional functionality.

---

# 6. Companion Specifications

The following implementation specifications constitute the complete implementation package.

{{COMPANION_DOCUMENT_LIST}}

Every companion document SHALL be implemented.

---

# 7. Cursor Requirements

Before implementation Cursor SHALL:

{{PRE_IMPLEMENTATION_CHECKLIST}}

Cursor SHALL NOT infer functionality not defined by the approved architecture.

---

# 8. Implementation Sequence

Implementation SHALL follow the approved sequence.

{{IMPLEMENTATION_SEQUENCE}}

Implementation order SHALL NOT modify the approved architecture.

---

# 9. Coding Standards

Implementation SHALL:

{{CODING_STANDARDS}}

Implementation SHALL remain consistent with previous implementation work packages.

---

# 10. Dependency Graph

The {{SERVICE_NAME}} SHALL depend upon:

{{SERVICE_DEPENDENCIES}}

The {{SERVICE_NAME}} SHALL NOT depend upon:

{{PROHIBITED_DEPENDENCIES}}

No circular dependencies may be introduced.

---

# 11. Deliverables

Implementation SHALL produce:

{{DELIVERABLES}}

---

# 12. Acceptance Criteria

Implementation SHALL satisfy:

{{ACCEPTANCE_CRITERIA}}

Acceptance SHALL require successful completion of every companion specification.

---

# End of Part 1A
