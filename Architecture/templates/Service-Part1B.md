# {{IWP_ID}} — {{SERVICE_DOCUMENT_TITLE}}

---

# 19. Service State Management

The {{SERVICE_NAME}} SHALL maintain its internal state in accordance with the approved architecture.

Service state SHALL include:

{{SERVICE_STATE}}

State transitions SHALL be deterministic.

The implementation SHALL prevent invalid state transitions.

---

# 20. Internal Processing

The service SHALL execute the following internal processing pipeline:

{{PROCESSING_PIPELINE}}

Each processing stage SHALL complete before the next stage begins unless explicitly defined otherwise by the approved architecture.

Internal processing SHALL remain encapsulated within the service boundary.

---

# 21. Dependency Management

The {{SERVICE_NAME}} SHALL manage all approved service dependencies.

Dependency management SHALL include:

{{DEPENDENCY_MANAGEMENT}}

Dependencies SHALL be validated during service initialization.

Unavailable dependencies SHALL be handled according to the approved architecture.

---

# 22. Health Monitoring

Where defined by the governing architecture, the service SHALL expose health status through the platform Health Monitoring Service.

Health reporting SHALL include:

{{HEALTH_REQUIREMENTS}}

Health monitoring SHALL NOT expose implementation-specific details.

---

# 23. Diagnostics

The service SHALL support platform diagnostics.

Diagnostics SHALL include:

{{DIAGNOSTIC_REQUIREMENTS}}

Diagnostic output SHALL remain deterministic and suitable for automated analysis.

Sensitive information SHALL be excluded.

---

# 24. Recovery Behaviour

The service SHALL implement recovery behaviour where defined by the architecture.

Recovery behaviour SHALL include:

{{RECOVERY_REQUIREMENTS}}

Recovery operations SHALL preserve service consistency.

---

# 25. Resource Cleanup

The service SHALL release all allocated resources during shutdown.

Resources include:

{{RESOURCE_CLEANUP}}

Resource cleanup SHALL be idempotent.

Repeated shutdown requests SHALL NOT result in resource leaks.

---

# 26. Extension Points

Extension points SHALL exist only where explicitly defined by the approved architecture.

Supported extension points include:

{{EXTENSION_POINTS}}

No undocumented extension mechanisms SHALL be introduced.

---

# 27. Testing Requirements

Cursor SHALL implement automated tests covering:

{{SERVICE_TEST_REQUIREMENTS}}

Tests SHALL be deterministic.

External services SHALL be mocked where appropriate.

---

# 28. Documentation Requirements

Implementation documentation SHALL include:

{{SERVICE_DOCUMENTATION}}

Documentation SHALL accurately describe the implemented behaviour.

Examples SHALL remain consistent with the approved architecture.

---

# 29. Acceptance Criteria

Implementation SHALL be accepted only when:

{{SERVICE_ACCEPTANCE_CRITERIA}}

All acceptance criteria SHALL be satisfied before implementation is considered complete.

---

# 30. Implementation Boundary

This document specifies only the implementation of the {{SERVICE_NAME}}.

Responsibilities assigned to companion implementation specifications SHALL remain unchanged.

Implementation SHALL NOT extend beyond the approved service boundary.

---

# End of Part 1B

# END OF FILE
