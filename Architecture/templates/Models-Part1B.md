# {{IWP_ID}} — {{MODELS_DOCUMENT_TITLE}}

---

# 19. Model Relationships

The implementation SHALL define relationships between models as specified by the approved architecture.

Relationships SHALL include:

{{MODEL_RELATIONSHIPS}}

Relationships SHALL remain unidirectional unless otherwise defined by the governing architecture.

Circular model dependencies SHALL NOT be introduced.

---

# 20. Data Integrity

The implementation SHALL preserve data integrity throughout the model lifecycle.

Integrity requirements include:

{{MODEL_DATA_INTEGRITY}}

Model instances SHALL remain internally consistent following construction and validation.

---

# 21. Immutability Requirements

Models SHALL be immutable where defined by the approved architecture.

Immutability requirements include:

{{MODEL_IMMUTABILITY}}

Mutable models SHALL be explicitly identified by the governing architecture.

---

# 22. Equality and Comparison

Models SHALL implement comparison behaviour where required.

Comparison behaviour SHALL include:

{{MODEL_COMPARISON}}

Equality SHALL remain deterministic and independent of runtime state.

---

# 23. Collection Models

Collection models SHALL support approved collection semantics.

Collection requirements include:

{{MODEL_COLLECTIONS}}

Collection ordering SHALL remain deterministic where required.

---

# 24. Factory Methods

Factory methods SHALL be implemented only where defined by the approved architecture.

Factory behaviour SHALL include:

{{MODEL_FACTORIES}}

Factory methods SHALL preserve model validation rules.

---

# 25. Mapping Requirements

Models SHALL support mapping between internal and external representations where required.

Mapping requirements include:

{{MODEL_MAPPING}}

Mapping SHALL remain lossless unless otherwise defined by the governing architecture.

---

# 26. Error Models

Error models SHALL represent standardized service errors.

Error model requirements include:

{{MODEL_ERROR_STRUCTURES}}

Implementation-specific exception details SHALL NOT be exposed.

---

# 27. Testing Requirements

Cursor SHALL implement automated tests covering:

{{MODEL_TEST_REQUIREMENTS}}

Testing SHALL verify:

- validation
- serialization
- deserialization
- equality
- mapping
- default values
- compatibility

All tests SHALL remain deterministic.

---

# 28. Documentation Requirements

Documentation SHALL include:

{{MODEL_DOCUMENTATION_REQUIREMENTS}}

Public model documentation SHALL remain synchronized with the implemented code.

Examples SHALL remain architecture compliant.

---

# 29. Acceptance Criteria

Implementation SHALL be accepted only when:

{{MODEL_ACCEPTANCE_CRITERIA}}

All acceptance criteria SHALL be satisfied before implementation is considered complete.

---

# 30. Implementation Boundary

This document specifies only the implementation of the {{MODEL_SET_NAME}}.

Responsibilities assigned to companion implementation specifications SHALL remain unchanged.

Implementation SHALL NOT extend beyond the approved model boundary.

---

# End of Part 1B

# END OF FILE
