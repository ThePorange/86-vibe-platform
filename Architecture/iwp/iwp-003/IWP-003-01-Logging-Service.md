Filename: IWP-003-01-Logging-Service.md

IWP-003-01 – Logging Service Implementation Specification

Document Control

Item	Value
Document ID	IWP-003-01
Document Name	Logging Service Implementation Specification
Implementation Package	IWP-003 – Logging Service
Platform	86-vibe
Implementation Repository	86-vibe-cli
Architecture Repository	86-vibe-platform
Status	Implementation Ready
Owner	Chief Systems Architect
Implementation Agent	Cursor
Depends On	IWP-001 – Repository Foundation, IWP-002 – Configuration Service
Supersedes	None

⸻

1. Purpose

This document defines the complete implementation instructions for IWP-003 – Logging Service.

The Logging Service is the authoritative mechanism through which all 86-vibe platform components generate diagnostic, operational, audit, and troubleshooting information.

This implementation package shall produce a working, independently testable Logging Service conforming to the approved architecture baseline.

Cursor shall treat this document as the complete implementation prompt.

Nothing required for implementation shall exist outside this Markdown document except the approved architecture repository and the existing 86-vibe-cli implementation baseline.

⸻

2. Implementation Objective

Implement the Logging Service in the 86-vibe-cli repository.

The implementation shall provide deterministic, structured, secure, configurable logging for platform components.

The implementation shall satisfy the public service contract defined by:

* ARP-002-03 – Logging Service Interface Contract
* AEP-002-06 – Logging & Diagnostics Specification
* AEP-002-16 – Platform Bootstrap Sequence Specification
* AEP-002-17 – Service Lifecycle & Dependency Management Specification
* AEP-002-18 – Platform Reference Implementation Specification
* ARP-002-16 – Platform Service Contract Compliance Matrix

This package shall not implement unrelated platform services.

⸻

3. Authoritative Architecture Inputs

Cursor shall use the following approved architecture documents as governing references.

3.1 Primary Governing Documents

Document	Relevance
AEP-002-06 – Logging & Diagnostics Specification	Defines logging architecture, log locations, levels, defaults, formatting, console output, error logging, debug logging, redaction, rotation, diagnostics, and testing
ARP-002-03 – Logging Service Interface Contract	Defines public service identity, responsibilities, lifecycle, operations, dependency contract, logger contract, output contract, formatting contract, thread safety, and testing requirements
ARP-002-16 – Platform Service Contract Compliance Matrix	Defines mandatory lifecycle, configuration, logging, error handling, health, thread-safety, testing, and documentation requirements
ARP-001-05 – Engineering Coding Standards	Defines mandatory Python engineering standards
ARP-001-03 – Package Dependency Matrix	Defines dependency direction and prohibits circular dependencies

3.2 Supporting Governing Documents

Document	Relevance
AEP-002-02 – Repository Directory Structure	Defines repository and package layout
AEP-002-03 – Python Package Architecture	Defines Python package responsibilities
AEP-002-05 – Configuration Management Specification	Defines logging configuration values consumed by this package
AEP-002-12 – Testing Strategy Specification	Defines test expectations
AEP-002-13 – Build & Packaging Specification	Defines package/build expectations
AEP-002-15 – Local Development Workflow Specification	Defines development workflow expectations
AEP-002-16 – Platform Bootstrap Sequence Specification	Defines bootstrap ordering
AEP-002-17 – Service Lifecycle & Dependency Management Specification	Defines lifecycle and dependency behavior

⸻

4. Implementation Baseline

The following work packages have already been implemented, approved, merged, and are now part of the immutable implementation baseline:

* IWP-001 – Repository Foundation
* IWP-002 – Configuration Service

Cursor shall:

1. Inspect the existing 86-vibe-cli repository.
2. Preserve all structure, tooling, conventions, and tests established by IWP-001.
3. Reuse the implemented Configuration Service from IWP-002.
4. Preserve the public Configuration Service interface.
5. Avoid changing IWP-001 or IWP-002 behavior unless required to correct a defect directly blocking IWP-003.
6. Treat any such defect as an implementation issue and document it in the completion report.

⸻

5. Strict Scope

This work package implements only:

IWP-003 – Logging Service

The following services are explicitly out of scope:

* IWP-004 – Bootstrap Service
* IWP-005 – Service Registry
* IWP-006 – Service Lifecycle Manager
* IWP-007 – CLI Framework
* IWP-008 – Validation Framework
* IWP-009 – Repository Service
* IWP-010 – Prompt Management Service
* IWP-011 – AI Provider Layer
* IWP-012 – MCP Client Service
* IWP-013 – Health Monitoring Service
* IWP-014 – Event Bus Service
* IWP-015 – Plugin Manager Service
* IWP-016 – Platform Context Service
* IWP-017 – End-to-End Platform Integration

Cursor shall not implement placeholder production versions of these services.

Minimal test doubles, local stubs, or interfaces may be used only when required by tests and only if they do not introduce architectural behavior.

⸻

6. Architectural Non-Negotiables

Cursor shall comply with the following rules.

6.1 No Architecture Redesign

Do not redesign architecture.

Do not reinterpret architecture.

Do not introduce new services.

Do not change approved package relationships.

Do not change approved public interfaces.

Do not add alternate logging frameworks.

Do not change configuration precedence.

Do not bypass the Configuration Service.

Do not bypass the Logging Service for diagnostic logging once implemented.

6.2 Stop Conditions

Cursor shall stop and report an ambiguity instead of inventing a solution if any of the following are discovered:

1. The approved architecture conflicts with existing IWP-001 or IWP-002 code.
2. The package location is ambiguous after inspecting the repository.
3. The public Logging Service contract cannot be implemented without changing another service contract.
4. Required behavior depends on a future IWP service that has not yet been implemented.
5. A new dependency appears necessary but is not already approved.
6. The service cannot satisfy thread-safety requirements using the existing baseline.
7. The implemented Configuration Service does not expose the logging configuration required by this package.

Cursor shall not proceed by assumption in those cases.

⸻

7. Service Identity

The Logging Service shall conform to the identity defined in ARP-002-03.

Property	Required Value
Service Name	Logging Service
Package	vibe.logging
Primary Class	LoggingService
Lifetime	Singleton
Thread Safety	Required
Public	Yes

If IWP-001 established a package naming convention that differs slightly from this package path, Cursor shall preserve the existing repository structure and expose the approved public package path where required.

Do not create multiple independent logging implementations.

⸻

8. Required Logging Behavior

The Logging Service shall provide centralized logging for the entire platform.

It shall be responsible for:

* initializing platform logging
* reading logging configuration from the Configuration Service
* creating logger instances
* applying configured logging levels
* routing log events to configured outputs
* maintaining deterministic log formatting
* supporting structured log events
* supporting correlation identifiers
* supporting exception logging
* supporting secret redaction
* supporting file-based logging
* supporting console logging
* supporting log flushing
* supporting clean shutdown

The service shall not:

* perform business logic
* modify application state unrelated to logging
* interpret business meaning of log content
* expose implementation-specific logging library details to consumers
* initialize unrelated platform services
* create repositories
* implement CLI command routing
* implement diagnostic report generation beyond logging primitives required for future commands

⸻

9. Dependency Contract

The Logging Service shall depend only upon:

* Configuration Service

The Logging Service shall not depend upon:

* Repository Service
* AI Provider Layer
* Prompt Manager
* MCP Client
* Validation Framework
* Event Bus
* Plugin Manager
* Platform Context Service

This dependency isolation ensures logging remains available during early platform startup and late platform shutdown.

⸻

10. Initialization Order

Initialization shall occur after the Configuration Service has successfully completed.

Required initialization sequence:

Construct Logging Service
Read logging configuration
Configure outputs
Configure log format
Configure log level
Publish logger availability

Because Bootstrap Service is not yet implemented, this work package shall expose lifecycle methods compatible with the future bootstrap sequence without implementing the Bootstrap Service itself.

⸻

11. Public Service Interface

The implementation shall conform to the interface defined by ARP-002-03.

The public service shall expose operations equivalent to:

class LoggingService:
    def initialize(self) -> None:
        ...
    def get_logger(self, name: str) -> PlatformLogger:
        ...
    def set_level(self, level: str) -> None:
        ...
    def flush(self) -> None:
        ...
    def shutdown(self) -> None:
        ...

Method names may differ only where required by the approved architecture or the existing implementation baseline.

Public APIs shall include:

* complete type hints
* docstrings
* deterministic behavior
* documented exceptions

⸻

12. Logger Interface

Logger instances returned by the Logging Service shall expose operations equivalent to:

class PlatformLogger:
    def trace(self, message: str, **fields: object) -> None:
        ...
    def debug(self, message: str, **fields: object) -> None:
        ...
    def info(self, message: str, **fields: object) -> None:
        ...
    def warning(self, message: str, **fields: object) -> None:
        ...
    def error(self, message: str, **fields: object) -> None:
        ...
    def critical(self, message: str, **fields: object) -> None:
        ...
    def exception(self, message: str, **fields: object) -> None:
        ...

Logger instances shall be:

* lightweight
* reusable
* named
* thread-safe
* structured-event capable
* exception-aware

Consumers shall obtain logger instances from the Logging Service.

Consumers shall not use the Python logging module directly once this service is available.

⸻

13. Log Levels

The Logging Service shall support the following levels:

Level	Purpose
TRACE	Detailed execution diagnostics
DEBUG	Development diagnostics
INFO	Normal operational events
WARNING	Recoverable issues
ERROR	Operation failure
CRITICAL	Unrecoverable platform failure

AEP-002-06 defines standard platform levels as DEBUG, INFO, WARNING, ERROR, and CRITICAL.

ARP-002-03 adds TRACE.

Cursor shall implement TRACE as a supported platform level without breaking standard Python logging semantics.

TRACE shall be lower severity than DEBUG.

Severity ordering shall remain stable and deterministic.

⸻

14. Default Logging Configuration

Unless explicitly overridden by configuration, the platform shall use:

Setting	Default
Console Level	INFO
File Level	DEBUG
Error Log	Enabled
Debug Log	Disabled
Colored Console Output	Enabled
Rotation	Enabled

The architecture-defined YAML configuration shape is:

logging:
  level: INFO
  console: true
  file: true
  debug: false
  rotation: true

The Logging Service shall read these values through the Configuration Service implemented in IWP-002.

The Logging Service shall not parse .86vibe/config.yaml directly.

⸻

15. Log Locations

Project logs shall be stored within the project metadata directory.

Required log directory:

.86vibe/logs/

Required log files:

.86vibe/logs/platform.log
.86vibe/logs/error.log
.86vibe/logs/debug.log

The Logging Service shall create the log directory if it does not exist.

The Logging Service shall not initialize a full repository or project structure.

Only logging-specific directories and files required for logging may be created.

⸻

16. Log Output Destinations

The Logging Service shall support one or more output destinations.

Required output types:

Output	Purpose
Console	Interactive execution
File	Persistent logs

Future destinations are out of scope for this work package.

Output routing shall be controlled through configuration.

If console logging is disabled, no console log handler shall emit diagnostic log events.

If file logging is disabled, no file log handler shall write persistent log events.

⸻

17. Log File Format

Each log entry shall contain:

* timestamp
* log level
* component
* command where available
* logger name
* message
* correlation identifier where available

Timestamp format shall be ISO-8601 UTC.

Architecture example:

2026-07-09T14:12:51Z INFO cli.init Repository initialized successfully

The implementation shall produce deterministic, human-readable, machine-parseable log entries.

⸻

18. Structured Logging

The Logging Service shall internally support structured log events.

Each event shall contain information equivalent to:

{
  "timestamp": "...",
  "level": "INFO",
  "component": "cli",
  "command": "doctor",
  "message": "Environment validation complete"
}

Structured fields shall be accepted through logger methods using keyword arguments.

Example:

logger.info(
    "Repository validated successfully",
    component="repository",
    command="repo.validate",
    document_count=42,
)

Structured metadata shall be formatted consistently.

Unsupported field values shall be converted safely to strings without raising formatting exceptions during normal logging.

⸻

19. Correlation Identifiers

The Logging Service shall support correlation identifiers.

When present, the identifier shall remain consistent throughout a logical operation.

The implementation shall provide a mechanism equivalent to:

set_correlation_id(correlation_id: str)
get_correlation_id()
clear_correlation_id()

or an approved context manager equivalent.

Correlation identifiers shall be thread-safe.

Correlation identifiers shall not leak between unrelated operations.

A context-local implementation is preferred.

⸻

20. Secret Redaction

The Logging Service shall automatically redact sensitive values.

The Logging Service shall never log:

* passwords
* API keys
* authentication tokens
* private credentials
* decrypted secrets
* private keys
* certificates
* authentication headers
* confidential user data where identifiable through configured sensitive field names

Sensitive values shall be masked before logging.

Architecture example:

Authorization: ********

At minimum, the redaction implementation shall mask values associated with keys containing:

password
passwd
pwd
secret
token
api_key
apikey
authorization
auth
private_key
certificate
credential

Matching shall be case-insensitive.

Redaction shall apply to:

* structured metadata fields
* exception messages where practical
* formatted output
* exported diagnostic event representations if any

Do not claim complete arbitrary-string secret detection.

Implement deterministic key-based and known-pattern redaction appropriate for the architecture baseline.

⸻

21. Exception Logging

Exceptions shall be logged with:

* exception type
* message
* stack trace when debug mode is enabled
* correlation identifier when available

Unexpected failures shall be written to:

.86vibe/logs/error.log

Raw Python stack traces shall not be displayed to the console unless debug mode is enabled.

The exception() logger method shall capture active exception information when called from an exception context.

Sensitive information shall be redacted.

⸻

22. Debug Logging

Debug logging shall be disabled by default.

When enabled, debug logging shall support events related to:

* configuration loading
* command execution
* validation steps
* provider selection
* MCP communication
* file discovery

This work package shall implement the debug logging mechanism, not the future services that emit those events.

Debug logging shall never expose secrets.

When debug logging is disabled, debug-specific file output shall not be written unless required for standard platform file logging at DEBUG level.

⸻

23. Log Rotation

The platform shall automatically rotate log files.

Minimum requirements:

* preserve the current log
* archive previous logs
* limit total retained log files
* prevent unlimited growth

Cursor shall implement rotation using Python standard library logging handlers where practical.

Preferred implementation:

logging.handlers.RotatingFileHandler

Do not introduce third-party logging dependencies.

Default rotation values shall be deterministic and documented in code.

If architecture or IWP-002 configuration already defines rotation size/count values, use those values.

If not defined, use conservative defaults and document them as implementation defaults, not architecture changes.

⸻

24. Thread Safety

The Logging Service shall be thread-safe.

Concurrent logging from multiple threads shall:

* preserve event integrity
* avoid message corruption
* avoid deadlocks

Message ordering shall be preserved within a single execution thread where practical.

The implementation may rely on Python standard logging thread-safety primitives where appropriate, but service-level mutable state shall still be protected.

⸻

25. Internal Package Layout

Cursor shall implement the Logging Service using the repository structure established by IWP-001.

Expected logical layout:

src/
└── vibe/
    └── logging/
        __init__.py
        service.py
        logger.py
        levels.py
        formatters.py
        handlers.py
        redaction.py
        correlation.py
        models.py
        exceptions.py

If IWP-001 already created equivalent modules, extend them rather than introducing parallel implementations.

No duplicate implementations shall exist.

⸻


## 26. Internal Module Responsibilities

The Logging Service shall be implemented using well-defined internal modules.

### service.py

Responsible for:

- service lifecycle
- initialization
- shutdown
- logger registry
- handler orchestration
- configuration integration

The service module shall not contain formatting or redaction logic.

---

### logger.py

Responsible for:

- PlatformLogger implementation
- structured logging interface
- log event creation
- exception logging helpers

Logger instances shall be lightweight wrappers over the configured logging infrastructure.

---

### levels.py

Responsible for:

- platform log level definitions
- TRACE level registration
- level conversion
- level validation

No formatting logic shall exist here.

---

### formatters.py

Responsible for:

- console formatting
- file formatting
- timestamp formatting
- structured field formatting
- deterministic message rendering

Formatting shall remain consistent across all handlers.

---

### handlers.py

Responsible for:

- console handler creation
- rotating file handler creation
- error handler creation
- debug handler creation
- handler lifecycle

---

### redaction.py

Responsible for:

- secret masking
- sensitive field detection
- metadata sanitization
- recursive object redaction

Redaction shall occur before events are written to any output.

---

### correlation.py

Responsible for:

- correlation identifier storage
- context propagation
- thread-local/context-local management

---

### models.py

Responsible for:

- structured log event models
- immutable logging metadata objects
- internal event representation

---

### exceptions.py

Responsible for:

- LoggingServiceError
- LoggerInitializationError
- LoggerConfigurationError
- LoggerShutdownError

If a common platform exception hierarchy exists from IWP-001, inherit from that hierarchy.

---

## 27. Configuration Integration

The Logging Service shall obtain all configuration through the Configuration Service implemented in IWP-002.

The Logging Service shall never:

- parse YAML directly
- inspect configuration files directly
- inspect environment variables directly

Expected configuration access:

```python
config = configuration_service.get_section("logging")
```

or the equivalent public API defined in IWP-002.

If logging configuration changes through a supported configuration reload, the Logging Service shall expose a mechanism compatible with future dynamic reconfiguration.

Full runtime log reconfiguration is not required in this work package unless explicitly defined by the approved architecture.

---

## 28. Logger Registry

The Logging Service shall maintain a registry of logger instances.

The registry shall ensure:

- identical logger names return the same logger instance
- duplicate logger creation is avoided
- logger instances remain thread-safe
- logger configuration remains centrally managed

Example:

```python
logger1 = logging_service.get_logger("repository")
logger2 = logging_service.get_logger("repository")

assert logger1 is logger2
```

Logger lifecycle shall be managed by the Logging Service.

Consumers shall not construct PlatformLogger objects directly.

---

## 29. Handler Configuration

The Logging Service shall configure handlers based upon platform configuration.

Supported handlers:

| Handler | Purpose |
|----------|----------|
| ConsoleHandler | Interactive console output |
| RotatingFileHandler | Persistent platform log |
| ErrorFileHandler | Error-only log |
| DebugFileHandler | Debug diagnostics |

Handlers shall be enabled or disabled using configuration.

Handler configuration shall occur during Logging Service initialization.

---

## 30. Formatter Requirements

Console output shall prioritize readability.

File output shall prioritize deterministic diagnostics.

Every formatter shall include:

- timestamp
- severity
- logger name
- message

Structured metadata shall appear in a predictable order.

Formatter implementations shall not mutate event data.

---

## 31. Structured Metadata

Logger methods shall accept arbitrary structured metadata.

Example:

```python
logger.info(
    "Prompt loaded",
    prompt="AP-002-17",
    repository="86-vibe-platform",
    duration_ms=14,
)
```

Metadata shall:

- remain optional
- support primitive Python types
- support dictionaries where practical
- support lists where practical

Unsupported objects shall be converted safely using deterministic string representations.

---

## 32. Performance Requirements

Logging shall introduce minimal runtime overhead.

Requirements include:

- lazy message formatting where practical
- avoidance of unnecessary object creation
- efficient handler reuse
- minimal locking
- reuse of formatter instances

The Logging Service shall not significantly impact command execution time.

---

## 33. Error Recovery

Failure writing to one output destination shall not automatically disable all logging.

Examples:

- console logging failure shall not prevent file logging
- debug handler failure shall not disable error logging

Critical initialization failures shall produce deterministic LoggingService exceptions.

---

## 34. Lifecycle

The Logging Service shall expose lifecycle operations compatible with the future Lifecycle Manager.

Required operations:

- initialize()
- shutdown()

Shutdown shall:

- flush all handlers
- close all handlers
- release file handles
- leave no background resources active

Multiple shutdown calls shall be safe.

---

## 35. Unit Testing Requirements

Cursor shall implement comprehensive unit tests.

Minimum coverage shall include:

### Initialization

- default initialization
- configuration-driven initialization
- invalid configuration
- repeated initialization

### Logger Registry

- identical names return identical instances
- distinct names return distinct instances

### Log Levels

Verify:

- TRACE
- DEBUG
- INFO
- WARNING
- ERROR
- CRITICAL

Verify conversion between configured values and runtime values.

### Formatting

Verify:

- timestamp formatting
- structured metadata formatting
- deterministic formatting
- exception formatting

### Console Handler

Verify:

- enabled
- disabled
- configured level
- formatting

### File Handler

Verify:

- file creation
- file writing
- rotation configuration
- error logging
- debug logging

### Redaction

Verify masking of:

- password
- secret
- token
- api_key
- authorization
- credential
- nested structured metadata

Verify values are never written unmasked.

### Correlation

Verify:

- correlation identifier creation
- retrieval
- clearing
- isolation between execution contexts

### Exception Logging

Verify:

- exception message
- stack trace
- correlation identifier
- structured metadata
- redaction

### Shutdown

Verify:

- flush
- handler closure
- repeated shutdown safety

---

## 36. Integration Testing Requirements

Integration tests shall verify Logging Service operation with the existing implementation baseline.

Tests shall include:

### Configuration Integration

Verify:

- logging configuration read through Configuration Service
- configuration changes reflected during initialization
- defaults applied correctly

### File System

Verify:

- log directory creation
- platform.log creation
- error.log creation
- debug.log creation

### Logging Behaviour

Verify:

- INFO events
- DEBUG events
- ERROR events
- structured events
- exception events

### Rotation

Verify:

- rollover
- archive creation
- retained log count

### Thread Safety

Verify concurrent logging from multiple threads.

No corruption shall occur.

---

## 37. Test Directory Layout

Follow the repository conventions established by IWP-001.

Expected logical layout:

```text
tests/
├── unit/
│   └── logging/
│       test_service.py
│       test_logger.py
│       test_levels.py
│       test_handlers.py
│       test_formatter.py
│       test_redaction.py
│       test_correlation.py
│
└── integration/
    └── logging/
        test_logging_service.py
```

If different conventions already exist, follow those conventions.

---

## 38. Documentation

Every public API shall include:

- docstring
- parameter documentation
- return documentation
- raised exceptions

Internal implementation shall include concise documentation where complexity warrants.

Public documentation shall be sufficient for future platform services to consume the Logging Service without requiring implementation knowledge.

---

## 39. Python Standards

Implementation shall comply with ARP-001-05.

Requirements include:

- Python 3.11+
- full type hints
- immutable models where appropriate
- descriptive naming
- no wildcard imports
- no hidden mutable globals
- deterministic behaviour
- repository formatting conventions

The implementation shall use the Python standard `logging` module as the underlying logging engine unless the approved architecture explicitly specifies otherwise.

No third-party logging frameworks shall be introduced.

---


## 40. Security Requirements

The Logging Service shall be implemented with security as a primary design consideration.

The service shall never:

- log plaintext passwords
- log API keys
- log authentication tokens
- log bearer tokens
- log private keys
- log certificates
- log decrypted secrets
- log personally identifiable information where explicitly marked as sensitive
- expose internal stack traces to end users unless debug mode is enabled

Sensitive values shall always be redacted before any formatter or handler writes the event.

Redaction shall be deterministic and consistently applied across:

- console output
- platform log
- error log
- debug log

---

## 41. Platform Integration Requirements

This package shall integrate cleanly with the existing implementation baseline.

### Integration with IWP-001

Reuse:

- package structure
- dependency management
- repository conventions
- build configuration
- test conventions
- quality tooling

Do not modify Repository Foundation behaviour.

---

### Integration with IWP-002

The Logging Service shall consume configuration exclusively through the Configuration Service.

Logging configuration shall never be duplicated.

Configuration values shall be treated as immutable after service initialization unless future lifecycle services trigger a controlled reload.

---

### Future Integration

The Logging Service shall expose interfaces suitable for future integration with:

| Future Package | Expected Integration |
|----------------|----------------------|
| IWP-004 | Bootstrap initialization |
| IWP-005 | Service Registry registration |
| IWP-006 | Lifecycle notifications |
| IWP-007 | CLI command logging |
| IWP-008 | Validation diagnostics |
| IWP-009 | Repository activity logging |
| IWP-010 | Prompt execution logging |
| IWP-011 | AI provider diagnostics |
| IWP-012 | MCP communication logging |
| IWP-013 | Health reporting |
| IWP-014 | Event Bus event tracing |
| IWP-015 | Plugin diagnostics |
| IWP-016 | Context correlation |
| IWP-017 | End-to-end platform tracing |

This work package shall not implement any of the above integrations.

It shall only expose the interfaces required to support them.

---

## 42. Dependency Injection

The Logging Service shall support constructor-based dependency injection.

The Configuration Service shall be supplied through dependency injection.

Example:

```python
logging_service = LoggingService(configuration_service)
```

The Logging Service shall not instantiate the Configuration Service internally.

This preserves service independence and supports future Service Registry integration.

---

## 43. Public API Stability

The public Logging Service interface defined by ARP-002 shall be considered stable.

Cursor shall not introduce:

- convenience APIs not defined by architecture
- alternate initialization paths
- multiple logger factories
- alternate logger implementations

Internal helper classes may evolve provided the public interface remains stable.

---

## 44. Repository Quality Requirements

Before completion Cursor shall execute all repository quality gates established by IWP-001.

At minimum this normally includes:

```bash
ruff check .

ruff format --check .

mypy .

pytest
```

If additional repository quality gates exist they shall also be executed.

No quality gate failures are permitted.

---

## 45. Acceptance Criteria

IWP-003 shall be considered complete only when all of the following are true.

### Functional

- Logging Service implemented.
- Public service contract implemented.
- PlatformLogger implemented.
- Console logging operational.
- File logging operational.
- Error logging operational.
- Debug logging operational.
- Structured logging implemented.
- Correlation identifiers implemented.
- Secret redaction implemented.
- Rotation implemented.
- Shutdown implemented.

### Architectural

- No architecture redesign.
- No public contract changes.
- Configuration consumed through Configuration Service.
- Package dependency rules satisfied.
- Platform Service Contract Compliance Matrix satisfied.
- Thread safety requirements satisfied.

### Engineering

- Complete type hints.
- Public API documentation.
- Unit tests implemented.
- Integration tests implemented.
- Repository quality gates pass.
- Existing IWP-001 and IWP-002 tests continue to pass.

---

## 46. Deliverables

This implementation package shall produce:

- Logging Service implementation
- PlatformLogger implementation
- formatter implementations
- handler implementations
- correlation support
- redaction support
- logging models
- exception hierarchy
- unit tests
- integration tests
- package exports
- documentation updates where required

No unrelated services shall be implemented.

---

## 47. Files Expected to Change

Exact files depend upon the implementation baseline.

Expected changes include:

```text
src/vibe/logging/
tests/unit/logging/
tests/integration/logging/
```

Potential updates may also include:

```text
pyproject.toml
README.md
src/vibe/__init__.py
```

Only where required by the approved architecture or repository conventions.

---

## 48. Commit Guidance

Create a single focused implementation commit.

Suggested commit message:

```text
feat(logging): implement IWP-003 Logging Service
```

Do not combine this work package with unrelated refactoring or cleanup.

Do not include generated files.

Do not include IDE metadata.

Do not include local configuration.

---

## 49. Completion Report

Upon completion Cursor shall provide the following report.

```text
IWP-003 Completion Report

Implementation Summary
----------------------
Brief description of the completed Logging Service.

Architecture Compliance
-----------------------
- ARP-002-03 Logging Service Interface Contract: PASS
- ARP-002-16 Platform Service Contract Compliance Matrix: PASS
- ARP-001-05 Engineering Coding Standards: PASS

Repository Impact
-----------------
Files Added:
- ...

Files Modified:
- ...

Files Removed:
- None (unless explicitly justified)

Testing
-------
Unit Tests:
- Passed

Integration Tests:
- Passed

Quality Checks:
- Ruff: PASS
- MyPy: PASS
- PyTest: PASS

Implementation Notes
--------------------
Document any implementation assumptions that were explicitly permitted by the approved architecture.

Known Limitations
-----------------
None unless explicitly identified.

Ready For Review
----------------
YES
```

---

## 50. Definition of Done

This implementation package is complete when:

- every acceptance criterion has been satisfied
- all quality gates pass
- all unit tests pass
- all integration tests pass
- existing implementation baseline tests continue to pass
- implementation conforms to the approved AP and ARP documents
- implementation introduces no architectural drift
- implementation is suitable for review and merge into the `86-vibe-cli` repository

No functionality beyond the scope of IWP-003 shall be implemented.

---

## 51. Cursor Execution Instructions

Before writing code, Cursor shall:

1. Inspect the current implementation baseline (IWP-001 and IWP-002).
2. Review the governing architecture documents referenced by this specification.
3. Confirm package locations and repository conventions.
4. Confirm the Configuration Service public interface before integrating.
5. Implement the Logging Service strictly within the scope defined by this document.
6. Execute all repository quality gates.
7. Produce the required completion report.
8. Stop.

Do **not** begin IWP-004.

---
