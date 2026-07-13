# IWP-008-05 – Validation CLI Integration Implementation Specification
## Part 1B – Rendering, Output Contracts, Exit Codes, Testing and Deliverables

---

# 40. Purpose

Part 1B defines the implementation requirements for the presentation layer of the Validation CLI.

The rendering layer is responsible for transforming immutable `ValidationResponse` models into deterministic user-visible output.

The rendering layer shall remain entirely separate from validation execution.

Validation logic shall never contain rendering code.

---

# 41. Scope

Part 1B defines:

- terminal rendering architecture
- output renderer interfaces
- human-readable output
- JSON output
- verbosity levels
- progress rendering
- diagnostic rendering
- color policy
- exit code mappings
- error rendering
- configuration integration
- package layout
- testing requirements
- acceptance criteria
- implementation deliverables

---

# 42. Rendering Architecture

Rendering shall occur only after the Validation Service has returned a completed `ValidationResponse`.

The rendering pipeline shall be:

```text
ValidationResponse
        │
        ▼
Output Renderer
        │
        ▼
Console / JSON Output
```

The renderer shall never invoke:

- Validation Service
- Validation Engine
- Repository Service
- Validators

---

# 43. Rendering Principles

Rendering shall be:

- deterministic
- read-only
- side-effect free (except output stream writes)
- presentation-only
- independent of validation logic

Renderers shall never mutate:

- ValidationResponse
- ValidationResult
- ValidationFinding
- RuleOutcome
- Summary models

---

# 44. Renderer Package Structure

The implementation shall reside beneath:

```text
src/
    cli/
        validation/
            renderers/
```

Minimum modules:

```text
base_renderer.py
console_renderer.py
json_renderer.py
progress_renderer.py
diagnostic_renderer.py
summary_renderer.py
finding_renderer.py
exit_code_mapper.py
```

---

# 45. Renderer Interface

All renderers shall implement a common interface.

```text
ValidationRenderer
```

Minimum operations:

- render()
- supports()
- format()

Renderers shall accept immutable response models only.

---

# 46. Console Renderer

The Console Renderer shall produce human-readable output.

It shall render:

- validation summary
- findings
- execution statistics
- elapsed time
- repository information

The renderer shall not expose implementation internals.

---

# 47. Summary Rendering

Summary output shall include:

- validation target
- execution status
- total rules
- executed rules
- skipped rules
- total findings
- warning count
- error count
- critical count
- elapsed time

Ordering shall remain deterministic.

---

# 48. Finding Rendering

Findings shall be rendered in canonical order.

Each finding shall display:

- severity
- rule identifier
- repository-relative path
- source location
- message

Optional information:

- remediation
- category
- validator

---

# 49. Severity Presentation

Severity shall be rendered using canonical labels.

```text
INFO

WARNING

ERROR

CRITICAL
```

Display labels shall not modify severity values contained within the models.

---

# 50. Source Location Rendering

Source locations shall be rendered as:

```text
document.md:42

document.md:42:18

document.md:42-47
```

Absolute filesystem paths shall not be displayed.

---

# 51. Rule Identifier Rendering

Rule identifiers shall be rendered exactly as stored.

Example:

```text
validation.naming.filename-prefix
```

Renderer shall not abbreviate identifiers.

---

# 52. Repository Rendering

Repository information may include:

- repository identifier
- repository name
- repository revision
- validation scope

Implementation paths shall not be displayed.

---

# 53. JSON Renderer

The JSON renderer shall serialize the immutable response model.

The renderer shall not create alternate data structures.

Serialization shall use:

- canonical field names
- canonical ordering
- model contract version

---

# 54. JSON Requirements

JSON output shall:

- be deterministic
- contain UTF-8
- avoid comments
- avoid trailing commas
- preserve numeric values
- preserve enumeration values

Pretty-printing may be supported.

---

# 55. Output Formats

Supported output formats:

```text
console

json
```

Additional formats require architecture approval.

---

# 56. Verbosity Levels

Verbosity shall support:

```text
quiet

normal

verbose
```

Verbose output may include:

- execution phases
- timing information
- diagnostics

Validation results shall remain unchanged.

---

# 57. Quiet Mode

Quiet mode shall minimize output.

Only:

- completion summary
- exit status

shall be displayed.

---

# 58. Verbose Mode

Verbose mode may display:

- repository resolution
- execution phases
- rule execution counts
- timing
- diagnostics

Verbose mode shall never expose:

- internal objects
- stack traces
- mutable state

---

# 59. Progress Renderer

Progress rendering shall consume immutable progress events.

Progress rendering shall not:

- affect execution
- alter scheduling
- change timing

---

# 60. Progress Display

Progress may display:

```text
Loading Repository...

Building Inventory...

Selecting Rules...

Executing Rules...

Aggregating Results...

Completed
```

Ordering shall follow ValidationPhase.

---

# 61. Progress Events

Progress events shall include:

- phase
- timestamp
- completion percentage
- message

Events shall remain immutable.

---

# 62. Diagnostic Renderer

Diagnostics shall remain separate from findings.

Diagnostics may include:

- execution timings
- cache usage
- registry snapshot
- configuration source

Diagnostics shall never alter exit codes.

---

# 63. Color Policy

Color output shall be optional.

Supported modes:

```text
auto

always

never
```

Color shall never be required to interpret results.

---

# 64. Accessibility

Rendering shall remain usable without color.

Severity shall always include textual labels.

---

# 65. Exit Code Enumeration

Exit codes shall be represented by:

```text
ValidationExitCode
```

Numeric literals shall not appear throughout command handlers.

---

# 66. Exit Code Mapping

Minimum mappings:

| Exit Code | Meaning |
|-----------|---------|
| 0 | Validation passed |
| 1 | Validation completed with findings |
| 2 | Invalid CLI arguments |
| 3 | Repository error |
| 4 | Configuration error |
| 5 | Validation execution failure |
| 6 | Internal CLI failure |

Mappings shall remain deterministic.

---

# 67. Exit Code Rules

Exit codes shall be determined solely from the completed ValidationResponse and command execution status.

Renderers shall not assign exit codes.

---

# 68. Error Rendering

Errors shall be rendered separately from findings.

Error output shall include:

- error category
- message
- remediation where available

Stack traces shall only be shown when explicitly enabled by the CLI framework.

---

# 69. Logging Relationship

CLI rendering shall not duplicate Logging Service output.

Logging remains machine-oriented.

Rendering remains user-oriented.

---

# 70. Configuration Support

Configuration shall control:

- default output format
- color mode
- verbosity
- progress display

Configuration shall not modify validation semantics.

---

# 71. Performance

Rendering shall operate in linear time relative to the response size.

Repeated serialization of identical models shall be avoided.

---

# 72. Streaming

Large result sets may be rendered incrementally.

Streaming shall preserve deterministic ordering.

---

# 73. Memory Requirements

Renderers shall avoid unnecessary duplication of immutable response models.

Output buffering shall remain bounded.

---

# 74. Thread Safety

Renderers shall remain stateless.

Mutable global rendering state is prohibited.

---

# 75. Testing Strategy

Testing shall include:

- renderer unit tests
- JSON serialization tests
- console rendering tests
- progress rendering tests
- exit code tests

---

# 76. Serialization Tests

Tests shall verify:

- deterministic ordering
- canonical field names
- enumeration values
- timestamps
- identifiers

Golden snapshot tests shall be used where appropriate.

---

# 77. Console Tests

Console rendering shall verify:

- summary output
- finding output
- severity labels
- deterministic ordering
- verbosity handling

---

# 78. Progress Tests

Progress tests shall verify:

- event ordering
- completion percentages
- phase transitions
- cancellation display

---

# 79. Exit Code Tests

Every exit code shall have dedicated tests.

Mappings shall remain stable throughout Contract Version 1.

---

# 80. Compatibility Tests

Compatibility testing shall verify:

- Validation Models
- Validation Service
- CLI Framework
- Repository Service

No contract regressions shall occur.

---

# 81. Acceptance Criteria

Implementation shall be accepted only when:

- all commands register successfully
- console rendering passes snapshot tests
- JSON output is deterministic
- exit codes conform to specification
- verbosity behaves correctly
- progress rendering functions correctly
- immutable response models remain unchanged
- all automated tests pass

---

# 82. Deliverables

The completed implementation shall include:

- Validation CLI package
- renderer package
- JSON renderer
- console renderer
- progress renderer
- diagnostic renderer
- summary renderer
- finding renderer
- exit code mapper
- configuration integration
- automated tests
- documentation

---

# 83. Definition of Done

The Validation CLI Integration implementation shall be considered complete only when:

- IWP-007 CLI Framework integration is complete
- Validation Service integration is complete
- Validation Models integration is complete
- deterministic rendering is verified
- acceptance criteria are satisfied
- all tests pass
- implementation is reviewed
- implementation is approved
- implementation is merged into the implementation baseline
