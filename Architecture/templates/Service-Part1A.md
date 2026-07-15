# {{IWP_ID}} — {{SERVICE_DOCUMENT_TITLE}}

---

# Document Control

| Item | Value |
|------|-------|
| Document ID | {{IWP_ID}} |
| Document Name | {{SERVICE_DOCUMENT_TITLE}} |
| Work Package | {{WORK_PACKAGE}} |
| Status | Implementation Ready |
| Repository | {{REPOSITORY_NAME}} |
| Governing Architecture | AP-001, AP-002, ARP-001, ARP-002 |
| Parent Document | {{MASTER_DOCUMENT_ID}} |
| Implementation Agent | Cursor |

---

# 1. Purpose

This document defines the implementation specification for the **{{SERVICE_NAME}}**.

The service SHALL implement the approved service contract defined by the governing architecture.

Implementation SHALL remain fully compliant with the approved architecture.

The implementation SHALL use only approved platform services and interfaces.

---

# 2. Responsibilities

The {{SERVICE_NAME}} SHALL be responsible for:

{{SERVICE_RESPONSIBILITIES}}

Responsibilities SHALL remain limited to those defined by the governing architecture.

---

# 3. Service Scope

The implementation SHALL provide:

{{SERVICE_CAPABILITIES}}

The implementation SHALL NOT introduce additional capabilities.

---

# 4. Service Lifecycle

The {{SERVICE_NAME}} SHALL participate in the standard platform lifecycle.

Lifecycle responsibilities SHALL include:

{{LIFECYCLE_REQUIREMENTS}}

Lifecycle sequencing SHALL remain consistent with the Bootstrap Service.

---

# 5. Public Interface

The service SHALL expose the following approved public interface.

{{PUBLIC_INTERFACE}}

Only architecture-approved interface methods SHALL be implemented.

No undocumented public methods SHALL be introduced.

---

# 6. Dependency Injection

Dependencies SHALL be obtained exclusively through the approved Dependency Injection framework.

Injected services SHALL include:

{{INJECTED_DEPENDENCIES}}

Manual construction of platform services SHALL NOT occur.

---

# 7. Configuration

The service SHALL obtain configuration exclusively through the Configuration Service.

Configuration SHALL include:

{{CONFIGURATION_ITEMS}}

Configuration SHALL follow the approved precedence rules.

Default values SHALL conform to the governing architecture.

---

# 8. Logging

Logging SHALL use the approved Logging Service.

The implementation SHALL log:

{{LOGGING_REQUIREMENTS}}

Logging SHALL use structured platform logging.

Sensitive information SHALL NOT be written to the log.

---

# 9. Service Registration

The service SHALL register itself using the approved Service Registry.

Registration SHALL include:

{{REGISTRATION_REQUIREMENTS}}

Duplicate registrations SHALL be rejected.

Service discovery SHALL occur only through the Service Registry.

---

# 10. Error Handling

The service SHALL normalize all externally visible errors.

Error categories SHALL include:

{{ERROR_CATEGORIES}}

Implementation-specific exceptions SHALL NOT cross service boundaries.

Errors SHALL remain deterministic.

---

# 11. Thread Safety

The implementation SHALL support concurrent operation where required.

Concurrency requirements SHALL include:

{{THREAD_SAFETY_REQUIREMENTS}}

Shared state SHALL be protected using approved synchronization mechanisms.

---

# 12. Resource Management

The service SHALL correctly manage all allocated resources.

Managed resources SHALL include:

{{RESOURCE_REQUIREMENTS}}

Resources SHALL be released during shutdown.

Resource cleanup SHALL be deterministic and idempotent.

---

# 13. Security Requirements

The implementation SHALL comply with approved platform security standards.

Security requirements SHALL include:

{{SECURITY_REQUIREMENTS}}

Secrets SHALL never be exposed.

Sensitive information SHALL remain protected.

---

# 14. Performance Requirements

The implementation SHALL satisfy the following performance objectives:

{{PERFORMANCE_REQUIREMENTS}}

Implementation SHALL minimise unnecessary allocations.

Blocking operations SHALL be avoided unless required by the approved architecture.

---

# 15. Internal Components

The service SHALL consist of the following internal components:

{{INTERNAL_COMPONENTS}}

Each component SHALL have a single clearly defined responsibility.

Component boundaries SHALL remain consistent with the approved architecture.

---

# 16. Package Structure

The implementation SHALL use the following package structure.

{{PACKAGE_STRUCTURE}}

Package naming SHALL follow platform conventions.

Package organisation SHALL remain consistent with the approved repository structure.

---

# 17. Implementation Notes

Cursor SHALL implement the service according to the approved architecture.

Implementation notes:

{{IMPLEMENTATION_NOTES}}

No architectural assumptions may be introduced.

No implementation behaviour may be invented.

---

# 18. Acceptance Criteria

The implementation SHALL be considered complete when:

{{SERVICE_ACCEPTANCE_CRITERIA}}

Implementation SHALL satisfy all approved architectural requirements.

---

# End of Part 1A
