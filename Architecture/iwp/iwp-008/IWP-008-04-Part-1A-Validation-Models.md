# IWP-008-04 – Validation Models Implementation Specification  

## Part 1A – Model Architecture, Enumerations, Requests, Identifiers and Execution Models

## Document Control

| Item | Value |
|---|---|
| Document ID | IWP-008-04 |
| Document Part | Part 1A |
| Document Name | Validation Models Implementation Specification |
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
| Related Specifications | IWP-008-01 – Validation Service; IWP-008-02 – Validation Engine; IWP-008-03 – Validators |
| Supersedes | None |

---

# 1. Purpose

This document defines the implementation requirements for the shared immutable models used by:

**IWP-008 – Validation Framework**

The Validation Framework requires one canonical model layer through which the Validation Service, Validation Engine, validator registry, validators, CLI integration, reporting components, tests, and future approved consumers exchange validation data.

This companion specification defines:
- immutable validation model architecture
- model ownership and dependency direction
- model construction requirements
- model validation requirements
- model serialization requirements
- model equality and hashing requirements
- model versioning requirements
- validation identifiers
- identifier generation and normalization
- validation enumerations
- validation target models
- validation request models
- repository validation request models
- document validation request models
- rule-selection models
- execution-option models
- validation execution context
- validation execution plan models
- rule execution models
- execution timing models
- cancellation and deadline models
- shared model contracts required by IWP-008-01 through IWP-008-03

The shared model layer shall provide the stable contract boundary between all Validation Framework components.
Validation Framework components shall not exchange arbitrary dictionaries, untyped objects, implementation-specific parser objects, mutable collections, or validator-private representations across component boundaries.
Cursor shall treat this document as one mandatory companion within the complete IWP-008 specification set.
No implementation shall begin until all IWP-008 companion specifications have been reviewed.

---

# 2. Part 1A Scope

Part 1A defines:
1. model architecture
2. model design principles
3. package ownership
4. identifier models
5. enumeration models
6. target models
7. request models
8. rule-selection models
9. execution-option models
10. execution context models
11. execution plan models
12. rule execution models
13. execution timing and deadline models
14. contracts governing those models

The following model groups remain within IWP-008-04 but are not defined in Part 1A:
- validation finding models
- source-location models
- rule outcome models
- validation response models
- validation result models
- validation summary models
- diagnostic models
- reporting models
- serialization envelopes
- result builders
- model-specific testing and acceptance requirements
- complete package layout and deliverables

Part 1A shall be implemented only as part of the complete approved IWP-008-04 specification.

---

# 3. Governing Specifications

Implementation shall conform to:
- ADR-001 – Implement Repository Service Before Validation Framework
- AEP-001-07 – Platform Architecture
- AEP-001-08 – Repository Strategy
- AEP-001-09 – Technology Strategy
- AEP-001-10 – Versioning Strategy
- AEP-001-17 – Architecture Review Checklist
- AEP-001-18 – Definition of Done
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
- AEP-002-18 – Platform Reference Implementation Specification
- ARP-001-02 – Architecture Terminology & Naming Standards
- ARP-001-03 – Package Dependency Matrix
- ARP-001-05 – Engineering Coding Standards
- ARP-002-02 – Configuration Service Interface Contract
- ARP-002-03 – Logging Service Interface Contract
- ARP-002-07 – Repository Service Interface Contract
- ARP-002-08 – Validation Service Interface Contract
- ARP-002-16 – Platform Service Contract Compliance Matrix
- IWP-008-00 – Validation Framework Master Implementation Specification
- IWP-008-01 – Validation Service Implementation Specification
- IWP-008-02 – Validation Engine Implementation Specification
- IWP-008-03 – Validators Implementation Specification
- IWP-008-05 – CLI Integration Implementation Specification
- IWP-008-06 – Testing and Acceptance Implementation Specification

Where this specification conflicts with an approved architecture document, the approved architecture document takes precedence.
Cursor shall stop and report a material ambiguity rather than inventing a model, field, enumeration value, default, identifier convention, serialization format, or compatibility behavior.

---

# 4. Implementation Baseline

The following work packages have been completed, approved, committed, and merged:
- IWP-001 – Repository Foundation
- IWP-002 – Configuration Service
- IWP-003 – Logging Service
- IWP-004 – Bootstrap Service
- IWP-005 – Service Registry
- IWP-006 – Service Lifecycle Manager
- IWP-007 – CLI Framework
- IWP-009 – Repository Service

These implementations form the immutable baseline for IWP-008.

Validation models shall:
1. preserve all existing package dependency directions
2. use baseline shared utilities where those utilities already satisfy an approved requirement
3. reuse approved baseline identifier, path, time, error, and repository abstractions where compatible
4. avoid duplicating Repository Service public models
5. avoid exposing Repository Service implementation internals
6. remain independent of CLI rendering
7. remain independent of logging implementations
8. remain independent of parser implementations
9. remain independent of validator implementations
10. remain independent of persistent storage
11. avoid modifying baseline services except for a minimal defect correction required for IWP-008
12. document every such defect correction in the final completion report

---

# 5. Strict Scope

The Validation Models implementation shall provide the canonical data contracts required for:
| Model Area | Purpose |
|---|---|
| Identity | Identify requests, executions, targets, rules, validators, findings, and results |
| Enumeration | Represent closed Validation Framework states and categories |
| Targets | Describe what shall be validated |
| Requests | Describe a caller’s validation intent |
| Rule Selection | Describe rule inclusion, exclusion, and category selection |
| Execution Options | Describe approved execution behavior |
| Execution Context | Carry immutable per-request execution state |
| Execution Planning | Describe the deterministic resolved execution plan |
| Rule Execution | Describe one rule invocation and its execution status |
| Timing | Represent timestamps, elapsed durations, deadlines, and timeout state |
| Serialization | Permit deterministic transport and diagnostic representation |
| Compatibility | Preserve stable contracts across Validation Framework components |

The model layer shall be usable by:
- Validation Service
- Validation Engine
- validator registry
- validators
- CLI integration
- validation report generation
- Validation Framework tests
- future approved platform components

---

# 6. Explicitly Out of Scope

Part 1A shall not implement:
- Validation Service orchestration
- Validation Engine scheduling
- validator logic
- validator discovery
- document parsing
- metadata extraction
- repository traversal
- direct filesystem access
- Git operations
- CLI command handlers
- CLI rendering
- report formatting
- persistent model storage
- database schemas
- network transport APIs
- REST endpoints
- message queues
- background validation
- cache storage
- cache eviction
- plugin loading
- dynamic model loading
- arbitrary user-defined enumeration values
- automatic model migration
- automatic document correction
- automatic result remediation
- architecture redesign
- replacement of Repository Service contracts
- replacement of platform-wide error contracts
- mutable domain entities
- ORM entities
- parser-specific abstract syntax trees
- Markdown rendering objects
- presentation-specific color, icon, or terminal-style models

No model shall contain behavior that belongs to the Validation Service, Validation Engine, validator implementations, Repository Service, CLI framework, or report renderer.

---

# 7. Model Architectural Position

The approved model relationship is:
```text
External Consumer
        |
        v
Validation Service
        |
        | ValidationRequest
        v
Validation Engine
        |
        | ValidationExecutionContext
        | ValidationPlan
        | RuleExecutionRequest
        v
Validator Registry and Validators
        |
        | RuleOutcome
        | ValidationFinding
        v
Validation Engine
        |
        | ValidationResult
        v
Validation Service
        |
        | ValidationResponse
        v
CLI / Approved Consumer

The shared Validation Models package shall sit below all Validation Framework orchestration components.

The required dependency direction is:

Validation Service -----------+
                              |
Validation Engine ------------+----> Validation Models
                              |
Validators -------------------+
                              |
CLI Integration --------------+

The Validation Models package shall not depend on:

* Validation Service
* Validation Engine
* validators
* validator registry
* CLI command implementations
* CLI renderers
* application bootstrap
* service lifecycle implementations
* report rendering implementations

The model package may depend only on approved lower-level shared platform abstractions and the standard library.

Circular dependencies are prohibited.

---

# 8. Model Design Principles

Every Validation Framework model shall comply with the following principles.

## 8.1 Immutable

A model shall not permit observable state mutation after successful construction.

## 8.2 Explicit

Every field shall have a declared type, documented meaning, defined nullability, and defined default behavior.

## 8.3 Deterministic

Construction, normalization, equality, hashing, ordering, and serialization shall be deterministic.

## 8.4 Validated

Invalid model state shall be rejected at the construction boundary.

## 8.5 Serializable

Public and cross-component models shall support deterministic serialization into approved primitive structures.

## 8.6 Framework Independent

Core models shall not require a CLI, web framework, persistence framework, parser framework, or dependency-injection container.

## 8.7 Minimal

Models shall contain only data and tightly scoped invariant enforcement.

## 8.8 Stable

Public field names and enumeration values shall be treated as compatibility-sensitive contracts.

## 8.9 Composable

Complex models shall be composed from smaller approved immutable models.

## 8.10 Safe

Models shall reject unsafe, malformed, excessively large, or ambiguous values at appropriate trust boundaries.

## 8.11 Repository Abstracted

Repository-managed target models shall use Repository Service abstractions and shall not expose direct filesystem or Git implementation details.

## 8.12 Presentation Neutral

Models shall not contain terminal formatting, UI labels, colors, icons, indentation, or prose-layout decisions.

---

# 9. Stop Conditions

Cursor shall stop implementation and report the conflict if any of the following conditions occurs:

1. An approved architecture contract requires a field incompatible with this specification.
2. A baseline service exposes a materially different required identifier type.
3. Repository Service does not expose sufficient stable identity for a repository target.
4. A required model would introduce a prohibited dependency.
5. Two approved companion specifications assign conflicting meanings to the same model or field.
6. An enumeration value cannot be resolved without inventing new architecture.
7. A request option would allow bypassing a mandatory rule.
8. A model would need to expose a parser-specific or repository-implementation-specific object.
9. Immutability cannot be preserved using the approved project conventions.
10. Deterministic serialization cannot be achieved without changing an approved public contract.
11. A required time, path, digest, or error abstraction already exists but has incompatible semantics.
12. A model name conflicts with an approved baseline public type.
13. An identifier convention conflicts with ARP-001-02.
14. A model requires direct filesystem access or direct Git access.
15. A default value would materially change validation behavior but is not specified by approved architecture.
16. A compatibility alias would create two canonical representations of the same state.
17. A model cannot be safely constructed from untrusted CLI or repository input.
18. Implementing the model requires redesigning IWP-008-01, IWP-008-02, or IWP-008-03.

Cursor shall not resolve any stop condition by silently adding fields, aliases, fallback values, conversion rules, or permissive parsing.

---

# 10. Model Implementation Form

Validation Framework models shall use the approved immutable model convention already present in the implementation repository.

Where the baseline does not establish one canonical convention, models shall use immutable dataclass-style value objects with:

* frozen instances
* explicit field types
* explicit constructors
* constructor-time invariant validation
* immutable collection fields
* deterministic equality
* deterministic hashing where all contained values are hashable
* deterministic serialization helpers
* no public mutation methods

The implementation shall not mix multiple competing model frameworks within IWP-008.

The following are prohibited unless already mandated by the baseline:

* mutable dictionaries as public model state
* mutable lists as public model state
* mutable sets as public model state
* unrestricted Any fields
* untyped dict payloads
* runtime-generated model classes
* dynamic attribute injection
* silent coercion of structurally unrelated values
* ORM base classes
* framework-specific request or response classes
* inheritance trees used only for code reuse
* public setters
* in-place normalization

Where inheritance is required for a genuine substitutable model contract, it shall remain shallow and explicitly documented.

Composition shall be preferred over inheritance.

---

# 11. Model Construction Contract

Every model constructor shall:

1. validate required fields
2. reject missing mandatory values
3. reject invalid enumeration values
4. normalize approved string values
5. convert approved input collections into immutable collections
6. enforce field-level size limits where defined
7. enforce cross-field invariants
8. reject unsupported combinations
9. preserve caller-provided values where normalization is not authorized
10. complete without external I/O
11. complete without repository access
12. complete without logging
13. complete without dependency resolution
14. complete without reading configuration
15. produce either one valid immutable model or one explicit construction error

Constructors shall not:

* silently remove invalid values
* silently replace invalid identifiers
* silently infer repository roots
* silently resolve document paths
* silently query available rules
* silently select validators
* silently convert an execution error into a validation finding
* perform asynchronous work
* mutate input collections
* retain references to caller-owned mutable collections

---

# 12. Model Validation Errors

Invalid model construction shall raise or return the approved platform model-validation error type.

Where no baseline model-validation error type exists, IWP-008 shall define one internal Validation Models construction error consistent with approved platform error conventions.

A model-validation error shall identify:

* model type
* invalid field where applicable
* stable error code
* concise reason
* whether the failure is due to a missing, malformed, unsupported, conflicting, or excessive value

The error shall not include:

* secrets
* full repository contents
* complete document contents
* arbitrary object representations
* terminal formatting
* stack traces intended for end users

Model-construction errors are not validation findings.

A malformed ValidationRequest shall be rejected before normal validation execution and shall not be represented as an ordinary failed validation result.

---

# 13. Nullability

Every field shall be either:

* mandatory and non-null
* optional with an explicit semantic meaning for absence
* optional with a documented immutable default

Null shall not be used to mean multiple different states.

For example:

* absent deadline means no caller-specified deadline
* empty rule inclusion set means no explicit inclusion filter
* absent repository revision means use the Repository Service resolved state
* absent correlation identifier means no upstream correlation identifier was supplied

The following practices are prohibited:

* null meaning both “unknown” and “not applicable”
* null meaning both “use default” and “disable”
* null collections where an empty immutable collection is sufficient
* null Boolean values unless a genuine three-state contract is approved

---

# 14. Immutable Collections

Public collection fields shall use immutable representations.

The model layer shall convert accepted constructor inputs as follows:

Accepted Input Concept	Stored Representation
Ordered sequence	Immutable tuple or approved immutable sequence
Unique unordered values	Immutable set or approved immutable set
Stable key-value metadata	Read-only mapping or immutable ordered entry sequence
Ordered key-value data	Immutable ordered entry sequence
Dependency edges	Immutable tuple of immutable edge models

Collection normalization shall:

1. preserve semantically meaningful order
2. sort values only where canonical ordering is required
3. remove duplicates only where the field contract defines set semantics
4. reject duplicates where duplicates indicate malformed input
5. reject null collection members
6. reject members of an invalid type
7. avoid exposing caller-owned mutable references

An empty immutable collection shall be used instead of a null collection unless absence has a distinct approved meaning.

---

# 15. String Normalization

String normalization shall be field-specific.

Permitted normalization may include:

* trimming leading and trailing whitespace
* canonical case normalization for case-insensitive identifiers
* Unicode normalization using one documented form
* normalization of approved path separators through the Repository Service path abstraction
* removal of prohibited surrounding delimiters where the field contract explicitly permits them

String normalization shall not:

* rewrite descriptive messages
* change document titles
* alter repository-relative path case
* guess missing identifier prefixes
* repair malformed identifiers
* replace unsupported characters with invented alternatives
* collapse meaningful internal whitespace
* infer enumeration values from approximate text
* accept display labels as canonical enumeration values

After normalization, empty required strings shall be rejected.

---

# 16. Serialization Contract

Public Validation Framework models shall support deterministic conversion to an approved primitive representation.

The primitive representation may contain only:

* strings
* integers
* finite decimal or floating-point values where approved
* Booleans
* null for explicitly optional fields
* ordered lists
* string-keyed objects
* nested approved primitive representations

Serialization shall:

1. use canonical public field names
2. serialize enumerations by canonical string value
3. serialize identifiers by canonical string value
4. serialize timestamps using UTC ISO 8601 format
5. serialize durations using one documented unit
6. preserve semantically meaningful collection order
7. produce deterministic key and value ordering where required for snapshots or hashing
8. omit or include absent optional fields consistently
9. avoid runtime-specific object representations
10. avoid memory addresses
11. avoid non-finite numeric values
12. avoid serializing private implementation state

Deserialization, where provided, shall apply the same invariant validation as direct construction.

Deserialization shall not bypass constructors or create partially valid objects.

---

# 17. Canonical Field Naming

Serialized field names shall use the repository’s approved public naming convention.

Within Python implementations, field names shall use snake_case.

Canonical serialized names shall also use snake_case unless an approved baseline serializer mandates another convention.

Field aliases may be accepted only when:

1. required for compatibility with an approved baseline contract
2. documented explicitly
3. mapped to exactly one canonical field
4. rejected when both alias and canonical field are supplied with conflicting values
5. omitted from canonical serialization output

No aliases shall be invented for convenience.

---

# 18. Equality and Hashing

Value models shall compare by canonical semantic content.

Equality shall not depend on:

* object identity
* memory address
* construction order for unordered fields
* mutable input collection identity
* non-semantic cached values
* logger instances
* service instances
* parser objects
* thread identity
* process identity

Hashing shall be implemented only where:

* the model is fully immutable
* all semantic fields are hashable
* hashing has a concrete internal use
* hash behavior remains consistent with equality

Models containing mappings shall use a canonical immutable representation before hashing.

Python runtime hash values shall not be persisted or used as cross-process stable identifiers.

---

# 19. Copy and Update Behavior

Public in-place mutation is prohibited.

Where a modified variant of a model is required, the implementation may provide a clearly named replacement method that:

1. returns a new validated model
2. leaves the original unchanged
3. reruns all relevant invariant checks
4. does not copy private caches
5. preserves unspecified fields
6. rejects unknown replacement fields

Generic unrestricted update dictionaries are prohibited.

Request normalization by the Validation Service shall produce a new normalized request rather than mutating the caller’s request.

Execution context enrichment shall produce a new context or shall occur only during initial context construction before the immutable context is published.

---

# 20. Model Versioning

The initial Validation Models contract version shall be:

1

Model contract versioning shall be independent from:

* application package version
* validator rule version
* repository revision
* document version
* CLI version
* architecture package version

Public serialized envelopes defined by the completed IWP-008-04 specification shall carry the model contract version where required.

Within contract version 1:

* existing field meanings shall not change
* existing canonical enumeration values shall not change
* required fields shall not be removed
* optional fields shall not become required without an approved breaking change
* identifiers shall retain their canonical format
* serialization shall remain deterministic

A future incompatible model change shall require:

1. architecture approval
2. a version increment
3. compatibility analysis
4. migration or rejection behavior
5. tests for supported versions

The implementation shall not provide speculative migration logic.

---

# 21. Public and Internal Models

Models shall be classified as either:

* public Validation Framework contract models
* internal Validation Framework execution models

Public models include models exchanged with approved consumers, including:

* validation requests
* validation targets
* validation responses
* validation results
* validation findings
* validation summaries
* public identifiers
* public enumerations

Internal models include models exchanged only within the Validation Framework, including:

* normalized execution context
* resolved validation plan
* rule execution request
* internal execution state
* internal cache descriptors
* parser analysis descriptors

Internal models shall not be exported from the package’s public API unless explicitly required by an approved companion specification.

Internal models shall still comply with immutability and deterministic behavior requirements.

---

# 22. Identifier Architecture

Every independently traceable Validation Framework entity shall use an explicit typed identifier.

The minimum identifier types are:

Identifier	Purpose
ValidationRequestId	Identifies one accepted validation request
ValidationExecutionId	Identifies one engine execution
ValidationResultId	Identifies one completed validation result
ValidationTargetId	Identifies one normalized validation target
ValidationFindingId	Identifies one validation finding
ValidatorId	Identifies one registered validator implementation
RuleId	Identifies one validation rule
RuleExecutionId	Identifies one invocation of one rule
DocumentId	Identifies one governed document
RepositoryId	References the approved Repository Service repository identity
CorrelationId	Preserves an approved cross-service correlation identity

Typed identifiers shall prevent accidental substitution of one identifier category for another.

Plain strings may be accepted at external construction boundaries, but shall be converted immediately into the appropriate typed identifier model.

---

# 23. Identifier Value Object Contract

Each Validation Framework identifier value object shall:

* be immutable
* contain one canonical string value
* reject null
* reject empty values
* reject leading or trailing whitespace after normalization
* reject control characters
* enforce its defined maximum length
* implement deterministic equality
* implement deterministic string conversion
* support canonical serialization
* avoid implicit conversion to unrelated identifier types

The default maximum length for generated opaque identifiers shall be 128 characters.

The default maximum length for governed symbolic identifiers shall be 160 characters.

A more restrictive identifier-specific limit shall take precedence.

Identifier repr output shall identify the identifier type without exposing unrelated model state.

---

# 24. Opaque Identifier Generation

Opaque runtime identifiers shall use a collision-resistant standard identifier mechanism already approved or used by the baseline implementation.

Where no baseline mechanism exists, UUID version 4 shall be used for:

* ValidationRequestId
* ValidationExecutionId
* ValidationResultId
* RuleExecutionId

Canonical UUID values shall:

* use lowercase hexadecimal characters
* use standard hyphenated representation
* exclude braces
* exclude prefixes
* reject non-canonical values when strict parsing is required

Opaque identifiers shall be generated at the component boundary that owns the identified entity:

Identifier	Owning Boundary
ValidationRequestId	Validation Service request acceptance
ValidationExecutionId	Validation Engine execution creation
ValidationResultId	Validation Engine result finalization
RuleExecutionId	Validation Engine rule invocation planning

Callers may supply a request identifier only where the approved public contract explicitly permits it.

Caller-supplied identifiers shall be validated and shall not be silently replaced.

---

# 25. Deterministic Identifiers

Identifiers representing stable semantic entities shall use deterministic canonical values.

The following identifiers shall be deterministic:

* RuleId
* ValidatorId
* architecture DocumentId
* specification DocumentId
* repository target identity where derived from an approved stable Repository Service identifier
* ValidationTargetId where the same normalized target identity must compare consistently within one repository state

Deterministic identifiers shall not depend on:

* process identity
* random values
* current time
* discovery order
* validator registration order
* absolute local filesystem path
* current working directory
* machine hostname
* user account
* mutable display labels

---

# 26. Rule Identifier

RuleId shall identify one validation rule independently of its implementing validator class.

Canonical rule identifiers shall follow:

validation.<category>.<rule-name>

Examples:

validation.naming.filename-prefix
validation.naming.numeric-sequence
validation.metadata.required-fields
validation.metadata.document-id-uniqueness
validation.structure.numbered-sections
validation.references.document-exists
validation.dependencies.no-cycles
validation.repository.governed-document-placement

A RuleId shall:

* be lowercase
* use ASCII letters, digits, periods, and hyphens only
* begin with validation.
* contain exactly one approved rule category segment after validation
* contain one or more descriptive rule-name segments
* not contain consecutive periods
* not begin or end a segment with a hyphen
* not contain whitespace
* not include a version suffix

Rule version shall be represented separately.

Rule identifiers are compatibility-sensitive public contracts.

A rule shall not change semantic meaning while retaining the same RuleId.

---

# 27. Validator Identifier

ValidatorId shall identify one validator implementation registered with the validator registry.

Canonical validator identifiers shall follow:

validator.<category>.<validator-name>

Examples:

validator.naming.filename
validator.metadata.document-control
validator.structure.document-structure
validator.references.cross-reference
validator.dependencies.dependency-graph
validator.repository.document-placement

One validator may implement one or more closely related rules.

ValidatorId and RuleId are not interchangeable.

A validator identifier shall not contain implementation module paths, class memory addresses, package installation paths, or runtime-generated values.

Validator implementation replacement may retain the same ValidatorId only when its contract and supported rules remain compatible.

---

# 28. Document Identifier

DocumentId shall represent the canonical identifier declared by or deterministically assigned to a governed document.

Examples include:

AEP-002-11
ARP-002-08
IWP-008-04
ADR-001

DocumentId shall:

* preserve the canonical uppercase document prefix
* preserve approved numeric segment width
* use hyphens as separators
* reject surrounding whitespace
* reject path components
* reject file extensions
* reject Markdown anchors
* reject repository names
* reject descriptive filename suffixes
* reject values not conforming to approved document identifier conventions

Document identifier parsing shall not infer an identifier from approximate free text.

Filename-derived and metadata-derived document identifiers shall be represented separately during extraction until a validator evaluates consistency.

---

# 29. Validation Target Identifier

ValidationTargetId shall identify one normalized validation target.

The identifier shall be derived from:

* target kind
* approved repository identity where applicable
* canonical repository-relative path where applicable
* document identity where applicable
* approved target selector where applicable

A target identifier shall not include:

* absolute local filesystem paths
* access tokens
* credentials
* temporary checkout paths
* machine-specific mount points
* mutable display names
* document contents

The same normalized target within the same repository identity shall produce the same ValidationTargetId.

A repository target and a document target shall not produce the same target identifier even when the document is the repository’s only governed document.

The implementation shall use one canonical deterministic encoding or digest strategy defined in the completed IWP-008-04 specification.

---

# 30. Correlation Identifier

CorrelationId shall preserve an approved upstream correlation identity across:

* Validation Service
* Validation Engine
* Repository Service calls
* Logging Service context
* diagnostic output
* validation result metadata

A correlation identifier is not a validation request identifier.

Where no upstream correlation identifier exists, the Validation Service may use the validation request identifier as the logging correlation value if consistent with the baseline Logging Service contract.

The original distinction between correlation identifier and request identifier shall remain represented in the models.

Correlation identifiers shall not be generated by validators.

---

# 31. Enumeration Architecture

Validation Framework enumerations shall be:

* closed sets
* immutable
* explicitly named
* string-valued for public serialization
* case-sensitive in canonical form
* deterministically ordered only where semantic ordering is defined
* independent of display labels

Enumerations shall reject unknown values.

Unknown enumeration values shall not silently map to:

* OTHER
* UNKNOWN
* the first member
* a default member
* a display label
* a case-insensitive approximation

An explicit UNKNOWN or UNSUPPORTED member may exist only where that state has approved semantics.

Canonical enumeration values shall use lowercase snake_case.

Enumeration member names may use uppercase constants in code.

Example:

class ValidationTargetKind(str, Enum):
    REPOSITORY = "repository"
    DOCUMENT = "document"

Public serialized contracts shall use the string value, not the language-specific member name.

---

# 32. Required Enumerations

Part 1A shall define at minimum:

Enumeration	Purpose
ValidationTargetKind	Classifies the target being validated
ValidationScope	Defines the breadth of requested validation
ValidationMode	Defines approved execution behavior
ValidationRuleCategory	Classifies rules
RuleSelectionMode	Defines how explicit rule selection is interpreted
ExecutionStrategy	Defines sequential or bounded-parallel execution
FailFastMode	Defines approved fail-fast behavior
ValidationPhase	Defines deterministic engine execution phases
RuleExecutionStatus	Defines one rule invocation’s execution state
ValidationExecutionStatus	Defines overall engine execution state
CachePolicy	Defines approved cache use
IncrementalValidationMode	Defines approved incremental-validation behavior
UnsupportedTargetPolicy	Defines behavior for unsupported artifacts
ParseFailurePolicy	Defines behavior for parse failures
RepositoryStateKind	Identifies the repository state representation used for execution
DeadlineState	Defines deadline availability or expiry state

Finding severity, finding category, finding status, result status, and response status shall be defined in the remaining portion of IWP-008-04.

---

# 33. Validation Target Kind

ValidationTargetKind shall contain:

Member	Canonical Value	Meaning
REPOSITORY	repository	Validate a repository-managed validation scope
DOCUMENT	document	Validate one governed document
DOCUMENT_SET	document_set	Validate an explicit immutable set of governed documents

DOCUMENT_SET shall not permit arbitrary in-memory documents.

Every document in a document set shall be addressable through Repository Service or an approved normalized document artifact supplied by Validation Service.

The initial implementation shall not add:

* directory
* filesystem
* Git branch
* URL
* raw text
* prompt string
* Python object
* database record

as target kinds unless required by an approved architecture change.

---

# 34. Validation Scope

ValidationScope shall contain:

Member	Canonical Value	Meaning
TARGET	target	Validate only the explicitly identified target
REFERENCED	referenced	Validate the target and approved directly referenced context
DEPENDENCY_CLOSURE	dependency_closure	Validate the target and its resolved dependency closure
REPOSITORY	repository	Validate the complete governed repository scope

Scope resolution shall be performed by the Validation Service and Validation Engine.

The request model shall represent intent only.

A request model shall not traverse references or dependencies during construction.

For a repository target, TARGET and REPOSITORY may resolve to the same execution inventory, but shall remain distinct caller intent values.

---

# 35. Validation Mode

ValidationMode shall contain:

Member	Canonical Value	Meaning
STANDARD	standard	Execute the complete mandatory validation behavior for the resolved scope
STRICT	strict	Execute mandatory validation with approved stricter optional checks or severity treatment
SELECTED	selected	Execute an explicitly selected approved rule subset while preserving mandatory rules

STANDARD shall be the default.

STRICT shall not invent additional architectural rules.

It may enable only stricter behaviors explicitly defined by approved validators, configuration, or severity policy.

SELECTED shall not permit mandatory rules to be bypassed.

A validation mode shall not directly determine CLI exit codes or rendering.

---

# 36. Validation Rule Category

ValidationRuleCategory shall contain:

Member	Canonical Value
NAMING	naming
METADATA	metadata
STRUCTURE	structure
REFERENCES	references
DEPENDENCIES	dependencies
REPOSITORY	repository

These values correspond to the minimum rule categories required by IWP-008-03.

The initial implementation shall not add generic categories such as:

* quality
* style
* semantic
* AI
* security
* compliance
* code
* testing
* performance

unless approved by a later architecture decision.

A rule shall belong to exactly one primary rule category.

---

# 37. Rule Selection Mode

RuleSelectionMode shall contain:

Member	Canonical Value	Meaning
ALL	all	Resolve all applicable registered rules
CATEGORIES	categories	Resolve rules from specified categories
EXPLICIT	explicit	Resolve specified rule identifiers
DEFAULT	default	Resolve the approved default rule set

DEFAULT shall be the request default.

ALL and DEFAULT may initially resolve to the same rule set, but shall remain semantically distinct.

EXPLICIT selection shall still include mandatory rules.

CATEGORIES selection shall reject an empty category collection.

EXPLICIT selection shall reject an empty rule identifier collection.

---

# 38. Execution Strategy

ExecutionStrategy shall contain:

Member	Canonical Value	Meaning
SEQUENTIAL	sequential	Execute rules one at a time in canonical order
BOUNDED_PARALLEL	bounded_parallel	Execute eligible rules concurrently within the approved concurrency bound

SEQUENTIAL shall be the default unless an approved configuration explicitly establishes bounded parallelism as the baseline default.

The enumeration shall not include unrestricted parallel execution.

Rule ordering and result normalization shall remain deterministic under both strategies.

A request may ask for bounded parallel execution only where the Validation Service contract permits caller control.

---

# 39. Fail-Fast Mode

FailFastMode shall contain:

Member	Canonical Value	Meaning
DISABLED	disabled	Continue executing eligible rules after findings and recoverable rule failures
CRITICAL	critical	Stop scheduling new rules after an approved critical finding condition
EXECUTION_ERROR	execution_error	Stop scheduling new rules after an unrecoverable execution error
CRITICAL_OR_ERROR	critical_or_error	Stop after either approved critical findings or an unrecoverable execution error

DISABLED shall be the default.

Fail-fast behavior shall affect rule scheduling only as defined by IWP-008-02.

It shall not:

* suppress already produced findings
* mutate completed outcomes
* convert warnings into errors
* terminate the process
* bypass result finalization
* bypass deterministic aggregation

---

# 40. Validation Phase

ValidationPhase shall contain the deterministic execution phases required by IWP-008-02:

Order	Member	Canonical Value
1	REQUEST_VALIDATION	request_validation
2	TARGET_RESOLUTION	target_resolution
3	INVENTORY_BUILD	inventory_build
4	ARTIFACT_LOADING	artifact_loading
5	ANALYSIS	analysis
6	RULE_SELECTION	rule_selection
7	PLAN_CONSTRUCTION	plan_construction
8	RULE_EXECUTION	rule_execution
9	RESULT_AGGREGATION	result_aggregation
10	RESULT_FINALIZATION	result_finalization

The enumeration order shall be canonical.

The implementation may use this order for:

* diagnostics
* phase timing
* execution-state validation
* deterministic reporting

No component shall skip a mandatory phase merely because the phase produces no artifacts for a particular request.

---

# 41. Rule Execution Status

RuleExecutionStatus shall contain:

Member	Canonical Value	Meaning
PLANNED	planned	Rule invocation is included in the immutable execution plan
RUNNING	running	Rule invocation has begun
COMPLETED	completed	Rule invocation completed normally
SKIPPED	skipped	Rule invocation was not applicable or was intentionally skipped under an approved condition
FAILED	failed	Rule invocation ended with a recoverable validator execution failure
CANCELLED	cancelled	Rule invocation was cancelled before normal completion
TIMED_OUT	timed_out	Rule invocation exceeded an approved timeout
NOT_RUN	not_run	Rule invocation was not started because execution stopped before scheduling

Status transition behavior belongs to the Validation Engine.

The immutable execution record shall preserve the final status.

A normal validation finding shall not cause FAILED status.

---

# 42. Validation Execution Status

ValidationExecutionStatus shall contain:

Member	Canonical Value	Meaning
CREATED	created	Execution identity and initial context exist
PREPARING	preparing	Target, inventory, analysis, and rule plan are being prepared
RUNNING	running	Rule execution is active
AGGREGATING	aggregating	Rule outcomes are being normalized and aggregated
COMPLETED	completed	Execution completed and produced a finalized result
FAILED	failed	Execution could not produce a normal finalized validation result
CANCELLED	cancelled	Execution was cancelled
TIMED_OUT	timed_out	Execution exceeded its approved deadline

A completed execution may contain validation findings of any severity.

COMPLETED does not mean that the target passed validation.

Execution status and validation result status shall remain separate concepts.

---

# 43. Cache Policy

CachePolicy shall contain:

Member	Canonical Value	Meaning
DEFAULT	default	Use the Validation Engine’s approved cache policy
BYPASS_READ	bypass_read	Do not use existing cached data, but permit approved cache writes
BYPASS_ALL	bypass_all	Do not read from or write to Validation Engine caches
REFRESH	refresh	Recompute and replace eligible cached data

DEFAULT shall be the default.

The model shall not include:

* force stale
* ignore fingerprint
* unlimited retention
* external cache key
* user-supplied serialized result

Cache policy shall not allow a caller to bypass mandatory repository-state verification.

---

# 44. Incremental Validation Mode

IncrementalValidationMode shall contain:

Member	Canonical Value	Meaning
DISABLED	disabled	Validate the complete resolved target scope
AUTO	auto	Use incremental validation only where the engine can prove it is safe
CHANGED_ONLY	changed_only	Validate approved changed artifacts and required dependent context

DISABLED shall be the default unless approved configuration establishes AUTO.

CHANGED_ONLY shall require an approved repository-state comparison or explicit normalized changed-target set.

Incremental validation shall not omit:

* mandatory repository-wide uniqueness checks where affected
* dependency-cycle checks where graph edges changed
* reference-existence checks where referenced inventory changed
* any rule whose correctness requires broader context

---

# 45. Unsupported Target Policy

UnsupportedTargetPolicy shall contain:

Member	Canonical Value	Meaning
REJECT	reject	Reject an unsupported target before normal execution
REPORT	report	Represent supported contextual diagnostics through approved result models
SKIP	skip	Skip unsupported artifacts only where the containing target remains valid

REJECT shall be the default for an unsupported top-level target.

SKIP may apply only to unsupported artifacts encountered within an otherwise supported repository target and only as defined by IWP-008-02 and IWP-008-03.

Unsupported-target policy shall not permit arbitrary files to be treated as governed documents.

---

# 46. Parse Failure Policy

ParseFailurePolicy shall contain:

Member	Canonical Value	Meaning
REPORT_AND_CONTINUE	report_and_continue	Report the parse failure through approved models and continue with independent artifacts
FAIL_TARGET	fail_target	Stop validation of the affected target while preserving valid completed work
FAIL_EXECUTION	fail_execution	Treat the parse failure as an unrecoverable execution failure

REPORT_AND_CONTINUE shall be the default for an individual document parse failure where safe continuation is possible.

The selected policy shall not permit validators to receive invalid parser objects.

Parse failures and policy application shall be represented explicitly in the completed IWP-008-04 response and diagnostic models.

---

# 47. Repository State Kind

RepositoryStateKind shall contain:

Member	Canonical Value	Meaning
WORKING_STATE	working_state	Repository Service resolved current working repository state
COMMITTED_STATE	committed_state	Repository Service resolved immutable committed revision
SNAPSHOT	snapshot	Repository Service supplied immutable repository snapshot

The Validation Framework shall not implement Git state resolution independently.

Repository state identity and availability shall be supplied through Repository Service.

A request shall not accept raw Git object identifiers unless the Repository Service contract exposes them as an approved repository revision selector.

---

# 48. Deadline State

DeadlineState shall contain:

Member	Canonical Value	Meaning
NONE	none	No execution deadline is configured
ACTIVE	active	A future deadline applies
EXPIRED	expired	The deadline has been reached or exceeded
CANCELLED	cancelled	Cancellation superseded normal deadline processing

Deadline state shall be derived from immutable timing and cancellation information.

The state shall not be supplied directly by an external caller.

---

# 49. Validation Target Model Hierarchy

The Validation Framework shall define one public target protocol or abstract base contract:

ValidationTarget

The required concrete target models are:

RepositoryValidationTarget
DocumentValidationTarget
DocumentSetValidationTarget

Every validation target shall expose:

Field	Type	Required	Meaning
target_id	ValidationTargetId	Yes	Canonical target identity
kind	ValidationTargetKind	Yes	Target category
display_name	str	Yes	Safe human-readable target name
repository_id	RepositoryId	Conditional	Repository identity for repository-managed targets

The base target contract shall not expose:

* absolute path
* open file handle
* repository client
* Git client
* parser object
* mutable metadata
* document contents
* logger
* service registry
* dependency-injection container

Target implementations shall be immutable value objects.

---

# 50. Repository Validation Target

RepositoryValidationTarget shall represent one repository managed by Repository Service.

Required fields:

Field	Type	Required	Meaning
target_id	ValidationTargetId	Yes	Canonical target identity
kind	ValidationTargetKind	Yes	Must equal repository
repository_id	RepositoryId	Yes	Repository Service identity
display_name	str	Yes	Safe repository display name
state_kind	RepositoryStateKind	Yes	Requested or resolved repository state kind
revision_selector	Approved repository revision model	No	Optional Repository Service revision selector
include_paths	Immutable sequence of repository-relative paths	No	Optional target-path inclusion filter
exclude_paths	Immutable sequence of repository-relative paths	No	Optional target-path exclusion filter

Invariants:

1. kind shall equal ValidationTargetKind.REPOSITORY.
2. repository_id shall be valid.
3. display_name shall be non-empty.
4. Paths shall use the approved Repository Service repository-relative path abstraction.
5. Absolute paths shall be rejected.
6. Parent traversal escaping repository scope shall be rejected.
7. Duplicate include paths shall be rejected or canonically deduplicated according to the approved path-filter contract.
8. Duplicate exclude paths shall be handled consistently with include paths.
9. The same canonical path shall not appear in both inclusion and exclusion collections.
10. revision_selector shall be compatible with state_kind.
11. Model construction shall not verify repository existence.
12. Model construction shall not enumerate repository contents.

An empty inclusion collection means no explicit inclusion restriction.

An empty exclusion collection means no explicit exclusion restriction.

---

# 51. Document Validation Target

DocumentValidationTarget shall represent one governed document within a repository.

Required fields:

Field	Type	Required	Meaning
target_id	ValidationTargetId	Yes	Canonical target identity
kind	ValidationTargetKind	Yes	Must equal document
repository_id	RepositoryId	Yes	Owning repository identity
document_path	Repository-relative path model	Yes	Canonical repository-relative document path
document_id	DocumentId	No	Expected canonical governed document identifier
display_name	str	Yes	Safe target display name
state_kind	RepositoryStateKind	Yes	Repository state kind
revision_selector	Approved repository revision model	No	Optional Repository Service revision selector

Invariants:

1. kind shall equal ValidationTargetKind.DOCUMENT.
2. document_path shall be repository-relative.
3. document_path shall identify a supported candidate document type.
4. document_path shall not end with a directory separator.
5. document_id, when supplied, shall be a valid canonical DocumentId.
6. A mismatch between supplied document_id and document content shall be evaluated by validators rather than repaired during target construction.
7. Model construction shall not read the document.
8. Model construction shall not infer document_id from file contents.
9. Model construction may derive a safe default display_name from the canonical path only where the public constructor contract explicitly permits it.

---

# 52. Document Set Validation Target

DocumentSetValidationTarget shall represent an explicit immutable set of governed documents in one repository state.

Required fields:

Field	Type	Required	Meaning
target_id	ValidationTargetId	Yes	Canonical set identity
kind	ValidationTargetKind	Yes	Must equal document_set
repository_id	RepositoryId	Yes	Owning repository identity
documents	Immutable tuple of DocumentValidationTarget	Yes	Explicit document targets
display_name	str	Yes	Safe set display name
state_kind	RepositoryStateKind	Yes	Shared repository state kind
revision_selector	Approved repository revision model	No	Shared repository revision selector

Invariants:

1. kind shall equal ValidationTargetKind.DOCUMENT_SET.
2. documents shall not be empty.
3. Every document shall use the same repository_id.
4. Every document shall use the same state_kind.
5. Every document shall use an equivalent revision selector.
6. Every document target identifier shall be unique.
7. Every canonical document path shall be unique.
8. The tuple shall be stored in canonical target order.
9. Nested document sets are prohibited.
10. Repository targets are prohibited within the document set.
11. Model construction shall not access repository contents.

Canonical document-set ordering shall use repository-relative path and then target identifier.

---

# 53. Validation Request

ValidationRequest shall be the canonical public request accepted by the Validation Service.

Required fields:

Field	Type	Required	Default
request_id	ValidationRequestId	Yes	Generated by Validation Service when omitted by an approved boundary
target	ValidationTarget	Yes	None
scope	ValidationScope	No	target
mode	ValidationMode	No	standard
rule_selection	RuleSelection	No	Default rule selection
execution_options	ValidationExecutionOptions	No	Approved defaults
correlation_id	CorrelationId	No	None
requested_at	UTC timestamp	Yes	Assigned at request acceptance
request_metadata	Immutable approved metadata	No	Empty
model_contract_version	Positive integer	Yes	1

The public request shall represent caller intent.

It shall not contain:

* resolved validators
* parser output
* repository inventory
* dependency graph
* cross-reference index
* cached result
* service instances
* loggers
* executors
* threads
* open resources
* mutable options
* CLI rendering preferences

---

# 54. Validation Request Invariants

A ValidationRequest shall enforce:

1. request_id is valid.
2. target is one approved target model.
3. scope is compatible with the target kind.
4. mode is compatible with rule selection.
5. rule_selection is internally valid.
6. execution_options is internally valid.
7. requested_at is timezone-aware and normalized to UTC.
8. model_contract_version equals a supported version.
9. request metadata contains only approved primitive values.
10. request metadata keys are non-empty strings.
11. request metadata does not contain reserved internal keys.
12. request metadata remains within approved size limits.
13. repository and document paths are not duplicated in request metadata.
14. execution controls are represented only through execution_options.
15. rule controls are represented only through rule_selection.

A request shall be rejected when:

* target is missing
* target kind is unsupported
* scope is unsupported for the target
* selected rules are empty under explicit selection
* selected categories are empty under category selection
* include and exclude rules conflict
* execution timeout is non-positive
* concurrency is outside approved bounds
* contract version is unsupported
* request timestamp is malformed
* metadata contains unsupported values

---

# 55. Validation Request Metadata

request_metadata shall provide limited non-functional caller context.

Permitted use cases include:

* approved invocation source
* approved workflow identifier
* approved parent operation identifier
* non-sensitive diagnostic tags

Request metadata shall not control:

* rule selection
* severity
* fail-fast behavior
* cache behavior
* timeout
* concurrency
* repository access
* parser selection
* result status
* CLI exit code

The initial limits shall be:

Constraint	Limit
Maximum metadata entries	32
Maximum key length	64 characters
Maximum scalar string value	256 characters
Maximum total serialized metadata	8 KiB
Maximum nesting depth	2

Metadata shall reject:

* credentials
* access tokens
* private keys
* document contents
* arbitrary binary data
* open objects
* callables
* service instances
* cyclic structures
* non-string mapping keys

---

# 56. Rule Selection Model

RuleSelection shall define the caller’s approved rule-selection intent.

Fields:

Field	Type	Required	Default
mode	RuleSelectionMode	Yes	default
categories	Immutable set of ValidationRuleCategory	No	Empty
include_rule_ids	Immutable set of RuleId	No	Empty
exclude_rule_ids	Immutable set of RuleId	No	Empty
include_optional_rules	bool	No	true

Invariants:

1. DEFAULT shall not require categories or included rule identifiers.
2. ALL shall not require categories or included rule identifiers.
3. CATEGORIES shall require at least one category.
4. EXPLICIT shall require at least one included rule identifier.
5. A rule identifier shall not appear in both include and exclude sets.
6. Mandatory rules shall not be excluded.
7. Duplicate values shall normalize according to set semantics.
8. Rule identifiers shall be canonical.
9. Category values shall be canonical enumeration members.
10. Rule selection shall not resolve registry contents during construction.

The Validation Engine shall resolve this intent against an immutable validator registry snapshot.

---

# 57. Mandatory Rule Preservation

The model layer shall prevent explicit representation of an approved mandatory-rule bypass.

Where mandatory rule identity is available only after registry preparation:

1. the request model may carry requested exclusions
2. the Validation Service shall validate syntactic correctness
3. the Validation Engine shall reject exclusions that resolve to mandatory rules
4. the normalized execution plan shall always contain all applicable mandatory rules

A request containing an attempted mandatory-rule exclusion shall not silently ignore the exclusion.

It shall produce an explicit invalid-request or planning error according to IWP-008-01 and IWP-008-02.

---

# 58. Validation Execution Options

ValidationExecutionOptions shall contain approved non-rule execution controls.

Fields:

Field	Type	Required	Default
strategy	ExecutionStrategy	No	sequential
max_concurrency	Positive integer	No	1 for sequential; approved configured bound for parallel
fail_fast	FailFastMode	No	disabled
cache_policy	CachePolicy	No	default
incremental_mode	IncrementalValidationMode	No	disabled
unsupported_target_policy	UnsupportedTargetPolicy	No	reject
parse_failure_policy	ParseFailurePolicy	No	report_and_continue
timeout	Optional duration model	No	None
rule_timeout	Optional duration model	No	None
collect_phase_timings	bool	No	true
collect_rule_timings	bool	No	true

Execution options shall not include:

* logger level
* terminal verbosity
* output format
* color settings
* exit-code mapping
* report destination
* filesystem paths
* validator class names
* parser class names
* arbitrary executor objects
* arbitrary callback functions

---

# 59. Execution Option Invariants

ValidationExecutionOptions shall enforce:

1. max_concurrency is at least 1.
2. max_concurrency does not exceed the approved platform safety bound.
3. SEQUENTIAL requires max_concurrency == 1.
4. BOUNDED_PARALLEL requires an approved max_concurrency.
5. timeout, when present, is positive.
6. rule_timeout, when present, is positive.
7. rule_timeout shall not exceed the overall timeout where both are specified, unless the engine explicitly clamps it.
8. CHANGED_ONLY requires changed-target information to be available during context construction.
9. cache and incremental settings shall not bypass repository-state fingerprint requirements.
10. fail-fast mode shall be one approved enumeration value.
11. collection of timings shall not alter execution semantics.
12. unsupported-target and parse-failure policies shall be compatible with target type and engine behavior.

The maximum concurrency safety bound shall be configuration-driven where approved, with a hard implementation ceiling that prevents unbounded thread or task creation.

The model shall not read configuration directly.

The Validation Service or Validation Engine shall supply resolved approved bounds during validation or normalization.

---

# 60. Duration Model

Execution durations shall use one immutable shared duration value object:

ValidationDuration

The canonical stored unit shall be integer nanoseconds or the approved baseline high-resolution duration representation.

The public serialized representation shall use integer milliseconds unless an approved baseline contract mandates another unit.

ValidationDuration shall:

* reject negative values
* support zero only for measured elapsed durations
* require positive values for configured timeouts
* avoid floating-point accumulation
* support deterministic comparison
* support safe conversion to seconds for standard-library timeout APIs
* reject values exceeding the approved maximum execution duration

Factory methods may include:

ValidationDuration.from_milliseconds(...)
ValidationDuration.from_seconds(...)
ValidationDuration.from_timedelta(...)

Conversion shall reject:

* non-finite values
* negative configured timeouts
* overflow
* ambiguous strings
* unitless external floating-point values

---

# 61. Timestamp Model

Validation timestamps shall use timezone-aware UTC values.

The implementation may use the approved baseline timestamp abstraction or one immutable wrapper:

ValidationTimestamp

A canonical timestamp shall:

* be timezone-aware
* normalize to UTC
* serialize using ISO 8601
* include sufficient precision for deterministic event ordering
* reject naive timestamps
* reject invalid dates
* avoid local timezone conversion within model construction

Canonical serialized example:

2026-07-11T17:30:45.123456Z

Model equality shall compare normalized instants.

Display-time localization belongs to presentation components and is out of scope.

---

# 62. Validation Execution Context

ValidationExecutionContext shall be the canonical immutable per-request context consumed by the Validation Engine and validators.

Fields:

Field	Type	Required	Meaning
request	Normalized ValidationRequest	Yes	Accepted normalized request
execution_id	ValidationExecutionId	Yes	Engine execution identity
repository_context	ValidationRepositoryContext	Conditional	Resolved repository state
registry_snapshot	Validator registry snapshot descriptor	Yes	Immutable registered-rule view
started_at	UTC timestamp	Yes	Execution start instant
deadline	Optional ValidationDeadline	No	Execution deadline
cancellation	ValidationCancellationContext	Yes	Cancellation state interface or immutable token descriptor
changed_targets	Immutable set of ValidationTargetId	No	Approved changed-target set
context_metadata	Immutable internal metadata	No	Approved internal diagnostic context
model_contract_version	Positive integer	Yes	1

The execution context shall be constructed by the Validation Engine from an accepted normalized request.

External consumers shall not construct execution contexts directly.

---

# 63. Execution Context Isolation

Every accepted validation request shall receive one isolated execution context.

An execution context shall not be reused across:

* separate request identifiers
* separate execution identifiers
* separate repository states
* separate resolved validator registry snapshots
* separate deadlines
* separate cancellation scopes

The context may reference immutable shared analysis artifacts where explicitly permitted by IWP-008-02.

It shall not contain mutable request-global state shared with another active execution.

Validators shall receive only the context data required by their protocol.

---

# 64. Validation Repository Context

ValidationRepositoryContext shall describe the Repository Service state resolved for one execution.

Fields:

Field	Type	Required	Meaning
repository_id	RepositoryId	Yes	Repository identity
state_kind	RepositoryStateKind	Yes	Resolved state category
state_fingerprint	RepositoryStateFingerprint	Yes	Stable execution-state fingerprint
revision	Approved immutable repository revision model	No	Resolved revision
repository_type	Approved Repository Service repository type	Yes	Repository classification
resolved_at	UTC timestamp	Yes	Resolution instant
root_descriptor	Approved opaque Repository Service descriptor	No	Non-filesystem repository root identity where contractually required

The repository context shall not expose:

* local absolute checkout path
* Git process handle
* Repository Service implementation object
* mutable repository client
* authentication material
* remote credentials
* unfiltered environment variables

Repository operations shall continue to occur through Repository Service.

---

# 65. Repository State Fingerprint

RepositoryStateFingerprint shall be an immutable opaque value representing the repository state relevant to deterministic validation and caching.

The fingerprint shall be supplied or constructed from Repository Service-approved state information.

It shall be:

* deterministic for equivalent repository state
* sensitive to governed content changes
* sensitive to relevant repository structure changes
* independent of discovery order
* independent of local absolute path
* safe to log in abbreviated form
* safe to use in cache keys
* unsuitable as an authorization token

The fingerprint value shall use a prefixed canonical format such as:

sha256:<lowercase-hex-digest>

The exact algorithm shall follow the approved baseline digest convention.

The Validation Models package shall validate fingerprint syntax but shall not traverse repository content to create it.

---

# 66. Validation Cancellation Context

Cancellation shall be represented by an approved cancellation abstraction:

ValidationCancellationContext

The abstraction shall provide read-only cancellation observation.

It may expose:

is_cancelled()
raise_if_cancelled()
cancelled_at
reason_code

The model shall not expose a public cancellation mutation method to validators.

Cancellation authority shall remain with the Validation Service or Validation Engine owner.

Where the implementation requires a mutable cancellation source and immutable cancellation token:

ValidationCancellationSource
        |
        v
ValidationCancellationContext

Only the read-only context shall be included in ValidationExecutionContext.

Cancellation reason text shall be bounded and shall not contain sensitive values.

---

# 67. Validation Deadline

ValidationDeadline shall represent an optional execution deadline.

Fields:

Field	Type	Required	Meaning
deadline_at	UTC timestamp	Yes	Absolute deadline
created_at	UTC timestamp	Yes	Time at which deadline was established
timeout	ValidationDuration	Yes	Original timeout duration

Invariants:

1. timeout shall be positive.
2. deadline_at shall equal or follow created_at.
3. all timestamps shall be UTC
4. the deadline shall be immutable
5. remaining time shall be calculated from an injected or explicitly supplied current timestamp where deterministic testing requires it
6. model construction shall not start timers
7. model construction shall not create threads or tasks

Derived operations may include:

state_at(now)
remaining_at(now)
is_expired_at(now)

These operations shall remain pure.

---

# 68. Validator Registry Snapshot Descriptor

The execution context shall reference an immutable registry snapshot descriptor rather than the mutable validator registry.

The descriptor shall include:

Field	Type	Required
snapshot_id	Stable snapshot identifier	Yes
created_at	UTC timestamp	Yes
validator_ids	Canonically ordered tuple of ValidatorId	Yes
rule_descriptors	Canonically ordered tuple of RuleDescriptor	Yes
registry_version	Positive integer or approved version value	Yes

The descriptor shall guarantee that rule selection and execution planning operate against one stable registry view.

Registry mutation after snapshot creation shall not alter the snapshot.

The snapshot descriptor shall not expose mutable validator collections.

Internal executable validator references may be maintained separately by the Validation Engine, keyed by the immutable descriptor identities.

---

# 69. Rule Descriptor

RuleDescriptor shall be the immutable metadata contract for one registered rule.

Fields:

Field	Type	Required
rule_id	RuleId	Yes
validator_id	ValidatorId	Yes
category	ValidationRuleCategory	Yes
name	str	Yes
description	str	Yes
version	RuleVersion	Yes
mandatory	bool	Yes
enabled_by_default	bool	Yes
execution_phase	Approved validator execution phase	Yes
supported_target_kinds	Immutable set of ValidationTargetKind	Yes
requires_repository_inventory	bool	Yes
requires_document_content	bool	Yes
requires_metadata	bool	Yes
requires_reference_index	bool	Yes
requires_dependency_graph	bool	Yes
thread_safe	bool	Yes
configuration_keys	Immutable tuple of approved configuration keys	No

Rule descriptors shall be validated during registry preparation.

A rule descriptor shall not contain:

* validator instance
* executable callback
* logger
* parser
* mutable configuration
* CLI display style
* current execution status

---

# 70. Rule Version

RuleVersion shall represent the semantic implementation contract version of one validation rule.

The initial format shall be a positive integer:

1

A rule version shall increase when a rule’s observable validation semantics change.

A version increase may be required when:

* applicability changes
* finding conditions change
* finding identity behavior changes
* source-location behavior changes
* default severity changes
* required analysis inputs change

Refactoring without observable semantic change shall not require a rule version increase.

RuleId and RuleVersion shall remain separate fields.

---

# 71. Validation Plan

ValidationPlan shall be the immutable resolved execution plan created by the Validation Engine.

Fields:

Field	Type	Required
plan_id	Stable execution-local plan identifier	Yes
execution_id	ValidationExecutionId	Yes
request_id	ValidationRequestId	Yes
target_id	ValidationTargetId	Yes
registry_snapshot_id	Registry snapshot identifier	Yes
created_at	UTC timestamp	Yes
strategy	ExecutionStrategy	Yes
max_concurrency	Positive integer	Yes
fail_fast	FailFastMode	Yes
rule_executions	Canonically ordered tuple of PlannedRuleExecution	Yes
skipped_rules	Canonically ordered tuple of SkippedRulePlanEntry	No
required_analysis	ValidationAnalysisRequirements	Yes
plan_fingerprint	Stable plan fingerprint	Yes

The plan shall contain only rules resolved from the immutable registry snapshot.

Once published for execution, the plan shall not change.

---

# 72. Validation Plan Invariants

A ValidationPlan shall enforce:

1. request, execution, target, and snapshot identifiers are valid.
2. every planned rule has a unique RuleExecutionId.
3. every planned rule has a unique RuleId.
4. mandatory applicable rules are present.
5. excluded optional rules are absent.
6. rule order is canonical.
7. planned phases are non-decreasing in canonical phase order.
8. strategy and concurrency are compatible.
9. required analysis is the union of planned rule prerequisites.
10. skipped rules do not also appear as planned rules.
11. the plan fingerprint reflects all execution-semantic fields.
12. plan construction does not invoke validators.
13. plan construction does not mutate the registry snapshot.
14. plan construction does not load additional repository artifacts beyond approved preparation behavior.
15. plan construction remains deterministic for equivalent inputs.

---

# 73. Planned Rule Execution

PlannedRuleExecution shall represent one rule invocation in an execution plan.

Fields:

Field	Type	Required
rule_execution_id	RuleExecutionId	Yes
rule_id	RuleId	Yes
validator_id	ValidatorId	Yes
rule_version	RuleVersion	Yes
category	ValidationRuleCategory	Yes
execution_phase	Approved validator execution phase	Yes
sequence	Non-negative integer	Yes
applicable_target_ids	Canonically ordered tuple of ValidationTargetId	Yes
timeout	Optional ValidationDuration	No
mandatory	bool	Yes
thread_safe	bool	Yes
analysis_requirements	ValidationAnalysisRequirements	Yes

The sequence shall provide deterministic canonical ordering.

Sequence values shall be unique within one plan.

A planned rule execution shall not contain the executable validator instance.

The Validation Engine shall resolve the executable implementation through the registry snapshot’s internal execution mapping.

---

# 74. Skipped Rule Plan Entry

SkippedRulePlanEntry shall represent a rule excluded before invocation under an approved condition.

Fields:

Field	Type	Required
rule_id	RuleId	Yes
validator_id	ValidatorId	Yes
reason_code	Stable skip-reason enumeration or code	Yes
reason	Bounded diagnostic string	Yes
target_ids	Immutable tuple of ValidationTargetId	No

Approved planning skip reasons may include:

* not applicable to target kind
* optional rule disabled
* category not selected
* explicitly excluded optional rule
* required artifact type absent
* unsupported document category

A mandatory applicable rule shall not be skipped because it was excluded by the caller.

Planning skip entries shall be distinguished from rule executions that become skipped after execution begins.

---

# 75. Validation Analysis Requirements

ValidationAnalysisRequirements shall describe the shared analysis artifacts required by a rule or complete plan.

Fields:

Field	Type	Default
repository_inventory	bool	false
document_contents	bool	false
parsed_documents	bool	false
metadata_index	bool	false
document_id_index	bool	false
reference_index	bool	false
dependency_graph	bool	false
repository_placement_index	bool	false
content_digests	bool	false

The model shall support deterministic union operations.

For two requirements models A and B, the union shall set each field to the logical OR of the corresponding fields.

The model shall remain descriptive.

It shall not load or calculate analysis artifacts.

---

# 76. Rule Execution Request

RuleExecutionRequest shall be the immutable internal model supplied by the Validation Engine for one validator invocation.

Fields:

Field	Type	Required
rule_execution_id	RuleExecutionId	Yes
rule_descriptor	RuleDescriptor	Yes
execution_context	Restricted immutable execution context view	Yes
targets	Canonically ordered tuple of normalized validation targets	Yes
analysis_snapshot	ValidationAnalysisSnapshot	Yes
started_at	UTC timestamp	Yes
deadline	Optional ValidationDeadline	No

The request shall contain only the information required for deterministic validator execution.

Validators shall not receive the complete public request metadata unless explicitly required by their approved protocol.

The rule execution request shall not expose:

* mutable engine state
* result builder
* shared finding list
* cache implementation
* executor
* service registry
* CLI context
* repository client
* direct filesystem path

---

# 77. Restricted Execution Context View

Validators shall receive a restricted context model rather than unrestricted engine internals.

The restricted context may expose:

Field	Purpose
request_id	Request traceability
execution_id	Execution traceability
correlation_id	Logging correlation
target_id	Target traceability
repository_id	Repository identity
repository_state_fingerprint	Deterministic state identity
validation_mode	Approved mode
cancellation	Read-only cancellation observation
deadline	Rule deadline observation
model_contract_version	Contract compatibility

It shall not expose rule-selection exclusions, mutable registry data, other validator instances, or result aggregation state.

---

# 78. Validation Analysis Snapshot

ValidationAnalysisSnapshot shall be the immutable shared analysis input consumed by validators.

Part 1A defines its container contract only.

Fields may include approved immutable references to:

Field	Type
repository_inventory	Optional immutable document inventory
document_artifacts	Immutable mapping of target identity to normalized document artifact
metadata_index	Optional immutable metadata index
document_id_index	Optional immutable document identifier index
reference_index	Optional immutable cross-reference index
dependency_graph	Optional immutable dependency graph
repository_placement_index	Optional immutable placement index
content_digests	Immutable mapping of target identity to digest
analysis_fingerprint	Stable analysis fingerprint

The snapshot shall:

1. satisfy the plan’s declared analysis requirements
2. be immutable before validator invocation
3. contain no parser-owned mutable state
4. contain no open file handles
5. contain no repository client
6. preserve deterministic ordering
7. remain safe for concurrent read access
8. expose absence explicitly where analysis was not requested
9. reject inconsistent indexes
10. identify the repository state from which it was built

Detailed normalized document, index, graph, and source-location models shall be completed within the remaining IWP-008-04 specification.

---

# 79. Execution Phase Timing

ValidationPhaseTiming shall record timing for one execution phase.

Fields:

Field	Type	Required
phase	ValidationPhase	Yes
started_at	UTC timestamp	Yes
completed_at	UTC timestamp	No
elapsed	ValidationDuration	No
status	Approved phase timing status	Yes

Invariants:

1. completed_at shall not precede started_at.
2. elapsed, when present, shall equal the approved measured difference within clock precision.
3. an incomplete phase shall not report a completed elapsed duration unless explicitly captured as partial elapsed time
4. one finalized execution shall contain at most one final timing record per canonical phase
5. timing information shall not affect validation findings or rule outcomes

Monotonic clocks shall be used for elapsed-duration measurement.

UTC wall-clock timestamps shall be used for traceable start and completion instants.

---

# 80. Rule Execution Timing

RuleExecutionTiming shall record timing for one rule invocation.

Fields:

Field	Type	Required
rule_execution_id	RuleExecutionId	Yes
queued_at	UTC timestamp	No
started_at	UTC timestamp	No
completed_at	UTC timestamp	No
queue_duration	ValidationDuration	No
execution_duration	ValidationDuration	No
total_duration	ValidationDuration	No

Timing invariants shall ensure:

* chronological timestamp ordering
* non-negative durations
* consistency between available timestamps and durations
* no fabricated zero duration for missing timing data
* deterministic serialization

Timing shall remain optional where collection is disabled.

A missing timing record shall not imply zero elapsed time.

---

# 81. Rule Execution Record

The Validation Engine shall produce one immutable RuleExecutionRecord for each planned rule execution.

Part 1A defines the execution identity and status fields:

Field	Type	Required
rule_execution_id	RuleExecutionId	Yes
rule_id	RuleId	Yes
validator_id	ValidatorId	Yes
rule_version	RuleVersion	Yes
status	RuleExecutionStatus	Yes
sequence	Non-negative integer	Yes
target_ids	Canonically ordered tuple of ValidationTargetId	Yes
timing	Optional RuleExecutionTiming	No
skip_reason	Optional stable reason model	No
failure	Optional approved execution-failure model	No

The completed IWP-008-04 specification shall extend this contract with the associated immutable rule outcome and finding models.

A record with COMPLETED status shall not contain an execution failure.

A record with FAILED status shall contain an approved execution-failure descriptor.

A record with SKIPPED or NOT_RUN status shall contain a stable reason.

A record shall not be mutated through successive statuses after publication.

The engine may use an internal mutable state tracker, but each published record shall be an immutable snapshot.

---

# 82. Execution Model State Separation

The implementation shall distinguish:

1. mutable internal orchestration state owned exclusively by the Validation Engine
2. immutable execution models shared across component boundaries

Mutable internal orchestration state may track:

* pending futures
* active tasks
* cancellation propagation
* executor slots
* temporary timing values
* partial aggregation

Such state shall not be:

* exported publicly
* supplied to validators
* serialized as a Validation Framework contract
* stored inside immutable domain models
* reused across executions

Before crossing a component boundary, internal state shall be converted into one approved immutable model.

---

# 83. Deterministic Execution Model Ordering

The canonical ordering rules shall be:

Model Collection	Canonical Order
Rule descriptors	Rule category, rule identifier, rule version
Planned rule executions	Execution phase, sequence, rule identifier
Target identifiers	Canonical target identifier
Document targets	Repository-relative path, target identifier
Skipped rule entries	Rule category, rule identifier, reason code
Phase timings	Canonical ValidationPhase order
Rule execution records	Plan sequence, rule identifier
Changed targets	Canonical target identifier

Parallel completion order shall never become canonical result order.

Dictionary insertion order shall not be used as an undocumented semantic ordering mechanism.

---

# 84. Execution Fingerprints

The following immutable execution models shall support stable fingerprints where required:

* normalized validation request
* repository state
* registry snapshot
* validation plan
* analysis snapshot

Fingerprints shall:

1. use canonical serialized semantic content
2. exclude non-semantic timestamps unless the timestamp affects execution semantics
3. exclude display names where identity does not depend on display name
4. exclude logger and runtime objects
5. include model contract version
6. include rule versions
7. include target identity
8. include repository state fingerprint
9. include execution-semantic options
10. use the approved digest algorithm

Opaque runtime request and execution identifiers shall not be used as the sole basis of deterministic cache keys.

---

# 85. Model Redaction

Model diagnostic representations shall redact or omit:

* credentials
* access tokens
* private keys
* environment secrets
* repository remote credentials
* full document contents
* unbounded metadata
* absolute local paths where prohibited
* arbitrary exception payloads

Request and execution model repr methods shall remain safe for diagnostic logging.

Safe diagnostic representation may include:

* typed identifiers
* target kind
* repository identity
* repository-relative paths
* rule identifiers
* execution status
* bounded counts
* abbreviated fingerprints
* canonical enumeration values

Redaction behavior shall not alter canonical serialization intended for approved internal processing unless the field itself is prohibited from serialization.

---

# 86. Model Security Requirements

Validation models shall be safe to construct from untrusted CLI and repository-derived values.

The implementation shall:

1. enforce string and collection limits
2. reject control characters in identifiers
3. reject repository path traversal
4. reject unsupported path schemes
5. reject cyclic metadata structures
6. reject excessively nested primitive payloads
7. reject non-finite numeric values
8. reject arbitrary executable objects
9. avoid unsafe deserialization
10. avoid dynamic imports based on model values
11. avoid evaluating strings
12. avoid using serialized model values as shell commands
13. avoid using display names as trusted paths
14. prevent resource exhaustion through unbounded collections
15. preserve Repository Service as the repository access boundary

No model constructor shall execute repository-supplied code.

---

# 87. Model Thread Safety

All published Validation Framework models shall be safe for concurrent read access.

Thread safety shall be achieved through immutability rather than internal locking wherever possible.

Models shall not use:

* lazy mutable caches without synchronization
* mutable default values
* shared class-level mutable collections
* thread-local semantic state
* process-global model registries
* in-place memoization visible to consumers

Where a computed property is cached, the cache shall:

* not alter equality
* not alter hashing
* not alter serialization
* be safely initialized
* be replaceable by recomputation
* remain internal

The preferred implementation is pure recomputation for inexpensive derived values.

---

# 88. Model Boundary Conversion

External primitive input shall be converted at the Validation Service boundary.

Repository Service models shall be converted or wrapped at the Validation Service or Validation Engine boundary only where required.

Validator-private data shall be converted into approved rule outcome and finding models before return to the Validation Engine.

CLI-specific arguments shall not be passed directly into the Validation Engine.

The boundary sequence shall be:

CLI / Consumer Input
        |
        v
Primitive Input Validation
        |
        v
ValidationRequest Construction
        |
        v
Validation Service Normalization
        |
        v
ValidationExecutionContext Construction
        |
        v
ValidationPlan Construction
        |
        v
RuleExecutionRequest Construction

Every conversion shall be explicit and testable.

---

# 89. Prohibited Model Shortcuts

The following implementation shortcuts are prohibited:

* using one generic dictionary for all requests
* using one generic dictionary for all results
* passing CLI namespace objects into Validation Service
* passing Repository Service implementation objects into validators
* passing parser objects between validators
* representing identifiers as interchangeable strings throughout the engine
* representing enumeration fields as unrestricted strings
* using exceptions as ordinary findings
* using null to represent skipped, failed, and unsupported states
* modifying request objects during normalization
* using mutable findings lists as shared validator output
* deriving canonical order from completion order
* accepting unknown fields without validation
* silently ignoring malformed optional fields
* exposing absolute filesystem paths in public targets
* embedding presentation labels in domain models
* combining execution status and validation result status

---

# 90. Part 1A Implementation Boundary

Part 1A establishes the mandatory contracts for:

* immutable model foundations
* typed identifiers
* closed enumerations
* validation targets
* validation requests
* rule selection
* execution options
* execution context
* repository context
* cancellation and deadlines
* registry snapshots
* rule descriptors
* validation plans
* analysis requirements
* rule execution requests
* execution timing
* execution records
* deterministic ordering
* fingerprints
* security and thread safety

Implementation shall not treat Part 1A as the complete IWP-008-04 package.

The Validation Models package shall not be considered complete or approved until the remaining IWP-008-04 specification defines and the implementation delivers:

* normalized document models
* document inventory models
* metadata models
* source-location models
* cross-reference models
* dependency graph models
* validation finding models
* validator outcome models
* execution failure models
* validation summary models
* validation result models
* validation response models
* reporting models
* serialization envelopes
* builders and factories
* public exports
* tests
* acceptance criteria
* final deliverables

