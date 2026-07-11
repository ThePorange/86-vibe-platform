# IWP-008-02 – Validation Engine Implementation Specification

## Document Control

| Item | Value |
|---|---|
| Document ID | IWP-008-02 |
| Document Name | Validation Engine Implementation Specification |
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
| Related Specification | IWP-008-01 – Validation Service Implementation Specification |
| Supersedes | None |

---

# 1. Purpose

This document defines the implementation requirements for the internal Validation Engine delivered by:

**IWP-008 – Validation Framework**

The Validation Engine is the deterministic execution core used by the public Validation Service.

It is responsible for:
- accepting normalized validation requests from the Validation Service
- resolving applicable validation rules
- constructing isolated execution contexts
- executing validation rules
- supporting deterministic parallel execution where appropriate
- aggregating findings
- classifying execution errors
- calculating overall validation status
- producing immutable validation results
- minimizing repeated artifact analysis
- supporting approved caching and incremental validation behavior
- preserving thread safety
- preserving non-destructive execution
- exposing internal health and diagnostic information to the Validation Service

The Validation Engine is not an independently consumable public platform service.
External consumers shall use `ValidationService`.
Cursor shall treat this document as one mandatory companion within the complete IWP-008 specification set.
No implementation shall begin until all IWP-008 companion specifications have been reviewed.

---

# 2. Governing Specifications

Implementation shall conform to:
- ADR-001 – Implement Repository Service Before Validation Framework
- AEP-001-07 – Platform Architecture
- AEP-001-08 – Repository Strategy
- AEP-001-09 – Technology Strategy
- AEP-001-10 – Versioning Strategy
- AEP-002-01 – Core Platform Overview
- AEP-002-02 – Repository Directory Structure
- AEP-002-03 – Python Package Architecture
- AEP-002-05 – Configuration Management Specification
- AEP-002-06 – Logging & Diagnostics Specification
- AEP-002-10 – Repository Services Specification
- AEP-002-11 – Document Validation Framework Specification
- AEP-002-13 – Build & Packaging Specification
- AEP-002-15 – Local Development Workflow Specification
- AEP-002-16 – Platform Bootstrap Sequence Specification
- AEP-002-17 – Service Lifecycle & Dependency Management Specification
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
- IWP-008-01 – Validation Service Implementation Specification
- IWP-008-03 – Validators Implementation Specification
- IWP-008-04 – Validation Models Implementation Specification
- IWP-008-05 – CLI Integration Implementation Specification
- IWP-008-06 – Testing and Acceptance Implementation Specification

Where this implementation specification conflicts with an approved architecture document, the approved architecture document takes precedence.
Cursor shall stop and report a material ambiguity rather than inventing a design.

---

# 3. Implementation Baseline

The following implementation work packages are complete and form the immutable baseline:
- IWP-001 – Repository Foundation
- IWP-002 – Configuration Service
- IWP-003 – Logging Service
- IWP-004 – Bootstrap Service
- IWP-005 – Service Registry
- IWP-006 – Service Lifecycle Manager
- IWP-007 – CLI Framework
- IWP-009 – Repository Service

The Validation Engine shall:
1. use Configuration Service through the Validation Service or an approved injected configuration snapshot
2. use Logging Service through an approved logger
3. use Repository Service through the approved Validation Framework boundary
4. preserve the lifecycle conventions implemented by prior work packages
5. preserve all existing public APIs
6. avoid direct dependencies on CLI
7. avoid direct dependencies on future services
8. avoid introducing a second repository abstraction
9. avoid modifying baseline behavior except for a minimal defect correction required for IWP-008
10. document any such correction in the final IWP-008 completion report

---

# 4. Strict Scope

This companion specification defines:
- Validation Engine identity and responsibility
- engine protocol
- engine construction
- engine preparation
- rule registry integration
- request execution
- request-scoped execution context
- rule selection
- rule filtering
- execution planning
- deterministic rule ordering
- sequential execution
- approved parallel execution
- fail-fast behavior where permitted
- rule isolation
- result aggregation
- finding ordering
- status calculation
- error handling
- execution summaries
- timing metadata
- engine caching
- cache keys
- cache invalidation
- incremental validation
- repository-change awareness
- concurrency control
- cancellation and shutdown behavior
- internal health and diagnostics
- engine-specific tests

---

# 5. Explicitly Out of Scope

This companion specification shall not define or implement:
- the public Validation Service lifecycle
- Service Registry registration
- Bootstrap registration
- CLI commands
- CLI rendering
- complete public validation models
- complete individual validator rules
- document naming rules
- metadata rules
- structural rules
- cross-reference rules
- dependency rules
- repository-placement rules
- AI-assisted validation
- document correction
- artifact modification
- prompt rendering
- MCP communication
- plugin discovery
- event publication
- platform-context aggregation
- remote repository operations
- Git writes
- background filesystem watching
- distributed validation
- remote validation workers
- persistent external caches
- validation databases

These concerns belong to other IWP-008 companion specifications or future work packages.

---

# 6. Architectural Position

The Validation Engine is internal to the Validation Framework.

The approved execution relationship is:
```text
External Consumer
       |
       v
Validation Service
       |
       v
Validation Engine
       |
       +--------------------+
       |                    |
       v                    v
Validator Registry     Repository Service
       |
       v
Validation Rules

The Validation Engine shall not be resolved directly by external consumers.

The Validation Service shall remain the single public validation entry point.

---

# 7. Engine Identity

The expected logical implementation is:

Property	Required Value
Logical Name	Validation Engine
Package	vibe.validation
Expected Class	ValidationEngine
Visibility	Internal
Lifetime	Owned by Validation Service
Thread Safety	Required
Public Platform Service	No
Artifact Modification	Forbidden
Repository Management	Forbidden

The exact class name may follow an existing repository convention if the logical role and behavior remain identical.

The engine shall not be independently registered in the Service Registry unless an approved architecture document explicitly requires it.

---

# 8. Design Principles

The engine shall follow these principles.

## 8.1 Deterministic Execution

The same:

* validation request
* rule set
* configuration
* repository state
* artifact contents

shall produce the same:

* selected rules
* findings
* finding order
* status
* summary counts
* error classifications

Parallel scheduling shall not change observable results.

## 8.2 Non-Destructive Execution

The engine and every rule shall be read-only.

The engine shall not:

* modify artifacts
* normalize files in place
* repair metadata
* create reports inside the repository
* update configuration
* stage files
* commit files

## 8.3 Rule Isolation

A failure in one validator shall not corrupt:

* another validator
* another request
* the registry
* cached results
* engine lifecycle state

## 8.4 Standardized Results

All rule outcomes shall be converted into the standard Validation Framework models.

## 8.5 Minimal Repository Access

The engine shall avoid repeated reads and unnecessary repository traversal.

## 8.6 Internal Encapsulation

Internal scheduling, caching, registry, parser, and concurrency objects shall not cross the public Validation Service boundary.

---

# 9. Stop Conditions

Cursor shall stop and report an architectural ambiguity if:

1. AEP-002-11 and ARP-002-08 cannot be satisfied using the existing Repository Service.
2. The public models required by the engine cannot be defined without contradicting IWP-008-04.
3. Rule discovery requires importing untrusted repository code.
4. Deterministic rule selection cannot be achieved with the approved registry contract.
5. A rule requires direct filesystem access outside Repository Service.
6. A rule requires artifact modification.
7. Parallel execution would change result semantics.
8. Incremental validation requires an unapproved persistent database.
9. Cache invalidation requires a new filesystem-watching service.
10. The engine would need to become a separately registered platform service.
11. Required cancellation behavior conflicts with the existing lifecycle contract.
12. A required rule has an undefined severity or status effect.
13. The baseline lacks an approved way to represent immutable results.
14. Implementing this specification requires future Event Bus behavior.
15. Broad unrelated refactoring would be required.

Cursor shall not invent a workaround for a stop condition.

---

# 10. Engine Protocol

The internal engine shall expose operations logically equivalent to:

prepare()
execute(request, context)
list_rules()
invalidate()
health()
shutdown()

The exact signatures shall use the typed models defined by IWP-008-04.

A logical protocol may resemble:

class ValidationEngineProtocol(Protocol):
    def prepare(self) -> None:
        ...
    def execute(
        self,
        request: ValidationRequest,
        context: ValidationExecutionContext,
    ) -> ValidationResult:
        ...
    def list_rules(self) -> tuple[RuleMetadata, ...]:
        ...
    def invalidate(self) -> None:
        ...
    def health(self) -> HealthResult:
        ...
    def shutdown(self) -> None:
        ...

This example is illustrative.

Cursor shall follow existing synchronous or asynchronous conventions in the codebase.

Observable behavior shall conform to this specification.

---

# 11. Construction

The engine constructor shall receive only explicit dependencies.

Logical dependencies may include:

* Validator Registry
* approved logger
* Repository Service protocol or an approved repository-access facade
* immutable validation configuration
* clock abstraction where existing testing conventions require one
* executor or scheduling abstraction where parallel execution is approved
* cache implementation owned by the Validation Framework

Construction shall not:

* discover rules
* read repository artifacts
* execute rules
* initialize baseline services
* modify repository contents
* create uncontrolled process-global executors
* publish readiness
* register public services

---

# 12. Engine Preparation

prepare() shall make the engine ready for validation execution.

It shall logically:

1. verify configuration
2. obtain the immutable registry snapshot
3. verify rule metadata
4. verify unique identifiers
5. establish deterministic rule ordering
6. establish allowed execution policies
7. initialize internal caches
8. initialize approved bounded execution resources
9. publish internal readiness to the Validation Service

Preparation shall not execute validation rules against production artifacts.

Preparation shall be idempotent according to the Validation Service lifecycle.

---

# 13. Preparation Failure

The engine shall fail preparation if:

* registry integrity is invalid
* rule identifiers are duplicated
* rule metadata is incomplete
* mandatory rules are unavailable
* configuration references unknown rules
* configured execution limits are invalid
* required dependencies are unavailable
* deterministic rule ordering cannot be established
* internal execution resources cannot be prepared safely

A failed engine shall not accept validation requests.

The failure shall be mapped to Configuration Error or Internal Validation Error as appropriate.

---

# 14. Validator Registry Integration

The engine shall consume an immutable validator registry snapshot.

The registry shall provide logical access to:

* validator implementation
* rule identifier
* rule metadata
* supported target types
* supported validation scopes
* default severity
* mandatory or optional classification
* enabled status
* execution-order metadata only where explicitly approved

The engine shall not permit runtime registry mutation after readiness.

Dynamic additions during an active runtime are out of scope unless the approved architecture explicitly defines them.

---

# 15. Unique Rule Identity

Every rule shall have one unique identifier.

Rule identifiers shall be:

* non-empty
* deterministic
* stable
* case-handled consistently
* valid according to the model defined by IWP-008-04
* unique within one initialized Validation Framework

Duplicate identifiers shall prevent successful engine preparation.

The engine shall not silently select one duplicate implementation.

---

# 16. Rule Independence

Rules shall remain independent where practical, as required by ARP-002-08.

A rule shall not depend on another rule’s mutable execution output.

Where cross-document or dependency validation requires shared indexed information, that information shall be supplied through an immutable execution context or approved analysis snapshot.

Rules shall not:

* invoke other rules directly
* mutate registry state
* alter another rule’s findings
* change request configuration
* suppress another rule’s findings
* rely on execution timing

---

# 17. Validation Execution Context

Every request shall receive an isolated immutable or safely encapsulated execution context.

The context shall contain only approved information such as:

* request identifier
* correlation identifier
* validation target
* validation scope
* normalized options
* reporting preferences
* repository metadata
* repository-relative target path
* document inventory
* artifact-access interface
* immutable configuration snapshot
* execution start timestamp
* cancellation state where supported
* shared immutable analysis data
* cache-access interface where approved

The context shall not expose:

* mutable registry state
* raw filesystem paths outside repository policy
* CLI objects
* terminal renderers
* global mutable configuration
* raw Git implementation objects
* write-capable repository operations

---

# 18. Context Construction

The Validation Service shall normalize the request before delegating to the engine.

The engine may enrich the execution context with derived immutable data.

Context construction shall:

1. verify engine readiness
2. preserve the normalized request
3. obtain required repository metadata
4. obtain the approved target inventory
5. calculate applicable cache identity
6. create request-scoped diagnostics
7. freeze or encapsulate shared request data
8. avoid repeated repository access where practical

A context-construction failure is an execution error, not a normal rule finding.

---

# 19. Validation Targets

The engine shall execute only against supported typed validation targets.

Targets are defined by IWP-008-04 and may include logical equivalents of:

* artifact
* document
* directory
* package
* repository
* approved in-memory document representation

The engine shall reject:

* unknown target types
* arbitrary untyped Python objects
* unsupported binary objects
* paths outside the active repository
* write-capable target handles
* executable repository modules

Target normalization shall occur before rule selection.

---

# 20. Validation Scope Resolution

Validation scope determines the rule groups eligible for execution.

Scope resolution shall:

* use typed scope values
* validate target compatibility
* apply deterministic defaults
* resolve configured rule groups
* include mandatory rules
* exclude only optional rules where permitted
* reject contradictory options
* reject unsupported target/scope combinations

The engine shall not infer scope using undocumented heuristics.

---

# 21. Rule Selection

For every request, the engine shall build a deterministic selected-rule set.

Selection shall consider:

* target type
* validation scope
* mandatory rules
* enabled optional rules
* explicitly requested rules
* explicitly excluded optional rules
* configuration
* rule compatibility
* rule availability

The selected set shall be immutable for the duration of the request.

---

# 22. Mandatory Rules

Mandatory rules shall always execute when:

* their target type is applicable
* their scope is applicable
* the request is otherwise valid

A request shall not disable a mandatory rule unless the architecture explicitly permits it.

A configuration value shall not disable a mandatory rule unless the architecture explicitly permits it.

Attempting to exclude a mandatory rule shall result in Invalid Request or Configuration Error, depending on where exclusion is declared.

---

# 23. Explicit Rule Requests

Where a request specifies one or more rule identifiers:

* every identifier shall be validated
* unknown identifiers shall produce Rule Not Found
* duplicate identifiers shall be normalized or rejected consistently
* selected rules shall remain subject to target compatibility
* mandatory applicable rules shall remain included unless the public contract explicitly defines rule-only execution

The exact request semantics shall be made explicit in IWP-008-04.

The engine shall not silently ignore an explicitly requested rule.

---

# 24. Optional Rule Exclusions

Optional rules may be excluded only where allowed by configuration and request semantics.

Exclusion shall:

* use rule identifiers
* preserve mandatory rules
* preserve deterministic selection
* be recorded in the execution summary where appropriate
* not modify registry state
* apply only to the current request

Unknown excluded identifiers shall not be silently ignored.

---

# 25. Rule Ordering

The engine shall establish deterministic rule order before execution.

Unless approved architecture defines explicit dependencies, ordering shall use stable rule metadata such as:

1. validation lifecycle phase
2. rule category
3. rule identifier

The precise ordering key shall be documented and tested.

Execution order may differ internally under parallel execution, but returned rule and finding order shall follow the deterministic canonical order.

---

# 26. Validation Lifecycle Pipeline

AEP-002-11 defines the logical validation lifecycle:

Discover Documents
         |
         v
Extract Metadata
         |
         v
Run Validation Rules
         |
         v
Validate References
         |
         v
Validate Dependencies
         |
         v
Generate Results

The engine shall preserve this lifecycle logically.

Implementation may optimize internal work, but it shall not:

* validate references before the required document index exists
* validate dependencies before identifiers are known
* generate final results before all required phases complete
* expose phase-order differences to consumers
* change result semantics

---

# 27. Execution Phases

The engine shall represent validation execution using approved logical phases.

Expected phases are:

Phase	Purpose
Discovery	Obtain target and governed-document inventory
Extraction	Extract normalized metadata and structural information
Core Validation	Execute naming, metadata, structure, and repository rules
Reference Validation	Validate cross-document references
Dependency Validation	Validate declared dependencies and cycles
Aggregation	Normalize findings and calculate overall status
Reporting Preparation	Produce immutable result and summary metadata

Exact enum names shall follow IWP-008-04.

Phase identities shall be stable and testable.

---

# 28. Phase Preconditions

Each phase shall verify required prior outputs.

Examples:

* Extraction requires discovered documents.
* Metadata rules require extracted metadata.
* Cross-reference rules require a document identifier index.
* Dependency validation requires normalized dependency declarations.
* Aggregation requires completed rule outcomes or classified execution errors.

A missing phase prerequisite caused by infrastructure failure shall produce an execution error.

A missing required document property shall produce a validation finding where that is the rule’s purpose.

---

# 29. Shared Analysis Snapshot

To minimize repeated artifact analysis, the engine may construct an immutable shared analysis snapshot.

The snapshot may contain:

* discovered document inventory
* extracted front matter or document-control metadata
* normalized headings
* normalized identifiers
* normalized references
* normalized dependency declarations
* repository-relative paths
* repository classification
* duplicate identifier index
* reference lookup index
* dependency graph input

The snapshot shall:

* be request-scoped or safely cached
* remain immutable once provided to rules
* contain no write-capable handles
* avoid exposing sensitive full contents unnecessarily
* be invalidated when relevant repository state changes

---

# 30. Artifact Loading

Artifact content shall be obtained through Repository Service.

The engine shall:

* request only required artifacts
* use repository-relative paths
* avoid direct filesystem reads
* avoid reading .git
* avoid loading binary artifacts unless explicitly supported
* avoid retaining full content beyond the request or approved cache policy
* classify repository read failures as execution errors
* avoid logging artifact contents

Rules should consume normalized artifact representations rather than repeatedly re-reading the same file.

---

# 31. Metadata Extraction

Metadata extraction is part of the validation lifecycle but shall remain distinct from metadata validation.

Extraction shall:

* parse required document-control information
* preserve missing values explicitly
* retain safe source locations
* avoid treating absence as an extraction crash
* provide normalized values to metadata validators
* remain deterministic
* avoid altering source text

Missing required metadata becomes a finding produced by a validator.

Unreadable or unparseable artifacts may produce execution errors or structural findings according to the approved validator contract.

---

# 32. Rule Invocation Contract

The engine shall invoke every validator through the public or internal validator protocol defined by IWP-008-03.

A logical invocation may be equivalent to:

outcome = validator.validate(context, target)

Every invocation shall receive:

* immutable rule metadata
* immutable or safely encapsulated context
* supported normalized target
* no write-capable artifact interface

Every invocation shall return:

* a standard rule outcome
* zero or more standard findings
* approved diagnostics
* no mutable engine state

---

# 33. Rule Outcome

Each rule invocation shall produce a normalized outcome defined by IWP-008-04.

It shall logically contain:

* rule identifier
* execution status
* findings
* diagnostics
* duration
* target reference
* optional error classification

Rule execution status shall be distinct from overall validation status.

Expected logical rule states may include:

* completed
* skipped
* error
* cancelled

The exact model is defined by IWP-008-04.

---

# 34. Rule Findings

A rule may return zero or more findings.

Every finding shall conform to the standard model and include logical information equivalent to:

* rule identifier
* severity
* message
* target
* repository-relative path where applicable
* source location where available
* safe remediation guidance where approved
* related document identifier where applicable

Rules shall not return arbitrary dictionaries as public outcomes.

The engine shall reject or classify invalid rule output.

---

# 35. Rule Output Validation

The engine shall validate rule outcomes before aggregation.

It shall check:

* matching rule identifier
* valid execution status
* valid finding model
* valid severities
* deterministic target references
* no mutable implementation objects
* no unsupported values
* no artifact-content leakage in prohibited fields

Malformed rule output shall be classified as Internal Validation Error.

The engine shall not silently repair materially invalid validator output.

---

# 36. Sequential Execution

The engine shall support deterministic sequential execution.

Sequential execution shall be used when:

* parallel execution is disabled
* a rule phase requires ordered prerequisites
* execution resources are constrained
* a test requires deterministic single-thread behavior
* a validator is not declared safe for concurrent execution
* the architecture prohibits parallelism for that rule group

Sequential execution shall use canonical rule order.

---

# 37. Parallel Execution

ARP-002-08 permits independent rules to execute in parallel where appropriate.

Parallel execution shall be limited to rules that:

* are independent
* operate on immutable context
* have no shared mutable state
* have no ordering dependency
* are declared thread-safe
* belong to a phase that permits parallel execution

Parallel execution shall not span dependencies that require ordered phase completion.

---

# 38. Bounded Parallelism

Parallel execution shall be bounded.

The engine shall not:

* create one unbounded thread per rule
* create unbounded process pools
* create a new uncontrolled executor for every request
* allow configuration to request invalid worker counts
* exhaust system resources
* use remote workers

Worker limits shall come from approved validated configuration.

A safe deterministic default shall be used where architecture permits.

---

# 39. Parallel Result Normalization

Parallel completion order shall never determine returned ordering.

After parallel execution, outcomes shall be normalized using canonical rule order.

Findings shall be normalized using the finding-order rules in this specification.

Summary counts shall not depend on task completion order.

Tests shall intentionally vary completion order and prove identical returned results.

---

# 40. Fail-Fast Behavior

Fail-fast execution may be supported only where approved by request options and architecture.

Fail-fast shall mean stopping further eligible rule execution after a defined condition.

Permitted trigger conditions may include:

* a Critical finding
* an unrecoverable execution error
* invalid shared analysis state
* cancellation request

Fail-fast shall not change findings already produced.

The result shall record:

* rules executed
* rules skipped
* fail-fast reason
* overall status

Default behavior shall be complete deterministic validation unless the architecture or configuration defines otherwise.

---

# 41. Critical Findings

AEP-002-11 defines Critical severity as:

Validation cannot continue.

When a Critical finding occurs:

* the engine shall preserve the finding
* dependent validation phases may be skipped
* independent safe rules may continue only if approved execution policy permits
* the final result shall not be Passed or Passed With Warnings
* the execution summary shall record the interruption
* no artifact shall be modified

Critical findings remain validation findings, not necessarily infrastructure exceptions.

---

# 42. Validator Exceptions

Unexpected validator exceptions shall not cross the engine boundary.

The engine shall:

1. capture the exception
2. log a safe diagnostic
3. create a classified rule execution error
4. preserve the rule identifier
5. preserve request integrity
6. apply fail-fast or continue policy
7. avoid corrupting shared state
8. avoid leaking stack traces in public output

The original exception may be chained internally.

---

# 43. Continue-on-Error Behavior

Where execution policy permits continuing after a validator error:

* the failed rule shall be marked as error
* remaining independent rules may continue
* dependent phases shall run only if prerequisites remain valid
* overall validation status shall be Error
* valid findings from completed rules shall be retained
* the summary shall distinguish findings from execution errors

An execution error shall not be downgraded to a normal validation failure.

---

# 44. Cancellation

Cancellation may be supported to enable safe lifecycle shutdown or approved caller cancellation.

Cancellation shall:

* use an internal cancellation token or baseline-approved mechanism
* stop scheduling new rule executions
* allow in-flight work to complete or cancel safely
* avoid returning partial mutable state
* classify cancelled rules explicitly
* produce an immutable final or classified cancellation outcome
* avoid deadlocks

Cancellation shall not rely on forcefully terminating unsafe Python threads.

---

# 45. Execution Timeout

Rule or request timeouts shall be implemented only if approved configuration or existing architecture requires them.

Where supported:

* timeouts shall be bounded and validated
* timeout behavior shall be deterministic
* timed-out rules shall be classified as execution errors
* no unsafe thread termination shall occur
* the result shall record the timeout safely
* artifact state shall remain unchanged

Do not invent arbitrary timeout defaults that alter contract behavior.

---

# 46. Result Aggregation

After rule execution, the engine shall aggregate outcomes into one immutable Validation Result.

Aggregation shall include:

* target identity
* selected rule identifiers
* executed rule identifiers
* skipped rule identifiers
* normalized findings
* execution errors
* severity counts
* rule-status counts
* overall status
* execution duration
* validation timestamp
* execution summary
* cache information where safely reportable
* request identifier where supported

The result model is defined by IWP-008-04.

---

# 47. Finding Deduplication

Duplicate references are permitted by AEP-002-11.

Therefore, the engine shall not blindly deduplicate all findings.

Findings may be deduplicated only where:

* they are semantically identical
* the originating rule explicitly permits consolidation
* source locations remain accurately represented
* no required occurrence information is lost

By default, separate source occurrences shall remain separate findings.

The deduplication policy shall be deterministic and tested.

---

# 48. Finding Ordering

Findings shall be returned in a deterministic canonical order.

Unless IWP-008-04 defines a stricter order, sort logically by:

1. canonical rule order
2. target repository-relative path
3. source location
4. severity rank
5. stable message or finding identifier

The ordering key shall be documented.

Parallel execution shall not alter this order.

---

# 49. Severity Model

AEP-002-11 requires these severities:

Severity	Description
Information	Informational only
Warning	Valid but requires attention
Error	Validation failure
Critical	Validation cannot continue

The engine shall use only the approved standard severity model.

Severity definitions shall remain consistent across all rules.

The engine shall not create validator-specific severity systems.

---

# 50. Severity Overrides

Severity overrides may be supported only where approved configuration explicitly permits them.

Overrides shall:

* be validated at initialization
* apply only to approved optional rules
* not downgrade Critical findings where architecture prohibits it
* not disable mandatory rules
* be recorded in rule metadata or execution diagnostics
* remain deterministic

If the architecture does not authorize severity overrides, they shall not be implemented.

---

# 51. Overall Status Calculation

Overall validation status shall be one of:

* Passed
* Passed With Warnings
* Failed
* Error

The engine shall calculate status deterministically.

Required precedence is logically:

1. Error when validation could not complete due to one or more execution errors that affect result completeness.
2. Failed when validation completed sufficiently and contains one or more Error or Critical validation findings.
3. Passed With Warnings when validation completed without Error or Critical findings but contains one or more Warning findings.
4. Passed when validation completed with no Warning, Error, or Critical findings.

Information findings do not prevent Passed.

The precise interaction between retained findings and execution errors shall be tested.

---

# 52. Critical Status Effect

A Critical finding shall result in overall status Failed unless an execution error also requires overall status Error.

Critical indicates that validation cannot continue, but it remains a validation finding when generated correctly by a rule.

The summary shall preserve both:

* Critical finding count
* any phases or rules skipped because of the Critical finding

---

# 53. Execution Error Status Effect

Any unrecoverable engine, repository, configuration, or validator-execution error that prevents complete validation shall produce overall status Error.

The engine may preserve valid findings generated before the error.

The report shall clearly distinguish:

* artifact non-compliance
* framework execution failure

---

# 54. Execution Summary

The execution summary shall contain logical information equivalent to:

* total rules selected
* total rules executed
* total rules completed
* total rules skipped
* total rules errored
* findings by severity
* documents discovered
* documents validated
* validation duration
* cache hits
* cache misses where appropriate
* phases completed
* early termination reason where applicable

The summary shall be immutable and deterministically generated.

---

# 55. Execution Timing

Validation results shall include execution duration.

Timing shall:

* use a monotonic clock for duration
* use an approved wall-clock value for execution timestamp
* avoid affecting result status
* avoid affecting cache identity
* remain suitable for deterministic tests through an existing clock abstraction or tolerant assertions

Results may naturally contain different timestamps and durations across runs.

Determinism applies to semantic validation outcomes, not identical runtime timestamps.

---

# 56. Validation Timestamp

The execution timestamp shall represent when validation began or another clearly documented approved point.

It shall:

* be timezone aware
* use the platform timestamp convention
* be immutable
* be preserved in reports
* not be regenerated by report(result)

The timestamp shall not be derived from filesystem modification time.

---

# 57. Engine Caching

ARP-002-08 requires minimizing repeated artifact analysis.

The engine may cache:

* normalized artifact analysis
* parsed metadata
* heading indexes
* document identifiers
* reference indexes
* dependency graph inputs
* complete rule outcomes where safe
* complete request results where safe and deterministic

Caching shall remain internal to the Validation Framework.

---

# 58. Cache Constraints

The cache shall:

* be in-process
* be thread-safe
* be bounded where practical
* avoid external databases
* avoid distributed infrastructure
* return immutable values
* avoid stale cross-repository reuse
* avoid storing sensitive content unnecessarily
* be cleared on shutdown
* be invalidated on explicit framework invalidation
* preserve identical semantics with caching enabled or disabled

A cache hit shall never skip a mandatory rule when the cached identity is uncertain.

---

# 59. Cache Key Requirements

A cache key shall include all state that can affect the cached outcome.

Logical inputs may include:

* repository identity
* repository revision or approved state marker
* repository-relative target path
* artifact content digest or approved state fingerprint
* validation scope
* selected rule identifiers
* rule versions
* normalized validation options
* relevant configuration version
* parser or framework version where required

Timestamps alone shall not be trusted as the sole cache identity.

Cache keys shall not contain secrets.

---

# 60. Repository State Fingerprint

The engine may use Repository Service metadata to establish a repository-state fingerprint.

Permitted inputs may include:

* repository identifier
* current commit identifier
* clean or modified status
* active branch
* target artifact content digest
* approved repository version

A clean repository at a stable commit may use commit identity as part of the fingerprint.

A modified repository requires additional artifact-level state information.

The engine shall not assume that the same commit means unchanged uncommitted files.

---

# 61. Content Digests

Where artifact content digests are required:

* use a stable standard-library cryptographic hash
* calculate only for artifacts required by the request
* avoid logging digest source contents
* avoid treating the digest as a security signature
* normalize input bytes consistently
* avoid storing full contents solely to retain the digest

The chosen digest algorithm shall follow existing repository conventions.

Do not add a dependency solely for hashing.

---

# 62. Complete Result Caching

Complete Validation Result caching may be used only when the engine can establish a complete deterministic identity for:

* target state
* rule set
* configuration
* validation options
* framework version

Cached results shall remain immutable.

The engine shall not update the original execution timestamp to make a cached result appear newly executed unless the model explicitly distinguishes cached retrieval time from validation execution time.

Cache usage shall be transparent in semantic outcome.

---

# 63. Analysis Caching

Where complete result caching is unsafe, the engine should prefer caching intermediate immutable analysis.

Examples include:

* parsed document metadata
* normalized heading structure
* document identifier extraction
* reference extraction
* dependency declaration extraction

Rules shall then execute normally against the cached analysis.

This preserves rule execution while minimizing repeated artifact parsing.

---

# 64. Cache Invalidation

The engine shall invalidate caches when:

* Validation Service initializes with changed configuration
* active repository changes
* Repository Service refresh identifies changed state
* a target artifact fingerprint changes
* selected rule versions change
* validator registry snapshot changes
* explicit invalidate() is called
* engine shuts down

The engine shall not require a filesystem watcher.

---

# 65. Explicit Invalidation

invalidate() shall:

* be thread-safe
* prevent publication of partially cleared state
* clear relevant cached results and analysis
* preserve registry integrity
* not modify repository contents
* allow subsequent requests to rebuild state
* log a concise diagnostic where appropriate

Invalidation shall not unregister validators.

---

# 66. Incremental Validation

Incremental validation may reuse analysis for unchanged artifacts while revalidating changed artifacts and dependent cross-document relationships.

Incremental validation shall:

* preserve identical final semantics to complete validation
* detect changed target artifacts using approved fingerprints
* rebuild affected indexes
* re-execute affected rules
* re-evaluate references affected by changed identifiers
* re-evaluate dependency validation affected by changed dependencies
* avoid stale findings from removed or renamed documents
* preserve deterministic ordering

Incremental validation is an optimization, not a different validation mode.

---

# 67. Incremental Validation Boundaries

For repository-wide validation, a change to one document may affect:

* its own naming findings
* its own metadata findings
* its own structural findings
* references from other documents
* references to other documents
* identifier uniqueness
* dependency graph cycles
* package inventory

The engine shall conservatively revalidate dependent relationships.

It shall prefer correct broader revalidation over unsafe narrow reuse.

---

# 68. No Background Monitoring

Incremental validation shall be triggered by an explicit validation request or approved refresh/invalidation event.

The engine shall not:

* watch the filesystem
* run background scans
* poll repositories
* subscribe to an Event Bus not yet implemented
* perform validation asynchronously without a request

Background monitoring belongs to future architecture if ever approved.

---

# 69. Thread Safety

The Validation Engine shall be thread-safe.

Thread safety shall cover:

* preparation
* request execution
* registry snapshot access
* cache access
* cache invalidation
* health inspection
* cancellation
* shutdown

Consumers shall not require external synchronization.

---

# 70. Shared Mutable State

The engine shall minimize shared mutable state.

Permitted shared mutable state may include:

* bounded thread-safe cache
* lifecycle state
* active execution counter
* shutdown cancellation state
* bounded executor state

All other request-specific state shall remain isolated.

The registry snapshot and rule metadata shall be immutable after preparation.

---

# 71. Request Isolation

Every request shall have:

* unique execution identity
* isolated execution context
* isolated result builder
* isolated finding collection
* isolated error collection
* isolated timing information
* isolated cancellation view

One request shall not:

* append findings to another
* reuse another result builder
* change another request’s options
* suppress another request’s rule execution
* corrupt another request’s cache identity

---

# 72. Cache Thread Safety

Cache behavior shall be safe under concurrent:

* reads
* writes
* invalidation
* repository changes
* engine shutdown

The cache shall not expose mutable objects.

Where duplicate concurrent computation occurs, it may be permitted if:

* results remain identical
* state remains correct
* no corruption occurs

Avoid excessive complexity solely to prevent harmless duplicate computation.

---

# 73. Executor Ownership

Where a thread or task executor is used, ownership shall be explicit.

The executor shall:

* be created during approved engine preparation
* be bounded
* be reused safely
* not be process-global unless established by baseline architecture
* reject new work during shutdown
* be shut down safely
* avoid leaking threads after tests

Do not create an executor inside every rule invocation.

---

# 74. Deadlock Prevention

The engine shall avoid:

* invoking validators while holding global registry locks
* invoking logging callbacks while holding avoidable cache locks
* waiting on executor tasks while holding locks required by those tasks
* invalidating caches while holding request result locks
* recursive engine invocation

Lock acquisition order shall be simple and documented where multiple locks are unavoidable.

---

# 75. Re-Entrancy

Validators shall not recursively invoke the public Validation Service or Validation Engine for the same request.

Recursive validation execution may cause:

* duplicate findings
* deadlocks
* uncontrolled work
* inconsistent caches

If nested validation is required for a normalized subtarget, it shall be represented as part of the engine’s approved execution plan, not as a validator-initiated service call.

---

# 76. Engine Health

The engine shall expose internal health information to the Validation Service.

Engine health shall logically reflect:

* preparation state
* registry snapshot availability
* registry integrity
* executor readiness where applicable
* cache integrity
* shutdown state
* recent persistent internal failure where appropriate

Engine health shall not execute a validation request.

It shall use the platform’s approved health model or an internal model mapped into it by Validation Service.

---

# 77. Diagnostics

Internal diagnostics may include:

* prepared rule count
* enabled rule count
* mandatory rule count
* optional rule count
* active request count
* cache entry count
* cache hit and miss counters
* executor state
* last execution status
* last internal error classification

Diagnostics shall not expose:

* artifact contents
* secrets
* raw exception tracebacks
* mutable validator instances
* internal thread objects
* raw cache keys containing sensitive path data

---

# 78. Logging Requirements

The engine shall use the Logging Service through an injected logger.

It shall log:

* engine preparation
* rule registry summary
* validation execution start
* validation execution completion
* rule execution errors
* phase interruption
* cache invalidation
* unexpected engine failures
* engine shutdown

Routine successful individual rules should not generate noisy informational logging.

Debug logging may provide rule timing and cache behavior where safe.

---

# 79. Logging Context

Where supported by the Logging Service, engine logs shall preserve:

* correlation identifier
* validation request identifier
* target type
* repository-relative target
* rule identifier
* execution phase
* overall status
* elapsed duration

The engine shall not create a separate correlation framework.

---

# 80. Error Classification

Engine errors shall map to the Validation Service error contract.

Logical mappings include:

Engine Failure	Public Classification
Invalid normalized request	Invalid Request
Unknown explicitly requested rule	Rule Not Found
Registry configuration invalid	Configuration Error
Repository data unavailable	Repository Failure
Validator returns normal failed finding	Validation Result with Failed status
Validator crashes unexpectedly	Internal Validation Error or Error result
Engine not prepared	Service lifecycle error or Internal Validation Error
Cache corruption	Internal Validation Error
Cancellation during shutdown	Classified cancellation/error behavior

Implementation-specific exceptions shall remain encapsulated.

---

# 81. Rule Failure Versus Engine Failure

A rule that correctly detects non-compliance has not failed operationally.

Examples:

* missing metadata
* invalid filename
* broken cross-reference
* duplicate document identifier
* dependency cycle warning

These produce findings.

A rule operational failure occurs when:

* it crashes
* returns malformed output
* cannot access required approved analysis
* violates the validator contract
* attempts a forbidden operation

Operational failures contribute to overall Error.

---

# 82. Repository Failures

Repository Service failures shall not be converted into ordinary artifact findings unless the specific rule is evaluating repository structure from an available normalized result.

Examples:

* Repository Service unavailable: execution error
* artifact cannot be read due to permissions: execution error
* document missing required declared metadata: validation finding
* referenced document identifier absent from discovered inventory: validation finding

The distinction shall be explicit in tests.

---

# 83. Security Requirements

The engine shall:

* execute only registered trusted validators
* avoid importing code from validated repositories
* avoid evaluating Markdown code blocks
* avoid executing document examples
* avoid shell invocation
* avoid network access
* use Repository Service for artifact access
* preserve repository boundaries
* sanitize diagnostics
* remain read-only
* reject malformed rule output
* fail securely

---

# 84. Untrusted Document Content

Governed documents shall be treated as untrusted data.

The engine and validators shall not:

* evaluate embedded Python
* execute shell commands
* resolve arbitrary external URLs
* import referenced modules
* load executable plugins declared in documents
* expand unsafe entity references
* deserialize arbitrary objects
* process templates as executable code

Markdown parsing shall remain data-only.

---

# 85. Parser Safety

Any parser used by the engine or validators shall:

* be architecture approved or already present
* operate without code execution
* be bounded where practical
* handle malformed input safely
* avoid external resource loading
* preserve source-location information where required
* encapsulate parser-specific exceptions

Before adding a parser dependency, Cursor shall inspect existing approved dependencies.

---

# 86. Resource Safety

The engine shall avoid uncontrolled resource use when processing:

* very large documents
* deeply nested headings
* very large document inventories
* large dependency graphs
* repeated references
* malformed Markdown

Do not invent arbitrary user-visible limits that contradict architecture.

Use reasonable internal safeguards consistent with existing platform standards.

Any explicit configured limits shall be documented and tested.

---

# 87. Dependency Graph Analysis

Dependency graph validation requires a deterministic graph representation.

The engine shall provide normalized graph input to dependency validators.

The graph shall:

* use document identifiers as nodes
* use declared dependencies as directed edges
* preserve duplicate declaration information where needed
* represent missing targets explicitly
* avoid nondeterministic set iteration
* remain immutable during validation

Cycle detection behavior is defined by IWP-008-03.

---

# 88. Cross-Reference Index

Cross-reference validation requires a normalized reference index.

The engine shall provide logical mappings for:

* document identifier to document metadata
* source document to referenced identifiers
* referenced identifier to source occurrences
* duplicate identifiers
* unresolved identifiers

The index shall be deterministic and immutable.

Duplicate references are permitted and shall remain representable.

---

# 89. Document Inventory

Repository-wide validation shall use a deterministic governed-document inventory.

The inventory shall include supported categories:

* Architecture
* Specifications
* Prompts
* Guides
* Policies
* Reference

The inventory shall use Repository Service discovery and listing behavior.

It shall not scan arbitrary files outside approved repository locations.

Classification rules are defined by IWP-008-03.

---

# 90. Unsupported Documents

Files outside supported governed-document categories shall not automatically be treated as validation errors.

The engine shall:

* include only approved validation targets
* skip unsupported files deterministically
* record skip information only where useful
* avoid reading unrelated repository contents
* preserve repository independence

A malformed file inside a governed-document location may still be a validation target according to rule definitions.

---

# 91. Skipped Rules

A rule may be skipped only for a defined reason, such as:

* incompatible target type
* incompatible scope
* optional rule excluded
* missing prerequisite after a Critical finding
* cancellation
* fail-fast interruption

Every skip shall be represented deterministically in the rule outcome or execution summary.

The engine shall not silently omit a selected applicable rule.

---

# 92. Rule Versioning

Rule metadata may include a version where required by the model.

Rule versions shall:

* be stable
* participate in cache identity where outcomes may change
* follow approved version conventions
* not be derived from runtime object identity

Adding or changing a rule version shall not modify public status definitions.

---

# 93. Extension Compatibility

ARP-002-08 requires future validators to integrate without affecting existing consumers.

The engine shall therefore:

* depend on the validator protocol, not concrete validator classes
* use standard findings
* use standard outcomes
* preserve stable result models
* preserve deterministic registry ordering
* avoid consumer-facing knowledge of specific implementations

Adding a compliant future validator may add findings or rule metadata but shall not require changing the Validation Service public interface.

---

# 94. Internal APIs

Internal engine APIs shall remain narrow.

Expected logical internal components may include:

* ValidationEngine
* ValidationExecutionContext
* ValidationPlan
* ValidationResultBuilder
* ValidationCache
* AnalysisSnapshot
* RuleExecutor
* ExecutionCoordinator

Not every component requires a separate class or file.

Cursor shall avoid unnecessary abstraction.

---

# 95. Expected Package Layout

Cursor shall inspect and follow the current repository structure.

The expected logical layout is:

src/
└── vibe/
    └── validation/
        ├── engine.py
        ├── registry.py
        ├── execution.py
        ├── context.py
        ├── planning.py
        ├── aggregation.py
        ├── cache.py
        ├── analysis.py
        ├── protocols.py
        ├── models.py
        └── errors.py

This companion primarily governs:

src/vibe/validation/engine.py
src/vibe/validation/execution.py
src/vibe/validation/context.py
src/vibe/validation/planning.py
src/vibe/validation/aggregation.py
src/vibe/validation/cache.py
src/vibe/validation/analysis.py

Files may be consolidated where that better matches established coding conventions.

---

# 96. Internal Visibility

Engine implementation types shall remain internal unless explicitly identified as public by IWP-008-04.

Do not export from vibe.validation.__init__:

* concrete cache classes
* executor classes
* result builders
* mutable planning objects
* internal analysis snapshots
* internal locks
* executor handles

The public Validation Service may export public protocols and models only as approved.

---

# 97. Engine Unit Tests

Unit tests shall verify at minimum:

## 97.1 Construction and Preparation

* construction has no repository side effects
* preparation validates the registry
* duplicate rule identifiers fail
* invalid rule metadata fails
* mandatory rule absence fails
* invalid execution configuration fails
* preparation is idempotent
* failed preparation prevents execution

## 97.2 Rule Selection

* target compatibility
* scope compatibility
* mandatory rule inclusion
* optional rule exclusion
* explicitly requested rules
* unknown requested rule
* unknown excluded rule
* deterministic selected-rule order

## 97.3 Execution Planning

* lifecycle phase ordering
* prerequisite enforcement
* deterministic plan
* safe handling of empty optional rule sets
* no selected mandatory rule omitted

## 97.4 Sequential Execution

* canonical execution order
* standard rule invocation
* correct finding collection
* correct skipped-rule handling
* correct exception encapsulation

## 97.5 Parallel Execution

* bounded parallelism
* independent rules execute concurrently
* dependent phases remain ordered
* completion order does not affect returned ordering
* no shared-state corruption
* worker resources are released

## 97.6 Aggregation

* findings normalized
* findings ordered deterministically
* execution errors retained
* severity counts correct
* rule counts correct
* status precedence correct
* execution summary correct

## 97.7 Caching

* analysis cache hit
* analysis cache miss
* complete result cache where implemented
* cache key changes with target content
* cache key changes with rules
* cache key changes with configuration
* active repository prevents cross-repository reuse
* invalidation clears stale entries
* immutable cached values
* caching disabled behavior

## 97.8 Incremental Validation

* unchanged documents reuse analysis
* changed document revalidates
* removed document removes stale findings
* renamed identifier affects references
* dependency change re-evaluates graph
* final result matches complete validation

## 97.9 Thread Safety

* concurrent execute calls
* concurrent cache access
* concurrent invalidation
* execute during shutdown
* isolated request contexts
* no result cross-contamination
* no deadlock

## 97.10 Cancellation and Shutdown

* cancellation stops new scheduling
* completed outcomes retained
* cancelled rules classified
* shutdown rejects new work
* executor closes
* caches clear
* repeated shutdown safe

---

# 98. Integration Tests

Integration tests shall verify:

* Validation Service delegates to the real engine
* Repository Service provides real temporary-repository artifacts
* governed documents are discovered
* metadata analysis is shared
* rules execute through the real registry
* cross-reference index is built
* dependency graph input is built
* complete result is immutable
* repository changes invalidate or bypass stale analysis
* parallel and sequential execution produce equivalent semantic results
* engine shutdown integrates with platform lifecycle

Integration tests shall not depend on real user repositories.

---

# 99. Determinism Tests

Tests shall run identical validation inputs repeatedly and compare:

* selected rule identifiers
* rule outcomes
* findings
* finding order
* severity counts
* overall status
* execution-summary semantic fields

Tests shall ignore naturally variable fields such as:

* execution timestamp
* duration
* correlation identifier

Parallel execution tests shall introduce controlled varied rule completion order and still produce equivalent results.

---

# 100. Error Tests

Tests shall cover:

* malformed request reaches no rule
* unknown rule
* validator exception
* malformed validator output
* Repository Service unavailable
* artifact read failure
* invalid analysis state
* cache corruption handling
* executor rejection
* cancellation
* engine not prepared
* shutdown state
* no raw exception leakage

---

# 101. Security Tests

Tests shall prove:

* validated document code blocks are not executed
* repository Python modules are not imported
* shell commands are not invoked
* external URLs are not fetched
* artifact access uses Repository Service
* artifact contents do not appear in logs
* secrets in documents are not echoed in findings
* invalid paths remain rejected
* write operations are unavailable
* parser failures are safely encapsulated

---

# 102. Performance Tests

Performance testing shall avoid fragile wall-clock thresholds.

Tests should verify:

* each artifact is not repeatedly read unnecessarily
* shared analysis is reused
* rule selection avoids unrelated rules
* cache hits avoid repeated parsing
* bounded parallelism respects configured limits
* repository-wide validation avoids .git
* no unbounded task creation occurs

Operation counts and controlled test doubles are preferred over machine-specific timings.

---

# 103. Backward Compatibility

The engine implementation shall preserve all existing approved behavior.

It shall not:

* change Repository Service APIs
* change Configuration Service APIs
* change Logging Service APIs
* change lifecycle states
* change Service Registry behavior
* change public CLI abstractions
* create import-time side effects
* introduce future service dependencies

All baseline regression tests shall continue to pass.

---

# 104. Coding Standards

Implementation shall follow ARP-001-05.

Requirements include:

* complete type hints
* public and internal protocol docstrings where appropriate
* explicit return types
* immutable data where practical
* no wildcard imports
* no mutable process-global state
* no import-time rule discovery
* no import-time repository access
* no import-time executor creation
* focused modules
* deterministic collection ordering
* explicit exception handling
* standard-library concurrency where sufficient
* repository formatting and linting conventions
* repository static-analysis conventions

---

# 105. Dependency Requirements

Use only architecture-approved dependencies.

Prefer the standard library for:

* threading
* bounded execution
* immutable collections
* hashing
* timing
* synchronization
* graph traversal where practical

Do not add:

* distributed task queues
* remote execution frameworks
* workflow engines
* database engines
* filesystem watchers
* external cache infrastructure
* AI libraries
* repository-hosting SDKs

A Markdown parser may be added only if already architecture-approved or clearly required after inspecting existing dependencies.

---

# 106. Engine Acceptance Criteria

This companion specification is functionally complete when:

* the Validation Engine exists
* the engine is internal to Validation Framework
* the engine prepares from an immutable registry snapshot
* rule identifiers are unique
* requests receive isolated execution contexts
* rules are selected deterministically
* mandatory rules cannot be silently disabled
* execution phases preserve the approved lifecycle
* sequential execution works
* approved bounded parallel execution works
* validator failures are isolated
* findings are normalized
* findings are ordered deterministically
* results are aggregated correctly
* overall status is calculated correctly
* execution summaries are complete
* artifact analysis is minimized
* caching preserves semantics
* invalidation works
* incremental validation preserves complete-validation semantics
* concurrent requests are safe
* cancellation is safe
* shutdown is safe
* engine health can be mapped by Validation Service
* unit and integration tests pass

---

# 107. Architectural Acceptance Criteria

The engine is architecturally conformant only when:

* AEP-002-11 is satisfied
* ARP-002-08 is satisfied
* Validation Service remains the single public entry point
* the engine is not a separately registered service
* repository access occurs through Repository Service
* no artifact modification occurs
* no AI inference occurs
* no CLI dependency exists
* no Prompt Management dependency exists
* no MCP dependency exists
* no Event Bus dependency exists
* rules remain deterministic
* independent rules may execute in parallel without semantic changes
* results are immutable
* severity definitions remain stable
* status definitions remain stable
* future validators can implement the standard protocol
* existing consumers remain unaffected by validator extensions

---

# 108. Engineering Acceptance Criteria

The engine is technically complete only when:

* type checking passes
* linting passes
* formatting checks pass
* public and protocol documentation is complete
* unit tests pass
* integration tests pass
* determinism tests pass
* concurrency tests pass
* security tests pass
* regression tests pass
* packaging checks pass where configured
* no threads leak after tests
* no temporary repositories remain
* no caches leak across tests
* no secrets or artifact contents appear in logs
* no generated local state is committed

---

# 109. Deliverables

This companion specification shall deliver:

* Validation Engine
* engine protocol
* execution context
* validation plan
* rule-selection behavior
* lifecycle-phase execution
* sequential execution
* bounded parallel execution
* result aggregation
* finding normalization
* status calculation
* execution summary generation
* analysis snapshot behavior
* in-process cache
* cache key behavior
* invalidation
* incremental validation support
* cancellation
* internal health and diagnostics
* engine unit tests
* engine integration tests
* determinism tests
* concurrency tests
* security tests

Completion of this companion alone does not complete IWP-008.

---

# 110. Files Expected to Change

Expected additions or modifications may include:

src/vibe/validation/engine.py
src/vibe/validation/execution.py
src/vibe/validation/context.py
src/vibe/validation/planning.py
src/vibe/validation/aggregation.py
src/vibe/validation/cache.py
src/vibe/validation/analysis.py
src/vibe/validation/registry.py
src/vibe/validation/protocols.py
src/vibe/validation/errors.py

Expected tests may include:

tests/unit/validation/test_engine.py
tests/unit/validation/test_execution_context.py
tests/unit/validation/test_validation_plan.py
tests/unit/validation/test_rule_selection.py
tests/unit/validation/test_aggregation.py
tests/unit/validation/test_validation_cache.py
tests/unit/validation/test_incremental_validation.py
tests/unit/validation/test_engine_thread_safety.py
tests/unit/validation/test_engine_shutdown.py
tests/integration/validation/test_validation_engine.py
tests/integration/validation/test_validation_engine_repository.py
tests/integration/validation/test_validation_determinism.py
tests/integration/validation/test_validation_incremental.py

Exact paths shall follow the current repository conventions.

---

# 111. Companion Specification Dependencies

This document relies on:

* IWP-008-01 for the public Validation Service boundary
* IWP-008-03 for concrete validators and rule contracts
* IWP-008-04 for request, context, outcome, finding, result, severity, status, and summary models
* IWP-008-05 for CLI consumption
* IWP-008-06 for complete testing, acceptance, quality-gate, and completion-report requirements

Cursor shall not invent final model fields before reviewing IWP-008-04.

Cursor shall not implement concrete validation semantics before reviewing IWP-008-03.

---

# 112. Implementation Guidance for Cursor

When implementing the complete IWP-008 package, Cursor shall:

1. Review IWP-008-00 through IWP-008-06.
2. Inspect the existing 86-vibe-cli implementation.
3. Confirm synchronous or asynchronous platform conventions.
4. Confirm the Repository Service public API.
5. Confirm shared health and lifecycle conventions.
6. Implement public models before finalizing engine signatures.
7. Implement the validator protocol and registry contract.
8. implement immutable registry snapshots.
9. implement deterministic rule selection.
10. implement the validation execution pipeline.
11. implement sequential execution first.
12. add bounded parallel execution only for declared independent rules.
13. implement normalized aggregation and status calculation.
14. implement analysis caching and safe invalidation.
15. implement incremental validation conservatively.
16. implement cancellation and shutdown.
17. add determinism, concurrency, security, and regression tests.
18. run all repository quality gates.
19. stop after completing the full IWP-008 specification set.
20. do not begin IWP-010.

---

# 113. Definition of Done for IWP-008-02

IWP-008-02 is complete when:

* the engine is implemented as an internal Validation Framework component
* the engine has no external consumer entry point
* preparation is deterministic
* the registry snapshot is immutable
* duplicate rule identifiers are rejected
* requests use isolated execution contexts
* rule selection is deterministic
* mandatory rules are protected
* execution phases follow AEP-002-11
* sequential execution works
* parallel execution is bounded and semantically deterministic
* validator exceptions are isolated
* rule output is validated
* findings are normalized
* finding order is deterministic
* result aggregation is correct
* severity counts are correct
* overall status precedence is correct
* execution summaries are correct
* repository reads occur only through Repository Service
* caching does not alter results
* cache invalidation works
* incremental validation matches complete validation
* concurrent requests do not corrupt state
* cancellation is safe
* shutdown is safe
* all engine tests pass
* all baseline regression tests pass
* no architectural drift is introduced

Completion of IWP-008-02 alone does not authorize approval or merge of IWP-008.

The complete work package requires successful implementation of all companion specifications.

---

# 114. Next Companion Specification

The next companion specification is:

IWP-008-03 – Validators Implementation Specification

Do not begin implementation from this document alone.

Cursor shall receive and review the complete IWP-008 specification set before implementation begins.

---
