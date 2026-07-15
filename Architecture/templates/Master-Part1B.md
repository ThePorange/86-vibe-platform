# {{IWP_ID}} — {{MASTER_DOCUMENT_TITLE}}

---

# 13. Implementation Constraints

The {{SERVICE_NAME}} SHALL be implemented strictly within the architectural boundaries defined by:

{{ARCHITECTURE_DOCUMENT_LIST}}

Implementation SHALL NOT:

{{IMPLEMENTATION_CONSTRAINTS}}

The implementation SHALL remain entirely consistent with the approved architecture.

---

# 14. Service Responsibilities

The {{SERVICE_NAME}} SHALL provide the platform implementation for:

{{SERVICE_RESPONSIBILITIES}}

Responsibilities SHALL remain limited to those defined by the governing architecture.

The service SHALL remain independent of business-specific functionality.

---

# 15. Service Boundaries

The {{SERVICE_NAME}} SHALL own:

{{SERVICE_OWNERSHIP}}

The {{SERVICE_NAME}} SHALL NOT own:

{{SERVICE_EXCLUSIONS}}

Responsibilities assigned to other platform services SHALL remain unchanged.

---

# 16. Integration Requirements

The {{SERVICE_NAME}} SHALL integrate only with approved platform services.

## Configuration Service

Used for:

{{CONFIGURATION_USAGE}}

Configuration SHALL be obtained exclusively through the Configuration Service.

---

## Logging Service

Used for:

{{LOGGING_USAGE}}

Logging SHALL follow the approved platform logging conventions.

---

## Service Registry

Used for:

{{REGISTRY_USAGE}}

No alternative registration mechanism SHALL be introduced.

---

## Bootstrap Service

Used for:

{{BOOTSTRAP_USAGE}}

Lifecycle sequencing SHALL remain unchanged.

---

## CLI Framework

Used for:

{{CLI_USAGE}}

CLI integration SHALL consume only approved public interfaces.

---

# 17. Quality Requirements

The implementation SHALL satisfy the following quality attributes.

## Reliability

{{RELIABILITY_REQUIREMENTS}}

---

## Availability

{{AVAILABILITY_REQUIREMENTS}}

---

## Maintainability

{{MAINTAINABILITY_REQUIREMENTS}}

---

## Testability

{{TESTABILITY_REQUIREMENTS}}

---

## Performance

{{PERFORMANCE_REQUIREMENTS}}

---

## Security

{{SECURITY_REQUIREMENTS}}

Sensitive information SHALL never be exposed.

---

# 18. Error Handling

The implementation SHALL normalize all externally visible errors.

Error categories include:

{{ERROR_CATEGORIES}}

Implementation-specific exceptions SHALL NOT cross service boundaries.

---

# 19. Concurrency Requirements

The implementation SHALL support concurrent execution where approved.

Concurrency requirements include:

{{CONCURRENCY_REQUIREMENTS}}

Implementation SHALL remain deterministic.

---

# 20. Documentation Requirements

Cursor SHALL produce implementation documentation including:

{{DOCUMENTATION_REQUIREMENTS}}

Documentation SHALL accurately describe the implemented solution.

---

# 21. Completion Requirements

The implementation SHALL be considered complete only when:

{{COMPLETION_REQUIREMENTS}}

Completion SHALL satisfy all governing architecture documents.

---

# 22. Implementation Boundary

This document defines only the implementation governance for:

{{WORK_PACKAGE}}

Detailed implementation specifications are contained within:

{{COMPANION_DOCUMENT_LIST}}

No implementation behaviour beyond this governance document shall be inferred.

Implementation SHALL rely exclusively upon the companion specifications.

---

# End of Part 1B

# END OF FILE
