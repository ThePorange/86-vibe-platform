Filename: IWP-006-01-Service-Lifecycle-Manager.md

IWP-006-01 – Service Lifecycle Manager Implementation Specification

Document Control

Item	Value
Document ID	IWP-006-01
Document Name	Service Lifecycle Manager Implementation Specification
Implementation Package	IWP-006 – Service Lifecycle Manager
Platform	86-vibe
Implementation Repository	86-vibe-cli
Architecture Repository	86-vibe-platform
Status	Implementation Ready
Owner	Chief Systems Architect
Implementation Agent	Cursor
Depends On	IWP-001 – Repository FoundationIWP-002 – Configuration ServiceIWP-003 – Logging ServiceIWP-004 – Bootstrap ServiceIWP-005 – Service Registry
Supersedes	None

⸻

1. Purpose

This document defines the complete implementation requirements for IWP-006 – Service Lifecycle Manager.

The Service Lifecycle Manager is the authoritative runtime component responsible for governing, recording, validating, and exposing the operational lifecycle of platform services after bootstrap has completed.

It shall provide deterministic lifecycle state management, dependency awareness, lifecycle event publication, service status inspection, health integration primitives, and graceful shutdown coordination.

The Service Lifecycle Manager shall not replace the Bootstrap Service or Service Registry.

Cursor shall treat this document as the complete implementation prompt.

Nothing required for implementation shall exist outside this Markdown document except:

* the approved architecture repository
* the existing 86-vibe-cli implementation baseline
* this implementation specification

⸻

2. Implementation Objective

Implement the Service Lifecycle Manager in the 86-vibe-cli repository.

The implementation shall satisfy the public service contract defined by:

* ARP-002-10 – Service Lifecycle Manager Interface Contract
* AEP-002-17 – Service Lifecycle & Dependency Management Specification
* AEP-002-16 – Platform Bootstrap Sequence Specification
* AEP-002-18 – Platform Reference Implementation Specification
* ARP-002-11 – Service Registry Interface Contract
* ARP-002-16 – Platform Service Contract Compliance Matrix
* ARP-001-03 – Package Dependency Matrix
* ARP-001-05 – Engineering Coding Standards

The implementation shall provide:

* managed service registration
* lifecycle state tracking
* transition validation
* dependency metadata
* lifecycle event delivery
* service status inspection
* health status recording
* dependency-aware shutdown sequencing
* deterministic failure handling
* thread-safe concurrent access

This package shall not implement unrelated platform services.

⸻

3. Current Implementation Baseline

The following implementation work packages have already been built, approved, committed, and are now immutable:

* IWP-001 – Repository Foundation
* IWP-002 – Configuration Service
* IWP-003 – Logging Service
* IWP-004 – Bootstrap Service
* IWP-005 – Service Registry

Cursor shall:

1. Inspect the current 86-vibe-cli repository before implementing.
2. Preserve all established repository, typing, testing, and package conventions.
3. Reuse the public interfaces of all existing services.
4. Integrate with the existing Service Registry rather than creating another registry.
5. Preserve Bootstrap ownership of service construction and initialization.
6. Avoid modifying previous implementation packages unless correcting a defect directly blocking IWP-006.
7. Document any such defect correction in the completion report.

⸻

4. Strict Scope

This work package implements only:

IWP-006 – Service Lifecycle Manager

The Service Lifecycle Manager shall provide:

* lifecycle manager initialization
* managed service registration
* managed service unregistration
* service lifecycle state management
* legal transition validation
* service dependency metadata
* lifecycle event publication
* service status lookup
* service listing
* health status recording
* platform operational status
* dependency-aware shutdown planning
* orderly service shutdown coordination
* thread-safe access

The Service Lifecycle Manager shall not implement:

* service construction
* initial platform bootstrap
* repository operations
* CLI command routing
* architecture validation
* AI inference
* MCP communication
* plugin loading
* event-bus infrastructure
* centralized active health probing
* platform context propagation

⸻

5. Explicitly Out of Scope

The following work packages remain out of scope:

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

Minimal test doubles may be implemented inside tests only.

Do not implement production placeholders for future services.

⸻

6. Architectural Non-Negotiables

Cursor shall comply with the following rules.

6.1 No Architecture Redesign

Do not redesign architecture.

Do not reinterpret service ownership.

Do not introduce a second Service Registry.

Do not move initial service construction out of Bootstrap.

Do not allow the Lifecycle Manager to initialize services directly.

Do not introduce a third-party dependency injection framework.

Do not introduce a global service locator.

Do not implement asynchronous lifecycle orchestration.

Do not create background monitoring threads.

Do not implement the future Event Bus.

Do not implement the future Health Monitoring Service.

6.2 Stop Conditions

Cursor shall stop and report an ambiguity instead of inventing a solution if:

1. The approved architecture conflicts with the committed IWP-001 through IWP-005 baseline.
2. Bootstrap integration requires changing an approved public interface in an unapproved manner.
3. The Service Registry cannot expose the service instances or metadata required by the Lifecycle Manager.
4. A dependency cycle exists among implemented services.
5. A required lifecycle state cannot be reconciled between ARP-002-10 and existing service state models.
6. Shutdown coordination requires lifecycle behavior not exposed by an implemented public service.
7. The implementation requires an unapproved external dependency.
8. Thread safety cannot be maintained using the existing repository approach.

Do not silently resolve architectural ambiguity.

⸻

7. Service Identity

The Service Lifecycle Manager shall conform to ARP-002-10.

Property	Required Value
Service Name	Service Lifecycle Manager
Package	vibe.lifecycle
Primary Class	ServiceLifecycleManager
Lifetime	Singleton
Thread Safety	Required
Public	Yes

Do not create multiple independent lifecycle managers.

Do not store the authoritative lifecycle state in module-level mutable globals.

⸻

8. Responsibility Boundaries

The platform services shall retain the following distinct responsibilities.

8.1 Bootstrap Service

Bootstrap remains responsible for:

* constructing platform services
* resolving startup prerequisites
* initializing services
* startup sequencing
* publishing initial platform readiness
* initiating platform shutdown

8.2 Service Registry

Service Registry remains responsible for:

* storing runtime service instances
* deterministic service lookup
* registration identity
* registry integrity
* exposing immutable registration metadata

8.3 Service Lifecycle Manager

Service Lifecycle Manager is responsible for:

* maintaining authoritative lifecycle records
* validating lifecycle transitions
* recording service dependencies
* recording service health state
* publishing lifecycle events through its own contract
* exposing operational service status
* coordinating dependency-aware shutdown after Bootstrap initiates shutdown

The Lifecycle Manager shall not duplicate Service Registry storage or lookup behavior.

⸻

9. Dependency Contract

Per ARP-002-10, the Service Lifecycle Manager shall depend upon:

* Configuration Service
* Logging Service
* Bootstrap Service

The existing Service Registry may also be injected and used as the authoritative service catalog, as explicitly permitted by ARP-002-11.

The Lifecycle Manager shall remain logically independent from the Registry.

This means:

* the Lifecycle Manager shall not subclass Service Registry
* the Lifecycle Manager shall not mutate Registry internals
* the Lifecycle Manager shall not replace Registry lookup semantics
* the Lifecycle Manager shall use only the Registry public interface
* lifecycle records shall remain distinct from registry records

The Lifecycle Manager shall not depend upon:

* Repository Service
* Prompt Management Service
* AI Provider Service
* MCP Client Service
* Validation Service
* Health Monitoring Service
* Event Bus Service
* Plugin Manager Service
* Platform Context Service

⸻

10. Bootstrap Integration

The Service Lifecycle Manager shall become active after successful platform bootstrap of the services required to construct it.

IWP-006 shall update Bootstrap only as necessary to:

1. construct the Lifecycle Manager
2. inject required existing services
3. initialize the Lifecycle Manager
4. register the Lifecycle Manager in the Service Registry
5. register existing services with the Lifecycle Manager
6. record their current lifecycle states
7. retain Bootstrap as the authority for initial service initialization

Bootstrap shall not delegate service construction to the Lifecycle Manager.

⸻

11. Updated Startup Sequence

The implementation baseline startup sequence shall be extended as follows:

Construct Bootstrap Service
Construct Service Registry
Construct Configuration Service
Initialize Configuration Service
Construct Logging Service
Initialize Logging Service
Register Service Registry
Register Bootstrap Service
Register Configuration Service
Register Logging Service
Construct Service Lifecycle Manager
Initialize Service Lifecycle Manager
Register Service Lifecycle Manager in Service Registry
Register existing managed services with Lifecycle Manager
Record current lifecycle states
Publish lifecycle management availability
Emit startup diagnostics
Transition platform to RUNNING

No future platform services shall be constructed.

⸻

12. Lifecycle Manager Public Interface

The implementation shall expose operations equivalent to:

class ServiceLifecycleManager:
    def initialize(self) -> None:
        ...
    def register(
        self,
        service: object,
        metadata: LifecycleServiceMetadata,
    ) -> None:
        ...
    def unregister(self, service_id: str) -> None:
        ...
    def get_service(self, service_id: str) -> ManagedServiceRecord:
        ...
    def list_services(self) -> tuple[ManagedServiceRecord, ...]:
        ...
    def transition(
        self,
        service_id: str,
        state: ServiceLifecycleState,
        *,
        message: str | None = None,
    ) -> LifecycleEvent:
        ...
    def status(self) -> PlatformLifecycleStatus:
        ...
    def health(self) -> PlatformHealthSummary:
        ...
    def shutdown(self) -> None:
        ...

Equivalent language-specific implementations are permitted where externally observable behavior remains identical.

The actual method names shall follow ARP-002-10 and existing repository conventions.

Every public API shall include:

* complete type hints
* docstrings
* deterministic behavior
* documented exceptions
* thread-safety guarantees where relevant

⸻

13. Lifecycle State Model

The Lifecycle Manager shall use the canonical managed-service states defined by ARP-002-10:

REGISTERED
INITIALIZING
READY
DEGRADED
STOPPING
STOPPED
FAILED

These are the authoritative states stored by the Lifecycle Manager.

AEP-002-17 also describes broader conceptual states such as Constructed, Configured, Initialized, Ready, Running, Stopping, Stopped, and Failed.

For IWP-006:

* REGISTERED represents a service accepted into lifecycle management
* INITIALIZING represents lifecycle initialization activity
* READY represents initialized and usable service state
* DEGRADED represents usable with reduced capability
* STOPPING represents shutdown in progress
* STOPPED represents shutdown complete
* FAILED represents initialization, execution, or shutdown failure

Do not introduce additional public lifecycle states without an ADR.

⸻

14. Legal State Transitions

The Lifecycle Manager shall validate all state changes centrally.

Required legal transitions:

REGISTERED -> INITIALIZING
REGISTERED -> READY
REGISTERED -> FAILED
INITIALIZING -> READY
INITIALIZING -> DEGRADED
INITIALIZING -> FAILED
INITIALIZING -> STOPPING
READY -> DEGRADED
READY -> STOPPING
READY -> FAILED
DEGRADED -> READY
DEGRADED -> STOPPING
DEGRADED -> FAILED
STOPPING -> STOPPED
STOPPING -> FAILED
FAILED -> STOPPING
FAILED -> STOPPED

STOPPED is terminal for a registration.

A stopped service shall not return to an active state without explicit unregistration and a new registration lifecycle.

Illegal transitions shall raise a lifecycle-specific exception.

No illegal transition shall mutate state.

⸻

15. Managed Service Registration

Each managed service registration shall include:

* unique service identifier
* service name
* service version
* current lifecycle state
* required dependencies
* optional dependencies
* supported capabilities
* health provider or health callback where available
* service instance reference or Service Registry identity
* registration timestamp

Duplicate service identifiers shall be rejected.

Registration shall be atomic.

Registration shall not initialize the service.

⸻

16. Managed Service Metadata

Implement an immutable metadata model equivalent to:

@dataclass(frozen=True)
class LifecycleServiceMetadata:
    service_id: str
    name: str
    version: str
    required_dependencies: tuple[str, ...] = ()
    optional_dependencies: tuple[str, ...] = ()
    capabilities: tuple[str, ...] = ()
    mandatory: bool = True

If the architecture or existing Service Registry metadata already provides equivalent fields, reuse or adapt that public model rather than duplicating conflicting data.

Lifecycle metadata shall remain immutable after successful registration.

⸻

17. Managed Service Record

Implement an immutable public record equivalent to:

@dataclass(frozen=True)
class ManagedServiceRecord:
    metadata: LifecycleServiceMetadata
    state: ServiceLifecycleState
    health: ServiceHealthState
    registered_at: datetime
    updated_at: datetime
    message: str | None = None

The manager may maintain mutable internal state, but public records shall be immutable snapshots.

Timestamps shall be timezone-aware UTC.

Use:

datetime.now(timezone.utc)

Do not use naive datetimes.

⸻

18. Service Health State

The Lifecycle Manager shall record service health independently from lifecycle state.

Minimum health states defined by AEP-002-17 are:

HEALTHY
DEGRADED
FAILED

To support incomplete assessment safely, an internal or public UNKNOWN state may be used only if already permitted by existing health contracts or repository conventions.

Lifecycle and health shall remain separate concepts.

Examples:

* a service may be READY and HEALTHY
* a service may be READY and DEGRADED
* a service may be FAILED and FAILED
* a newly registered service may not yet have a completed health assessment

IWP-006 shall record health state but shall not implement active centralized health polling.

Active health aggregation belongs to IWP-013.

⸻

19. Dependency Metadata

Every managed service shall explicitly declare:

* required dependencies
* optional dependencies

Hidden runtime dependencies shall not be permitted.

Dependency identifiers shall reference stable registered service identifiers.

Dependency metadata shall be validated at registration.

Validation shall include:

* no self-dependency
* no duplicate dependency identifiers
* no identifier listed as both required and optional
* valid identifier format
* dependency cycle detection where all referenced services are known

Unknown optional dependencies shall not prevent registration.

Unknown required dependencies shall be recorded but shall prevent transition to READY until satisfied.

⸻

20. Dependency Satisfaction

Before a managed service transitions to READY, the Lifecycle Manager shall verify that every required dependency:

* is registered
* is present in the Service Registry where applicable
* is not in FAILED, STOPPING, or STOPPED
* is in an acceptable operational state
* satisfies readiness requirements defined by the lifecycle contract

For IWP-006, an acceptable dependency state is:

READY

A dependency in DEGRADED shall not satisfy mandatory readiness unless explicitly allowed by metadata or architecture.

Optional dependency absence shall not prevent readiness.

⸻

21. Dependency Graph

The Lifecycle Manager shall maintain a deterministic logical dependency graph.

The graph shall support:

* readiness verification
* dependency status inspection
* shutdown ordering
* cycle detection
* operational diagnostics

The graph shall not be exposed as a mutable public object.

Public dependency inspection shall return immutable data.

No third-party graph library shall be introduced.

⸻

22. Cycle Detection

The manager shall reject dependency cycles.

Examples of invalid cycles:

service_a -> service_b -> service_a
service_a -> service_b -> service_c -> service_a

Cycle detection shall be deterministic.

A failed cycle validation shall not mutate registry or lifecycle state.

The implementation may use standard depth-first traversal or another deterministic standard-library algorithm.

⸻

23. Lifecycle Events

The manager shall publish lifecycle events through an internal synchronous lifecycle event interface.

Required event types:

* service registered
* service initializing
* service ready
* service degraded
* service stopping
* service stopped
* service failed
* service unregistered
* lifecycle manager initialized
* lifecycle shutdown started
* lifecycle shutdown completed

IWP-006 shall not implement the future Event Bus Service.

Lifecycle event delivery in this work package shall be:

* synchronous
* deterministic
* in-process
* thread-safe
* isolated from the future Event Bus contract

⸻

24. Lifecycle Event Model

Implement an immutable lifecycle event model equivalent to:

@dataclass(frozen=True)
class LifecycleEvent:
    event_type: LifecycleEventType
    service_id: str
    previous_state: ServiceLifecycleState | None
    current_state: ServiceLifecycleState
    timestamp: datetime
    message: str | None = None

Events shall use timezone-aware UTC timestamps.

Event payloads shall not contain secrets or mutable service internals.

⸻

# 25. Lifecycle Event Subscribers

The Service Lifecycle Manager shall support synchronous lifecycle event subscribers.

Subscribers shall be used internally by the platform until the Event Bus Service (IWP-014) is implemented.

The Lifecycle Manager shall expose operations equivalent to:

```python
subscribe(handler)

unsubscribe(handler)
```

Subscribers shall:

- execute synchronously
- execute in registration order
- not modify lifecycle state directly
- not prevent lifecycle transitions

If a subscriber raises an exception:

- the exception shall be logged
- remaining subscribers shall still execute
- lifecycle state shall remain consistent

The Lifecycle Manager shall not implement asynchronous event dispatch.

---

# 26. Platform Lifecycle Status

The Lifecycle Manager shall expose an immutable platform lifecycle summary.

Expected model:

```python
@dataclass(frozen=True)
class PlatformLifecycleStatus:
    state: PlatformState
    service_count: int
    ready_services: int
    degraded_services: int
    failed_services: int
    stopping_services: int
```

Platform state shall be derived from managed service state.

The Lifecycle Manager shall not maintain duplicate platform state.

---

# 27. Platform State Rules

Platform state shall be derived deterministically.

Examples:

| Condition | Platform State |
|-----------|----------------|
| All mandatory services READY | READY |
| One or more mandatory services INITIALIZING | INITIALIZING |
| One or more mandatory services DEGRADED | DEGRADED |
| One or more mandatory services FAILED | FAILED |
| Shutdown in progress | STOPPING |
| All services STOPPED | STOPPED |

No additional public platform states shall be introduced.

---

# 28. Health Summary

The Lifecycle Manager shall expose an immutable health summary.

Expected model:

```python
@dataclass(frozen=True)
class PlatformHealthSummary:
    healthy: int
    degraded: int
    failed: int
    unknown: int
```

Health summaries shall be computed from managed service records.

The Lifecycle Manager shall not actively poll services.

---

# 29. Shutdown Coordination

Bootstrap remains responsible for initiating shutdown.

The Lifecycle Manager shall coordinate shutdown order.

Shutdown ordering shall be dependency-aware.

Required algorithm:

1. determine dependency graph
2. identify reverse dependency order
3. request shutdown in dependency-safe order
4. update lifecycle state
5. publish lifecycle events
6. record failures

The Lifecycle Manager shall not destroy service instances.

---

# 30. Reverse Dependency Ordering

Example:

```text
Configuration

↓

Logging

↓

Lifecycle Manager
```

Shutdown shall occur in reverse dependency order.

Dependent services shall always stop before the services they depend upon.

The implementation shall calculate ordering dynamically from dependency metadata.

Hard-coded shutdown ordering is not permitted beyond the currently implemented services.

---

# 31. Failure Handling

Failures shall be deterministic.

Expected exception hierarchy:

```text
LifecycleManagerError

├── LifecycleRegistrationError

├── LifecycleTransitionError

├── DependencyValidationError

├── DependencyCycleError

├── LifecycleShutdownError
```

Reuse the existing platform exception hierarchy where available.

Exceptions shall:

- preserve original causes
- include useful diagnostics
- never expose secrets

---

# 32. Thread Safety

The Lifecycle Manager shall be fully thread-safe.

Concurrent operations shall not produce:

- duplicate registrations
- invalid state transitions
- inconsistent dependency graph
- lost lifecycle events
- corrupted health records

Preferred synchronization:

```python
threading.RLock
```

or equivalent.

State mutation shall be atomic.

---

# 33. Internal Package Layout

Expected logical layout:

```text
src/
    vibe/
        lifecycle/
            __init__.py
            manager.py
            state.py
            health.py
            events.py
            dependency.py
            metadata.py
            models.py
            exceptions.py
```

Follow repository conventions established by previous IWPs.

Do not duplicate functionality already implemented elsewhere.

---

# 34. Internal Module Responsibilities

## manager.py

Responsible for:

- lifecycle orchestration
- registration
- transitions
- shutdown coordination

---

## dependency.py

Responsible for:

- dependency graph
- dependency validation
- cycle detection
- shutdown ordering

---

## events.py

Responsible for:

- lifecycle events
- subscriber management
- event dispatch

---

## state.py

Responsible for:

- lifecycle state enums
- transition validation
- platform state calculation

---

## health.py

Responsible for:

- health summaries
- health state management

---

## metadata.py

Responsible for:

- immutable lifecycle metadata

---

## models.py

Responsible for:

- lifecycle models
- managed service records
- lifecycle status

---

## exceptions.py

Responsible for:

- lifecycle exception hierarchy

---

# 35. Public Package Exports

Expected exports:

```python
from vibe.lifecycle.manager import ServiceLifecycleManager

from vibe.lifecycle.state import (
    ServiceLifecycleState,
    PlatformState,
    ServiceHealthState,
)

from vibe.lifecycle.events import (
    LifecycleEvent,
    LifecycleEventType,
)

from vibe.lifecycle.models import (
    ManagedServiceRecord,
    PlatformLifecycleStatus,
    PlatformHealthSummary,
)

from vibe.lifecycle.metadata import LifecycleServiceMetadata

from vibe.lifecycle.exceptions import (
    LifecycleManagerError,
    LifecycleRegistrationError,
    LifecycleTransitionError,
    DependencyValidationError,
    DependencyCycleError,
    LifecycleShutdownError,
)
```

Do not export internal helper classes.

---

# 36. Logging Requirements

The Lifecycle Manager shall integrate with the Logging Service.

Lifecycle logging shall include:

- manager initialized
- service registered
- service transitioned
- dependency validation failure
- dependency cycle detected
- shutdown started
- shutdown completed
- lifecycle failure

Sensitive information shall never be logged.

Managed service instances shall never be serialized into logs.

---

# 37. Unit Testing Requirements

Cursor shall implement comprehensive unit tests.

Minimum coverage:

## Registration

- valid registration
- duplicate registration
- invalid metadata
- dependency validation

## State Transitions

- every legal transition
- every illegal transition
- READY transition
- FAILED transition
- STOPPED transition

## Dependency Graph

- dependency ordering
- reverse ordering
- cycle detection
- missing dependency

## Health

- health summary
- health updates
- degraded services
- failed services

## Lifecycle Events

- subscriber registration
- subscriber removal
- event ordering
- subscriber exception isolation

## Platform Status

- READY
- INITIALIZING
- DEGRADED
- FAILED
- STOPPING
- STOPPED

## Thread Safety

Verify concurrent:

- registrations
- transitions
- event publication
- shutdown coordination

Registry and lifecycle integrity shall always be preserved.

---

# 38. Integration Testing Requirements

Integration tests shall verify:

- Bootstrap initializes Lifecycle Manager
- Lifecycle Manager registers implemented services
- Service Registry integration
- Configuration Service integration
- Logging Service integration
- Bootstrap integration
- dependency graph correctness
- lifecycle event publication
- platform status computation
- orderly shutdown

Existing tests from IWP-001 through IWP-005 shall continue to pass.

---

# 39. Test Directory Layout

Follow repository conventions established by IWP-001.

Expected logical layout:

```text
tests/

    unit/

        lifecycle/

            test_manager.py

            test_state.py

            test_events.py

            test_dependency.py

            test_health.py

    integration/

        lifecycle/

            test_lifecycle_manager.py
```

Follow existing repository naming conventions where they differ.

---


# 40. Bootstrap Integration Requirements

Bootstrap shall remain the owner of platform startup.

The Lifecycle Manager shall integrate with Bootstrap using only approved public interfaces.

Bootstrap responsibilities remain:

- construct services
- initialize services
- register services with the Service Registry
- construct the Lifecycle Manager
- initialize the Lifecycle Manager
- register implemented services with the Lifecycle Manager

The Lifecycle Manager shall not assume ownership of service construction.

The Lifecycle Manager shall not instantiate services.

The Lifecycle Manager shall not modify Bootstrap sequencing.

---

# 41. Service Registry Integration Requirements

The Lifecycle Manager shall integrate with the Service Registry implemented in IWP-005.

The Service Registry remains the authoritative catalog of runtime service instances.

The Lifecycle Manager shall use the registry to:

- verify service existence
- retrieve service references
- validate dependency identifiers
- enumerate currently registered services

The Lifecycle Manager shall never duplicate the registry.

Service lookup shall always occur through the Service Registry public interface.

---

# 42. Configuration Requirements

The Lifecycle Manager shall consume configuration exclusively through the Configuration Service.

Configuration may include lifecycle-specific values such as:

- lifecycle logging level
- dependency validation behavior
- graceful shutdown timeout
- lifecycle diagnostics

The Lifecycle Manager shall not:

- parse configuration files
- read environment variables
- maintain independent configuration

All configuration shall originate from IWP-002.

---

# 43. Graceful Shutdown

Shutdown coordination shall follow these principles.

1. Bootstrap initiates shutdown.
2. Lifecycle Manager computes dependency order.
3. Services transition to STOPPING.
4. Shutdown requests are issued.
5. Successful shutdown transitions to STOPPED.
6. Failures transition to FAILED.
7. Platform shutdown completes.

The Lifecycle Manager shall never leave services in partially transitioned states.

Transition updates shall be atomic.

---

# 44. Recovery Behaviour

The Lifecycle Manager shall support recovery only where permitted by the approved architecture.

For IWP-006:

- FAILED services may transition to STOPPING.
- FAILED services may transition to STOPPED.

Automatic restart is explicitly out of scope.

Automatic dependency recovery is out of scope.

Automatic service reconstruction is out of scope.

Those capabilities belong to future implementation work packages if approved.

---

# 45. Immutability Requirements

The following public models shall be immutable:

- LifecycleServiceMetadata
- ManagedServiceRecord
- PlatformLifecycleStatus
- PlatformHealthSummary
- LifecycleEvent

Internal mutable state shall never be exposed through public APIs.

Enumeration methods shall return immutable collections.

---

# 46. Documentation Requirements

Every public Lifecycle Manager API shall include:

- purpose
- parameters
- return values
- raised exceptions
- lifecycle semantics
- thread-safety expectations

Internal implementation shall include concise documentation where necessary to explain lifecycle algorithms.

---

# 47. Python Standards

Implementation shall comply with ARP-001-05.

Requirements include:

- Python 3.11+
- complete type hints
- immutable dataclasses
- enums for lifecycle state
- descriptive naming
- explicit exceptions
- no wildcard imports
- no hidden mutable globals
- deterministic behaviour
- repository formatting conventions

Only approved dependencies may be used.

No third-party lifecycle framework shall be introduced.

---

# 48. Repository Quality Requirements

Before completion Cursor shall execute all repository quality gates established by IWP-001.

At minimum this normally includes:

```bash
ruff check .

ruff format --check .

mypy .

pytest
```

If the repository defines additional mandatory quality commands they shall also be executed.

No quality gate failures are permitted.

---

# 49. Acceptance Criteria

IWP-006 shall be considered complete only when all of the following are satisfied.

## Functional Acceptance Criteria

- Service Lifecycle Manager implemented.
- Managed service registration implemented.
- Lifecycle state tracking implemented.
- Legal transition validation implemented.
- Dependency metadata implemented.
- Dependency graph implemented.
- Dependency cycle detection implemented.
- Lifecycle event publication implemented.
- Lifecycle subscribers implemented.
- Platform lifecycle status implemented.
- Platform health summary implemented.
- Dependency-aware shutdown coordination implemented.
- Bootstrap integration completed.
- Service Registry integration completed.

## Architectural Acceptance Criteria

- No architecture redesign introduced.
- Bootstrap retains ownership of service construction.
- Service Registry retains ownership of service lookup.
- Lifecycle Manager owns lifecycle state only.
- No Event Bus implementation introduced.
- No Health Monitoring implementation introduced.
- Package dependency rules preserved.
- Public service contracts preserved.

## Engineering Acceptance Criteria

- Public APIs fully typed.
- Public APIs documented.
- Lifecycle exception hierarchy implemented.
- Unit tests implemented.
- Integration tests implemented.
- Existing IWP-001 through IWP-005 tests continue to pass.
- Repository quality gates pass.

---

# 50. Deliverables

This implementation package shall deliver:

- Service Lifecycle Manager
- lifecycle state model
- platform state model
- dependency graph implementation
- lifecycle event model
- lifecycle subscriber implementation
- lifecycle metadata
- managed service records
- platform health summary
- lifecycle exception hierarchy
- Bootstrap integration updates
- Service Registry integration updates
- unit tests
- integration tests
- documentation updates where required

No unrelated platform functionality shall be implemented.

---

# 51. Files Expected to Change

Exact files depend upon the committed implementation baseline.

Expected additions or modifications include:

```text
src/vibe/lifecycle/
tests/unit/lifecycle/
tests/integration/lifecycle/
```

Potential updates may include:

```text
src/vibe/bootstrap/
src/vibe/registry/
src/vibe/__init__.py
README.md
pyproject.toml
```

Only update these files where required by the approved architecture or existing repository conventions.

---

# 52. Commit Guidance

Create a single focused implementation commit.

Suggested commit message:

```text
feat(lifecycle): implement IWP-006 Service Lifecycle Manager
```

Do not combine unrelated refactoring.

Do not include generated files.

Do not include IDE metadata.

Do not include local configuration.

---

# 53. Completion Report

Upon completion, Cursor shall provide the following report.

```text
IWP-006 Completion Report

Implementation Summary
----------------------
Brief description of the completed Service Lifecycle Manager.

Architecture Compliance
-----------------------
- ARP-002-10 Service Lifecycle Manager Interface Contract: PASS
- ARP-002-11 Service Registry Interface Contract: PASS
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
Document any implementation assumptions explicitly permitted by the approved architecture.

Known Limitations
-----------------
None unless explicitly identified.

Ready For Review
----------------
YES
```

---

# 54. Definition of Done

This implementation package is complete when:

- all functional acceptance criteria are satisfied
- all architectural acceptance criteria are satisfied
- all engineering acceptance criteria are satisfied
- all unit tests pass
- all integration tests pass
- all existing baseline tests continue to pass
- all repository quality gates pass
- implementation introduces no architectural drift
- implementation is ready for review and merge into `86-vibe-cli`

No functionality beyond the scope of IWP-006 shall be implemented.

---

# 55. Cursor Execution Instructions

Before writing code, Cursor shall:

1. Inspect the current implementation baseline (IWP-001 through IWP-005).
2. Review the governing architecture documents referenced by this specification.
3. Confirm Bootstrap integration points.
4. Confirm Service Registry public interfaces.
5. Confirm Configuration and Logging service public APIs.
6. Implement the Service Lifecycle Manager strictly within the scope defined by this specification.
7. Execute all repository quality gates.
8. Produce the required completion report.
9. Stop.

Do **not** begin IWP-007.

---
