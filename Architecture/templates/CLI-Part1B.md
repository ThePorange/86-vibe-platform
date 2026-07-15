# {{IWP_ID}} — {{CLI_DOCUMENT_TITLE}}

---

# 19. Command Execution Flow

The CLI SHALL execute commands in accordance with the approved architecture.

Execution SHALL include:

{{CLI_EXECUTION_FLOW}}

The execution flow SHALL remain deterministic.

Each execution stage SHALL complete successfully before progressing to the next stage unless otherwise defined by the governing architecture.

---

# 20. Command Context

The CLI SHALL construct and manage command execution context.

Context SHALL include:

{{CLI_CONTEXT}}

Command context SHALL remain isolated for each invocation.

No state SHALL persist beyond command completion unless explicitly defined by the governing architecture.

---

# 21. Output Rendering

The CLI SHALL render command results using approved output renderers.

Rendering SHALL support:

{{CLI_OUTPUT_RENDERING}}

Output SHALL remain consistent across supported execution environments.

---

# 22. Progress Reporting

Where approved by the governing architecture, the CLI SHALL provide progress reporting.

Progress reporting SHALL include:

{{CLI_PROGRESS_REPORTING}}

Progress reporting SHALL NOT alter command execution behaviour.

---

# 23. Interactive Operation

Where supported by the approved architecture, the CLI SHALL support interactive execution.

Interactive behaviour SHALL include:

{{CLI_INTERACTIVE}}

Interactive prompts SHALL remain deterministic and script-safe when disabled.

---

# 24. Diagnostics

The CLI SHALL provide diagnostic capabilities.

Diagnostics SHALL include:

{{CLI_DIAGNOSTICS}}

Diagnostic output SHALL exclude confidential information.

---

# 25. Configuration Integration

The CLI SHALL obtain configuration exclusively through the Configuration Service.

Configuration integration SHALL include:

{{CLI_CONFIGURATION}}

Command-line overrides SHALL follow the precedence rules defined by the approved architecture.

---

# 26. Extension Points

CLI extension points SHALL exist only where explicitly defined by the governing architecture.

Supported extension mechanisms include:

{{CLI_EXTENSION_POINTS}}

Undocumented extension mechanisms SHALL NOT be introduced.

---

# 27. Testing Requirements

Cursor SHALL implement automated tests covering:

{{CLI_TEST_REQUIREMENTS}}

Testing SHALL verify:

- command registration
- argument parsing
- input validation
- service invocation
- output formatting
- JSON output
- exit codes
- error handling
- diagnostics

All tests SHALL remain deterministic.

---

# 28. Documentation Requirements

Implementation documentation SHALL include:

{{CLI_DOCUMENTATION}}

Documentation SHALL accurately describe all supported commands and options.

Examples SHALL remain architecture compliant.

---

# 29. Acceptance Criteria

Implementation SHALL be accepted only when:

{{CLI_ACCEPTANCE_CRITERIA}}

All acceptance criteria SHALL be satisfied before implementation is considered complete.

---

# 30. Implementation Boundary

This document specifies only the implementation of the CLI integration for the {{SERVICE_NAME}}.

Responsibilities assigned to companion implementation specifications SHALL remain unchanged.

Implementation SHALL NOT extend beyond the approved CLI boundary.

---

# End of Part 1B

# END OF FILE
