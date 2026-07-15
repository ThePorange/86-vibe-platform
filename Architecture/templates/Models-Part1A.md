# {{IWP_ID}} — {{MODELS_DOCUMENT_TITLE}}

---

# Document Control

| Item | Value |
|------|-------|
| Document ID | {{IWP_ID}} |
| Document Name | {{MODELS_DOCUMENT_TITLE}} |
| Work Package | {{WORK_PACKAGE}} |
| Status | Implementation Ready |
| Repository | {{REPOSITORY_NAME}} |
| Governing Architecture | AP-001, AP-002, ARP-001, ARP-002 |
| Parent Document | {{MASTER_DOCUMENT_ID}} |
| Implementation Agent | Cursor |

---

# 1. Purpose

This document defines the implementation specification for the **{{MODEL_SET_NAME}}**.

The model package provides the data structures required by the {{SERVICE_NAME}}.

Models SHALL represent the architecture-defined data contract and SHALL remain free of business logic.

---

# 2. Responsibilities

The model package SHALL be responsible for:

{{MODEL_RESPONSIBILITIES}}

Models SHALL represent only the data structures defined by the approved architecture.

---

# 3. Model Scope

The implementation SHALL provide the following models:

{{MODEL_LIST}}

No additional public models SHALL be introduced.

---

# 4. Model Classification

Models SHALL be classified into the following categories:

{{MODEL_CLASSIFICATION}}

Classification SHALL remain consistent with the approved architecture.

---

# 5. Public Data Contracts

The following public data contracts SHALL be implemented:

{{PUBLIC_DATA_CONTRACTS}}

Public models SHALL remain backward compatible with approved service interfaces.

---

# 6. Internal Models

The implementation SHALL provide the following internal models:

{{INTERNAL_MODELS}}

Internal models SHALL NOT be exposed outside the approved service boundary.

---

# 7. Validation Rules

Each model SHALL implement validation requirements defined by the governing architecture.

Validation SHALL include:

{{MODEL_VALIDATION}}

Validation SHALL be deterministic.

---

# 8. Serialization

Models SHALL support serialization where required by the approved architecture.

Serialization requirements include:

{{MODEL_SERIALIZATION}}

Serialization SHALL remain platform independent.

---

# 9. Deserialization

Models SHALL support deserialization where required.

Deserialization SHALL include:

{{MODEL_DESERIALIZATION}}

Invalid data SHALL be rejected according to the approved architecture.

---

# 10. Version Compatibility

Model evolution SHALL preserve compatibility.

Compatibility requirements include:

{{MODEL_COMPATIBILITY}}

Breaking changes SHALL NOT be introduced without an approved ADR.

---

# 11. Default Values

Models SHALL define default values only where explicitly approved.

Default values include:

{{MODEL_DEFAULTS}}

Implicit defaults SHALL NOT be introduced.

---

# 12. Enumerations

The implementation SHALL provide the following enumerations:

{{MODEL_ENUMS}}

Enumeration values SHALL remain stable.

---

# 13. Type Definitions

The implementation SHALL define the following shared types:

{{MODEL_TYPES}}

Shared types SHALL eliminate duplication across the implementation.

---

# 14. Package Structure

The model package SHALL use the following structure:

{{MODEL_PACKAGE_STRUCTURE}}

Package organization SHALL follow platform conventions.

---

# 15. Naming Standards

Model naming SHALL comply with platform standards.

Naming conventions include:

{{MODEL_NAMING}}

Names SHALL remain consistent across companion implementation specifications.

---

# 16. Documentation Requirements

Each public model SHALL include implementation documentation.

Documentation SHALL include:

{{MODEL_DOCUMENTATION}}

Examples SHALL remain architecture compliant.

---

# 17. Implementation Notes

Cursor SHALL implement the model package according to the approved architecture.

Implementation notes:

{{MODEL_IMPLEMENTATION_NOTES}}

Implementation SHALL remain deterministic.

---

# 18. Acceptance Criteria

Implementation SHALL be considered complete when:

{{MODEL_ACCEPTANCE_CRITERIA}}

---

# End of Part 1A
