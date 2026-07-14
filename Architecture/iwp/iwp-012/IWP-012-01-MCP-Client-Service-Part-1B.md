# IWP-012-01 — MCP Client Service Implementation Specification

## Part 1B

---

# 14. Server Registration

The MCP Client Service SHALL expose server registration operations through the approved public interface.

Server registration SHALL:

- validate the registration request
- validate server identifiers
- prevent duplicate registrations
- delegate registration to the MCP Client Engine
- return normalized registration results

Registration SHALL NOT establish a connection.

Connection establishment SHALL remain the responsibility of the MCP Client Engine.

---

# 15. Server Lookup

The service SHALL provide deterministic lookup operations.

Lookup operations SHALL support:

- server identifier
- configured name
- registered instance

Lookup operations SHALL return:

- normalized server model
- normalized status information
- normalized error information

Lookup SHALL NOT expose internal engine implementation.

---

# 16. Capability Discovery

Capability discovery SHALL be exposed through the public service.

The service SHALL:

- validate requests
- delegate execution
- normalize responses
- normalize failures

Capability discovery SHALL include only capabilities defined by the approved MCP architecture.

No implementation-specific capability extensions shall be introduced.

---

# 17. Tool Discovery

The MCP Client Service SHALL expose tool discovery.

The service SHALL:

- retrieve available tools
- normalize tool metadata
- normalize capability metadata
- preserve deterministic ordering

Tool metadata SHALL conform to the approved models defined within IWP-012.

---

# 18. Resource Discovery

Resource discovery SHALL be implemented through delegation.

The service SHALL:

- validate requests
- retrieve resource metadata
- normalize responses
- normalize protocol failures

Resources SHALL remain immutable from the perspective of the client service.

The service SHALL NOT cache mutable resource content unless approved elsewhere within the implementation package.

---

# 19. Prompt Discovery

Prompt discovery SHALL expose prompt metadata supported by the connected MCP server.

The service SHALL:

- delegate discovery
- normalize returned metadata
- normalize protocol failures

Prompt rendering SHALL NOT occur within the MCP Client Service.

---

# 20. Tool Invocation

Tool invocation SHALL:

- validate request models
- validate server availability
- validate tool identifiers
- delegate execution
- normalize results
- normalize failures

Business logic SHALL remain external to the MCP Client Service.

The service SHALL NOT interpret tool-specific payloads.

---

# 21. Resource Retrieval

Resource retrieval SHALL:

- validate requests
- delegate retrieval
- preserve protocol semantics
- normalize responses
- normalize errors

Retrieved resources SHALL remain read-only.

Transformation of retrieved resources SHALL occur outside the MCP Client Service unless explicitly defined elsewhere in the approved architecture.

---

# 22. Prompt Retrieval

Prompt retrieval SHALL:

- validate prompt identifiers
- delegate execution
- normalize returned prompt models
- normalize failures

Prompt execution SHALL NOT occur within the MCP Client Service.

---

# 23. Diagnostics

The service SHALL expose diagnostics information suitable for operational troubleshooting.

Diagnostics SHALL include:

- initialization status
- registered servers
- connected servers
- connection failures
- retry statistics
- protocol negotiation status
- recent operational events

Diagnostics SHALL exclude:

- credentials
- secrets
- authentication tokens
- environment variable values
- sensitive transport information

---

# 24. Metrics

The MCP Client Service SHALL expose metrics suitable for platform monitoring.

Representative metrics include:

- registered server count
- connected server count
- active request count
- successful requests
- failed requests
- retry count
- initialization duration
- average request duration

Metric collection SHALL introduce minimal runtime overhead.

---

# 25. Testing Requirements

Implementation SHALL include unit tests covering:

- initialization
- shutdown
- dependency validation
- request validation
- server registration
- server lookup
- capability discovery
- tool discovery
- resource discovery
- prompt discovery
- tool invocation
- resource retrieval
- prompt retrieval
- diagnostics
- health reporting
- error normalization

All public interface methods SHALL be covered by automated tests.

---

# 26. Acceptance Criteria

Implementation SHALL be accepted only when:

- all public interface operations function correctly
- dependency injection conforms to platform standards
- lifecycle management is deterministic
- registration is deterministic
- duplicate registrations are rejected
- request validation succeeds
- response normalization is complete
- diagnostics are available
- health reporting functions correctly
- logging conforms to platform standards
- automated tests pass
- no architectural deviations are introduced

---

# End of Part 1B

# END OF FILE
