Filename: IWP-005-01-Service-Registry.md

IWP-005-01 – Service Registry Implementation Specification

Document Control

Item	Value
Document ID	IWP-005-01
Document Name	Service Registry Implementation Specification
Implementation Package	IWP-005 – Service Registry
Platform	86-vibe
Implementation Repository	86-vibe-cli
Architecture Repository	86-vibe-platform
Status	Implementation Ready
Owner	Chief Systems Architect
Implementation Agent	Cursor
Depends On	IWP-001 – Repository FoundationIWP-002 – Configuration ServiceIWP-003 – Logging ServiceIWP-004 – Bootstrap Service
Supersedes	None

⸻

1. Purpose

This document defines the implementation requirements for IWP-005 – Service Registry.

The Service Registry is the platform mechanism for registering, resolving, inspecting, and exposing platform service instances by stable service identity.

The Service Registry shall provide deterministic access to implemented platform services while preserving explicit dependency relationships and avoiding global service locator behavior.

Cursor shall treat this document as the complete implementation prompt.

Nothing required for implementation shall exist outside this Markdown document except:

* the approved architecture repository
* the existing 86-vibe-cli implementation baseline
* this implementation specification

⸻

2. Implementation Objective

Implement the Service Registry in the 86-vibe-cli repository.

The implementation shall provide a central registry for already constructed platform services and shall support deterministic lookup, registration, duplicate protection, service metadata, lifecycle compatibility, and thread-safe access.

The implementation shall satisfy the public service contract defined by:

* ARP-002-11 – Service Registry Interface Contract
* AEP-002-16 – Platform Bootstrap Sequence Specification
* AEP-002-17 – Service Lifecycle & Dependency Management Specification
* AEP-002-18 – Platform Reference Implementation Specification
* ARP-002-16 – Platform Service Contract Compliance Matrix
* ARP-001-03 – Package Dependency Matrix
* ARP-001-05 – Engineering Coding Standards

This package shall not implement unrelated platform services.

⸻

3. Current Implementation Baseline

The following implementation work packages have already been built, approved, committed, and are now immutable:

* IWP-001 – Repository Foundation
* IWP-002 – Configuration Service
* IWP-003 – Logging Service
* IWP-004 – Bootstrap Service

Cursor shall:

1. Inspect the current 86-vibe-cli repository.
2. Preserve all established repository conventions.
3. Reuse implemented service public APIs.
4. Avoid changing previous work unless a defect directly blocks IWP-005.
5. Document any required defect correction in the completion report.
6. Treat previous implementation packages as immutable implementation baseline.

⸻

4. Scope

This work package implements only:

IWP-005 – Service Registry

The Service Registry shall provide:

* service registration
* service lookup
* service metadata
* duplicate registration protection
* missing service handling
* optional lookup semantics
* service listing
* registry state inspection
* thread-safe access
* compatibility with Bootstrap and future Lifecycle Manager

The Service Registry shall not implement:

* service lifecycle orchestration
* dependency graph resolution
* service startup sequencing
* service shutdown sequencing
* health monitoring
* plugin discovery
* event publication
* CLI routing
* repository validation
* AI provider behavior
* MCP behavior

Those responsibilities belong to later IWPs.

⸻

5. Explicitly Out of Scope

The following services are explicitly out of scope for this work package:

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

Cursor shall not implement production placeholders for these services.

Minimal local test doubles may be used only inside tests.

⸻

6. Architectural Non-Negotiables

Cursor shall comply with the following rules.

6.1 No Architecture Redesign

Do not redesign architecture.

Do not reinterpret architecture.

Do not change approved public service contracts.

Do not introduce alternate dependency management.

Do not introduce a third-party dependency injection container.

Do not introduce a framework-level IoC container.

Do not create hidden global mutable service state.

Do not implement the Lifecycle Manager in this package.

Do not modify Configuration Service, Logging Service, or Bootstrap Service public interfaces unless correcting a blocking defect.

6.2 Stop Conditions

Cursor shall stop and report an ambiguity instead of inventing a solution if:

1. The approved architecture conflicts with existing implementation baseline.
2. The Service Registry package location cannot be reconciled with current repository structure.
3. The Service Registry contract requires lifecycle behavior that belongs to IWP-006.
4. Bootstrap integration requires changing Bootstrap public behavior in a way not already approved.
5. Thread safety cannot be satisfied with the existing implementation approach.
6. The service identity model conflicts with implemented services.
7. Required behavior cannot be implemented without adding an unapproved dependency.

Do not proceed by assumption in these cases.

⸻

7. Service Identity

The Service Registry shall conform to ARP-002-11.

Property	Required Value
Service Name	Service Registry
Package	vibe.registry
Primary Class	ServiceRegistry
Lifetime	Singleton
Thread Safety	Required
Public	Yes

If the current repository uses a different naming convention established by IWP-001, preserve repository consistency while exposing the approved public package path.

Do not create more than one independent Service Registry implementation.

⸻

8. Registry Responsibility Model

The Service Registry is responsible for knowing what services exist.

It is not responsible for deciding when services start or how services transition lifecycle states.

The registry shall:

* store service instances
* associate each service with a stable service name
* associate service metadata where available
* reject duplicate service registrations
* expose registered services by name
* expose registered services by type where supported
* expose a deterministic list of registered services
* support unregistering services where approved by contract
* support clearing registry state for test isolation where safe
* be thread-safe

The registry shall not:

* initialize services
* shut down services
* resolve dependency graphs
* decide service readiness
* poll service health
* perform plugin loading
* create services automatically
* instantiate missing services lazily

⸻

9. Dependency Contract

The Service Registry may depend on:

* Python standard library
* repository foundation utilities from IWP-001
* common platform exceptions or models from IWP-001 where present
* Logging Service only if the approved baseline already supports safe dependency injection

The Service Registry shall not require Logging Service for core functionality.

The Service Registry shall not depend on:

* Configuration Service for registration semantics
* Bootstrap Service for registration semantics
* Lifecycle Manager
* Health Monitoring
* Event Bus
* Plugin Manager
* CLI Framework
* Repository Service
* AI Provider Service
* MCP Client Service

The registry must remain usable by Bootstrap without creating circular dependencies.

⸻

10. Bootstrap Integration

IWP-004 introduced Bootstrap Service.

IWP-005 shall update Bootstrap only as needed to register already-created services.

After Bootstrap initializes implemented services, Bootstrap shall register them in the Service Registry.

The minimum registered services after successful bootstrap shall be:

* Configuration Service
* Logging Service
* Bootstrap Service
* Service Registry

Registration shall happen deterministically.

Bootstrap shall not use the registry to perform lifecycle orchestration in this work package.

The registry shall be available for future IWP-006 Lifecycle Manager integration.

⸻

11. Initialization Sequence Impact

Current startup sequence from IWP-004 is:

Construct Bootstrap Service
Construct Configuration Service
Initialize Configuration Service
Construct Logging Service
Initialize Logging Service
Emit startup diagnostics
Transition platform to RUNNING

IWP-005 updates the sequence to include Service Registry construction and registration:

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
Emit startup diagnostics
Transition platform to RUNNING

No other service shall be constructed or registered.

⸻

12. Public Service Interface

The Service Registry shall expose operations equivalent to:

class ServiceRegistry:
    def register(self, name: str, service: object, metadata: ServiceMetadata | None = None) -> None:
        ...
    def unregister(self, name: str) -> None:
        ...
    def get(self, name: str) -> object:
        ...
    def get_optional(self, name: str) -> object | None:
        ...
    def contains(self, name: str) -> bool:
        ...
    def list_services(self) -> tuple[ServiceDescriptor, ...]:
        ...
    def clear(self) -> None:
        ...

Actual method names may differ only where required by the approved ARP-002-11 contract or existing repository conventions.

Public APIs shall include:

* complete type hints
* docstrings
* deterministic behavior
* documented exceptions

⸻

13. Service Name Rules

Service names shall be stable, deterministic, and unique.

Expected canonical names for implemented services:

configuration
logging
bootstrap
service_registry

If ARP-002-11 or the current implementation baseline defines explicit service identifiers, use those exact identifiers.

Service names shall be:

* non-empty
* normalized consistently
* case-sensitive or case-normalized according to architecture
* safe for diagnostic output
* unique within the registry

If no normalization is specified, use exact string identity and reject empty or whitespace-only names.

Do not silently normalize in a way that can hide duplicate registrations.

⸻

14. Service Metadata

The registry shall support service metadata.

Minimum metadata fields:

@dataclass(frozen=True)
class ServiceMetadata:
    name: str
    service_type: str
    package: str | None = None
    version: str | None = None
    description: str | None = None
    dependencies: tuple[str, ...] = ()
    optional: bool = False

If the architecture defines a different metadata model, follow the architecture.

Metadata shall be immutable.

Metadata shall not contain mutable service instances.

⸻

15. Service Descriptor

The registry shall expose immutable service descriptors for listing and inspection.

Expected descriptor fields:

@dataclass(frozen=True)
class ServiceDescriptor:
    name: str
    service_type: str
    metadata: ServiceMetadata
    registered_at: datetime

Descriptors shall not expose mutable registry internals.

registered_at shall use timezone-aware UTC datetime.

Use:

datetime.now(timezone.utc)

Do not use naive datetimes.

⸻

16. Registration Behavior

Registering a service shall:

1. validate service name
2. validate service instance is not None
3. validate metadata where supplied
4. reject duplicate service names
5. create or store immutable descriptor metadata
6. store the service instance
7. make the service available for lookup atomically

Duplicate registration shall raise a registry-specific exception.

Registration shall never silently overwrite existing services.

⸻

17. Lookup Behavior

Service lookup by name shall be deterministic.

get(name) shall:

* return the registered service instance when present
* raise ServiceNotFoundError when absent
* never construct a service
* never return partially registered state

get_optional(name) shall:

* return the service instance when present
* return None when absent
* never raise for missing service

⸻

18. Unregistration Behavior

If supported by ARP-002-11, unregistration shall remove a service from the registry by name.

Unregistration shall:

* validate the service name
* remove the service atomically
* raise ServiceNotFoundError if the service is absent, unless architecture defines idempotent behavior

The registry shall not call service shutdown during unregister.

Lifecycle behavior belongs to IWP-006.

If the architecture prohibits unregistration for singleton platform services, implement unregister only for test isolation or do not expose it publicly.

⸻

# 19. Thread Safety

The Service Registry shall be fully thread-safe.

Concurrent operations shall not result in:

- duplicate registrations
- lost registrations
- inconsistent lookups
- partially registered services
- registry corruption

The implementation shall protect internal mutable state using an appropriate synchronization primitive.

Preferred implementation:

```python
threading.RLock
```

or equivalent.

The lock shall protect:

- registration
- unregistration
- registry clearing
- registry mutation

Read operations should remain efficient while maintaining correctness.

---

# 20. Internal Storage

The registry shall internally maintain separate structures for:

- registered service instances
- service metadata
- service descriptors

The internal implementation may resemble:

```python
_services: dict[str, object]

_metadata: dict[str, ServiceMetadata]

_descriptors: dict[str, ServiceDescriptor]
```

These structures are implementation details.

They shall never be exposed directly through the public API.

Consumers shall receive immutable views or immutable objects.

---

# 21. Registry State

The registry shall expose deterministic inspection methods.

Required capabilities include:

- registry contains service
- number of registered services
- list of registered services
- retrieve immutable descriptors

The registry shall not expose mutable collections.

Preferred return types:

- tuple
- frozenset
- MappingProxyType

Do not expose internal dictionaries.

---

# 22. Duplicate Registration Protection

Attempting to register an existing service name shall fail deterministically.

Expected exception hierarchy:

```text
ServiceRegistryError

├── DuplicateServiceRegistrationError

├── ServiceNotFoundError

├── InvalidServiceRegistrationError
```

Duplicate registration shall never overwrite an existing service.

Example:

```python
registry.register("logging", logging_service)

registry.register("logging", another_logging_service)
```

shall raise:

```text
DuplicateServiceRegistrationError
```

---

# 23. Validation Rules

Registration shall validate:

- service name supplied
- service name not empty
- service instance supplied
- metadata type
- duplicate registration
- descriptor consistency

Validation failures shall occur before registry mutation.

---

# 24. Registry Enumeration

The registry shall provide deterministic enumeration.

Enumeration order shall be stable.

Preferred ordering:

- alphabetical by service name

Example:

```text
bootstrap

configuration

logging

service_registry
```

The registry shall not depend upon dictionary insertion order.

---

# 25. Logging Integration

The Service Registry shall integrate with the Logging Service when available.

Logging is optional during construction.

Logging shall record:

- service registered
- duplicate registration
- service unregistered
- registry cleared
- registry lookup failures where appropriate

The registry shall never require the Logging Service to function.

If Logging Service is unavailable, registry operations shall still succeed.

---

# 26. Bootstrap Integration

Bootstrap shall register services immediately after successful initialization.

Expected registration order:

```text
Service Registry

Bootstrap

Configuration

Logging
```

The registry shall not initialize services.

Bootstrap remains responsible for lifecycle.

The registry only records service availability.

---

# 27. Registry Clearing

The registry shall support clearing only where permitted by architecture.

Primary purpose:

- unit testing
- integration test isolation

Production code shall not routinely clear the registry.

Clearing shall remove:

- service instances
- metadata
- descriptors

Clearing shall not call service shutdown.

---

# 28. Constructor Requirements

The constructor shall require no future platform services.

Expected conceptual design:

```python
ServiceRegistry()
```

Optional logger injection may be supported.

Example:

```python
ServiceRegistry(
    logger=platform_logger
)
```

Do not require Configuration Service.

Do not require Bootstrap Service.

---

# 29. Internal Modules

Expected logical package layout:

```text
src/
    vibe/
        registry/
            __init__.py
            service.py
            metadata.py
            descriptor.py
            exceptions.py
```

Reuse repository conventions where different.

Do not duplicate existing utilities.

---

# 30. Module Responsibilities

## service.py

Responsible for:

- registration
- lookup
- enumeration
- thread safety

---

## metadata.py

Responsible for:

- immutable ServiceMetadata

---

## descriptor.py

Responsible for:

- immutable ServiceDescriptor

---

## exceptions.py

Responsible for:

- registry exception hierarchy

---

# 31. Public Package Exports

Expected exports:

```python
from vibe.registry.service import ServiceRegistry

from vibe.registry.metadata import ServiceMetadata

from vibe.registry.descriptor import ServiceDescriptor

from vibe.registry.exceptions import (
    ServiceRegistryError,
    DuplicateServiceRegistrationError,
    InvalidServiceRegistrationError,
    ServiceNotFoundError,
)
```

Do not export internal helpers.

---

# 32. Unit Testing Requirements

Cursor shall implement comprehensive unit tests.

Minimum coverage:

## Registration

- valid registration
- duplicate registration
- invalid service name
- missing service
- invalid metadata

## Lookup

- successful lookup
- optional lookup
- failed lookup
- lookup after unregister

## Enumeration

- deterministic ordering
- descriptor correctness
- metadata correctness

## Thread Safety

Verify:

- concurrent registrations
- concurrent lookups
- concurrent unregister
- concurrent clear

Registry integrity shall always be preserved.

## Registry Clearing

Verify:

- services removed
- descriptors removed
- metadata removed

## Service Metadata

Verify:

- immutable
- dependency list
- optional flag
- description

## Service Descriptor

Verify:

- immutable
- UTC timestamp
- metadata linkage
- service type

---

# 33. Integration Testing Requirements

Integration tests shall verify registry behaviour with implemented platform services.

Tests shall verify:

- Bootstrap registers services
- Configuration Service available
- Logging Service available
- Bootstrap available
- Registry available

Verify lookup succeeds for each service.

Verify registry enumeration returns expected services.

Verify duplicate registration fails.

Verify registry state after shutdown where applicable.

---

# 34. Test Directory Layout

Follow repository conventions established by IWP-001.

Expected layout:

```text
tests/

    unit/

        registry/

            test_service.py

            test_metadata.py

            test_descriptor.py

    integration/

        registry/

            test_service_registry.py
```

If repository conventions differ, follow existing conventions.

---

# 35. Logging Requirements

When Logging Service exists:

Registry shall log:

- registration
- unregister
- lookup failure
- duplicate registration
- clear operation

Sensitive data shall never be logged.

Service instances shall never be serialized into log output.

---


# 36. Error Handling

The Service Registry shall fail deterministically.

Expected exception hierarchy:

```python
class ServiceRegistryError(Exception):
    ...

class DuplicateServiceRegistrationError(ServiceRegistryError):
    ...

class ServiceNotFoundError(ServiceRegistryError):
    ...

class InvalidServiceRegistrationError(ServiceRegistryError):
    ...
```

If a common platform exception hierarchy exists from previous implementation work packages, the registry exceptions shall inherit from the approved base exception.

Exceptions shall:

- provide meaningful diagnostic messages
- preserve the original exception where applicable
- never expose secrets
- never expose mutable registry internals

Example:

```python
try:
    registry.register("logging", logging_service)
except DuplicateServiceRegistrationError:
    ...
```

---

# 37. Immutability Requirements

The registry shall never expose mutable internal state.

The following shall be immutable:

- ServiceMetadata
- ServiceDescriptor
- registry enumeration results
- registry inspection results

Consumers shall never receive references to internal dictionaries or mutable collections.

---

# 38. Time Requirements

All timestamps recorded by the registry shall use timezone-aware UTC values.

Use:

```python
datetime.now(timezone.utc)
```

Do not use:

- local time
- naive datetime objects
- platform-specific timestamps

The timestamp associated with a ServiceDescriptor shall represent the successful completion of registration.

---

# 39. Dependency Injection Compatibility

The Service Registry shall support constructor-based dependency injection where appropriate.

The registry shall not create service instances.

Future work packages shall be able to receive a ServiceRegistry instance through constructor injection.

Example:

```python
class FutureService:
    def __init__(
        self,
        registry: ServiceRegistry,
    ):
        ...
```

No dependency injection framework shall be introduced.

---

# 40. Lifecycle Compatibility

Although IWP-006 introduces the Lifecycle Manager, the Service Registry shall expose behavior compatible with future lifecycle orchestration.

The registry shall support:

- registration after successful initialization
- inspection of registered services
- removal where supported
- deterministic enumeration

The registry shall not:

- start services
- stop services
- determine lifecycle state
- invoke lifecycle methods

Lifecycle ownership remains outside this work package.

---

# 41. Bootstrap Compatibility

The Bootstrap Service implemented in IWP-004 shall be updated only as required to register implemented services.

Bootstrap shall continue to own:

- service construction
- initialization order
- shutdown order

Service Registry shall own:

- registration
- lookup
- inspection

These responsibilities shall remain separate.

---

# 42. Documentation Requirements

Every public API shall include:

- docstring
- parameter documentation
- return documentation
- raised exceptions

Internal implementation shall include concise documentation where complexity warrants.

Documentation shall be sufficient for future work packages to consume the registry without knowledge of internal implementation.

---

# 43. Python Standards

Implementation shall comply with ARP-001-05.

Requirements include:

- Python 3.11+
- complete type hints
- immutable dataclasses
- explicit exceptions
- descriptive naming
- no wildcard imports
- no hidden mutable globals
- deterministic behavior
- repository formatting conventions

The implementation shall use only approved dependencies.

No third-party dependency injection framework shall be introduced.

---

# 44. Repository Quality Requirements

Before completion, Cursor shall execute all repository quality gates established by IWP-001.

At minimum, run the equivalent of:

```bash
ruff check .

ruff format --check .

mypy .

pytest
```

If the repository defines additional mandatory quality commands, they shall also be executed.

No quality gate failures are permitted.

---

# 45. Acceptance Criteria

IWP-005 shall be considered complete only when all of the following are true.

## Functional Acceptance Criteria

- Service Registry implemented.
- Service registration implemented.
- Service lookup implemented.
- Optional lookup implemented.
- Duplicate registration protection implemented.
- Service metadata implemented.
- Service descriptors implemented.
- Deterministic service enumeration implemented.
- Thread-safe registry implemented.
- Bootstrap registers implemented services.
- Registry inspection implemented.

## Architectural Acceptance Criteria

- No architecture redesign introduced.
- No Lifecycle Manager implemented.
- No service startup logic moved into the registry.
- No service shutdown logic moved into the registry.
- Package dependency rules preserved.
- Public service contracts preserved.
- Registry remains compatible with future Lifecycle Manager integration.

## Engineering Acceptance Criteria

- Public APIs fully typed.
- Public APIs documented.
- Registry exception hierarchy implemented.
- Unit tests implemented.
- Integration tests implemented.
- Existing IWP-001 through IWP-004 tests continue to pass.
- Repository quality gates pass.

---

# 46. Deliverables

This implementation package shall deliver:

- Service Registry implementation
- ServiceMetadata model
- ServiceDescriptor model
- Registry exception hierarchy
- Bootstrap integration updates
- Thread-safe registry implementation
- Unit tests
- Integration tests
- Public package exports
- Documentation updates where required

No unrelated platform functionality shall be implemented.

---

# 47. Files Expected to Change

Exact files depend upon the current implementation baseline.

Expected additions or modifications include:

```text
src/vibe/registry/
tests/unit/registry/
tests/integration/registry/
```

Potential updates may include:

```text
src/vibe/bootstrap/
src/vibe/__init__.py
README.md
pyproject.toml
```

Only where required by the approved architecture or existing repository conventions.

---

# 48. Commit Guidance

Create a single focused implementation commit.

Suggested commit message:

```text
feat(registry): implement IWP-005 Service Registry
```

Do not combine unrelated refactoring.

Do not include generated files.

Do not include IDE metadata.

Do not include local configuration.

---

# 49. Completion Report

Upon completion, Cursor shall provide the following report.

```text
IWP-005 Completion Report

Implementation Summary
----------------------
Brief description of the completed Service Registry.

Architecture Compliance
-----------------------
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

# 50. Definition of Done

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

No functionality beyond the scope of IWP-005 shall be implemented.

---

# 51. Cursor Execution Instructions

Before writing code, Cursor shall:

1. Inspect the current implementation baseline (IWP-001 through IWP-004).
2. Review the governing architecture documents referenced by this specification.
3. Confirm repository package conventions.
4. Confirm Bootstrap integration points.
5. Implement the Service Registry strictly within the scope defined by this specification.
6. Execute all repository quality gates.
7. Produce the required completion report.
8. Stop.

Do **not** begin IWP-006.

---
