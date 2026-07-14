# IWP-012-05 — MCP CLI Integration Implementation Specification

## Part 1B

---

# 14. Prompt Reporting

The `mcp prompts` command SHALL display normalized prompt metadata.

Displayed information SHALL include:

- prompt identifier
- prompt name
- description
- supported parameters
- availability

Prompt content SHALL NOT be rendered by the CLI.

Prompt execution SHALL remain the responsibility of the MCP Client Service.

---

# 15. Health Reporting

The `mcp health` command SHALL present the operational health of the MCP Client Service.

Health reporting SHALL include:

- overall service status
- registered server count
- connected server count
- disconnected server count
- degraded server count
- initialization status
- last health evaluation

Health information SHALL be obtained exclusively from the MCP Client Service.

The CLI SHALL NOT calculate health independently.

---

# 16. Diagnostics Reporting

The `mcp diagnostics` command SHALL provide operational diagnostic information.

Diagnostics SHALL include:

- registered servers
- transport type
- connection state
- retry count
- timeout count
- request statistics
- protocol initialization status
- recent diagnostic events

Diagnostics SHALL exclude:

- credentials
- authentication tokens
- API keys
- environment variable values
- executable command lines
- sensitive configuration values

Diagnostic output SHALL remain suitable for troubleshooting.

---

# 17. Error Reporting

All CLI errors SHALL use the platform-standard error models.

Errors SHALL include:

- validation errors
- configuration errors
- server lookup errors
- connection failures
- timeout failures
- protocol failures
- unexpected internal failures

The CLI SHALL display concise human-readable error messages.

JSON output SHALL return normalized error models.

Internal implementation details SHALL never be displayed.

---

# 18. Exit Codes

The CLI SHALL return standardized exit codes.

Representative exit codes SHALL include:

| Exit Code | Meaning |
|------------|---------|
| 0 | Success |
| 1 | General failure |
| 2 | Validation failure |
| 3 | Configuration failure |
| 4 | Connection failure |
| 5 | Protocol failure |
| 6 | Timeout |
| 7 | Internal error |

Exit codes SHALL remain consistent with existing CLI conventions.

---

# 19. Logging Requirements

The CLI SHALL use the Logging Service.

CLI logging SHALL include:

- command execution
- validation failures
- service invocation
- execution duration
- unexpected failures

Interactive CLI output SHALL remain separate from structured logs.

Sensitive information SHALL be redacted.

---

# 20. Performance Requirements

CLI execution SHALL:

- initialize rapidly
- avoid unnecessary service initialization
- avoid duplicate service requests
- minimize output generation overhead
- minimize memory allocation
- return deterministic execution times where practical

Large result sets SHALL be streamed or formatted efficiently using approved platform conventions.

---

# 21. Security Requirements

The CLI SHALL:

- validate all user input
- reject malformed identifiers
- sanitize displayed values
- redact confidential information
- prevent terminal escape sequence injection
- avoid exposing internal implementation details

The CLI SHALL never display:

- secrets
- authentication tokens
- API keys
- environment variables
- executable arguments containing confidential values

---

# 22. Documentation Requirements

CLI documentation SHALL include:

- command reference
- command syntax
- option descriptions
- output examples
- JSON output examples
- troubleshooting guidance
- exit code reference

Documentation SHALL remain consistent with the CLI Framework documentation established in IWP-007.

---

# 23. Automated Testing

Implementation SHALL include automated tests covering:

- command registration
- argument validation
- server listing
- capability reporting
- tool reporting
- resource reporting
- prompt reporting
- diagnostics reporting
- health reporting
- JSON output
- error reporting
- exit code generation
- logging behavior

Mock MCP Client Service implementations SHALL be used wherever practical.

---

# 24. Acceptance Criteria

Implementation SHALL be accepted only when:

- all approved CLI commands are implemented
- command registration succeeds
- argument validation succeeds
- human-readable output is deterministic
- JSON output conforms to approved models
- diagnostics reporting functions correctly
- health reporting functions correctly
- standardized exit codes are returned
- logging conforms to platform standards
- automated tests pass
- implementation conforms to the approved architecture

---

# End of Part 1B

# END OF FILE
