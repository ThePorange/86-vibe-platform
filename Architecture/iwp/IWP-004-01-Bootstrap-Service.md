Filename: IWP-004-01-Bootstrap-Service.md

IWP-004-01 – Bootstrap Service Implementation Specification

Document Control

Item	Value
Document ID	IWP-004-01
Document Name	Bootstrap Service Implementation Specification
Implementation Package	IWP-004 – Bootstrap Service
Platform	86-vibe
Implementation Repository	86-vibe-cli
Architecture Repository	86-vibe-platform
Status	Implementation Ready
Owner	Chief Systems Architect
Implementation Agent	Cursor
Depends On	IWP-001 – Repository FoundationIWP-002 – Configuration ServiceIWP-003 – Logging Service
Supersedes	None

⸻

1. Purpose

This document defines the implementation requirements for the Bootstrap Service.

The Bootstrap Service is responsible for bringing the 86-vibe platform into a fully operational state.

It owns platform startup.

It does not own service discovery, dependency management, lifecycle orchestration, CLI execution, or health monitoring. Those capabilities are implemented in later work packages.

The Bootstrap Service shall initialise only those services that currently exist within the implementation baseline.

⸻

2. Implementation Objective

Implement the Bootstrap Service in accordance with:

* AP-002
* ARP-002-04 – Bootstrap Service Interface Contract
* AEP-002-16 – Platform Bootstrap Sequence Specification
* AEP-002-17 – Service Lifecycle & Dependency Management Specification
* ARP-002-16 – Platform Service Contract Compliance Matrix
* ARP-001-05 – Engineering Coding Standards

The implementation shall provide deterministic platform startup and shutdown while remaining fully compliant with the approved architecture.

⸻

3. Current Implementation Baseline

The following work packages are complete and immutable:

* IWP-001 Repository Foundation
* IWP-002 Configuration Service
* IWP-003 Logging Service

Bootstrap shall integrate only with these implemented services.

It shall not assume the existence of:

* Service Registry
* Lifecycle Manager
* CLI Framework
* Repository Service
* Event Bus
* Plugin Manager
* AI Provider Layer
* MCP Client
* Health Monitoring
* Platform Context

⸻

4. Responsibilities

The Bootstrap Service is responsible for:

* platform startup
* platform shutdown
* construction of implemented services
* dependency ordering
* startup validation
* startup failure handling
* shutdown ordering
* exposing bootstrap state

It is not responsible for:

* service registration
* dependency graph resolution
* lifecycle policies
* repository discovery
* command execution
* plugin loading
* AI provider selection
* MCP communication

⸻

5. Architectural Constraints

Cursor shall not redesign startup behaviour.

Cursor shall not introduce dependency injection frameworks.

Cursor shall not introduce IoC containers.

Cursor shall not introduce global service locators.

Cursor shall not introduce asynchronous startup.

Cursor shall not create background threads.

Cursor shall not implement future lifecycle behaviour.

⸻

6. Public Service

The service identity shall conform to ARP-002.

Property	Value
Service Name	Bootstrap Service
Lifetime	Singleton
Thread Safe	Yes
Public	Yes

Primary implementation class:

BootstrapService

⸻

7. Package Layout

Follow the package conventions established by IWP-001.

Expected logical layout:

src/
    vibe/
        bootstrap/
            __init__.py
            service.py
            state.py
            exceptions.py
            models.py

Reuse existing repository conventions where they differ.

Do not duplicate functionality.

⸻

8. Bootstrap States

Implement a deterministic bootstrap state machine.

States:

NOT_STARTED
↓
INITIALISING
↓
RUNNING
↓
SHUTTING_DOWN
↓
STOPPED

Error state:

FAILED

State transitions shall be deterministic.

Invalid transitions shall raise BootstrapStateError.

⸻

9. Startup Sequence

Startup shall occur in the following order.

Step 1

Create Bootstrap Service.

Step 2

Construct Configuration Service.

Step 3

Initialise Configuration Service.

Step 4

Construct Logging Service.

Step 5

Initialise Logging Service.

Step 6

Emit startup diagnostics.

Step 7

Transition platform to RUNNING.

No additional services shall be initialised.

⸻

10. Shutdown Sequence

Shutdown shall occur in reverse order.

Shutdown order:

Logging Service
↓
Configuration Service
↓
Bootstrap Service

Shutdown shall continue where safe if one service fails.

Errors shall be logged.

Shutdown shall leave the platform in STOPPED state.

⸻

11. Startup Validation

Bootstrap shall validate:

* Configuration Service initialised successfully
* Logging Service initialised successfully
* bootstrap state transitions
* dependency ordering

Bootstrap shall not perform repository validation.

Bootstrap shall not perform environment validation.

Those belong to future work packages.

⸻

12. Public API

The Bootstrap Service shall expose functionality equivalent to:

class BootstrapService:
    def initialize(self) -> None:
        ...
    def shutdown(self) -> None:
        ...
    @property
    def state(self):
        ...
    @property
    def is_running(self):
        ...
    @property
    def configuration_service(self):
        ...
    @property
    def logging_service(self):
        ...

Method names may differ only where required by repository conventions.

⸻

13. Bootstrap Result

Successful initialization shall produce an immutable bootstrap result containing:

* bootstrap state
* configuration service reference
* logging service reference
* startup timestamp

Future services shall not appear in this model.

⸻

14. Error Handling

Expected exception hierarchy:

BootstrapError
├── BootstrapInitializationError
├── BootstrapShutdownError
└── BootstrapStateError

Reuse the common platform exception hierarchy if it already exists.

Errors shall contain sufficient diagnostic information.

Secrets shall never be exposed.

⸻

15. Logging

The Bootstrap Service shall use the Logging Service implemented in IWP-003.

Bootstrap shall not:

* call print()
* create independent loggers
* bypass the Logging Service

Startup shall log:

* startup initiated
* configuration loaded
* logging initialised
* platform running

Shutdown shall log:

* shutdown initiated
* logging shutdown
* configuration shutdown
* bootstrap complete

⸻

16. Thread Safety

Bootstrap initialization shall be protected against concurrent execution.

Only one initialization sequence may execute at a time.

Repeated initialization after successful startup shall return successfully without reinitializing services unless the approved architecture specifies otherwise.

Shutdown shall likewise be safe when called multiple times.

⸻

17. Internal Modules

service.py

Responsible for:

* startup orchestration
* shutdown orchestration
* state transitions
* service ownership

state.py

Responsible for:

* bootstrap state enumeration
* transition validation

models.py

Responsible for:

* immutable bootstrap result model

exceptions.py

Responsible for:

* bootstrap exception hierarchy

⸻

18. Dependency Rules

Bootstrap may depend only upon:

* Configuration Service
* Logging Service

Bootstrap shall not reference future platform services.

Bootstrap shall expose extension points required for future Service Registry integration but shall not implement them.

⸻


# 19. Configuration Service Integration

The Bootstrap Service shall construct and initialize the Configuration Service according to the existing IWP-002 implementation baseline.

Bootstrap shall not bypass Configuration Service public APIs.

Bootstrap shall not read configuration files directly.

Bootstrap shall not parse environment variables directly.

Bootstrap shall not duplicate configuration logic.

The Configuration Service shall remain the sole authority for effective platform configuration.

If the IWP-002 implementation requires constructor arguments, file paths, runtime overrides, or initialization options, Bootstrap shall supply them through the approved public API only.

---

# 20. Logging Service Integration

The Bootstrap Service shall construct and initialize the Logging Service according to the existing IWP-003 implementation baseline.

The Logging Service shall receive the Configuration Service through dependency injection.

Expected conceptual flow:

```python
configuration_service = ConfigurationService(...)
configuration_service.initialize()

logging_service = LoggingService(configuration_service)
logging_service.initialize()
```

Use actual IWP-002 and IWP-003 constructor signatures.

Do not modify the public Configuration Service or Logging Service APIs to fit Bootstrap unless a defect is found and documented.

---

# 21. Startup Diagnostics

After Logging Service initialization, Bootstrap shall emit startup diagnostics using the platform logger.

The logger name shall be deterministic.

Preferred logger name:

```text
bootstrap
```

Startup diagnostics shall include:

- bootstrap start
- configuration service initialized
- logging service initialized
- platform running

Diagnostics shall not include secrets.

Diagnostics shall not include full local filesystem paths unless required for troubleshooting and already permitted by existing logging rules.

---

# 22. Failure Handling During Startup

If startup fails before Logging Service initialization:

- Bootstrap shall raise a BootstrapInitializationError.
- Bootstrap shall not call print().
- Bootstrap shall avoid swallowing the original exception.
- Bootstrap shall preserve the original exception as the cause.

If startup fails after Logging Service initialization:

- Bootstrap shall log the failure.
- Bootstrap shall attempt safe shutdown of already initialized services.
- Bootstrap shall raise BootstrapInitializationError.
- Bootstrap shall preserve the original exception as the cause.

Partial startup shall never result in RUNNING state.

Failure state shall be deterministic.

---

# 23. Failure Handling During Shutdown

Shutdown shall attempt to stop initialized services in reverse order.

If one shutdown operation fails:

- record the failure
- continue shutting down remaining initialized services where safe
- log failures when Logging Service is still available
- raise BootstrapShutdownError after shutdown attempts complete

Shutdown failures shall not leave service-owned resources open where avoidable.

The final state after shutdown attempts shall be:

- STOPPED if all shutdown operations succeed
- FAILED if one or more shutdown operations fail and the architecture requires explicit failure state

If the architecture or existing state model does not define a FAILED shutdown terminal state, use FAILED consistently as the error state already defined in this specification.

---

# 24. Idempotency

Bootstrap lifecycle operations shall be idempotent where safe.

## initialize()

If called when already RUNNING:

- do not recreate Configuration Service
- do not recreate Logging Service
- return successfully

If called when INITIALISING:

- raise BootstrapStateError or otherwise prevent concurrent duplicate initialization

If called after FAILED:

- do not retry automatically unless explicitly supported by architecture
- raise BootstrapStateError explaining the invalid transition

## shutdown()

If called when NOT_STARTED:

- return successfully or transition directly to STOPPED according to state implementation

If called when STOPPED:

- return successfully

If called when SHUTTING_DOWN:

- prevent concurrent duplicate shutdown

---

# 25. State Transition Rules

State transitions shall be validated centrally.

Valid transitions:

```text
NOT_STARTED -> INITIALISING
INITIALISING -> RUNNING
INITIALISING -> FAILED
RUNNING -> SHUTTING_DOWN
SHUTTING_DOWN -> STOPPED
SHUTTING_DOWN -> FAILED
FAILED -> SHUTTING_DOWN
FAILED -> STOPPED
```

No other transition shall occur silently.

Invalid transitions shall raise BootstrapStateError.

State changes shall be atomic and thread-safe.

---

# 26. Bootstrap Ownership Model

For IWP-004, Bootstrap owns the services it constructs:

- Configuration Service
- Logging Service

Ownership means Bootstrap is responsible for:

- construction
- initialization
- exposing references
- shutdown

This ownership model is temporary and must remain compatible with future Service Registry and Lifecycle Manager work packages.

Do not implement the future Service Registry in IWP-004.

Do not implement dynamic service registration.

---

# 27. Service Accessors

Bootstrap shall expose read-only accessors for implemented services.

Required accessors:

```python
@property
def configuration_service(self):
    ...

@property
def logging_service(self):
    ...
```

Accessors shall:

- return initialized service references after successful bootstrap
- raise BootstrapStateError if accessed before initialization where appropriate
- never create services lazily
- never return partially initialized services

---

# 28. Bootstrap Result Model

Implement an immutable model representing successful bootstrap.

Expected fields:

```python
@dataclass(frozen=True)
class BootstrapResult:
    state: BootstrapState
    started_at: datetime
    configuration_service: ConfigurationService
    logging_service: LoggingService
```

Use actual imported types where available.

The model shall not contain future services.

The model shall not expose mutable internals.

---

# 29. Time Handling

Bootstrap timestamps shall use timezone-aware UTC datetimes.

Use:

```python
datetime.now(timezone.utc)
```

Do not use naive datetimes.

Do not use local time.

---

# 30. Testability

The Bootstrap Service shall be testable without invoking real file system side effects beyond those already required by Configuration and Logging integration tests.

Use dependency injection or constructor parameters where compatible with architecture to allow:

- fake Configuration Service
- fake Logging Service
- temporary configuration paths
- temporary log directories

Do not introduce a dependency injection framework.

Simple constructor injection is sufficient.

---

# 31. Constructor Design

The Bootstrap Service constructor shall support real platform operation and testability.

Recommended conceptual design:

```python
class BootstrapService:
    def __init__(
        self,
        configuration_service: ConfigurationService | None = None,
        logging_service: LoggingService | None = None,
    ) -> None:
        ...
```

If existing architecture or implementation baseline defines a different factory pattern, follow the baseline.

Constructor injection may be used for tests.

When services are injected:

- Bootstrap shall not reconstruct them
- Bootstrap shall still own lifecycle invocation unless explicitly configured otherwise
- injected services shall be initialized in the correct order if not already initialized

Do not create service locator behavior.

---

# 32. Public Package Exports

The package shall export only stable public symbols.

Expected exports:

```python
from vibe.bootstrap.service import BootstrapService
from vibe.bootstrap.state import BootstrapState
from vibe.bootstrap.models import BootstrapResult
from vibe.bootstrap.exceptions import (
    BootstrapError,
    BootstrapInitializationError,
    BootstrapShutdownError,
    BootstrapStateError,
)
```

Do not export internal helpers.

---

# 33. Unit Testing Requirements

Cursor shall implement unit tests for Bootstrap Service behavior.

Minimum unit test coverage:

## State Machine

- valid transitions
- invalid transitions
- FAILED transitions
- STOPPED transitions

## Initialization

- successful initialization
- initialization order
- repeated initialization after RUNNING
- initialization failure before logging exists
- initialization failure after logging exists
- state after initialization failure

## Shutdown

- successful shutdown
- shutdown order
- repeated shutdown
- shutdown before startup
- shutdown failure in Logging Service
- shutdown failure in Configuration Service
- state after shutdown failure

## Accessors

- access before initialization
- access after initialization
- no lazy construction from accessors
- returns expected service objects

## Bootstrap Result

- result created after successful startup
- result uses timezone-aware UTC timestamp
- result is immutable
- result contains only implemented services

## Thread Safety

- concurrent initialize calls
- concurrent shutdown calls
- initialize while shutdown is running where practical

---

# 34. Integration Testing Requirements

Cursor shall implement integration tests covering Bootstrap with real implemented services.

Integration tests shall verify:

- Bootstrap initializes real Configuration Service
- Bootstrap initializes real Logging Service
- Bootstrap creates usable platform logger
- Bootstrap state becomes RUNNING
- Bootstrap shutdown succeeds
- Bootstrap state becomes STOPPED
- existing Configuration Service tests still pass
- existing Logging Service tests still pass

Integration tests shall use temporary directories and shall not write to the developer’s real home directory or repository state unless intentionally scoped to a temporary test repository.

---

# 35. Test Directory Layout

Follow the repository conventions established by IWP-001.

Expected logical layout:

```text
tests/
├── unit/
│   └── bootstrap/
│       test_state.py
│       test_service.py
│       test_models.py
│
└── integration/
    └── bootstrap/
        test_bootstrap_service.py
```

If the repository uses a different convention, follow the existing convention.

---


# 36. Logging Requirements

Bootstrap shall use the Logging Service after it has been initialized.

Bootstrap logging shall include:

- initialization started
- configuration initialized
- logging initialized
- bootstrap running
- shutdown started
- service shutdown success
- service shutdown failure
- shutdown complete
- bootstrap failure

Bootstrap shall not log secrets.

Bootstrap shall not log raw configuration content.

Bootstrap shall not log raw exception stack traces directly; exception formatting belongs to the Logging Service.

---

# 37. Error and Exception Requirements

Bootstrap exceptions shall be deterministic and specific.

Expected hierarchy:

```python
class BootstrapError(Exception):
    ...

class BootstrapInitializationError(BootstrapError):
    ...

class BootstrapShutdownError(BootstrapError):
    ...

class BootstrapStateError(BootstrapError):
    ...
```

If the repository already defines a platform base exception, BootstrapError shall inherit from it.

Exceptions shall:

- include clear messages
- preserve original exception causes using exception chaining
- avoid leaking secrets
- avoid hiding underlying implementation errors

Example:

```python
try:
    configuration_service.initialize()
except Exception as exc:
    raise BootstrapInitializationError(
        "Failed to initialize Configuration Service"
    ) from exc
```

Do not use broad exception handling except at lifecycle boundaries where required to convert errors into Bootstrap-specific exceptions.

---

# 38. Concurrency Requirements

Bootstrap shall protect lifecycle transitions using an appropriate synchronization primitive.

Preferred approach:

```python
threading.RLock
```

or equivalent.

Concurrent lifecycle calls shall not result in:

- duplicate service construction
- duplicate service initialization
- partially initialized service exposure
- invalid state transitions
- corrupted bootstrap result

---

# 39. Lifecycle Compatibility

Although IWP-006 will implement the Service Lifecycle Manager, Bootstrap shall expose lifecycle behavior compatible with future integration.

This means:

- initialize() performs startup
- shutdown() performs shutdown
- state can be inspected
- lifecycle failures are explicit
- services are initialized in dependency order
- services are shut down in reverse dependency order

Do not implement the Lifecycle Manager in this package.

---

# 40. Service Registry Compatibility

Although IWP-005 will implement the Service Registry, Bootstrap shall remain compatible with future registration.

This means:

- keep service construction isolated and explicit
- do not hard-code future service discovery behavior
- do not create global registries
- do not introduce singleton module globals
- keep constructed service references accessible through Bootstrap

Do not implement Service Registry in this package.

---

# 41. Configuration Requirements

Bootstrap may accept runtime configuration options only where required to initialize the Configuration Service.

Bootstrap shall not define independent platform configuration.

Bootstrap shall not duplicate Configuration Service defaults.

Bootstrap shall not read `.86vibe/config.yaml` directly.

Any bootstrap options passed to Configuration Service shall be passed through approved public constructors or methods.

---

# 42. File System Requirements

Bootstrap itself shall not create platform directories except where delegated to implemented services.

Directory creation remains the responsibility of:

- Configuration Service where configuration requires it
- Logging Service where logging requires it
- future Repository Service where repository structure requires it

Bootstrap shall not create `.86vibe` directly unless the existing implementation baseline already establishes that responsibility.

---

# 43. Documentation Requirements

Every public Bootstrap API shall include docstrings.

Required documentation:

- purpose
- parameters
- return values
- raised exceptions
- lifecycle behavior
- thread-safety expectations

Internal code shall include concise comments only where they clarify non-obvious lifecycle behavior.

---

# 44. Python Standards

Implementation shall comply with ARP-001-05.

Required standards include:

- Python 3.11+
- full type hints
- docstrings for public APIs
- dataclasses for immutable models
- enums for state definitions
- no wildcard imports
- no hidden mutable globals
- deterministic behavior
- small focused functions
- explicit exceptions
- repository formatting conventions

---

# 45. Quality Gates

Before completion, Cursor shall run all repository quality gates established by IWP-001.

At minimum, run the equivalent of:

```bash
ruff check .

ruff format --check .

mypy .

pytest
```

If the repository defines additional quality commands, run those as well.

No quality gate failures are permitted.

---

# 46. Acceptance Criteria

IWP-004 shall be considered complete only when all of the following criteria are met.

## Functional Acceptance Criteria

- Bootstrap Service implemented.
- Bootstrap state machine implemented.
- Bootstrap initializes Configuration Service.
- Bootstrap initializes Logging Service.
- Bootstrap exposes initialized service references.
- Bootstrap emits startup diagnostics after logging initialization.
- Bootstrap shuts services down in reverse order.
- Bootstrap supports idempotent initialize and shutdown behavior.
- Bootstrap handles startup failure deterministically.
- Bootstrap handles shutdown failure deterministically.
- Bootstrap result is immutable.
- Bootstrap timestamps are timezone-aware UTC.

## Architectural Acceptance Criteria

- No architecture redesign introduced.
- No future services implemented.
- No Service Registry implemented.
- No Lifecycle Manager implemented.
- No CLI Framework implemented.
- Bootstrap depends only on Configuration Service and Logging Service.
- Bootstrap follows approved startup sequence.
- Bootstrap follows approved shutdown sequence.
- Package dependency rules are preserved.
- Public service contracts remain stable.

## Engineering Acceptance Criteria

- Public APIs have type hints.
- Public APIs have docstrings.
- Bootstrap exceptions implemented.
- Unit tests implemented.
- Integration tests implemented.
- Existing IWP-001 tests pass.
- Existing IWP-002 tests pass.
- Existing IWP-003 tests pass.
- Repository quality gates pass.

---

# 47. Deliverables

This implementation package shall deliver:

- Bootstrap Service package
- Bootstrap state enum
- Bootstrap result model
- Bootstrap exception hierarchy
- startup orchestration logic
- shutdown orchestration logic
- unit tests
- integration tests
- public package exports
- documentation updates where required

No unrelated functionality shall be delivered.

---

# 48. Files Expected to Change

Exact files depend on the current implementation baseline.

Expected additions or modifications include:

```text
src/vibe/bootstrap/
tests/unit/bootstrap/
tests/integration/bootstrap/
```

Potential updates may include:

```text
src/vibe/__init__.py
README.md
pyproject.toml
```

Only update these files if required by existing repository conventions or quality gates.

---

# 49. Commit Guidance

Create a single focused implementation commit.

Suggested commit message:

```text
feat(bootstrap): implement IWP-004 Bootstrap Service
```

Do not combine this work with unrelated refactoring.

Do not include generated files.

Do not include IDE metadata.

Do not include local configuration files.

---

# 50. Completion Report

Upon completion, Cursor shall provide the following report.

```text
IWP-004 Completion Report

Implementation Summary
----------------------
Brief description of the completed Bootstrap Service.

Architecture Compliance
-----------------------
- ARP-002-04 Bootstrap Service Interface Contract: PASS
- AEP-002-16 Platform Bootstrap Sequence Specification: PASS
- AEP-002-17 Service Lifecycle & Dependency Management Specification: PASS
- ARP-002-16 Platform Service Contract Compliance Matrix: PASS
- ARP-001-05 Engineering Coding Standards: PASS

Repository Impact
-----------------
Files Added:
- ...

Files Modified:
- ...

Files Removed:
- None unless explicitly justified

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
Document any assumptions made that were explicitly permitted by the approved architecture.

Known Limitations
-----------------
None unless explicitly identified.

Ready For Review
----------------
YES
```

---

# 51. Definition of Done

This implementation package is complete when:

- all functional acceptance criteria are satisfied
- all architectural acceptance criteria are satisfied
- all engineering acceptance criteria are satisfied
- all unit tests pass
- all integration tests pass
- all existing baseline tests pass
- all repository quality gates pass
- no architectural drift has been introduced
- implementation is ready for review and merge into `86-vibe-cli`

No functionality beyond IWP-004 shall be implemented.

---

# 52. Cursor Execution Instructions

Before writing code, Cursor shall:

1. Inspect the current implementation baseline.
2. Confirm the public Configuration Service API.
3. Confirm the public Logging Service API.
4. Confirm repository package conventions.
5. Implement Bootstrap Service strictly within this specification.
6. Run all repository quality gates.
7. Produce the required completion report.
8. Stop.

Do **not** begin IWP-005.

---
