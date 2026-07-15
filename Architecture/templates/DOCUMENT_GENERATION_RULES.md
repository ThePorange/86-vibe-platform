# 86-Vibe Implementation Template Library
# Document Generation Rules
# Version 1.0

---

# 1. Purpose

This document defines the mandatory process for generating Implementation Work Package (IWP) specifications using the approved 86-Vibe implementation templates.

These rules ensure that every implementation specification is produced with identical structure, terminology, formatting, and implementation precision.

The implementation library SHALL appear as a single, continuously authored document set.

---

# 2. Governing Documents

Every generated implementation specification SHALL conform to:

- AP-001 – Product Inception & Foundation
- AP-002 – Core Platform Implementation
- ARP-001 – Architecture Readiness Package
- ARP-002 – Service Interface Contracts
- All approved Architecture Decision Records (ADR)

No implementation document may override or reinterpret the governing architecture.

---

# 3. Template Library

The approved Template Library Version 1.0 consists of:

- Master-Part1A.md
- Master-Part1B.md
- Service-Part1A.md
- Service-Part1B.md
- Engine-Part1A.md
- Engine-Part1B.md
- Models-Part1A.md
- Models-Part1B.md
- CLI-Part1A.md
- CLI-Part1B.md
- Testing-Part1A.md
- Testing-Part1B.md
- TEMPLATE_VARIABLES.md
- DOCUMENT_GENERATION_RULES.md

Only these templates shall be used.

---

# 4. Generation Inputs

Document generation SHALL use only the following inputs:

1. AP-001
2. AP-002
3. ARP-001
4. ARP-002
5. Approved ADR documents
6. Template Library Version 1.0

No implementation document shall be used as an architectural source.

Previous implementation documents may be referenced only to preserve writing style and formatting.

---

# 5. Variable Resolution

All template variables SHALL be resolved according to TEMPLATE_VARIABLES.md.

If a required variable cannot be resolved from the approved architecture:

- generation SHALL stop;
- the missing architectural information SHALL be identified; and
- no assumptions shall be made.

---

# 6. Generation Sequence

Each document SHALL be generated using the following sequence:

1. Load the appropriate template.
2. Resolve all template variables.
3. Validate variable values against the approved architecture.
4. Remove all unresolved placeholders.
5. Validate section numbering.
6. Validate document formatting.
7. Validate implementation terminology.
8. Validate architectural compliance.
9. Produce the final implementation specification.

---

# 7. Mandatory Rules

Every generated implementation specification SHALL:

- preserve section numbering;
- preserve heading hierarchy;
- preserve document structure;
- preserve writing style;
- preserve terminology;
- preserve implementation precision;
- preserve formatting;
- preserve end-of-part markers.

Only architecture-derived content may change.

---

# 8. Prohibited Actions

Document generation SHALL NOT:

- redesign architecture;
- introduce new capabilities;
- modify service interfaces;
- invent dependencies;
- invent implementation behaviour;
- alter lifecycle sequencing;
- create undocumented public APIs;
- change terminology established by the architecture.

---

# 9. Validation

Before a document is considered complete, the generator SHALL verify:

- all placeholders have been replaced;
- no unresolved variables remain;
- section numbering is correct;
- formatting matches the template;
- implementation remains within architectural boundaries;
- document references are correct;
- companion document references are correct.

---

# 10. Output Requirements

Each generated document SHALL:

- be GitHub-ready Markdown;
- require no manual editing;
- contain no template variables;
- conform to the approved implementation library;
- be suitable for direct implementation by Cursor.

---

# 11. Versioning

This document defines **Template Library Version 1.0**.

Template Library Version 1.0 SHALL remain stable throughout the implementation of:

- IWP-013
- IWP-014
- IWP-015
- IWP-016
- IWP-017

No structural changes shall be introduced unless approved by a future Architecture Decision Record (ADR).

---

# 12. End of Document

This document is normative for all implementation specifications generated using Template Library Version 1.0.

# END OF FILE
