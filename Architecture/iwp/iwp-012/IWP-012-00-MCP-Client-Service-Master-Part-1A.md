# IWP-012-00 — MCP Client Service Master Implementation Specification

---

# Document Control

| Item | Value |
|------|-------|
| Document ID | IWP-012-00 |
| Document Name | MCP Client Service Master Implementation Specification |
| Work Package | IWP-012 – MCP Client Service |
| Status | Implementation Ready |
| Repository | 86-vibe-cli |
| Governing Architecture | AP-001, AP-002, ARP-001, ARP-002 |
| Sequencing Authority | ADR-001 |
| Depends On | IWP-001 through IWP-011 |
| Implementation Agent | Cursor |

---

# 1. Purpose

This document is the **master implementation specification** for **IWP-012 – MCP Client Service**.

The MCP Client Service is one implementation work package.

Due to the size and complexity of the implementation, the implementation specification has been divided into multiple companion documents.

All companion documents together constitute the complete implementation specification.

Cursor SHALL treat all companion documents as a single implementation package.

No companion document may be ignored.

---

# 2. Governing Architecture

Implementation SHALL conform to:

- AP-001
- AP-002
- ARP-001
- ARP-002
- ADR-001

Architecture is immutable.

Implementation SHALL NOT redesign architecture.

Implementation SHALL NOT reinterpret architecture.

Implementation SHALL NOT introduce new services.

Implementation SHALL NOT change dependency relationships.

Implementation SHALL conform to all approved service contracts.

---

# 3. Implementation Baseline

The following implementation work packages are complete and shall be treated as immutable:

- IWP-001 Repository Foundation
- IWP-002 Configuration Service
- IWP-003 Logging Service
- IWP-004 Bootstrap Service
- IWP-005 Service Registry
- IWP-006 Service Lifecycle Manager
- IWP-007 CLI Framework
- IWP-008 Validation Framework
- IWP-009 Repository Service
- IWP-010 Document Parser Framework
- IWP-011 AI Provider Layer

The MCP Client Service SHALL consume these services only through approved public interfaces and only where permitted by the governing architecture.

No previous work package may be modified except to correct a defect that directly prevents implementation of IWP-012.

Any such correction SHALL be documented in the Completion Report.

---

# 4. Scope

IWP-012 implements the platform MCP Client Service.

The MCP Client Service provides:

- MCP Client Service
- MCP server registry
- configured server discovery
- server registration
- server lifecycle management
- transport abstraction
- connection establishment
- connection management
- protocol initialization
- capability negotiation
- tool discovery
- resource discovery
- prompt discovery
- tool invocation
- resource retrieval
- prompt retrieval
- request validation
- response normalization
- error normalization
- connection health reporting
- MCP diagnostics
- MCP configuration integration
- MCP logging integration
- MCP CLI integration
- MCP request and result models

The service SHALL implement only the capabilities defined by the approved architecture.

---

# 5. Out of Scope

IWP-012 SHALL NOT implement:

- MCP server implementations
- automatic MCP server installation
- MCP server marketplace
- remote MCP server architecture not defined by the approved baseline
- authentication federation
- generalized tool authorization policy management
- MCP server sandboxing
- high-availability MCP server clusters
- tool usage analytics
- Prompt Manager
- Health Monitoring Service
- Event Bus Service
- Plugin Manager Service
- Platform Context Service
- End-to-End Platform Integration
- AI prompt construction
- AI model communication
- repository workflow execution
- third-party tool business logic

The MCP Client Service SHALL not introduce MCP capabilities beyond those explicitly defined by the governing architecture.

---

# 6. Companion Specifications

The following implementation specifications together define IWP-012.

## IWP-012-00

Master Implementation Specification

(this document)

---

## IWP-012-01

MCP Client Service

Defines:

- MCP Client Service
- public API
- lifecycle
- dependency injection
- service registration
- initialization
- shutdown
- consumer contract

---

## IWP-012-02

MCP Client Engine

Defines:

- MCP client execution engine
- server registry
- server orchestration
- connection management
- capability discovery
- request routing
- execution pipeline

---

## IWP-012-03

MCP Transport Implementations

Defines:

- transport interfaces
- transport adapters
- server process management
- protocol connection handling
- capability negotiation
- transport diagnostics
- transport isolation

---

## IWP-012-04

MCP Models

Defines:

- server model
- connection model
- capability model
- tool model
- resource model
- prompt model
- invocation request model
- normalized result model
- health model
- diagnostics model
- serialization model

---

## IWP-012-05

CLI Integration

Defines:

- MCP commands
- server listing
- configuration validation
- MCP diagnostics
- CLI output
- JSON output
- reporting
- exit codes

---

## IWP-012-06

Testing & Acceptance

Defines:

- package layout
- mock MCP servers
- unit testing
- integration testing
- concurrency testing
- security testing
- regression testing
- acceptance criteria
- completion report
- definition of done

---

# 7. Cursor Requirements

Before implementation Cursor SHALL:

1. Review every implementation specification.

2. Confirm the current implementation baseline.

3. Review all governing architecture documents.

4. Review ADR-001.

5. Review AEP-002-08 – Model Context Protocol Integration Specification.

6. Review ARP-002-05 – MCP Client Service Interface Contract.

7. Identify existing Configuration Service APIs.

8. Identify existing Logging Service APIs.

9. Identify existing AI Provider Service APIs.

10. Identify existing Bootstrap and service lifecycle integration patterns.

11. Identify existing Service Registry registration patterns.

12. Identify the MCP CLI command shells established by IWP-007.

13. Implement only the functionality described in the companion specifications.

---

# 8. Implementation Sequence

Cursor SHALL implement the companion specifications as one work package.

The recommended implementation order is:

1. MCP Client Service
2. MCP Models
3. MCP Client Engine
4. MCP Transport Implementations
5. CLI Integration
6. Tests
7. Documentation
8. Completion Report

This implementation order does not alter the architecture.

---

# 9. Coding Standards

Implementation SHALL:

- Follow all approved platform coding standards.
- Use existing dependency injection patterns.
- Use the approved Service Registry.
- Use the approved Logging Service.
- Use the approved Configuration Service.
- Integrate with the approved AI Provider Service only through its public interface.
- Maintain strict TypeScript typing.
- Avoid circular dependencies.
- Follow existing repository structure.
- Produce deterministic behaviour.
- Preserve protocol isolation.
- Normalize all externally exposed MCP responses.
- Normalize all externally exposed MCP errors.
- Encapsulate transport-specific behaviour.
- Support safe concurrent access.
- Redact sensitive values from logs and diagnostics.
- Cleanly release all server processes and connections.

Implementation SHALL NOT introduce undocumented architectural abstractions.

---

# 10. Dependency Graph

The MCP Client Service SHALL depend upon:

- Configuration Service
- Logging Service
- AI Provider Service

The MCP Client Service SHALL NOT depend upon:

- Prompt Manager
- Repository Service
- Validation Framework
- CLI Framework

CLI integration SHALL consume the MCP Client Service public interface without creating a reverse dependency from the service to the CLI Framework.

The MCP Client Service SHALL expose only the interfaces defined by the approved architecture.

No reverse dependencies shall be introduced.

---

# 11. Deliverables

Completion of IWP-012 SHALL produce:

- MCP Client Service
- MCP Client Engine
- MCP Server Registry
- MCP Connection Manager
- MCP Transport Abstraction
- MCP Transport Implementations
- Server Lifecycle Management
- Capability Discovery
- Tool Discovery
- Resource Discovery
- Prompt Discovery
- Tool Invocation
- Resource Retrieval
- Prompt Retrieval
- MCP Models
- Response Normalization
- Error Normalization
- Health Reporting
- MCP Diagnostics
- CLI Integration
- Mock MCP Servers
- Unit Tests
- Integration Tests
- Documentation
- Completion Report

---

# 12. Acceptance Criteria

The implementation SHALL be considered complete only when:

- All companion specifications have been fully implemented.
- All required interfaces conform to the approved architecture.
- Existing platform services remain backward compatible.
- MCP server configuration integration is complete.
- Configured servers are registered deterministically.
- Duplicate server identifiers are rejected.
- Server connections initialize and shut down correctly.
- Capability negotiation operates as specified.
- Tool discovery operates as specified.
- Resource discovery operates as specified.
- Prompt discovery operates as specified.
- Tool invocation returns normalized results.
- Resource retrieval preserves approved protocol semantics.
- Prompt retrieval returns normalized prompt information.
- MCP-specific errors are normalized.
- Transport-specific implementation details remain encapsulated.
- Connection health information is available.
- Concurrent requests preserve connection integrity.
- Sensitive values are not exposed through logs or diagnostics.
- CLI commands operate as specified.
- Test coverage satisfies project requirements.
- No architectural deviations are introduced.
- The Completion Report has been produced.

---

# 13. Definition of Done

IWP-012 SHALL be complete only when:

- All implementation specifications have been implemented.
- All automated tests pass.
- Mock MCP server tests pass.
- Connection lifecycle tests pass.
- Capability discovery tests pass.
- Tool invocation tests pass.
- Resource retrieval tests pass.
- Prompt retrieval tests pass.
- Error normalization tests pass.
- Concurrency tests pass.
- Security and secret-redaction tests pass.
- Linting passes without error.
- Type checking passes without error.
- Build succeeds.
- Documentation is complete.
- Acceptance criteria are satisfied.
- Cursor has produced the required Completion Report.

---

# End of Part 1A
