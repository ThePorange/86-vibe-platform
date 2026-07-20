# IWP-002-01 — Configuration Service Implementation Specification

---

# Document Control

| Item | Value |
|------|-------|
| Document ID | IWP-002-01 |
| Document Name | Configuration Service Implementation Specification |
| Work Package | IWP-002 – Configuration Service |
| Status | Implementation Ready |
| Repository | 86-vibe-cli |
| Governing Architecture | AP-001, AP-002, ARP-001, ARP-002, ADR-001 |
| Parent Document | None — single-document work package |
| Implementation Agent | Cursor |

---

# 1. Purpose

This document defines the implementation specification for the **Configuration Service**.

The service SHALL implement the approved service contract defined by AEP-002-05 and ARP-002-02.

Implementation SHALL remain fully compliant with the approved architecture.

The implementation SHALL use only approved platform services and interfaces.

---

# 2. Responsibilities

The Configuration Service SHALL be responsible for:

- discovering supported configuration sources;
- loading built-in defaults and project configuration;
- applying documented environment-variable and CLI/runtime overrides;
- resolving approved precedence;
- validating configuration before dependent services initialize;
- exposing typed, read-only configuration values;
- testing for configuration existence;
- exporting effective configuration as structured data;
- reloading configuration where supported; and
- reporting configuration failures consistently.

Responsibilities SHALL remain limited to those defined by AEP-002-05 and ARP-002-02.

---

# 3. Service Scope

The implementation SHALL provide:

- deterministic configuration loading;
- YAML project configuration at `.86vibe/config.yaml`;
- the precedence order CLI parameters, environment variables, project configuration, and built-in defaults;
- schema, required-field, type, enumeration, file, and directory validation where applicable;
- immutable runtime configuration following validation;
- thread-safe configuration access;
- atomic reload where reload is supported;
- configuration-specific error classification; and
- automated verification of the approved behavior.

The implementation SHALL NOT introduce additional configuration sources, precedence rules, services, public APIs, dependencies, or configuration behavior.

The implementation SHALL NOT implement unrelated work packages.

---

# 4. Service Lifecycle

The Configuration Service SHALL participate in the standard platform lifecycle.

Lifecycle responsibilities SHALL include:

1. Construct the service.
2. Discover approved configuration sources.
3. Load configuration.
4. Resolve precedence.
5. Validate configuration.
6. Freeze runtime configuration.
7. Publish service availability through the approved lifecycle and registration mechanisms when those mechanisms are present in the implementation baseline.
8. Reload configuration atomically where supported.
9. Release allocated resources during shutdown.

Initialization and shutdown SHALL be deterministic and idempotent where applicable.

Lifecycle sequencing SHALL remain consistent with AEP-002-16, AEP-002-17, ARP-001-03, and ARP-002-02.

The Configuration Service SHALL remain the first runtime service initialized after bootstrap and SHALL NOT require another platform service during initialization.

---

# 5. Public Interface

The service SHALL expose the following approved logical interface through `vibe.configuration.ConfigurationService`:

```text
initialize()
load()
reload()
get(key)
contains(key)
validate()
export()
shutdown()
```

The implementation language MAY refine signatures and return types provided equivalent behavior is preserved.

Only architecture-approved interface methods SHALL be implemented.

No undocumented public methods SHALL be introduced.

Public interfaces SHALL include complete type annotations and documentation consistent with ARP-001-05.

---

# 6. Dependency Injection

The Configuration Service has no mandatory platform-service dependencies.

No platform services SHALL be injected during Configuration Service initialization.

Where a later lifecycle, registry, logging, or health integration is attached after Configuration Service initialization, the integration SHALL use only the approved public interface and approved dependency-management mechanism.

Manual construction of other platform services SHALL NOT occur.

The absence of mandatory injected dependencies SHALL be represented explicitly in implementation and tests.

---

# 7. Configuration

The Configuration Service SHALL manage the following approved configuration sources:

1. Built-in platform defaults.
2. Project configuration from `.86vibe/config.yaml`.
3. Documented environment variables.
4. CLI parameters or runtime overrides supplied through the approved initialization interface.

Configuration SHALL follow this precedence from highest to lowest:

1. CLI parameters or runtime overrides.
2. Environment variables.
3. Project configuration.
4. Built-in platform defaults.

Only environment variables documented by AEP-002-05 or another approved architecture document SHALL be supported.

Supported platform examples include:

- `VIBE_AI_PROVIDER`;
- `VIBE_AI_MODEL`;
- `VIBE_LOG_LEVEL`; and
- `VIBE_MCP_ENABLED`.

Provider credentials SHALL retain their provider-defined environment-variable names.

No general underscore-to-dot mapping rule SHALL be introduced unless approved by architecture.

Default values SHALL conform to AEP-002-05 §16 and SHALL be documented and deterministic.

---

# 8. Logging

The implementation SHALL satisfy the Configuration Service logging contract defined by AEP-002-05 §18 and ARP-002-02 §15.

The implementation SHALL record:

- initialization;
- configuration source discovery;
- configuration sources used;
- validation results and warnings;
- applied overrides;
- reload operations; and
- configuration errors.

Logging SHALL use the approved platform logging integration when available through the implementation baseline.

The Configuration Service SHALL NOT acquire a mandatory initialization dependency on the Logging Service.

If the implementation baseline cannot satisfy both the required logging behavior and the approved dependency order, implementation SHALL stop and report the conflict rather than introduce an alternate logging framework.

Sensitive information SHALL NOT be written to logs, and secret values SHALL always be masked.

---

# 9. Service Registration

The Configuration Service SHALL publish availability at the end of successful initialization as required by ARP-002-02.

When the approved Service Registry is present in the implementation baseline, registration SHALL include:

- the canonical service identity `Configuration Service`;
- the public package `vibe.configuration`;
- the primary class `ConfigurationService`;
- singleton lifetime; and
- current lifecycle availability.

The Configuration Service SHALL NOT depend on the Service Registry during its own initialization.

Registration integration SHALL occur only through the approved lifecycle and registry interfaces.

Duplicate registrations SHALL be rejected by the approved Service Registry contract.

---

# 10. Error Handling

The service SHALL normalize externally visible configuration failures using the categories defined by ARP-002-02:

- Missing Configuration;
- Invalid Type;
- Invalid Format;
- Validation Failure; and
- Source Failure.

Errors SHALL remain deterministic and SHALL provide sufficient information to identify the failing configuration item without exposing sensitive values.

Implementation-specific exceptions SHALL NOT cross service boundaries unless they constitute the documented public error contract.

Invalid configuration SHALL prevent dependent command execution.

---

# 11. Thread Safety

The implementation SHALL support concurrent configuration access.

Concurrency requirements SHALL include:

- safe concurrent reads;
- atomic initialization;
- atomic replacement during reload;
- immutable active runtime configuration;
- deterministic behavior;
- absence of race conditions; and
- absence of deadlocks.

Concurrent readers SHALL observe an identical complete configuration snapshot and SHALL NOT require consumer-side synchronization.

---

# 12. Resource Management

The service SHALL correctly manage all resources allocated for configuration loading and synchronization.

Managed resources SHALL include:

- opened configuration files;
- temporary parsed and validation state; and
- synchronization primitives used by the service.

Resources SHALL be released during shutdown.

Resource cleanup SHALL be deterministic and idempotent.

The service SHALL NOT create the full `.86vibe` directory structure; that responsibility remains with approved CLI and bootstrap behavior.

---

# 13. Security Requirements

The implementation SHALL:

- keep secrets outside `config.yaml` and source control;
- accept credentials only from approved environment variables or approved secure operating-system credential stores;
- redact secrets from logs, errors, diagnostics, and diagnostic exports;
- validate external configuration input;
- reject malformed configuration; and
- fail securely.

Secrets SHALL never be exposed.

The service SHALL NOT write configuration without an explicit architecture-approved request.

---

# 14. Performance Requirements

The implementation SHALL optimize configuration lookup for frequent read access.

Configuration SHALL normally be loaded once during bootstrap.

Repeated parsing of configuration sources SHOULD be avoided.

Implementation SHALL minimize unnecessary allocations while preserving readability and correctness.

Blocking operations SHALL be limited to operations required to load or reload approved local configuration sources.

No performance target not defined by the approved architecture SHALL be introduced.

---

# 15. Internal Components

The service SHALL consist of focused internal components corresponding to the following responsibilities:

- service lifecycle and public-interface orchestration;
- approved-source discovery and loading;
- deterministic precedence resolution;
- schema and value validation;
- documented environment-variable processing;
- immutable typed configuration models; and
- configuration error definitions.

Each component SHALL have a single clearly defined responsibility.

Component boundaries SHALL remain consistent with the approved architecture and the IWP-001 implementation baseline.

The internal component split SHALL NOT create additional public services or public interfaces.

---

# 16. Package Structure

The implementation SHALL use the package structure established by the approved IWP-001 baseline:

```text
86-vibe-cli/
├── src/
│   └── vibe/
│       └── configuration/
└── tests/
```

The public package SHALL be `vibe.configuration`, and the primary public class SHALL be `ConfigurationService`.

Internal modules MAY be added beneath `vibe.configuration` only where required to implement approved responsibilities and consistent with repository conventions.

No `vibe.config` compatibility module or parallel implementation SHALL be created.

---

# 17. Implementation Notes

Cursor SHALL implement the service according to the approved architecture and the existing IWP-001 baseline in `86-vibe-cli`.

Implementation notes:

- Preserve existing repository tooling, package exports, exception conventions, typing conventions, and test organization.
- Use YAML for project configuration.
- Use an existing approved YAML dependency where present.
- If no YAML dependency exists, add one only when permitted by the approved dependency policy; otherwise stop and report the unresolved dependency.
- Separate raw, parsed, validated, and immutable runtime configuration.
- Do not expose mutable internal state.
- Do not implement an ad hoc YAML parser.
- Do not introduce user-level, global, remote, database, cloud, or secrets-manager configuration sources.
- Do not implement production versions of later-IWP services.
- Use test doubles only where needed to verify approved integration boundaries.

No architectural assumptions may be introduced.

No implementation behavior may be invented.

---

# 18. Acceptance Criteria

The implementation SHALL be considered complete when:

- the public `vibe.configuration.ConfigurationService` contract is implemented;
- approved configuration sources load deterministically;
- approved precedence is enforced;
- valid configuration passes validation;
- invalid configuration is rejected before dependent services initialize;
- typed, read-only runtime configuration is exposed;
- configuration lookup, existence testing, validation, export, reload, and shutdown behave as documented;
- reload preserves the previous configuration following a failed replacement;
- configuration access and reload are thread-safe;
- approved logging, security, lifecycle, registration, and health integration boundaries are preserved;
- no unsupported environment-variable mapping is implemented;
- no additional configuration source, service, dependency, or public API is introduced;
- unit and integration tests pass;
- formatting, linting, type checking, and repository-defined quality gates pass; and
- existing IWP-001 tests continue to pass.

Implementation SHALL satisfy all approved architectural requirements.

---

# End of Part 1A

# IWP-002-01 — Configuration Service Implementation Specification

---

# 19. Service State Management

The Configuration Service SHALL maintain its internal state in accordance with the approved architecture.

Service state SHALL distinguish at minimum:

- constructed but not initialized;
- initialization in progress;
- initialized with immutable active configuration;
- reload in progress;
- failed initialization or failed reload; and
- shutdown.

State transitions SHALL be deterministic.

The implementation SHALL prevent invalid state transitions.

A failed reload SHALL leave the previously active configuration and initialized state unchanged.

---

# 20. Internal Processing

The service SHALL execute the following internal processing pipeline:

```text
Load Platform Defaults
        ↓
Load Project Configuration
        ↓
Load Documented Environment Variables
        ↓
Apply CLI or Runtime Overrides
        ↓
Validate Configuration
        ↓
Freeze Runtime Configuration
        ↓
Publish Service Availability
```

Each processing stage SHALL complete before the next stage begins.

Reload SHALL repeat the approved loading, precedence, validation, and freezing stages before atomically replacing the active configuration.

Internal processing SHALL remain encapsulated within the service boundary.

---

# 21. Dependency Management

The Configuration Service SHALL have no mandatory platform-service dependencies during initialization.

Dependency management SHALL include:

- use of Python standard-library facilities;
- use of repository utilities established by IWP-001 where architecturally permitted;
- use of an approved YAML dependency; and
- later attachment to approved lifecycle, registry, logging, and health integrations only through their public interfaces.

The service SHALL NOT depend on Logging, Bootstrap, Repository, AI Provider, MCP Client, Event Bus, Service Registry, Lifecycle Manager, or Health Monitoring services during its initialization.

No circular, hidden, or direct cross-service dependency SHALL be introduced.

Unavailable optional integration points SHALL NOT cause the Configuration Service to construct future services manually.

---

# 22. Health Monitoring

The Configuration Service SHALL expose health information through the approved Health Monitoring Service when that service is present in the implementation baseline.

Health reporting SHALL include:

- operational lifecycle state;
- readiness following successful validation and freezing;
- failure information that does not expose secrets; and
- dependency status where applicable.

The Configuration Service SHALL NOT expose an independent health API.

IWP-002 SHALL preserve the approved health integration boundary and SHALL NOT implement the Health Monitoring Service assigned to IWP-013.

---

# 23. Diagnostics

The service SHALL support approved platform diagnostics.

Diagnostics SHALL include:

- configuration source identity without unnecessary local-path disclosure;
- validation status;
- applied override names without sensitive values;
- current lifecycle and readiness state; and
- deterministic error classification.

Diagnostic output SHALL remain deterministic and suitable for automated analysis.

Sensitive information SHALL be excluded or masked.

---

# 24. Recovery Behaviour

The service SHALL implement recovery behavior only where defined by the approved reload contract.

Recovery behavior SHALL include:

- building and validating a replacement configuration separately from the active configuration;
- retaining the previous active configuration when reload fails;
- preventing partial configuration from becoming active; and
- permitting a later valid reload when lifecycle state allows it.

Recovery operations SHALL preserve service consistency.

The implementation SHALL NOT introduce automatic remote recovery, fallback sources, or undocumented retry behavior.

---

# 25. Resource Cleanup

The service SHALL release all allocated resources during shutdown.

Resources include:

- open file handles;
- temporary loading and validation data; and
- synchronization resources owned by the service.

Resource cleanup SHALL be idempotent.

Repeated shutdown requests SHALL NOT result in resource leaks or partially released state.

---

# 26. Extension Points

Extension points SHALL exist only where explicitly defined by the approved architecture.

Supported extension points include future configuration providers that:

- implement the approved public configuration-provider interface;
- participate in approved precedence resolution;
- support validation and deterministic loading; and
- preserve existing consumer behavior.

Any new provider or precedence change SHALL require approved architecture.

No undocumented extension mechanisms SHALL be introduced.

Unknown configuration sections SHALL remain backward-compatible and SHALL be ignored by consumers unless explicitly configured to reject them.

---

# 27. Testing Requirements

Cursor SHALL implement deterministic automated tests covering:

- initialization and shutdown;
- the complete public interface;
- built-in defaults;
- project YAML discovery and parsing;
- documented environment-variable overrides;
- CLI/runtime overrides;
- exact source precedence;
- schema, required-field, type, enumeration, file, and directory validation where applicable;
- missing and malformed configuration;
- unknown sections and properties;
- duplicate-key handling;
- immutable runtime configuration;
- typed values;
- secret redaction;
- successful and failed reload;
- preservation of active configuration after failed reload;
- concurrent readers and reads during reload;
- lifecycle behavior;
- approved logging behavior;
- health-reporting integration boundaries;
- registration integration boundaries; and
- absence of dependencies on later-IWP services during initialization.

Tests SHALL be deterministic.

External services SHALL be represented only by local test doubles where required to verify an approved boundary.

The full repository test suite and repository-defined quality checks SHALL pass.

---

# 28. Documentation Requirements

Implementation documentation SHALL include:

- module documentation;
- public API documentation;
- parameter, return, and documented-exception information;
- approved configuration sources and precedence;
- supported documented environment variables;
- default values;
- validation and reload behavior;
- security and redaction behavior;
- architecture references; and
- implementation notes where beneficial.

Documentation SHALL accurately describe the implemented behavior.

Examples SHALL remain consistent with the approved architecture and SHALL NOT contain secrets.

Documentation SHALL remain synchronized with implementation.

---

# 29. Acceptance Criteria

Implementation SHALL be accepted only when:

- all criteria in §18 are satisfied;
- every SHALL statement in this specification is implemented or verified as an integration boundary assigned to a later approved work package;
- Configuration Service requirements in AEP-002-05, ARP-002-02, and ARP-002-16 are traceable to implementation and tests;
- dependency direction conforms to ARP-001-03 and AEP-002-17;
- the public package remains `vibe.configuration` in `86-vibe-cli`;
- no `vibe.config` compatibility package or duplicate implementation exists;
- no architecture or template modification is required;
- no Critical or Major implementation-review findings remain open; and
- the implementation is ready for human review and merge.

All acceptance criteria SHALL be satisfied before implementation is considered complete.

---

# 30. Implementation Boundary

This document specifies only the implementation of the Configuration Service for IWP-002.

Responsibilities assigned to Logging Service, Bootstrap Service, Service Registry, Service Lifecycle Manager, CLI Framework, Validation Framework, Repository Service, Prompt Management Service, AI Provider Layer, MCP Client Service, Health Monitoring Service, Event Bus Service, Plugin Manager Service, Platform Context Service, and end-to-end integration SHALL remain unchanged.

Implementation SHALL NOT extend beyond the approved Configuration Service boundary.

Implementation SHALL stop and report an ambiguity if compliance would require:

- changing an approved public interface;
- changing the approved repository or public package;
- introducing a new dependency or configuration source;
- implementing a future service;
- changing approved precedence or lifecycle ordering; or
- resolving conflicting architecture through an implementation assumption.

---

# End of Part 1B

# END OF FILE
