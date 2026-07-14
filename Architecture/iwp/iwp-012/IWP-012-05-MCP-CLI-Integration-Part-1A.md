# IWP-012-05 — MCP CLI Integration Implementation Specification

---

# Document Control

| Item | Value |
|------|-------|
| Document ID | IWP-012-05 |
| Document Name | MCP CLI Integration Implementation Specification |
| Work Package | IWP-012 – MCP Client Service |
| Status | Implementation Ready |
| Parent Document | IWP-012-00 |
| Repository | 86-vibe-cli |
| Governing Architecture | AP-001, AP-002, ARP-001, ARP-002 |
| Sequencing Authority | ADR-001 |
| Depends On | IWP-001 through IWP-011 |
| Implementation Agent | Cursor |

---

# 1. Purpose

This document defines the implementation requirements for **MCP CLI Integration**.

The MCP CLI Integration provides the command-line interface through which platform operators and developers interact with the MCP Client Service.

The CLI SHALL expose only the public interface of the MCP Client Service.

The CLI SHALL NOT communicate directly with the MCP Client Engine or any transport implementation.

---

# 2. Scope

The MCP CLI Integration SHALL implement:

- MCP command registration
- command execution
- command validation
- output formatting
- JSON output
- diagnostics reporting
- health reporting
- server inspection
- capability inspection
- tool inspection
- resource inspection
- prompt inspection
- error reporting
- exit code generation

The CLI SHALL remain consistent with the existing CLI Framework implemented by IWP-007.

---

# 3. Responsibilities

The CLI SHALL be responsible for:

- parsing user input
- validating command arguments
- invoking the MCP Client Service
- formatting returned models
- displaying normalized errors
- producing deterministic output
- returning standardized exit codes

The CLI SHALL NOT:

- implement protocol logic
- communicate directly with MCP servers
- maintain server state
- perform capability discovery
- access transport implementations

---

# 4. Dependencies

The MCP CLI Integration SHALL depend upon:

- CLI Framework
- MCP Client Service
- Logging Service

The CLI SHALL NOT depend directly upon:

- MCP Client Engine
- Transport Implementations
- Configuration Service
- Validation Framework

All communication SHALL occur through approved service interfaces.

---

# 5. Command Registration

All MCP commands SHALL be registered using the CLI Framework established in IWP-007.

Commands SHALL be grouped beneath the:

```text
mcp
```

command namespace.

Subcommands SHALL be registered during CLI initialization.

Registration SHALL be deterministic.

---

# 6. Supported Commands

The implementation SHALL support the approved command set including:

- mcp list
- mcp show
- mcp health
- mcp diagnostics
- mcp capabilities
- mcp tools
- mcp resources
- mcp prompts

Only commands defined by the approved architecture SHALL be implemented.

Additional commands SHALL NOT be introduced.

---

# 7. Command Processing

Each command SHALL execute the following sequence:

1. parse command
2. validate arguments
3. invoke MCP Client Service
4. receive normalized result
5. format output
6. display result
7. return exit code

The command execution sequence SHALL be deterministic.

---

# 8. Argument Validation

Argument validation SHALL verify:

- required parameters
- identifier format
- mutually exclusive options
- supported output format
- command syntax

Validation SHALL occur before service invocation.

Invalid arguments SHALL generate standardized CLI validation errors.

---

# 9. Output Formats

The CLI SHALL support:

- human-readable output
- JSON output

JSON output SHALL use the approved MCP Models.

Human-readable output SHALL remain consistent with existing platform CLI conventions.

Output SHALL be deterministic.

---

# 10. Server Listing

The `mcp list` command SHALL display:

- registered servers
- operational status
- transport type
- connection status

Server ordering SHALL remain deterministic.

No internal implementation objects SHALL be displayed.

---

# 11. Capability Reporting

Capability reporting SHALL display:

- available tools
- available resources
- available prompts
- supported protocol capabilities

Capability information SHALL be obtained exclusively through the MCP Client Service.

---

# 12. Tool Reporting

Tool reporting SHALL display normalized tool metadata.

Displayed information SHALL include:

- identifier
- name
- description
- availability

Tool schemas SHALL be displayed only where approved by the architecture.

---

# 13. Resource Reporting

Resource reporting SHALL display:

- resource identifier
- resource URI
- content type
- availability
- metadata summary

Resource contents SHALL NOT be displayed automatically unless explicitly requested through approved command behavior.

---

# End of Part 1A
