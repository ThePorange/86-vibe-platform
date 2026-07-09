IWP-002-01 – Configuration Service Implementation Specification

Document Control

Item	Value
Document ID	IWP-002-01
Document Name	Configuration Service Implementation Specification
Implementation Package	IWP-002 – Configuration Service
Platform	86-vibe
Implementation Repository	86-vibe-cli
Architecture Repository	86-vibe-platform
Status	Implementation Ready
Owner	Chief Systems Architect
Implementation Agent	Cursor
Depends On	IWP-001 – Repository Foundation
Supersedes	None

⸻

1. Purpose

This document defines the complete implementation instructions for IWP-002 – Configuration Service.

The Configuration Service is the authoritative runtime mechanism through which all 86-vibe platform components obtain configuration.

This implementation package shall produce a working, independently testable Configuration Service conforming to the approved architecture baseline.

Cursor shall treat this document as the complete implementation prompt.

Nothing required for implementation shall exist outside this Markdown document except the approved architecture repository and the existing 86-vibe-cli implementation baseline.

⸻

2. Implementation Objective

Implement the Configuration Service in the 86-vibe-cli repository.

The implementation shall provide deterministic loading, merging, validation, retrieval, export, reload, lifecycle participation, and shutdown behavior for platform configuration.

The implementation shall satisfy the public service contract defined by:

* ARP-002-02 – Configuration Service Interface Contract
* AEP-002-05 – Configuration Management Specification
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
AEP-002-05 – Configuration Management Specification	Defines configuration hierarchy, precedence, file format, validation, runtime access, defaults, invalid handling, and testing requirements
ARP-002-02 – Configuration Service Interface Contract	Defines public service identity, responsibilities, lifecycle, data contract, validation contract, error contract, thread safety, extension contract, and conformance criteria
ARP-002-16 – Platform Service Contract Compliance Matrix	Defines mandatory lifecycle, configuration, logging, error handling, health, testing, and documentation requirements
ARP-001-05 – Engineering Coding Standards	Defines mandatory Python engineering standards
ARP-001-03 – Package Dependency Matrix	Defines dependency direction and prohibits circular dependencies

3.2 Supporting Governing Documents

Document	Relevance
AEP-002-02 – Repository Directory Structure	Defines repository and package layout
AEP-002-03 – Python Package Architecture	Defines Python package responsibilities
AEP-002-12 – Testing Strategy Specification	Defines test expectations
AEP-002-13 – Build & Packaging Specification	Defines package/build expectations
AEP-002-15 – Local Development Workflow Specification	Defines development workflow expectations
AEP-002-16 – Platform Bootstrap Sequence Specification	Defines bootstrap ordering
AEP-002-17 – Service Lifecycle & Dependency Management Specification	Defines lifecycle and dependency behavior

⸻

4. Implementation Baseline

IWP-001 – Repository Foundation has already been implemented, approved, merged, and is now part of the immutable implementation baseline.

Cursor shall:

1. Inspect the existing 86-vibe-cli repository.
2. Preserve all structure, tooling, conventions, and tests established by IWP-001.
3. Reuse existing package scaffolding, exception patterns, typing conventions, test layout, and project configuration.
4. Avoid changing IWP-001 behavior unless required to correct a defect directly blocking IWP-002.
5. Treat any such defect as an implementation issue and document it in the completion report.

⸻

5. Strict Scope

This work package implements only:

IWP-002 – Configuration Service

The following services are explicitly out of scope:

* IWP-003 – Logging Service
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

Do not add alternate configuration mechanisms.

Do not change configuration precedence.

6.2 Stop Conditions

Cursor shall stop and report an ambiguity instead of inventing a solution if any of the following are discovered:

1. The approved architecture conflicts with existing IWP-001 code.
2. The package location is ambiguous after inspecting the repository.
3. The public Configuration Service contract cannot be implemented without changing another service contract.
4. Required behavior depends on a future IWP service that has not yet been implemented.
5. A new dependency appears necessary but is not already approved.
6. The YAML parsing dependency is absent and cannot be added without violating existing project dependency policy.
7. The service cannot satisfy thread-safety requirements using the existing baseline.

Cursor shall not proceed by assumption in those cases.

⸻

7. Service Identity

The Configuration Service shall conform to the identity defined in ARP-002-02.

Property	Required Value
Service Name	Configuration Service
Package	vibe.configuration
Primary Class	ConfigurationService
Lifetime	Singleton
Thread Safety	Required
Public	Yes

If IWP-001 established the package as vibe.config in accordance with AEP-002-02 and AEP-002-03, Cursor shall preserve the existing repository package structure and expose compatibility through the approved public package path where required.

The architecture documents contain both package references:

* repository/package structure includes src/vibe/config/
* ARP-002-02 identifies the public service package as vibe.configuration

Cursor shall resolve this without redesign by using the existing IWP-001 baseline as primary implementation layout and, if needed, providing a thin compatibility module that exposes the public ConfigurationService without duplicating logic.

Do not create two independent configuration implementations.

⸻

8. Required Configuration Behavior

The Configuration Service shall provide centralized, deterministic runtime configuration.

It shall be responsible for:

* loading configuration
* discovering supported configuration sources
* parsing project configuration
* applying supported environment variable overrides
* applying CLI/runtime overrides
* resolving precedence
* validating required values and supported types
* freezing effective runtime configuration
* exposing typed configuration values
* supporting key existence checks
* exporting effective configuration as structured data
* supporting reload behavior where safe
* shutting down cleanly

The service shall not:

* perform business logic
* initialize unrelated services
* communicate with AI providers
* communicate with MCP servers
* create repositories
* perform CLI command routing
* implement the future Validation Framework
* read secrets from committed files
* log secrets
* mutate configuration after initialization except through documented reload behavior

⸻

9. Configuration Source Model

The implementation shall support the architecture-defined source hierarchy.

Configuration source precedence from highest to lowest is:

CLI command arguments / runtime overrides
Environment variables
Project configuration
Built-in platform defaults

Loading sequence from lowest to highest is:

Load platform defaults
Load project configuration
Load environment variables
Apply CLI/runtime overrides
Validate configuration
Freeze runtime configuration

No additional configuration source precedence shall be introduced.

The service shall not support user-level config, global config, remote config, database config, cloud config, or secrets-manager-backed config in this work package unless already present in IWP-001 and explicitly required by the approved architecture.

⸻

10. Project Configuration Location

The project configuration file shall be:

.86vibe/config.yaml

The associated project configuration directory layout defined by architecture is:

.86vibe/
├── config.yaml
├── cache/
├── logs/
├── state/
└── templates/

IWP-002 shall implement reading and validating .86vibe/config.yaml.

IWP-002 shall not implement full project initialization. Directory creation belongs to bootstrap behavior unless IWP-001 already provided foundational utilities.

For this package:

* missing optional project config may be handled according to the service initialization mode
* missing required project config shall fail deterministically
* tests shall cover both optional and required project config behavior
* the service shall not silently create a full .86vibe project structure unless architecture baseline code already does so

⸻

11. Configuration File Format

The project configuration format shall be YAML.

Cursor shall not implement TOML or JSON configuration unless already approved elsewhere in the architecture baseline.

Example architecture-defined configuration:

platform:
  name: 86-vibe
repository:
  type: platform
  authoritative: true
ai:
  provider: openrouter
  default_model: anthropic/claude-opus-4
mcp:
  enabled: true
logging:
  level: INFO
governance:
  require_human_approval: true
  architecture_lock: true

The implementation shall parse YAML deterministically.

If the repository already includes a YAML dependency from IWP-001, use it.

If no YAML dependency exists, Cursor shall inspect pyproject.toml.

* If adding PyYAML is consistent with existing project dependency policy, add it in the minimal supported way.
* If adding dependencies is not permitted by the existing baseline, stop and report the ambiguity.
* Do not implement an ad hoc YAML parser.

⸻

12. Configuration Domains

The implementation shall support the architecture-defined configuration domains.

Domain	Purpose
platform	General platform metadata
repository	Repository-specific metadata and behavior
ai	AI provider configuration and model selection
mcp	Model Context Protocol configuration
logging	Logging levels and output behavior
governance	Architecture governance controls
experimental	Optional features under development

The implementation shall allow unknown future sections unless explicitly configured to reject them.

Unknown sections shall not break existing consumers.

Known sections shall be validated according to the schema implemented in this package.

---

## 13. Configuration Schema

The Configuration Service shall expose strongly typed configuration objects internally while providing convenient read access to consumers.

The implementation shall separate:

- raw configuration data
- parsed configuration
- validated configuration
- immutable runtime configuration

Recommended internal flow:

```text
YAML
    │
    ▼
Dictionary
    │
    ▼
Schema Validation
    │
    ▼
Typed Configuration Models
    │
    ▼
Immutable Effective Configuration
```

Consumers shall never receive mutable internal configuration state.

---

## 14. Public Service Interface

The implementation shall conform to the interface defined by **ARP-002-02**.

The public service shall expose functionality equivalent to:

```python
class ConfigurationService:

    def initialize(...) -> None:
        ...

    def shutdown(...) -> None:
        ...

    def reload(...) -> None:
        ...

    def get(self, key: str, default=None):
        ...

    def exists(self, key: str) -> bool:
        ...

    def get_section(self, section: str):
        ...

    def export(self):
        ...

    @property
    def configuration(self):
        ...
```

Method names may differ only where required by the approved architecture or the IWP-001 implementation baseline.

Public APIs shall include:

- complete type hints
- docstrings
- deterministic behavior
- documented exceptions

---

## 15. Configuration Access

The service shall support hierarchical access.

Examples:

```python
config.get("platform.name")

config.get("logging.level")

config.get("repository.type")
```

Nested lookups shall not require callers to understand the underlying YAML structure.

Missing optional values shall return defaults where defined.

Missing required values shall raise a ConfigurationValidationError.

---

## 16. Environment Variable Overrides

The implementation shall support environment variable overrides.

The approved prefix is:

```text
VIBE_
```

Examples:

```text
VIBE_LOGGING_LEVEL=DEBUG

VIBE_PLATFORM_NAME=86-vibe

VIBE_AI_PROVIDER=openrouter
```

Mapping rules:

- uppercase
- underscore separated
- deterministic conversion
- nested configuration supported

Example mapping:

```text
VIBE_PLATFORM_NAME
```

becomes

```text
platform.name
```

Environment variables shall override YAML configuration.

Environment variables shall not modify configuration files.

---

## 17. Runtime Overrides

The service shall support runtime overrides supplied during initialization.

These overrides shall have the highest precedence.

Runtime overrides shall:

- remain immutable after initialization
- be included in exported effective configuration
- participate in validation

Example:

```python
ConfigurationService(
    overrides={
        "logging.level": "DEBUG"
    }
)
```

---

## 18. Validation

Validation shall occur immediately after configuration loading.

Validation shall verify:

- required sections exist
- required keys exist
- supported value types
- supported enumerations
- supported numeric ranges
- supported boolean values
- valid nested structures

Validation failures shall never be deferred until first use.

---

## 19. Configuration Reload

The service contract requires reload capability.

Reload shall:

1. reload supported configuration sources
2. reapply precedence
3. perform validation
4. replace runtime configuration atomically

If validation fails:

- previous configuration shall remain active
- partial configuration shall never become active

Reload shall be thread safe.

---

## 20. Thread Safety

ARP-002 identifies the Configuration Service as thread safe.

The implementation shall therefore ensure:

- atomic initialization
- atomic reload
- immutable runtime configuration
- safe concurrent readers

Readers shall never observe partially updated configuration.

---

## 21. Error Handling

The service shall define configuration-specific exceptions.

Expected hierarchy:

```text
ConfigurationError
│
├── ConfigurationLoadError
├── ConfigurationValidationError
├── ConfigurationKeyError
└── ConfigurationReloadError
```

If IWP-001 already defines a common platform exception hierarchy, these exceptions shall inherit from the approved base class.

Exceptions shall include useful diagnostic information without exposing:

- secrets
- API keys
- tokens
- credentials
- local filesystem information beyond what is necessary

---

## 22. Internal Package Layout

Cursor shall implement the Configuration Service using the repository structure established by IWP-001.

Expected logical layout:

```text
src/
└── vibe/
    └── config/
        __init__.py
        service.py
        loader.py
        validator.py
        merger.py
        models.py
        exceptions.py
        environment.py
```

If IWP-001 already created equivalent modules, extend them rather than introducing parallel implementations.

No duplicate implementations shall exist.

---

## 23. Internal Responsibilities

### service.py

Responsible for:

- lifecycle
- public API
- orchestration

Shall not perform parsing directly.

---

### loader.py

Responsible for:

- reading YAML
- loading defaults
- locating project configuration
- reading configuration files

---

### merger.py

Responsible for:

- precedence rules
- merging dictionaries
- runtime override application

---

### validator.py

Responsible for:

- schema validation
- required values
- supported values
- type checking

---

### environment.py

Responsible for:

- discovering environment variables
- converting names
- parsing values
- producing override dictionaries

---

### models.py

Responsible for:

- immutable configuration models
- typed objects
- configuration sections

---

### exceptions.py

Responsible for:

- configuration-specific exception hierarchy

---

## 24. Dependency Rules

The Configuration Service shall not depend upon:

- Logging Service
- Bootstrap Service
- Event Bus
- AI Provider Layer
- MCP Client
- Repository Service

Those services are implemented in later IWPs.

The Configuration Service may use:

- Python standard library
- repository utilities established in IWP-001
- approved YAML dependency

No additional architectural dependencies shall be introduced.

---

## 25. Lifecycle Integration

Although Bootstrap Service is not yet implemented, the Configuration Service shall expose lifecycle methods compatible with the future lifecycle manager.

Required lifecycle methods:

- initialize()
- shutdown()
- reload()

Lifecycle methods shall be idempotent where appropriate.

Shutdown shall release resources cleanly.

---

## 26. Logging

Because Logging Service has not yet been implemented, the Configuration Service shall not implement its own logging framework.

If IWP-001 established a temporary logging abstraction, use it.

Otherwise:

- avoid print()
- avoid custom logging implementations
- keep diagnostics limited to exceptions

Future integration with IWP-003 shall require minimal modification.

---

## 27. Documentation

Every public class shall include:

- docstring
- purpose
- parameter documentation
- return documentation
- exception documentation where appropriate

Internal helper methods should also include concise documentation where complexity warrants.

---

## 28. Python Standards

Implementation shall comply with ARP-001-05.

Requirements include:

- Python 3.11+
- complete type hints
- dataclasses where appropriate
- immutable configuration objects
- small focused methods
- descriptive naming
- no wildcard imports
- no global mutable state
- no hidden side effects

Cursor shall follow repository formatting conventions.

---

## 29. Security Requirements

The Configuration Service shall never:

- log secrets
- export secrets unintentionally
- commit generated configuration
- write configuration without explicit request

Sensitive values shall be masked where exported for diagnostics.

---

## 30. Unit Testing Requirements

Cursor shall implement a comprehensive unit test suite for the Configuration Service.

The objective is to verify the Configuration Service independently of all future platform services.

The unit test suite shall include, at minimum, the following scenarios.

### Initialization

- Service initializes successfully using default configuration.
- Service initializes successfully using project configuration.
- Service initializes successfully using runtime overrides.
- Initialization is idempotent where defined by the service contract.
- Initialization fails deterministically on invalid configuration.

### Configuration Loading

- Built-in defaults are loaded.
- Project configuration is loaded.
- Missing optional configuration behaves as defined.
- Missing mandatory configuration produces the correct exception.
- Invalid YAML produces the correct exception.
- Unsupported configuration keys are handled according to the approved architecture.
- Invalid value types are rejected.

### Configuration Merge

Verify precedence exactly matches the approved architecture.

Tests shall verify:

```
Defaults
    ↓
Project Configuration
    ↓
Environment Variables
    ↓
Runtime Overrides
```

Each higher precedence source shall override the lower source without modifying the original source.

### Environment Variables

Tests shall verify:

- supported environment variables are detected
- nested values are correctly mapped
- invalid values are rejected
- unsupported variables are ignored unless otherwise specified
- environment overrides supersede project configuration

### Runtime Overrides

Tests shall verify:

- runtime overrides supersede all other configuration
- overrides participate in validation
- exported configuration contains runtime overrides
- overrides remain immutable after initialization

### Access Methods

Tests shall verify:

- get()
- exists()
- get_section()
- export()

Examples include:

- existing keys
- missing keys
- nested keys
- optional defaults
- invalid lookups

### Validation

Tests shall verify:

- required sections
- required keys
- invalid enumerations
- invalid numeric ranges
- invalid boolean values
- invalid nested structures

### Reload

Tests shall verify:

- successful reload
- failed reload
- previous configuration remains active after failed reload
- atomic replacement of configuration

### Thread Safety

Tests shall verify:

- concurrent readers
- concurrent reads during reload
- configuration consistency

The objective is to demonstrate that readers never observe partially updated configuration.

---

## 31. Integration Testing Requirements

Integration tests shall verify the Configuration Service operating within the repository baseline established by IWP-001.

The integration suite shall include, at minimum:

### Repository Integration

- configuration file discovered correctly
- configuration loaded from repository
- expected directory layout supported

### Service Lifecycle

- initialize()
- reload()
- shutdown()

### Effective Configuration

Verify exported effective configuration accurately reflects:

- defaults
- project configuration
- environment overrides
- runtime overrides

### Failure Behaviour

Integration tests shall verify:

- invalid YAML
- missing files
- malformed values
- reload failures

---

## 32. Test Directory Layout

Cursor shall follow the repository structure established by IWP-001.

Expected logical layout:

```text
tests/
├── unit/
│   └── config/
│       test_loader.py
│       test_merger.py
│       test_validator.py
│       test_environment.py
│       test_service.py
│
└── integration/
    └── config/
        test_configuration_service.py
```

If IWP-001 established different naming conventions, follow those conventions consistently.

---

## 33. Quality Requirements

Before completion, Cursor shall execute all repository quality checks established by IWP-001.

This normally includes the equivalent of:

```bash
ruff check .

ruff format --check .

mypy .

pytest
```

If the repository defines additional mandatory commands, they shall also be executed.

No failing quality checks are permitted.

---

## 34. Acceptance Criteria

IWP-002 shall be considered complete only when all of the following are true.

### Functional

- Configuration Service implemented.
- Public service contract implemented.
- Configuration successfully loaded.
- Configuration precedence implemented.
- Environment overrides implemented.
- Runtime overrides implemented.
- Typed configuration available.
- Configuration validation implemented.
- Configuration reload implemented.
- Configuration export implemented.

### Architectural

- No architecture redesign.
- No new platform services introduced.
- Public service contract preserved.
- Package dependencies conform to ARP-001.
- Platform Service Contract Compliance Matrix satisfied.
- Thread safety requirements satisfied.

### Engineering

- Complete type hints.
- Public API documentation.
- Unit tests implemented.
- Integration tests implemented.
- Repository quality checks pass.
- Existing IWP-001 tests continue to pass.

---

## 35. Deliverables

This implementation package shall produce:

- Configuration Service implementation
- supporting internal modules
- configuration models
- validation logic
- merge logic
- environment variable support
- exception hierarchy
- unit tests
- integration tests
- updated package exports
- updated developer documentation where required

No unrelated code shall be modified.

---

## 36. Files Expected to Change

The exact file list shall depend upon the IWP-001 baseline.

Expected changes include:

```text
src/vibe/config/
tests/unit/config/
tests/integration/config/
```

Potential updates may also include:

```text
pyproject.toml
README.md
src/vibe/__init__.py
```

Only where required by the approved architecture or repository conventions.

---

## 37. Commit Guidance

Create a single implementation commit.

Suggested commit message:

```text
feat(config): implement IWP-002 Configuration Service
```

Do not combine unrelated refactoring.

Do not include generated files.

Do not include local configuration.

Do not include IDE artifacts.

---

## 38. Completion Report

When implementation has completed, Cursor shall provide the following report.

```text
IWP-002 Completion Report

Implementation Summary
----------------------
Brief description of the completed Configuration Service.

Architecture Compliance
-----------------------
- ARP-002-02 Configuration Service Interface Contract: PASS
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
Document any assumptions made that were explicitly permitted by the approved architecture.

Known Limitations
-----------------
None unless explicitly identified.

Ready For Review
----------------
YES
```

---

## 39. Definition of Done

This implementation package is complete when:

- all acceptance criteria have been satisfied
- all tests pass
- repository quality gates pass
- implementation complies with the approved architecture
- no architectural drift has been introduced
- implementation is ready for human review
- implementation is suitable for merge into the `86-vibe-cli` implementation repository

No additional functionality shall be implemented beyond the scope of IWP-002.

---

## 40. Cursor Execution Instructions

Before writing code, Cursor shall:

1. Inspect the existing repository produced by IWP-001.
2. Review the governing architecture documents referenced by this specification.
3. Confirm package locations and existing implementation conventions.
4. Implement the Configuration Service strictly within the scope defined by this document.
5. Execute all quality gates.
6. Produce the required completion report.
7. Stop.

Do **not** begin IWP-003.

---

# END OF FILE