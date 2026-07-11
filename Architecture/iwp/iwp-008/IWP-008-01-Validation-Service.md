# IWP-008-01 – Validation Service Implementation Specification

## Document Control

| Item | Value |
|---|---|
| Document ID | IWP-008-01 |
| Document Name | Validation Service Implementation Specification |
| Implementation Package | IWP-008 – Validation Framework |
| Companion Set | IWP-008-00 through IWP-008-06 |
| Platform | 86-vibe |
| Implementation Repository | `86-vibe-cli` |
| Architecture Repository | `86-vibe-platform` |
| Status | Implementation Ready |
| Owner | Chief Systems Architect |
| Implementation Agent | Cursor |
| Depends On | IWP-001 through IWP-007; IWP-009 – Repository Service |
| Sequencing Authority | ADR-001 – Implement Repository Service Before Validation Framework |
| Parent Specification | IWP-008-00 – Validation Framework Master Implementation Specification |
| Supersedes | None |

---

# 1. Purpose

This document defines the implementation requirements for the public Validation Service delivered by:

**IWP-008 – Validation Framework**

The Validation Service is the authoritative platform entry point for validating platform artifacts against approved architectural, structural, configuration, and implementation rules.

This companion specification defines:
- Validation Service identity
- public service boundary
- dependency injection
- initialization
- lifecycle behavior
- Service Registry integration
- Service Lifecycle Manager integration
- Bootstrap Service integration
- Repository Service integration
- validation request handling
- validator orchestration boundary
- reporting boundary
- health behavior
- shutdown behavior
- service-level errors
- service-level logging
- thread safety
- service-level tests

The following concerns are defined by separate IWP-008 companion specifications:
- validation engine internals
- validator registry internals
- rule execution pipeline
- individual validation rules
- validation models
- CLI integration
- complete testing and acceptance requirements
Cursor shall treat this document as one mandatory part of the complete IWP-008 specification set.

No implementation of IWP-008 shall begin until all IWP-008 companion specifications have been reviewed.

---

# 2. Governing Specifications

Implementation shall conform to the following approved documents:
- ADR-001 – Implement Repository Service Before Validation Framework
- AEP-001-07 – Platform Architecture
- AEP-001-08 – Repository Strategy
- AEP-001-09 – Technology Strategy
- AEP-001-10 – Versioning Strategy
- AEP-002-01 – Core Platform Overview
- AEP-002-02 – Repository Directory Structure
- AEP-002-03 – Python Package Architecture
- AEP-002-04 – CLI Specification
- AEP-002-05 – Configuration Management Specification
- AEP-002-06 – Logging & Diagnostics Specification
- AEP-002-10 – Repository Services Specification
- AEP-002-11 – Document Validation Framework Specification
- AEP-002-13 – Build & Packaging Specification
- AEP-002-15 – Local Development Workflow Specification
- AEP-002-16 – Platform Bootstrap Sequence Specification
- AEP-002-17 – Service Lifecycle & Dependency Management Specification
- AEP-002-18 – Platform Reference Implementation Specification
- ARP-001-03 – Package Dependency Matrix
- ARP-001-05 – Engineering Coding Standards
- ARP-002-02 – Configuration Service Interface Contract
- ARP-002-03 – Logging Service Interface Contract
- ARP-002-07 – Repository Service Interface Contract
- ARP-002-08 – Validation Service Interface Contract
- ARP-002-09 – Bootstrap Service Interface Contract
- ARP-002-10 – Service Lifecycle Manager Interface Contract
- ARP-002-11 – Service Registry Interface Contract
- ARP-002-16 – Platform Service Contract Compliance Matrix
- IWP-008-00 – Validation Framework Master Implementation Specification
- IWP-008-02 – Validation Engine Implementation Specification
- IWP-008-03 – Validators Implementation Specification
- IWP-008-04 – Validation Models Implementation Specification
- IWP-008-05 – CLI Integration Implementation Specification
- IWP-008-06 – Testing and Acceptance Implementation Specification

Where wording differs between this implementation specification and an approved architecture document, the approved architecture document takes precedence.
Cursor shall stop and report a material conflict rather than silently reinterpret the architecture.

---

# 3. Implementation Baseline

The following implementation work packages have been completed, approved, committed, and merged into the `86-vibe-cli` repository:
- IWP-001 – Repository Foundation
- IWP-002 – Configuration Service
- IWP-003 – Logging Service
- IWP-004 – Bootstrap Service
- IWP-005 – Service Registry
- IWP-006 – Service Lifecycle Manager
- IWP-007 – CLI Framework
- IWP-009 – Repository Service

These implementations form the immutable implementation baseline for IWP-008.
Cursor shall:
1. Inspect the current `86-vibe-cli` repository before writing code.
2. Identify the actual public interfaces implemented by the baseline services.
3. Preserve all existing public APIs.
4. Preserve all existing package dependency directions.
5. Use dependency injection conventions established by the baseline.
6. Use lifecycle conventions established by IWP-004 and IWP-006.
7. Use the Service Registry conventions established by IWP-005.
8. use the Repository Service through its approved public interface.
9. use the Configuration Service through its approved public interface.
10. use the Logging Service through its approved public interface.
11. avoid direct filesystem access for repository-managed artifacts.
12. avoid modifying previous work unless correcting a defect that directly prevents IWP-008 implementation.
13. document every required baseline defect correction in the IWP-008 completion report.
The implementation baseline shall not be redesigned to make IWP-008 easier to implement.


---

# 4. Sequencing Compliance

ADR-001 requires IWP-009 to be completed before IWP-008.
That requirement has now been satisfied.
The Validation Service shall therefore consume the implemented Repository Service as a mandatory dependency.

Cursor shall not:
- create a temporary repository adapter
- create a second repository abstraction
- access repository-managed files directly
- reproduce Repository Service behavior inside the Validation Service
- retain any obsolete deferred repository workaround
- make the Repository Service depend on the Validation Service

The dependency direction is:
```text
Validation Service
        |
        v
Repository Service
        |
        v
Configuration Service and Logging Service

The Validation Service shall initialize only after its mandatory dependencies are ready.

---

# 5. Strict Scope

This companion specification implements only the public Validation Service and its integration with the existing platform lifecycle.

The scope includes:

* public vibe.validation package
* public ValidationService class
* service construction
* dependency injection
* configuration consumption
* logging integration
* Repository Service integration
* validation-service initialization
* validator discovery orchestration
* validator registration orchestration
* validator integrity verification orchestration
* validation request acceptance
* validation request precondition checking
* delegation to the Validation Engine
* validation-path orchestration
* repository-validation orchestration
* rule-listing orchestration
* report-generation orchestration
* health reporting through established platform conventions
* lifecycle-state handling
* Service Registry integration
* Service Lifecycle Manager integration
* Bootstrap Service integration
* safe shutdown
* service-level errors
* service-level tests

This document does not define the complete internal behavior of the Validation Engine or individual validators.

Those requirements are defined in the remaining IWP-008 companion specifications.

---

# 6. Explicitly Out of Scope

This companion specification shall not implement:

* individual architecture validation rules
* individual metadata validation rules
* individual naming validation rules
* individual cross-reference validation rules
* individual dependency validation rules
* implementation-document validation rules
* validation report formatting details
* CLI command rendering
* AI-assisted validation
* probabilistic validation
* prompt loading
* prompt rendering
* AI-provider selection
* MCP communication
* plugin discovery
* plugin execution
* event publication
* event subscription
* health aggregation across services
* platform-context aggregation
* repository management
* direct Git operations
* remote repository operations
* automatic artifact correction
* automatic architecture approval
* architecture-document rewriting

The Validation Service is read-only with respect to validated artifacts.

---

# 7. Architectural Non-Negotiables

## 7.1 Central Validation Boundary

All platform validation shall be invoked through the Validation Service.

Consumers shall not:

* instantiate validators directly
* invoke validation rules directly
* invoke the Validation Engine directly
* construct internal validator registries
* bypass validation request checks
* aggregate validation findings independently

Internal Validation Framework components may invoke one another according to the IWP-008 package design.

External consumers shall use the public Validation Service.

## 7.2 No Artifact Modification

The Validation Service shall not modify validated artifacts.

It shall not:

* rewrite files
* repair metadata
* normalize documents in place
* create missing architecture documents
* approve documents
* update document status
* stage files
* commit files
* write reports into the repository unless a later approved specification explicitly authorizes report persistence

Validation shall remain observational and deterministic.

## 7.3 No Business Workflow Logic

The Validation Service shall not implement:

* approval workflow
* implementation workflow
* release workflow
* repository workflow
* AI workflow
* prompt workflow
* plugin workflow

It shall report validation outcomes only.

## 7.4 No Direct Repository Management

The Validation Service shall not:

* discover repository roots independently
* interpret Git metadata independently
* read repository configuration files directly
* traverse repository paths outside the Repository Service
* perform repository writes
* implement repository path-security logic already supplied by IWP-009

All repository interaction shall use the public Repository Service interface.

## 7.5 No CLI Dependency

The vibe.validation package shall not import from vibe.cli.

CLI handlers may depend on the Validation Service.

The Validation Service shall remain independent of presentation and terminal concerns.

## 7.6 Stop Conditions

Cursor shall stop and report an architectural ambiguity if:

1. The implemented Repository Service lacks a public operation required by ARP-002-08 or AEP-002-11.
2. The implemented Configuration Service cannot provide validation configuration through a public interface.
3. The implemented Logging Service cannot provide an approved logger.
4. The Service Registry cannot register the Validation Service without changing its public contract.
5. The Service Lifecycle Manager cannot represent the required dependency order.
6. The Bootstrap Service cannot initialize Validation after Repository Service.
7. The baseline service-health model materially conflicts with ARP-002-08.
8. A required validation target cannot be accessed without bypassing Repository Service.
9. A required service method would expose an internal validator implementation.
10. A required feature would make validation non-deterministic.
11. A required feature would modify validated artifacts.
12. A required feature belongs to a future implementation work package.
13. A public service signature cannot conform to ARP-002-08 without changing an approved contract.
14. The architecture leaves a required validation behavior materially undefined.
15. Implementing the service would require broad unrelated refactoring.

Cursor shall not invent a solution to a stop condition.

---

# 8. Service Identity

The Validation Service shall conform to ARP-002-08.

Property	Required Value
Service Name	Validation Service
Public Package	vibe.validation
Primary Class	ValidationService
Lifetime	Singleton
Thread Safety	Required
Public	Yes
Mandatory Dependencies	Configuration Service, Logging Service, Repository Service
Forbidden Dependencies	AI Provider Service, MCP Client Service, Prompt Management Service, CLI

The service shall use the canonical service identifier convention established by IWP-005.

Cursor shall not create a second identifier where the baseline already defines one.

---

# 9. Public Interface

The Validation Service shall expose operations logically equivalent to:

initialize()
validate(request)
validate_path(path)
validate_repository()
list_rules()
report(result)
health()
shutdown()

Equivalent language-specific signatures are permitted where required by:

* existing lifecycle protocols
* existing dependency-injection conventions
* typed request and result models
* synchronous or asynchronous conventions already established by the baseline
* established health-result models
* established error-handling conventions

Observable behavior shall remain consistent with ARP-002-08.

The public interface shall be explicit, typed, documented, deterministic, and testable.

Required operations shall not be hidden behind dynamic method dispatch.

---

# 10. Construction

The Validation Service constructor shall receive its dependencies through the dependency-injection mechanism established by the implementation baseline.

Mandatory logical dependencies are:

* Configuration Service
* Logging Service
* Repository Service
* Validation Engine or an internal engine protocol defined by IWP-008-02

The Validation Engine is an internal IWP-008 implementation dependency, not an external platform service unless the approved architecture explicitly requires otherwise.

Construction shall not:

* access repository artifacts
* discover validators
* execute rules
* load configuration files directly
* initialize the platform
* publish service availability
* register duplicate service instances
* write artifacts
* create uncontrolled global state

Any default engine or registry construction shall follow the existing composition conventions.

Public constructor behavior shall remain suitable for deterministic unit testing.

---

# 11. Dependency Injection

Dependencies shall be explicit.

The service shall not:

* import a global Configuration Service instance
* import a global Logging Service instance
* import a global Repository Service instance
* obtain dependencies through hidden mutable globals
* create a second Service Registry
* use an unapproved service locator
* instantiate baseline services internally

Where the baseline uses constructor injection, continue using constructor injection.

Where the baseline uses registered factories or bootstrap composition, follow that approach without introducing a parallel pattern.

Tests shall be able to provide controlled test doubles for all mandatory dependencies.

Test doubles shall not become production implementations.

---

# 12. Service Lifetime

The Validation Service shall have singleton lifetime within one platform runtime.

Singleton lifetime shall be provided through the existing Service Registry and Bootstrap composition mechanisms.

The implementation shall not use an independent module-level singleton.

The singleton instance shall:

* be created once per platform runtime
* be initialized once according to lifecycle rules
* support approved idempotent initialization behavior
* be resolved consistently through the Service Registry
* be shut down through the lifecycle infrastructure
* not persist across unrelated process executions

Tests may instantiate isolated service instances outside the registry where required for unit testing.

---

# 13. Initialization Contract

Initialization shall occur after:

1. Configuration Service
2. Logging Service
3. Repository Service

The initialization sequence shall be logically equivalent to:

1. Validate lifecycle state.
2. Verify mandatory dependency availability.
3. Load validation configuration.
4. Construct or prepare internal validation components.
5. Discover validation rules.
6. Register validators.
7. Verify validator integrity.
8. Verify deterministic rule identity.
9. Verify required validators where architecture mandates them.
10. prepare service-level caches or immutable snapshots.
11. publish service readiness.
12. log initialization outcome.

The exact internal distribution of steps between the Validation Service, Validation Engine, and Validator Registry is defined by IWP-008-02.

The Validation Service remains responsible for orchestrating successful initialization.

---

# 14. Initialization Preconditions

Before initialization succeeds:

* Configuration Service shall be ready.
* Logging Service shall be ready.
* Repository Service shall be ready.
* validation configuration shall be valid.
* the validator registry shall be constructible.
* all discovered rule identifiers shall be valid.
* all discovered rule identifiers shall be unique.
* mandatory validators shall pass integrity checks.
* the Validation Engine shall be ready to accept requests.

A failure in any mandatory precondition shall prevent the Validation Service from entering the ready state.

The service shall not report readiness when validation cannot be executed safely.

---

# 15. Idempotent Initialization

Initialization shall follow the idempotence semantics established by IWP-004 and IWP-006.

Repeated initialization shall not:

* register duplicate validators
* register duplicate service instances
* duplicate logging handlers
* duplicate lifecycle callbacks
* change rule ordering unpredictably
* corrupt internal caches
* execute validation
* modify repository contents

Where initialization is called after successful initialization, the service shall either:

* return the existing ready state, or
* perform the baseline-approved no-op behavior

It shall not rebuild mutable state unnecessarily.

---

# 16. Initialization Failure

Initialization failures shall:

* prevent readiness publication
* transition the service into the baseline-approved failed state
* preserve a classified public error
* log the failure safely
* avoid exposing implementation details
* avoid leaving a partially populated public registry
* avoid leaving partially initialized validators available to consumers
* permit safe shutdown

A failed initialization shall not be silently downgraded to a warning where the service cannot execute validation correctly.

---

# 17. Validation Configuration

The Validation Service shall obtain configuration exclusively through the Configuration Service.

It shall not:

* read configuration files directly
* inspect environment variables directly
* parse project configuration independently
* maintain a second configuration source
* modify configuration

Validation configuration may include architecture-approved settings such as:

* enabled validator groups
* disabled optional rules
* validation scope defaults
* reporting defaults
* concurrency settings
* cache settings
* strictness settings where approved
* allowed validation targets
* repository-validation behavior

Configuration shall not allow mandatory architectural rules to be disabled unless the approved architecture explicitly permits it.

The complete configuration model shall be defined consistently across IWP-008-02, IWP-008-03, and IWP-008-04.

---

# 18. Configuration Validation

Validation configuration shall be checked before rule discovery and execution.

Configuration errors shall include logical cases such as:

* unknown rule identifier
* duplicate configured rule
* invalid scope
* invalid severity override where overrides are permitted
* invalid concurrency value
* unsupported target type
* contradictory options
* invalid reporting preference

Configuration errors shall prevent initialization where they make deterministic validation impossible.

Optional configuration defects may produce warnings only where the architecture explicitly permits fallback behavior.

Defaults shall be deterministic and documented.

---

# 19. Logging Integration

All Validation Service logs shall use the Logging Service.

The service shall log:

* initialization start
* initialization completion
* validator discovery summary
* validator registration summary
* validator integrity failures
* validation request start
* validation request completion
* validation summary
* requested-rule lookup failures
* Repository Service failures
* configuration failures
* unexpected internal failures
* shutdown start
* shutdown completion

Routine successful validation logs should remain concise.

The service shall not produce direct console output.

---

# 20. Logging Safety

Logs shall not include:

* complete artifact contents
* secret values
* authentication tokens
* private keys
* credentials
* full environment-variable collections
* confidential configuration values
* raw internal exception dumps at normal log levels
* unbounded lists of findings
* arbitrary repository file contents

Logs may include safe normalized information such as:

* validation request identifier
* target type
* repository-relative path
* validation scope
* number of executed rules
* number of findings by severity
* overall status
* elapsed duration
* normalized error classification
* correlation identifier where supported

Path logging shall follow the redaction and diagnostic conventions established by the platform baseline.

---

# 21. Repository Service Integration

The Validation Service shall use Repository Service for all repository-managed artifact access.

Permitted uses include:

* verifying active repository availability
* obtaining normalized repository metadata
* obtaining normalized repository status
* checking artifact existence
* reading permitted artifacts
* listing permitted repository paths
* discovering architecture-document metadata
* obtaining repository-relative paths
* obtaining repository health information where required

The Validation Service shall not:

* call raw filesystem APIs for repository-managed artifacts
* run Git commands
* inspect .git
* resolve repository roots independently
* implement traversal protection independently where Repository Service already enforces it
* perform repository writes
* change the active repository without an explicit approved request
* reinterpret Repository Service errors as successful validation results

---

# 22. Repository Availability

Operations requiring repository context shall verify that Repository Service is ready and an active repository is available.

Where repository context is unavailable:

* validate_repository() shall fail with a classified Repository Failure.
* validate_path() shall fail if the path is repository-relative and cannot be resolved.
* validate(request) shall fail when the request requires repository access.
* list_rules() may continue if rule metadata is already available and does not require repository access.
* report(result) may continue for a previously completed immutable result where no repository access is required.

Repository unavailability shall not be misreported as a validation failure caused by the artifact.

It is an execution error.

---

# 23. Repository Error Mapping

Repository Service errors shall be mapped into Validation Service errors without exposing implementation details.

Logical mapping shall include:

Repository Failure	Validation Classification
Repository unavailable	Repository Failure
Repository not initialized	Repository Failure
Invalid repository path	Invalid Request or Repository Failure, depending on request validity
Artifact not found	Invalid Request or validation finding, depending on rule semantics
Access denied	Repository Failure
Read failure	Repository Failure
Invalid repository structure	Repository Failure or rule finding according to the invoked validation scope

The distinction between execution errors and validation findings shall remain explicit.

An infrastructure failure shall not be converted into a normal failed-validation status.

---

# 24. Validation Request Entry Point

validate(request) shall be the general validation entry point.

It shall:

1. verify service readiness
2. validate the request model
3. normalize request options
4. verify authorization and target eligibility
5. resolve the requested validation scope
6. resolve applicable rules
7. establish an immutable execution context
8. delegate execution to the Validation Engine
9. receive the immutable result
10. log the validation summary
11. return the immutable result

The Validation Service shall not contain individual rule logic.

Request and result models are defined by IWP-008-04.

Execution behavior is defined by IWP-008-02.

---

# 25. Request Validation

Every validation request shall be validated before execution.

Request validation shall check logical information equivalent to:

* validation target
* validation scope
* validation options
* reporting preferences
* requested rule identifiers
* excluded rule identifiers where permitted
* repository context where required
* path validity where required
* concurrency or execution options where exposed
* compatibility of target and scope

Invalid requests shall fail before rule execution begins.

A malformed request shall not produce a normal validation result with status Failed.

It shall raise or return the approved Invalid Request error behavior.

---

# 26. Validation Target

The Validation Service shall support the validation targets approved by AEP-002-11 and the companion validator specification.

Targets may logically include:

* one repository-managed artifact
* one repository-relative path
* one document
* one directory or package
* the active repository
* an approved in-memory representation where explicitly supported

Target representation shall be typed and explicit.

The service shall not accept arbitrary untyped objects as validation targets.

The complete target model is defined by IWP-008-04.

---

# 27. Validation Scope

Validation scope determines which approved rule groups apply.

Scope shall be:

* explicit
* typed
* deterministic
* validated before execution
* compatible with the selected target

Scopes shall not be inferred from undocumented filename heuristics where approved metadata is available.

Default scope behavior shall be deterministic.

The complete scope definitions are specified in IWP-008-03 and IWP-008-04.

---

# 28. Validation Options

Validation options shall be represented by a typed immutable model.

Options may include only architecture-approved execution behavior such as:

* requested rule identifiers
* excluded optional rules
* warning treatment
* fail-fast behavior where approved
* parallel execution where approved
* cache usage
* reporting preference
* strictness level where approved

Options shall not:

* disable mandatory architecture rules without authorization
* change rule semantics
* mutate artifact contents
* permit repository-boundary bypass
* enable probabilistic validation
* expose internal validator objects

The same request and repository state shall produce the same result regardless of internal execution scheduling.

---

# 29. validate_path(path)

validate_path(path) shall provide a convenience entry point for validating a repository-managed path.

It shall:

1. verify service readiness
2. validate the supplied path
3. use Repository Service to resolve and inspect the target
4. determine the supported target classification
5. construct a normalized validation request
6. delegate to validate(request)
7. return the immutable result

The method shall not duplicate rule execution logic.

The path shall be repository-relative unless the approved Repository Service contract explicitly permits another representation.

Unsafe paths shall be rejected through Repository Service and mapped into the appropriate Validation Service error.

---

# 30. validate_repository()

validate_repository() shall validate the active repository using the approved repository-level validation scope.

It shall:

1. verify service readiness
2. verify Repository Service readiness
3. obtain active repository metadata
4. obtain the approved repository artifact inventory
5. construct a repository validation request
6. delegate to validate(request)
7. return the immutable result

The method shall not reproduce the Repository Service’s structural repository checks.

Repository Service checks and Validation Framework rules may both contribute information, but their responsibilities shall remain distinct:

* Repository Service verifies repository usability and structural access.
* Validation Framework evaluates approved validation rules.

---

# 31. Repository Structural Results

Where Repository Service exposes structural validation results, the Validation Service may consume those normalized results as inputs to approved validation rules.

It shall not:

* reinterpret raw filesystem state independently
* alter Repository Service classifications
* duplicate repository path checks
* overwrite Repository Service status
* conflate repository unavailability with artifact non-compliance

Any rule that evaluates repository structure shall use normalized Repository Service information where available.

---

# 32. list_rules()

list_rules() shall return immutable metadata for the registered validation rules.

Rule metadata shall include logical information equivalent to:

* unique identifier
* descriptive name
* description
* target type
* validation scope
* default severity
* enabled status
* mandatory or optional status
* version where applicable

The returned collection shall:

* be deterministically ordered
* contain no mutable validator instances
* contain no executable callbacks
* avoid exposing implementation-specific classes
* remain stable for a stable initialized service state

Consumers shall not use list_rules() to invoke rules directly.

---

# 33. Rule Filtering

Where the public contract permits rule filtering, filtering shall occur using typed request criteria.

Filtering may logically include:

* target type
* validation scope
* severity
* enabled status
* mandatory status
* rule identifier

Filtering shall preserve deterministic ordering.

Unknown requested rule identifiers shall result in Rule Not Found or Invalid Request according to the operation.

The service shall not silently ignore an explicitly requested unknown rule.

---

# 34. Rule Discovery Boundary

The Validation Service shall orchestrate rule discovery through the internal Validator Registry defined by IWP-008-02.

The service shall not discover validators by:

* scanning arbitrary Python modules
* executing repository code
* importing untrusted files
* loading arbitrary entry points not approved by architecture
* evaluating dynamic expressions
* running AI inference

Discovery shall use only approved deterministic mechanisms.

The complete rule-discovery mechanism is defined in IWP-008-02 and IWP-008-03.

---

# 35. Rule Registration Boundary

The Validation Service shall ensure that discovered validators are registered before readiness is published.

Registration shall enforce:

* unique rule identifiers
* valid rule metadata
* supported target declarations
* supported scope declarations
* deterministic ordering
* interface conformance
* absence of forbidden dependencies
* read-only behavior
* approved lifecycle participation

Registration failures shall prevent successful initialization where the affected validator is mandatory.

Optional-validator failure behavior shall follow the approved companion specifications.

---

# 36. Validator Integrity Verification

Before the Validation Service becomes ready, it shall verify validator integrity through the internal registry or engine.

Integrity checks shall include logical verification that:

* the validator implements the approved public validator protocol
* the rule identifier is present
* the rule identifier is unique
* metadata is valid
* target declarations are supported
* severity is valid
* execution behavior can be invoked through the standard interface
* prohibited service dependencies are absent where deterministically inspectable
* no validator is exposed directly as a public platform service unless architecture defines it

Integrity verification shall not execute full validation against production artifacts during initialization.

---

# 37. Validation Engine Delegation

The Validation Service shall delegate rule execution to the Validation Engine.

The boundary shall preserve:

* typed requests
* immutable execution context
* deterministic selected-rule set
* deterministic rule ordering in results
* standard finding models
* standard result aggregation
* classified execution errors
* thread safety
* cancellation or failure behavior where approved

The Validation Service shall not:

* contain individual validation algorithms
* aggregate findings differently from the engine contract
* reorder findings nondeterministically
* mutate returned results
* expose internal execution futures or tasks

---

# 38. Validation Result Handling

The Validation Service shall return the standard immutable Validation Result defined by IWP-008-04.

The result shall contain logical information equivalent to:

* validation status
* validation target
* executed rules
* findings
* errors
* warnings
* informational messages
* execution summary
* execution duration

Overall status shall use the stable statuses defined by ARP-002-08:

Status	Meaning
Passed	No validation issues detected
Passed With Warnings	Validation completed with non-fatal findings
Failed	One or more validation failures detected
Error	Validation could not complete

The service shall not introduce additional overall statuses without an approved ADR.

---

# 39. Execution Errors Versus Findings

The implementation shall distinguish between:

* a validation finding, and
* a validation execution error

Examples of validation findings include:

* required metadata missing
* invalid document identifier
* broken approved reference
* prohibited dependency declared

Examples of execution errors include:

* Repository Service unavailable
* artifact cannot be read
* invalid request
* validator crashes unexpectedly
* configuration invalid
* internal engine failure

A finding contributes to Passed With Warnings or Failed.

An execution error contributes to Error or raises the approved public exception according to the public operation contract.

The distinction shall be deterministic and tested.

---

# 40. report(result)

report(result) shall generate a structured validation report from an immutable Validation Result.

It shall:

1. verify that the supplied result is valid
2. avoid re-executing validation
3. delegate formatting or report construction to the approved reporting component
4. preserve deterministic ordering
5. include required summary information
6. return the approved immutable report representation

Reports shall include logical information equivalent to:

* validation target
* execution timestamp
* executed rules
* findings
* overall status
* summary statistics

The complete report model and serialization requirements are defined by IWP-008-04.

CLI rendering requirements are defined by IWP-008-05.

---

# 41. Reporting Boundary

The Validation Service may produce structured reports.

It shall not:

* print reports directly
* write reports to arbitrary files
* select terminal colors
* include ANSI formatting
* invoke CLI renderers
* expose raw validator objects
* expose exception tracebacks by default
* include sensitive artifact contents
* modify validated artifacts

Presentation belongs to consumers such as the CLI.

Report persistence is not authorized by this companion specification.

---

# 42. Deterministic Reporting

For the same immutable Validation Result and reporting preferences, report(result) shall produce the same structured report.

Determinism shall include:

* stable rule ordering
* stable finding ordering
* stable summary counts
* stable field names
* stable severity representation
* stable status representation
* stable handling of optional values

Execution timestamps already contained in the result may be included.

The reporting operation shall not generate a new validation execution timestamp.

---

# 43. Health Behavior

ARP-002-08 exposes a logical health() operation.

The implementation shall integrate that operation with the health-result and lifecycle conventions established by the implementation baseline.

This work package shall not create the future Health Monitoring Service.

The Validation Service health result shall logically reflect:

* lifecycle state
* readiness
* Configuration Service dependency state
* Logging Service dependency state
* Repository Service dependency state
* Validation Engine readiness
* Validator Registry integrity
* last validation execution outcome where appropriate
* shutdown state

Health behavior shall not execute a complete validation run.

---

# 44. Health Contract Compatibility

ARP-002-16 states that public services participate in platform health reporting and shall not create unrelated independent health frameworks.

Therefore:

* health() shall use the approved shared health model.
* health() shall not introduce a separate health type.
* health() shall not create a background monitor.
* health() shall not aggregate unrelated services.
* IWP-013 remains responsible for the future Health Monitoring Service.
* the future Health Monitoring Service may call or consume the Validation Service health result.

If the implemented baseline materially prevents conformance with both ARP-002-08 and ARP-002-16, Cursor shall stop and report the conflict.

---

# 45. Service Registry Integration

The Validation Service shall be registered through the Service Registry implemented by IWP-005.

Registration shall:

* use the canonical service identifier
* register one singleton instance
* declare mandatory dependencies
* preserve deterministic resolution
* expose the approved public Validation Service interface
* avoid registering internal validators as public platform services
* avoid registering the internal Validation Engine as a public service unless architecture explicitly requires it
* avoid duplicate registration
* integrate with registry diagnostics

The service dependency declaration shall include:

* Configuration Service
* Logging Service
* Repository Service

---

# 46. Service Lifecycle Manager Integration

The Validation Service shall integrate with the Service Lifecycle Manager implemented by IWP-006.

Lifecycle integration shall cover:

* construction
* dependency validation
* initialization
* readiness
* operational state
* failed state
* health reporting
* shutdown

The Lifecycle Manager shall initialize the Validation Service only after its mandatory dependencies are ready.

The Validation Service shall not manage the lifecycle of baseline services.

Internal validators may have internal lifecycle behavior, but they shall remain encapsulated by the Validation Framework.

---

# 47. Bootstrap Integration

The Bootstrap Service shall include the Validation Service in platform startup.

Bootstrap integration shall:

1. construct or obtain the Validation Service through approved composition
2. inject mandatory dependencies
3. register the service
4. declare dependency ordering
5. initialize the service after Repository Service
6. publish readiness only after successful validator integrity verification
7. include safe shutdown
8. preserve all existing platform startup behavior

Bootstrap integration shall not initialize:

* Prompt Management Service
* AI Provider Service
* MCP Client Service
* Health Monitoring Service
* Event Bus Service
* Plugin Manager Service
* Platform Context Service

unless those services already exist through separately approved work packages.

---

# 48. Readiness

The Validation Service is ready only when:

* lifecycle initialization succeeded
* mandatory dependencies are ready
* validation configuration is valid
* validator discovery completed
* required validators are registered
* validator integrity checks passed
* the Validation Engine is ready
* the service is available through the Service Registry

A service with zero required validators shall not be considered ready if the approved companion specifications define mandatory validators.

Readiness shall be explicit and testable.

---

# 49. Runtime Preconditions

Public validation operations shall verify lifecycle readiness.

Operations requiring readiness include:

* validate(request)
* validate_path(path)
* validate_repository()
* list_rules()
* report(result) where reporting infrastructure requires initialization

The implementation shall follow the baseline convention for operations invoked:

* before initialization
* during initialization
* after initialization failure
* during shutdown
* after shutdown

These states shall not produce undefined behavior.

---

# 50. Shutdown Contract

shutdown() shall:

1. stop accepting new validation executions according to baseline lifecycle rules
2. allow or cancel in-flight requests according to IWP-008-02
3. release internal validation resources
4. clear service-level caches where applicable
5. release validator resources
6. transition lifecycle state correctly
7. log shutdown completion
8. avoid modifying validated artifacts
9. remain safe after partial initialization failure

Shutdown behavior shall be idempotent where required by the baseline lifecycle contract.

---

# 51. In-Flight Validation During Shutdown

The behavior of in-flight requests shall be deterministic.

The complete execution policy is defined by IWP-008-02.

At the service boundary:

* no new request shall begin after shutdown acceptance is closed
* accepted requests shall not produce partially constructed public results
* cancellation shall produce a classified execution outcome
* service state shall not be corrupted
* shutdown shall not deadlock
* returned results shall remain immutable

Cursor shall not invent background processing beyond approved engine behavior.

---

# 52. Thread Safety

The Validation Service shall support concurrent validation requests.

Concurrent execution shall:

* preserve result integrity
* preserve service state
* avoid shared mutable request state
* avoid validator registry mutation after readiness
* avoid nondeterministic rule selection
* avoid duplicate initialization
* avoid shutdown races
* preserve identical outcomes for identical inputs and repository state

Consumers shall not need to perform explicit synchronization.

---

# 53. Immutable Runtime Snapshot

After successful initialization, validator registration and rule metadata should be represented by an immutable runtime snapshot or equivalent thread-safe structure.

The service shall not allow consumers to:

* add validators
* remove validators
* mutate rule metadata
* reorder rules
* replace the engine
* change configuration through returned objects

Any future dynamic validator extension shall require the approved extension mechanism and lifecycle rules defined by architecture.

IWP-008 shall not introduce uncontrolled runtime mutation.

---

# 54. Concurrent Requests

Each validation request shall use an isolated execution context.

The context shall not contain mutable state shared with unrelated requests.

Concurrent requests may share only approved immutable or thread-safe resources, such as:

* rule metadata
* immutable registry snapshots
* safe caches
* Configuration Service snapshots
* Repository Service read operations
* Logging Service

A failure in one request shall not corrupt another request.

---

# 55. Determinism Under Concurrency

Parallel execution is permitted only where approved by AEP-002-11 and IWP-008-02.

Parallel scheduling shall not alter:

* selected rules
* overall status
* finding contents
* finding ordering in the returned result
* summary statistics
* error classifications

The implementation shall normalize asynchronous or parallel completion into deterministic result ordering.

---

# 56. Public Error Contract

The Validation Service shall expose an exception hierarchy consistent with established platform exception conventions.

Required logical error categories from ARP-002-08 are:

Error Type	Description
Invalid Request	Validation request is malformed or unsupported
Rule Not Found	Explicitly requested rule is unavailable
Validation Failure	Artifact violates one or more validation rules where exception behavior is required
Repository Failure	Repository or repository artifact cannot be accessed
Configuration Error	Validation configuration is invalid
Internal Validation Error	Unexpected validation infrastructure failure

Additional service-state errors may be represented using existing lifecycle exceptions, including:

* service not initialized
* service not ready
* service shutting down
* service stopped

The exact hierarchy shall follow baseline conventions.

---

# 57. Error Encapsulation

Implementation-specific exceptions shall not cross the public Validation Service boundary.

The service shall encapsulate:

* filesystem exceptions
* Repository Service implementation exceptions
* parser implementation exceptions
* internal registry exceptions
* internal engine exceptions
* concurrency implementation exceptions
* cache implementation exceptions
* serialization implementation exceptions

Exception chaining may be retained internally for diagnostics.

Public messages shall remain safe, concise, and deterministic.

---

# 58. Validation Failure Behavior

A normal rule violation shall usually be represented as a Validation Result with status:

* Passed With Warnings, or
* Failed

It shall not automatically raise an exception.

A Validation Failure exception classification shall be used only where required by the approved public operation or baseline error convention.

Cursor shall preserve the architecture-defined distinction between:

* completed validation with failed findings, and
* validation execution that could not complete

This distinction shall be consistent across service, engine, report, and CLI integration.

---

# 59. Unexpected Validator Failure

An unexpected validator exception shall be handled according to the execution policy in IWP-008-02.

At the service boundary:

* the raw exception shall not escape
* the failure shall be logged
* the affected execution shall produce a standard error classification
* the service shall preserve integrity
* unrelated concurrent requests shall remain unaffected
* the service shall not automatically become unhealthy unless the failure indicates persistent framework corruption

Mandatory fail-fast or continue-on-error behavior shall be controlled by approved request and engine semantics.

---

# 60. Security Requirements

The Validation Service shall:

* validate only authorized repository artifacts
* use Repository Service path controls
* avoid direct arbitrary filesystem access
* sanitize error messages
* avoid sensitive data in reports
* avoid sensitive data in logs
* fail securely
* remain read-only
* reject unsupported targets
* reject malformed requests
* reject unsafe rule identifiers
* avoid executing artifact content
* avoid importing code from validated repositories
* avoid evaluating embedded expressions
* avoid network access unless a future approved rule explicitly requires it

Validation shall not create an execution path for untrusted repository code.

---

# 61. Authorization Boundary

The Validation Service shall respect Repository Service access controls.

Successful repository access through Repository Service is the minimum authorization boundary for repository-managed validation targets.

The Validation Service shall not:

* elevate filesystem permissions
* bypass read restrictions
* retry using broader paths
* access sibling repositories
* inspect user-home files
* inspect temporary files outside the repository
* follow unsafe links outside the repository

Where future platform authorization services are introduced, they may add controls without changing the public Validation Service contract.

---

# 62. Sensitive Data Handling

Validation findings and reports shall contain only the minimum information required to identify and explain a finding.

They shall prefer:

* repository-relative paths
* rule identifiers
* line or section references
* concise descriptions
* safe remediation guidance where approved

They shall avoid:

* full document contents
* secrets detected in artifacts
* credentials
* tokens
* private keys
* complete stack traces
* arbitrary binary data
* environment dumps

A rule detecting a sensitive value shall report the type and location without reproducing the value.

---

# 63. Performance Requirements

The Validation Service shall:

* avoid unnecessary repository access
* avoid repeated request normalization
* avoid repeated rule discovery after initialization
* use immutable rule metadata snapshots
* delegate caching to approved engine or registry components
* support efficient concurrent requests
* avoid reading unrelated artifacts
* avoid executing rules outside the selected scope
* avoid generating reports unless requested
* preserve deterministic behavior under optimization

Service-level performance optimization shall not duplicate engine-level caching.

---

# 64. Service-Level Caching

The Validation Service may retain only service-level state required for:

* initialized rule metadata
* immutable registry snapshots
* validated configuration snapshot
* readiness information
* safe health information

Artifact-analysis caching and incremental-validation caching are defined by IWP-008-02.

The service shall not maintain a second competing validation-result cache.

Returned cached objects shall remain immutable.

---

# 65. Correlation and Diagnostics

Where the Logging Service and baseline execution context support correlation identifiers, validation requests shall preserve them.

Correlation behavior shall:

* pass the identifier into engine execution
* include it in safe logs
* avoid embedding it into rule semantics
* avoid changing deterministic validation outcomes
* preserve it in diagnostics where the standard model permits

The service shall not invent a separate tracing framework.

---

# 66. Public Models Used by the Service

The Validation Service shall use typed public models defined by IWP-008-04, including logical equivalents of:

* Validation Request
* Validation Target
* Validation Scope
* Validation Options
* Validation Result
* Validation Report
* Validation Status
* Rule Metadata
* Validation Finding
* Validation Summary
* Validation Health Result where required by baseline conventions

This companion specification shall not create duplicate model definitions.

Models shall be imported from the approved vibe.validation public model location.

---

# 67. Public Package Exports

The vibe.validation package shall expose only approved public symbols.

Expected public exports include logical equivalents of:

* ValidationService
* public request models
* public result models
* public status and severity enums
* public report models
* public rule metadata models
* public Validation Service exceptions
* approved validator protocol where intended for extension authors

Internal exports shall remain private.

Do not export:

* internal registry implementations
* internal engine implementations
* cache implementations
* repository adapters
* parser implementation details
* internal locks
* internal execution tasks

---

# 68. Internal Service Components

The Validation Service implementation may use internal components logically equivalent to:

* validation configuration loader adapter
* validator registry
* validation engine
* report builder
* health evaluator
* request normalizer

These are internal implementation concerns.

They shall not become independent platform services unless approved by architecture.

The physical module layout shall follow existing repository conventions and the complete IWP-008 package design.

---

# 69. Expected Package Layout

Cursor shall inspect and follow the current repository structure.

The expected logical package structure is:

src/
└── vibe/
    └── validation/
        ├── __init__.py
        ├── service.py
        ├── protocols.py
        ├── models.py
        ├── errors.py
        ├── engine.py
        ├── registry.py
        ├── reporting.py
        ├── health.py
        └── validators/

This document primarily governs:

src/vibe/validation/service.py
src/vibe/validation/__init__.py
src/vibe/validation/errors.py
src/vibe/validation/protocols.py

The complete package layout is finalized by IWP-008-06.

Not every listed module is mandatory.

Cursor may consolidate modules where that better matches the existing codebase and preserves cohesive responsibilities.

---

# 70. Integration Files

Implementation may require focused changes in:

src/vibe/bootstrap/
src/vibe/registry/
src/vibe/lifecycle/
src/vibe/config/

CLI integration shall be governed by IWP-008-05.

Changes to baseline packages shall be limited to:

* registering Validation Service
* declaring dependencies
* adding lifecycle integration
* exposing approved configuration
* exposing approved health or diagnostics integration

Do not refactor unrelated baseline behavior.

---

# 71. Service-Level Unit Tests

Service-level unit tests shall verify at minimum:

71.1 Construction

* mandatory dependencies are accepted
* missing mandatory dependencies fail deterministically
* constructor performs no repository access
* constructor performs no rule execution
* constructor performs no service registration
* constructor performs no artifact mutation

## 71.2 Initialization

* successful initialization
* dependency readiness checks
* configuration loading
* validator discovery orchestration
* validator registration orchestration
* validator integrity verification
* readiness publication
* initialization logging
* idempotent initialization
* duplicate-rule failure
* invalid configuration failure
* Repository Service unavailable failure
* internal engine preparation failure
* safe partially initialized shutdown

## 71.3 Public Operations

* validate(request) delegates correctly
* invalid request fails before engine execution
* validate_path(path) uses Repository Service
* validate_repository() uses active Repository Service context
* list_rules() returns immutable metadata
* report(result) does not re-execute validation
* health() uses the baseline health model
* shutdown() releases resources safely

## 71.4 Error Mapping

* invalid request mapping
* unknown rule mapping
* Repository Service failure mapping
* configuration error mapping
* internal engine error mapping
* lifecycle-state error mapping
* no raw implementation exception leakage

## 71.5 Thread Safety

* concurrent request entry
* initialization race prevention
* shutdown race handling
* immutable rule metadata
* isolated request context
* no result corruption

Detailed test requirements are defined in IWP-008-06.

---

# 72. Service Integration Tests

Integration tests shall verify:

* Configuration Service injection
* Logging Service injection
* Repository Service injection
* Service Registry registration
* Service Lifecycle Manager ordering
* Bootstrap startup
* Validation Service readiness
* validation through an actual temporary repository
* repository unavailability behavior
* platform shutdown
* existing baseline services remain operational

Tests shall use the real public interfaces of baseline services where practical.

Tests shall not depend on the developer’s real repository state.

---

# 73. Lifecycle Ordering Tests

Automated tests shall demonstrate the initialization order:

Configuration Service
        |
        v
Logging Service
        |
        v
Repository Service
        |
        v
Validation Service

The exact relative ordering of Configuration and Logging shall follow the already approved bootstrap baseline.

Tests shall verify that Validation Service initialization does not occur before all three mandatory dependencies are ready.

---

# 74. Public API Documentation

Every public Validation Service method shall include a docstring describing:

* purpose
* parameters
* return value
* lifecycle preconditions
* public exceptions
* thread-safety behavior
* repository behavior where applicable
* read-only behavior

The package documentation shall identify the Validation Service as the single public validation entry point.

Usage examples may be added where consistent with repository documentation conventions.

---

# 75. Backward Compatibility

The implementation shall preserve all approved public behavior from previous IWPs.

In particular:

* existing imports shall continue to work
* existing service identifiers shall remain stable
* existing lifecycle states shall remain stable
* existing Bootstrap APIs shall remain stable
* existing Repository Service APIs shall remain stable
* existing CLI framework APIs shall remain stable
* existing health-result models shall remain stable
* existing tests shall continue to pass

IWP-008 may activate deferred validation integration points established by IWP-007.

It shall not alter unrelated deferred command behavior.

---

# 76. Service Acceptance Criteria

This companion specification is complete only when:

* vibe.validation exists
* ValidationService exists
* the service is registered as a singleton
* Configuration Service is injected
* Logging Service is injected
* Repository Service is injected
* initialization occurs after mandatory dependencies
* validation configuration is loaded through Configuration Service
* validator discovery is orchestrated
* validators are registered
* validator integrity is verified
* readiness is published only after successful initialization
* validate(request) is implemented
* validate_path(path) is implemented
* validate_repository() is implemented
* list_rules() is implemented
* report(result) is implemented
* health() conforms to baseline health conventions
* shutdown() is implemented
* service-level errors are classified
* service-level logging is implemented
* artifact access occurs only through Repository Service
* validation remains read-only
* concurrent requests are supported
* service-level tests pass
* baseline regression tests pass

---

# 77. Architectural Acceptance Criteria

The Validation Service is architecturally conformant only when:

* ADR-001 is observed
* ARP-002-08 is satisfied
* AEP-002-11 is satisfied
* ARP-002-16 is satisfied
* the public package is vibe.validation
* the primary class is ValidationService
* lifetime is singleton
* mandatory dependencies are Configuration, Logging, and Repository
* no dependency on CLI exists
* no dependency on AI Provider exists
* no dependency on MCP Client exists
* no dependency on Prompt Management exists
* consumers validate exclusively through the Validation Service
* validators remain encapsulated
* Repository Service behavior is not duplicated
* validated artifacts are not modified
* public results are immutable
* status definitions remain stable
* errors remain encapsulated
* thread safety is demonstrated
* lifecycle behavior is deterministic

---

# 78. Engineering Acceptance Criteria

The service implementation is technically complete only when:

* public APIs are fully typed
* public APIs have docstrings
* request handling is deterministic
* lifecycle-state handling is deterministic
* initialization is idempotent according to baseline conventions
* duplicate registration is prevented
* duplicate rule identifiers are rejected
* public errors are documented
* logs are safe
* thread safety is tested
* shutdown is tested
* integration with baseline services is tested
* all repository quality gates pass
* packaging checks pass where configured
* no generated files are committed
* no secrets are committed
* no direct repository filesystem access exists in Validation Service code

---

# 79. Deliverables

This companion specification shall deliver:

* public Validation Service class
* public service protocol where required
* public service exceptions
* service construction and dependency injection
* configuration integration
* logging integration
* Repository Service integration
* lifecycle integration
* Service Registry integration
* Bootstrap integration
* validation request orchestration
* path-validation orchestration
* repository-validation orchestration
* rule-listing orchestration
* reporting orchestration
* health integration
* shutdown behavior
* service-level unit tests
* service-level integration tests
* required public documentation

The complete IWP-008 deliverable also requires every other companion specification.

---

# 80. Files Expected to Change

Exact files depend on the current implementation baseline.

Expected additions or modifications include:

src/vibe/validation/__init__.py
src/vibe/validation/service.py
src/vibe/validation/protocols.py
src/vibe/validation/errors.py
tests/unit/validation/test_service.py
tests/unit/validation/test_service_lifecycle.py
tests/unit/validation/test_service_errors.py
tests/unit/validation/test_service_thread_safety.py
tests/integration/validation/test_validation_service.py
tests/integration/validation/test_validation_bootstrap.py

Potential focused integration changes include:

src/vibe/bootstrap/
src/vibe/registry/
src/vibe/lifecycle/
src/vibe/config/

Do not implement CLI integration in this companion unless the complete IWP-008 implementation is being performed from the full specification set.

---

# 81. Companion Specification Dependencies

This document depends on definitions supplied by:

* IWP-008-02 for Validation Engine behavior
* IWP-008-03 for validator and rule behavior
* IWP-008-04 for public request, result, finding, rule, health, and report models
* IWP-008-05 for CLI consumers
* IWP-008-06 for complete testing, acceptance, quality gates, and completion reporting

Where a type or internal behavior is referenced but not fully defined here, Cursor shall use the corresponding companion specification.

Cursor shall not invent missing behavior before reviewing the full companion set.

---

# 82. Implementation Guidance for Cursor

When implementing the complete IWP-008 package, Cursor shall:

1. Review IWP-008-00 through IWP-008-06.
2. Inspect the current 86-vibe-cli baseline.
3. Confirm the implemented Repository Service public API.
4. Confirm Configuration Service access patterns.
5. Confirm Logging Service access patterns.
6. Confirm Service Registry identifiers and registration patterns.
7. Confirm Service Lifecycle Manager protocols and states.
8. Confirm Bootstrap composition and ordering.
9. Confirm shared health-result conventions.
10. Implement public models before finalizing service signatures.
11. Implement engine and registry protocols before wiring service orchestration.
12. Implement the Validation Service without individual rule logic.
13. register the service as a singleton.
14. declare Configuration, Logging, and Repository as mandatory dependencies.
15. integrate lifecycle readiness and shutdown.
16. add service-level tests.
17. run complete IWP-008 and baseline test suites.
18. report any architecture ambiguity instead of inventing a solution.

---

# 83. Definition of Done for IWP-008-01

IWP-008-01 is complete when:

* the Validation Service public boundary is implemented
* the public interface conforms to ARP-002-08
* the service uses the approved package and class names
* dependencies are injected
* Repository Service is used exclusively for repository-managed access
* initialization order is correct
* validator discovery and registration are orchestrated
* validator integrity is verified
* the service publishes readiness correctly
* public validation operations delegate to the Validation Engine
* rule metadata can be listed
* reports can be generated from immutable results
* health uses the established platform model
* shutdown is safe
* errors are classified
* logs are safe
* concurrent access is supported
* service-level unit tests pass
* service-level integration tests pass
* baseline regression tests pass
* no future work package behavior is introduced
* no architectural drift is introduced

Completion of IWP-008-01 alone does not authorize implementation or approval of IWP-008.

The complete work package requires successful implementation of every IWP-008 companion specification.

---

# 84. Next Companion Specification

The next companion specification is:

IWP-008-02 – Validation Engine Implementation Specification

Do not begin implementation from this document alone.

Cursor shall receive and review the complete IWP-008 specification set before implementation begins.

---
