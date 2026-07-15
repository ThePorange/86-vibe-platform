# 86-Vibe Implementation Template Variable Dictionary

---

# Purpose

This document defines every placeholder variable used by the Implementation Specification templates.

Variables SHALL be populated only from the approved architecture documentation.

No values may be invented.

If a required value cannot be derived from the architecture, implementation SHALL stop and report the missing architectural information.

---

# Variable Resolution Order

Every variable SHALL be resolved using the following precedence.

1. AP-001
2. AP-002
3. ARP-001
4. ARP-002
5. ADR documents
6. Previous approved implementation specifications (style only)

Implementation specifications SHALL NEVER redefine architecture.

---

# General Variables

| Variable | Description | Source |
|------------|-------------|--------|
| {{IWP_ID}} | Current Implementation Work Package identifier | IWP Roadmap |
| {{WORK_PACKAGE}} | Full work package name | IWP Roadmap |
| {{MASTER_DOCUMENT_TITLE}} | Master document title | IWP Roadmap |
| {{SERVICE_DOCUMENT_TITLE}} | Service document title | IWP Roadmap |
| {{ENGINE_DOCUMENT_TITLE}} | Engine document title | IWP Roadmap |
| {{MODELS_DOCUMENT_TITLE}} | Models document title | IWP Roadmap |
| {{CLI_DOCUMENT_TITLE}} | CLI document title | IWP Roadmap |
| {{TEST_DOCUMENT_TITLE}} | Testing document title | IWP Roadmap |
| {{SERVICE_NAME}} | Primary service name | ARP-002 |
| {{REPOSITORY_NAME}} | Repository containing implementation | AP-002 |
| {{MASTER_DOCUMENT_ID}} | Parent implementation document | IWP Roadmap |

---

# Architecture Variables

| Variable | Description | Source |
|------------|-------------|--------|
| {{ARCHITECTURE_DOCUMENT_LIST}} | Governing architecture documents | AP / ARP |
| {{ADR_LIST}} | Applicable Architecture Decision Records | ADR |
| {{DEPENDENCY_LIST}} | Previous implementation dependencies | IWP Roadmap |
| {{IMPLEMENTATION_BASELINE}} | Previously completed IWPs | IWP Roadmap |

---

# Scope Variables

| Variable | Description | Source |
|------------|-------------|--------|
| {{IMPLEMENTATION_SCOPE}} | Approved implementation scope | AP-002 |
| {{OUT_OF_SCOPE}} | Explicit exclusions | AP-002 |
| {{SERVICE_CAPABILITIES}} | Service capabilities | ARP-002 |
| {{SERVICE_RESPONSIBILITIES}} | Service responsibilities | ARP-002 |
| {{IMPLEMENTATION_SEQUENCE}} | Approved implementation order | ADR / IWP Roadmap |

---

# Interface Variables

| Variable | Description | Source |
|------------|-------------|--------|
| {{PUBLIC_INTERFACE}} | Public service interface | ARP-002 |
| {{CONFIGURATION_ITEMS}} | Configuration parameters | ARP-002 |
| {{INJECTED_DEPENDENCIES}} | Constructor dependencies | ARP-002 |
| {{SERVICE_DEPENDENCIES}} | Allowed dependencies | AP-002 / ARP |
| {{PROHIBITED_DEPENDENCIES}} | Forbidden dependencies | AP-002 |

---

# Lifecycle Variables

| Variable | Description | Source |
|------------|-------------|--------|
| {{LIFECYCLE_REQUIREMENTS}} | Startup / shutdown behaviour | ARP-002 |
| {{REGISTRATION_REQUIREMENTS}} | Service registration rules | ARP-002 |
| {{RESOURCE_REQUIREMENTS}} | Resource lifecycle | AP-002 |

---

# Behaviour Variables

| Variable | Description | Source |
|------------|-------------|--------|
| {{ERROR_CATEGORIES}} | Error definitions | ARP-002 |
| {{THREAD_SAFETY_REQUIREMENTS}} | Concurrency behaviour | AP-002 |
| {{PERFORMANCE_REQUIREMENTS}} | Performance objectives | AP-002 |
| {{SECURITY_REQUIREMENTS}} | Security requirements | AP-001 |
| {{LOGGING_REQUIREMENTS}} | Logging behaviour | AP-002 |
| {{IMPLEMENTATION_NOTES}} | Additional implementation guidance | AP / ARP |

---

# Structural Variables

| Variable | Description | Source |
|------------|-------------|--------|
| {{PACKAGE_STRUCTURE}} | Package layout | AP-002 |
| {{INTERNAL_COMPONENTS}} | Internal implementation components | ARP-002 |
| {{COMPANION_DOCUMENT_LIST}} | Companion implementation specifications | IWP Roadmap |
| {{DELIVERABLES}} | Required implementation deliverables | AP-002 |
| {{PRE_IMPLEMENTATION_CHECKLIST}} | Cursor preparation checklist | Previous IWP template |

---

# Acceptance Variables

| Variable | Description | Source |
|------------|-------------|--------|
| {{SERVICE_ACCEPTANCE_CRITERIA}} | Service acceptance requirements | AP-002 / ARP |
| {{ACCEPTANCE_CRITERIA}} | Overall implementation acceptance | AP-002 |

---

# Template Rules

All placeholders SHALL be replaced before an implementation specification is produced.

No placeholder SHALL remain in the final document.

No variable values may be inferred from implementation code.

Variables SHALL always be resolved from the approved architecture.

---

# Generation Rules

The implementation document generator SHALL:

1. Read AP-001.
2. Read AP-002.
3. Read ARP-001.
4. Read ARP-002.
5. Read all applicable ADR documents.
6. Load the appropriate document template.
7. Resolve every template variable.
8. Validate that no unresolved placeholders remain.
9. Produce the implementation specification.
10. Verify compliance with the governing architecture.

---

# Consistency Rules

Implementation templates SHALL:

- Preserve section numbering.
- Preserve heading hierarchy.
- Preserve document formatting.
- Preserve writing style.
- Preserve implementation precision.
- Preserve terminology.
- Preserve document ordering.

Only architecture-derived values may vary between Implementation Work Packages.

This ensures the implementation library appears as a single, continuously authored document set.

---

# Future Template Extensions

Additional template variables SHALL only be introduced when:

- required by an approved architecture change;
- introduced by an approved ADR; or
- necessary to support a new document type within the implementation library.

Template variables SHALL remain backwards compatible wherever possible.

# END OF FILE
