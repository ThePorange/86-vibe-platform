# IWP-008-03 – Validators Implementation Specification

## Document Control

| Item | Value |
|---|---|
| Document ID | IWP-008-03 |
| Document Name | Validators Implementation Specification |
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
| Related Specifications | IWP-008-01 – Validation Service; IWP-008-02 – Validation Engine |
| Supersedes | None |

---

# 1. Purpose

This document defines the implementation requirements for the validation rules and validators delivered by:

**IWP-008 – Validation Framework**

Validators are the deterministic, read-only components that evaluate governed platform documents and repository artifacts against the rules defined by the approved architecture.

This companion specification defines:
- validator identity
- validator protocol
- validator metadata
- validator registration requirements
- validator lifecycle
- document discovery rules
- document classification rules
- document identification rules
- naming validators
- metadata validators
- structural validators
- cross-reference validators
- dependency validators
- repository-placement validators
- validator finding behavior
- validator severity behavior
- source-location behavior
- validator independence
- validator security requirements
- validator unit and integration testing
- the minimum rule catalog required for IWP-008

Validators shall execute only through the Validation Engine defined by IWP-008-02.

External consumers shall not invoke validators directly.

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
- IWP-008-04 – Validation Models Implementation Specification
- IWP-008-05 – CLI Integration Implementation Specification
- IWP-008-06 – Testing and Acceptance Implementation Specification

Where this specification conflicts with an approved architecture document, the approved architecture document takes precedence.
Cursor shall stop and report a material ambiguity rather than inventing a validation convention.

---

# 3. Implementation Baseline

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
Validators shall:
1. consume repository-managed artifacts through Repository Service
2. use approved Validation Framework models
3. execute only through the Validation Engine
4. use Logging Service through approved injected logging facilities
5. preserve all existing package dependency directions
6. remain independent of CLI presentation
7. remain independent of future services
8. avoid direct filesystem access
9. avoid direct Git access
10. avoid modifying baseline services except for a minimal defect correction required for IWP-008
11. document every such defect correction in the final completion report

---

# 4. Strict Scope

This companion specification defines the minimum validators required by AEP-002-11:
| Rule Category | Purpose |
|---|---|
| Naming | Filename validation |
| Metadata | Required metadata validation |
| Structure | Document organization validation |
| References | Cross-document reference validation |
| Dependencies | Dependency declaration and graph validation |
| Repository | Repository placement and governed-document location validation |

It also defines the supporting behavior required to:
- discover governed documents
- classify governed documents
- extract normalized document information
- identify unique document identifiers
- build reference indexes
- build dependency graph inputs
- report deterministic findings
- preserve source locations
- register validators safely
- test every required rule category

---

# 5. Explicitly Out of Scope

This companion specification shall not implement:
- the public Validation Service
- Validation Engine scheduling
- public CLI commands
- CLI rendering
- full public validation model definitions
- AI-assisted validation
- natural-language quality scoring
- subjective architecture evaluation
- automatic document correction
- automatic document generation
- automatic approval
- document rewriting
- Git operations
- remote URL validation
- external website availability checks
- semantic implementation-code analysis
- Python static analysis
- source-code linting
- test execution
- package building
- prompt rendering
- prompt quality evaluation
- plugin loading
- dynamic validator loading from untrusted repositories
- external rule marketplaces
- persistent validation databases
- background document watching
Validation shall be limited to deterministic rules authorized by the approved architecture.

---

# 6. Validator Architectural Position

Validators are internal components of the Validation Framework.
The approved relationship is:
```text
Validation Service
        |
        v
Validation Engine
        |
        v
Validator Registry
        |
        +-----------------------------+
        |              |              |
        v              v              v
Naming Validators  Metadata       Structural
                   Validators      Validators
        |
        +-----------------------------+
        |              |              |
        v              v              v
Reference         Dependency       Repository
Validators        Validators       Validators

Validators shall not be exposed as independently registered platform services.

The Validation Service remains the single public validation entry point.

---

# 7. Validator Design Principles

Every validator shall follow these principles.

## 7.1 Deterministic

The same normalized target and context shall produce the same findings.

## 7.2 Read-Only

Validators shall never modify repository contents or target objects.

## 7.3 Independent

Validators shall remain independent of one another where practical.

## 7.4 Focused

Each validator shall evaluate one cohesive rule or tightly related rule set.

## 7.5 Standardized

Every validator shall return findings through the approved Validation Framework models.

## 7.6 Repository Independent

Validators shall operate consistently across supported repository types unless a rule is explicitly repository-type-specific.

## 7.7 Extensible

Future validators shall be addable through the approved validator protocol without changing consumers.

## 7.8 Safe

Validators shall treat repository content as untrusted data and shall not execute it.

---

# 8. Stop Conditions

Cursor shall stop and report an architectural ambiguity if:

1. An exact naming convention required for a rule is not defined by the approved architecture or approved configuration.
2. Two approved documents define incompatible filename conventions.
3. Required metadata labels cannot be mapped deterministically to approved document-control fields.
4. The approved architecture does not define how a document category is identified.
5. A rule would require subjective interpretation.
6. A rule would require natural-language inference.
7. A rule would require AI inference.
8. A rule would require accessing files outside Repository Service.
9. A rule would require modifying documents.
10. A rule would require executing repository code.
11. A rule would require remote URL access.
12. A required reference syntax cannot be identified deterministically.
13. A required dependency syntax cannot be identified deterministically.
14. A required approval structure cannot be validated from approved conventions.
15. A validator would require a dependency on CLI or a future service.
16. The validator contract cannot be satisfied using the models defined by IWP-008-04.
17. A mandatory rule’s severity is materially undefined.
18. Implementing a rule would require broad unrelated refactoring.

Cursor shall not invent a regex, metadata alias, exception, or fallback for a stop condition.

---

# 9. Validator Identity

Each validator shall possess immutable metadata.

Minimum logical metadata shall include:

Field	Requirement
Rule Identifier	Unique stable identifier
Rule Name	Human-readable descriptive name
Description	Concise explanation
Category	Approved rule category
Supported Target Types	Explicit typed set
Supported Scopes	Explicit typed set
Default Severity	Approved standard severity
Mandatory	Whether the rule is mandatory
Enabled	Whether enabled by approved configuration
Version	Stable version where required
Execution Phase	Approved lifecycle phase
Thread Safe	Explicit declaration
Repository Types	Applicable repository types where constrained

The exact models are defined by IWP-008-04.

---

# 10. Rule Identifier Convention

Every rule shall have a unique stable identifier.

Rule identifiers shall:

* be deterministic
* remain stable across executions
* remain stable across refactoring
* avoid runtime-generated values
* avoid class memory addresses
* avoid file paths
* avoid random identifiers
* follow one consistent platform convention
* participate in cache identity where applicable

A logical convention may group rules by category, such as:

DOC-NAME-001
DOC-META-001
DOC-STRUCT-001
DOC-REF-001
DOC-DEP-001
DOC-REPO-001

Cursor shall use the exact rule-identifier convention established by the completed companion models or existing architecture conventions.

If no convention exists, Cursor shall stop rather than invent one.

---

# 11. Validator Protocol

Every validator shall implement one approved protocol.

A logical protocol may resemble:

class ValidatorProtocol(Protocol):
    @property
    def metadata(self) -> RuleMetadata:
        ...
    def validate(
        self,
        context: ValidationExecutionContext,
        target: ValidationTarget,
    ) -> RuleOutcome:
        ...

Equivalent signatures are permitted where required by the completed model design.

The validator protocol shall:

* be typed
* be documented
* be deterministic
* expose no write-capable operation
* return only approved models
* avoid lifecycle management outside the Validation Framework
* remain compatible with future validators

---

# 12. Validator Construction

Validator construction shall be side-effect free.

Constructors shall not:

* read repository files
* inspect Git
* execute validation
* mutate registries
* register themselves globally
* create threads
* open network connections
* write files
* load arbitrary repository modules
* read environment variables directly

Validators may receive immutable configuration or approved helper protocols through explicit dependency injection.

---

# 13. Validator Dependencies

A validator may depend only on approved internal abstractions required for its rule.

Examples may include:

* immutable execution context
* normalized document representation
* document inventory
* reference index
* dependency graph input
* approved parser abstraction
* approved rule configuration
* safe logger

A validator shall not depend directly on:

* CLI
* Service Registry
* Bootstrap Service
* Service Lifecycle Manager
* AI Provider
* MCP Client
* Prompt Management
* Event Bus
* Plugin Manager
* Platform Context
* raw filesystem APIs
* raw Git APIs

Repository access shall be mediated through the execution context and Repository Service.

---

# 14. Validator Registration

Validators shall be registered through the internal Validator Registry.

Registration shall verify:

* protocol conformance
* unique identifier
* complete metadata
* valid category
* valid severity
* valid phase
* valid target declarations
* valid scope declarations
* thread-safety declaration
* absence of duplicate registrations
* mandatory-rule availability

A mandatory validator registration failure shall prevent Validation Service readiness.

---

# 15. Validator Registry Snapshot

After successful initialization, the validator registry shall be immutable for the active runtime.

Consumers shall not be able to:

* add validators
* remove validators
* reorder validators
* replace validators
* mutate metadata
* disable mandatory rules

Future dynamic extension behavior requires approved architecture and is not part of IWP-008.

---

# 16. Governed Document Categories

AEP-002-11 defines the supported governed-document categories:

Category	Description
Architecture	Architecture packages and ADRs
Specifications	Technical specifications
Prompts	Prompt templates
Guides	User and developer documentation
Policies	Governance documentation
Reference	Supporting reference material

The implementation shall define a typed category model in IWP-008-04.

Document classification shall be deterministic.

---

# 17. Document Discovery

The framework shall discover governed documents recursively and deterministically.

Discovery shall:

* use Repository Service
* remain within approved repository locations
* exclude .git
* respect Repository Service hidden-file behavior
* respect approved ignored-file behavior
* return repository-relative paths
* return stable ordering
* avoid reading unrelated files unnecessarily
* classify supported document candidates
* preserve unsupported files as skipped rather than failures unless a rule explicitly applies

Discovery belongs to the framework lifecycle, but discovery-related classification rules are governed by this specification.

---

# 18. Discovery Locations

Governed documents shall be discovered only from approved locations established by:

* AEP-002-02
* AEP-002-10
* repository-type-specific approved structure
* current implementation configuration where architecture permits configuration

Expected logical locations may include:

architecture/
docs/
prompts/

Cursor shall inspect the approved repository structure before finalizing discovery locations.

Do not scan the entire user filesystem.

Do not scan sibling repositories.

Do not infer additional governed locations from arbitrary folder names.

---

# 19. Recursive Discovery

Recursive discovery shall:

* traverse approved locations only
* return deterministic normalized paths
* avoid cycles
* avoid unsafe symlink traversal
* avoid .git
* avoid unsupported hidden directories by default
* use Repository Service listing and path controls
* classify each supported document once

The same document path shall not appear more than once in the inventory.

---

# 20. Supported File Types

Markdown is the required governed-document format identified by the approved architecture examples.

The framework shall validate .md documents where governed-document discovery applies.

Additional file extensions shall not be treated as governed documents unless explicitly authorized by approved architecture or configuration.

Filename extension validation shall report an invalid extension only when the target is otherwise identified as a governed document.

---

# 21. Document Classification

Each discovered document shall be assigned one category or Unknown.

Classification shall use deterministic evidence such as:

* approved repository-relative location
* approved document identifier prefix
* approved metadata
* approved filename convention
* repository type

Classification precedence shall be explicit and tested.

The implementation shall not use subjective document-content interpretation.

---

# 22. Unknown Classification

A document that cannot be classified deterministically shall:

* remain representable
* receive an explicit Unknown category
* generate a finding only where the approved rule set requires all documents in that governed location to be classified
* not crash discovery
* not be silently assigned a category

Unknown classification shall be distinguishable from an execution error.

---

# 23. Normalized Document Representation

Validators shall consume a normalized immutable document representation.

It shall logically contain:

* repository-relative path
* filename
* extension
* category
* document identifier
* title
* document-control metadata
* headings
* numbered sections
* approval-section information
* extracted references
* extracted dependencies
* source locations
* parse diagnostics
* optional normalized content fingerprint

The exact model is defined by IWP-008-04.

Validators shall not repeatedly parse raw content independently where shared extraction is available.

---

# 24. Metadata Extraction Versus Validation

Metadata extraction and metadata validation are distinct.

Extraction shall:

* identify metadata fields
* preserve missing fields explicitly
* preserve duplicate fields where relevant
* preserve source locations
* avoid creating findings except for unrecoverable parse behavior

Validation shall:

* evaluate required fields
* evaluate values
* evaluate consistency
* produce findings

Validators shall not mutate extracted metadata to conceal source defects.

---

# 25. Source Locations

Findings should identify source locations where deterministically available.

A source location may include:

* repository-relative path
* line number
* column number where available
* section heading
* document-control row
* reference occurrence
* dependency declaration occurrence

Source locations shall:

* be immutable
* use one-based line numbers unless existing conventions specify otherwise
* avoid absolute user paths in public results
* remain safe for CLI reporting
* not require full document content in the finding

---

# 26. Finding Contract

Every validator shall return findings through the approved ValidationFinding model.

A finding shall logically include:

* rule identifier
* severity
* concise message
* target reference
* source location where available
* finding code or stable identifier where defined
* safe remediation guidance where approved
* related document identifier where applicable
* structured details where approved

Findings shall not be arbitrary strings or dictionaries.

---

# 27. Finding Message Requirements

Finding messages shall be:

* deterministic
* concise
* actionable
* free of raw stack traces
* free of secrets
* free of complete artifact contents
* free of unstable object representations
* consistent across equivalent failures

Messages shall identify:

* what is invalid
* where it is invalid
* what approved requirement applies

Where remediation guidance is included, it shall not rewrite the document automatically.

---

# 28. Severity Requirements

Validators shall use only the approved severities:

Severity	Meaning
Information	Informational only
Warning	Valid but requires attention
Error	Validation failure
Critical	Validation cannot continue

Required minimum behaviors from AEP-002-11 are:

* duplicate document identifiers: validation failure
* missing required metadata: validation failure
* missing referenced document: validation failure
* dependency cycle: warning
* condition preventing continued validation: Critical where applicable

Rule-specific default severity shall be declared in immutable metadata.

---

# 29. Information Findings

Information findings may be used only for deterministic, non-problematic observations that are useful to consumers.

Information findings shall not:

* replace required warnings
* replace required errors
* conceal an invalid condition
* generate excessive noise
* report normal execution internals

Information findings do not prevent overall Passed.

---

# 30. Warning Findings

Warnings indicate that validation completed and the artifact remains valid but requires attention.

Required warning behavior includes:

* dependency cycles

Other warnings may be implemented only where approved architecture defines a non-fatal concern.

Validators shall not downgrade required validation failures to warnings.

---

# 31. Error Findings

Errors indicate validation failure.

Required error conditions include:

* duplicate document identifier
* missing required metadata
* invalid required naming element
* missing required structure
* invalid cross-reference
* missing referenced document
* invalid dependency identifier
* missing declared dependency target
* repository placement violation where the placement rule is mandatory

---

# 32. Critical Findings

Critical findings indicate that validation cannot continue safely.

Examples may include:

* the target cannot be parsed sufficiently to establish document identity
* a mandatory shared index is internally inconsistent
* a document representation is structurally unusable for subsequent required phases

Critical shall not be used merely because a normal required field is absent when that absence can be reported as an Error.

Critical use shall remain narrow and tested.

---

# 33. Required Rule Categories

The following rule categories are mandatory:

1. Naming
2. Metadata
3. Structure
4. References
5. Dependencies
6. Repository

At least one registered mandatory validator shall exist for each category where applicable to the governed target set.

The exact rule count may exceed six where cohesive rules are separated.

---

# 34. Minimum Rule Catalog

The implementation shall provide logical rules equivalent to:

Naming

* governed filename format
* filename identifier consistency
* file extension

Metadata

* required metadata fields
* document identifier validity
* document identifier uniqueness
* metadata/title consistency where deterministically defined
* metadata/platform consistency where deterministically defined

Structure

* title presence
* Document Control presence
* numbered sections
* approval section

References

* referenced identifier validity
* referenced document existence
* duplicate references permitted
* invalid-reference reporting

Dependencies

* dependency identifier validity
* dependency existence
* duplicate dependency declarations
* dependency cycle warning

Repository

* governed-document placement
* category/location consistency
* repository-boundary compliance through Repository Service
* deterministic inventory inclusion

This catalog represents minimum required behavior.

Do not add subjective rule categories.

---

# 35. Naming Validation Scope

Naming validators shall verify document filenames against the approved naming conventions.

AEP-002-11 requires validation of:

* prefix
* numeric sequence
* descriptive suffix
* file extension

Examples include:

AEP-002-11-document-validation-framework.md
ADR-0003-ai-provider-selection.md

The implementation shall derive exact patterns from approved naming standards and configuration.

It shall not invent undocumented filename grammar.

---

# 36. Filename Prefix Validator

The prefix validator shall verify that:

* the filename begins with an approved document-type prefix
* the prefix matches the document category or document identifier
* prefix comparison uses the approved case convention
* unsupported prefixes produce an Error finding
* absence of an expected prefix produces an Error finding

Approved prefixes shall come from architecture or validated configuration.

---

# 37. Numeric Sequence Validator

The numeric-sequence validator shall verify:

* required numeric segments are present
* segment width follows approved convention
* separators follow approved convention
* numeric content contains digits only
* sequence structure matches the document type

The validator shall not determine whether a missing sequence number should be allocated.

It shall not renumber documents.

---

# 38. Descriptive Suffix Validator

Where the approved document type requires a descriptive suffix, the validator shall verify:

* suffix presence
* approved separator usage
* non-empty descriptive text
* approved character convention
* absence of prohibited path separators
* deterministic case convention where specified

If architecture does not define exact suffix character rules, Cursor shall stop rather than invent them.

---

# 39. File Extension Validator

The file-extension validator shall verify:

* governed documents use the approved extension
* extension comparison follows approved case rules
* unsupported extensions produce an Error
* directories are not treated as files
* multiple-extension tricks do not bypass the rule

The rule shall not rename files.

---

# 40. Filename Identifier Consistency

Where a document identifier is available from metadata or title, the filename identifier shall match it according to approved normalization rules.

The validator shall report:

* filename identifier missing
* metadata identifier missing where required
* mismatched identifiers
* incompatible document-type prefix

The validator shall not choose which identifier is authoritative by inventing precedence.

Precedence shall follow approved architecture or the normalized extraction contract.

---

# 41. Naming Findings

Naming findings shall identify:

* the invalid filename
* the failed naming component
* the expected approved convention without inventing a replacement filename
* the repository-relative path

A finding shall not expose absolute paths.

---

# 42. Metadata Validation Scope

AEP-002-11 defines the minimum required metadata:

* Architecture Package
* Document Identifier
* Title
* Platform
* Status
* Owner

Metadata validators shall validate presence and deterministic value constraints.

---

# 43. Required Metadata Validator

The required-metadata validator shall verify the presence of every mandatory metadata field applicable to the document type.

Missing required metadata shall produce an Error finding.

The validator shall:

* report each missing field deterministically
* preserve canonical field order
* avoid stopping after the first missing field
* avoid manufacturing default values
* avoid modifying source metadata

---

# 44. Metadata Field Aliases

Field aliases may be supported only where approved architecture or existing baselined documents establish deterministic equivalent labels.

Examples might include presentation differences such as:

Document
Document ID
Document Identifier

Cursor shall not invent aliases merely to make validation pass.

Alias mapping shall:

* be explicit
* be tested
* normalize to one canonical field
* preserve original source label
* detect conflicting duplicate values

---

# 45. Duplicate Metadata Fields

Where the same canonical metadata field appears more than once:

* identical repeated values may generate a Warning or Error only according to approved convention
* conflicting values shall produce an Error
* all source locations shall remain representable
* one value shall not be silently selected without deterministic approved precedence

If duplicate-field severity is not defined, use the framework’s approved metadata integrity policy from IWP-008-04 or stop for clarification.

---

# 46. Document Identifier Validity

The identifier validator shall verify:

* identifier presence
* approved prefix
* approved numeric structure
* approved separators
* approved case
* no leading or trailing whitespace
* no hidden control characters
* compatibility with document category

The exact identifier grammar shall come from approved naming standards.

The validator shall not allocate identifiers.

---

# 47. Document Identifier Uniqueness

Each governed document shall possess a unique identifier within the validated platform document set.

Duplicate identifiers shall produce Error findings.

The uniqueness validator shall:

* use the normalized document inventory
* compare canonical identifiers
* report every conflicting document
* preserve deterministic ordering
* avoid treating repeated references as duplicate identifiers
* operate across all governed categories where identifiers share the platform namespace

The finding shall identify all conflicting repository-relative paths.

---

# 48. Missing Document Identifier

A governed document without an identifier shall produce an Error finding.

The validator shall not generate a synthetic identifier.

The missing identifier may also prevent:

* reference indexing
* dependency indexing
* filename consistency validation

Dependent validators shall skip deterministically rather than crash.

---

# 49. Title Metadata Validator

The title metadata validator shall verify:

* required title metadata is present
* the title is not empty
* the title does not contain only whitespace
* the title is consistent with the document heading where approved conventions define equivalence

It shall not enforce subjective title quality.

---

# 50. Platform Metadata Validator

The platform metadata validator shall verify:

* the Platform field is present
* the value matches the approved platform identity
* comparison uses approved normalization
* conflicting platform values produce an Error

The validator shall not accept arbitrary platform names.

---

# 51. Status Metadata Validator

The status validator shall verify:

* Status is present
* the status value is from the approved status vocabulary
* the status is non-empty
* status comparison follows approved normalization

The validator shall not approve or change document status.

Where status vocabularies differ by document type, rules shall use the approved type-specific vocabulary.

---

# 52. Owner Metadata Validator

The owner validator shall verify:

* Owner is present
* Owner is non-empty
* Owner uses an approved representation where one is defined

It shall not verify a person against an external directory.

It shall not infer ownership from Git history.

---

# 53. Architecture Package Metadata Validator

For document types requiring Architecture Package metadata, the validator shall verify:

* field presence
* approved package identifier
* compatibility with document identifier
* compatibility with repository location where deterministically defined

An ADR or other category that does not require Architecture Package shall not fail solely for omitting it unless architecture defines it as mandatory for that type.

---

# 54. Metadata Consistency

Metadata consistency validators may compare:

* filename identifier
* title identifier
* Document Control identifier
* architecture package
* repository path
* document category
* platform value

A mismatch shall produce an Error where the relationship is defined by approved architecture.

Validators shall not compare subjective descriptive text unless normalization is explicitly defined.

---

# 55. Structural Validation Scope

AEP-002-11 defines the minimum required structure:

* Title
* Document Control
* Numbered sections
* Approval section

Structural validators shall inspect normalized Markdown structure.

They shall not enforce unrelated stylistic preferences.

---

# 56. Title Structure Validator

The title structure validator shall verify:

* a document title heading exists
* the title is at the approved heading level
* the title is non-empty
* only the approved number of primary title headings exists where architecture defines this

Missing required title structure shall produce an Error.

The validator shall preserve title source location.

---

# 57. Document Control Structure Validator

The Document Control validator shall verify:

* a recognizable Document Control section exists
* it appears in the approved structural location where defined
* required metadata is extractable from it
* the section is not empty

The validator shall not require one exact Markdown table syntax unless architecture mandates it.

Equivalent approved representations may be supported through deterministic parsing.

---

# 58. Numbered Sections Validator

The numbered-sections validator shall verify that required substantive sections use the approved numbering convention.

It shall validate only conventions defined by architecture.

The validator may verify:

* numbered top-level sections
* valid numeric ordering
* absence of malformed numbering
* approved treatment of unnumbered front matter
* deterministic handling of appendices where defined

It shall not invent section names.

---

# 59. Section Sequence

Where architecture requires monotonically ordered numbered sections, the validator shall report:

* duplicate section numbers
* descending sequence
* malformed numbering
* missing number where numbering is required

It shall not automatically report every numeric gap unless the approved convention forbids gaps.

---

# 60. Approval Section Validator

The approval-section validator shall verify:

* an approval section exists where required
* the section is identifiable through approved heading conventions
* required approval information is present where architecture defines it
* the section is not empty

The validator shall not approve the document.

It shall not infer approval from repository merge status.

---

# 61. Revision History

Revision History validation may be implemented only where approved architecture requires it for the applicable document type.

Where required, validation may verify:

* section presence
* required columns
* at least one revision record
* valid version representation
* deterministic row parsing

Do not make Revision History mandatory solely because many architecture documents contain it unless the governing architecture requires it.

---

# 62. Document-Type-Specific Structure

Future or specialized document types may define additional required structure.

IWP-008 shall implement only structures already approved.

Type-specific validators shall:

* declare supported categories
* remain independent
* avoid affecting unrelated document types
* use the standard finding model

Do not add speculative prompt, guide, or policy structure rules.

---

# 63. Markdown Parsing

Structural validators shall use a safe deterministic Markdown parsing approach.

The parser shall:

* treat Markdown as data
* preserve heading hierarchy
* preserve source locations where possible
* not execute embedded code
* not resolve remote resources
* handle malformed Markdown safely
* encapsulate parser-specific exceptions

Before adding a parser dependency, Cursor shall inspect existing approved dependencies.

---

# 64. Cross-Reference Validation Scope

AEP-002-11 requires cross-reference validation to verify:

* referenced document exists
* referenced identifier is valid
* duplicate references are permitted
* invalid references are reported

Missing referenced documents shall generate validation failures.

---

# 65. Reference Extraction

Reference extraction shall identify document references using approved deterministic syntax.

Possible sources may include:

* explicit document identifiers in governed reference contexts
* dependency sections
* approved metadata references
* approved Markdown links carrying governed document identifiers

The implementation shall not treat every identifier-like token in arbitrary prose as a reference unless architecture defines that behavior.

If exact reference syntax is undefined, Cursor shall stop rather than infer broadly.

---

# 66. Reference Identifier Validity

Each extracted reference identifier shall be validated against the approved identifier grammar.

Invalid reference identifiers shall produce Error findings.

The finding shall identify:

* source document
* invalid identifier
* source location
* applicable rule

The validator shall not rewrite the reference.

---

# 67. Referenced Document Existence

Each valid extracted reference identifier shall resolve to exactly one governed document in the document index.

Outcomes shall be:

* one match: valid
* zero matches: Error
* more than one match: Error due to duplicate document identity

A missing reference target shall not be treated as an execution error.

It is a validation finding.

---

# 68. Duplicate References

Duplicate references are explicitly permitted.

Therefore:

* repeated references to the same valid document shall not produce a finding merely because they repeat
* each source occurrence may remain in the reference index
* invalid duplicate references shall still produce findings for each relevant source occurrence or a deterministic consolidated finding
* duplicate reference occurrences shall not be confused with duplicate identifiers

---

# 69. Self-References

A document referencing itself shall not automatically be considered invalid unless approved architecture forbids it.

Self-dependency behavior is handled by dependency validation where applicable.

Ordinary descriptive self-reference shall remain valid unless a specific rule exists.

---

# 70. Reference Path Validation

Where a Markdown link includes both a document identifier and a repository-relative path, validators may check consistency only where approved syntax defines that relationship.

The validator shall not access external URLs.

Remote link availability checks are out of scope.

---

# 71. Reference Index

The cross-reference validator shall consume the immutable reference index created by the Validation Engine.

The index shall preserve:

* source document
* source location
* referenced identifier
* normalized target match
* duplicate occurrences
* unresolved references

Validators shall not mutate the index.

---

# 72. Dependency Validation Scope

Documents may declare dependencies.

Dependency validation shall verify:

* dependency existence
* dependency identifier validity
* duplicate dependencies
* dependency cycles

Dependency cycles shall generate Warnings.

---

# 73. Dependency Extraction

Dependency extraction shall use approved deterministic locations and syntax.

A logical declaration may resemble:

Dependencies
- AEP-001-07
- AEP-002-05

The extractor shall:

* preserve declaration order
* preserve duplicates
* preserve source locations
* normalize identifiers
* avoid interpreting arbitrary bullet lists as dependencies
* avoid resolving remote resources

---

# 74. Dependency Identifier Validity

Every declared dependency identifier shall conform to the approved identifier grammar.

Invalid dependency identifiers shall produce Error findings.

A malformed identifier shall not be inserted into the normal dependency graph as a valid node.

It may remain in diagnostics for reporting.

---

# 75. Dependency Existence

Every valid declared dependency shall resolve to one governed document.

Outcomes shall be:

* one document: valid
* no document: Error
* multiple documents with same identifier: Error

The validator shall not infer missing dependencies from filenames alone where normalized identity is unavailable.

---

# 76. Duplicate Dependencies

AEP-002-11 requires duplicate dependencies to be validated but does not explicitly define their severity.

The implementation shall use the severity and behavior defined by the approved validation model/configuration.

Cursor shall not invent a severity.

The validator shall preserve duplicate source locations and produce deterministic output.

If the companion specifications do not resolve the severity, Cursor shall stop and report the ambiguity.

---

# 77. Dependency Cycles

Dependency cycles shall produce Warning findings.

Cycle detection shall:

* use a directed graph
* operate on normalized document identifiers
* detect direct self-cycles where declared as dependencies
* detect multi-document cycles
* avoid duplicate reporting of the same canonical cycle
* return deterministic cycle ordering
* preserve source document information
* remain safe for disconnected graphs

A cycle shall not automatically produce Error or Critical.

---

# 78. Canonical Cycle Representation

To avoid duplicate cycle warnings, each cycle shall be normalized deterministically.

A canonical representation shall:

* choose one stable start node
* preserve directed edge order
* represent the closing edge
* avoid reporting rotations of the same cycle as separate cycles
* avoid reporting reverse order as equivalent where graph direction differs

The precise algorithm shall be documented and tested.

---

# 79. Dependency Graph Isolation

Dependency validators shall consume an immutable graph representation.

They shall not:

* mutate document metadata
* add inferred dependencies
* remove duplicates silently
* change document identifiers
* call other validators
* access Repository Service directly

---

# 80. Repository Validation Scope

Repository validators shall validate governed-document placement and consistency with approved repository structure.

They shall not duplicate Repository Service responsibilities.

The distinction is:

* Repository Service validates repository usability and secure access.
* Repository validators evaluate whether governed documents are placed and categorized according to approved documentation rules.

---

# 81. Governed-Document Placement

A governed-document placement validator shall verify:

* the document is under an approved governed-document location
* the category is compatible with its location
* the repository-relative path is normalized
* the document does not escape the repository
* repository-type-specific placement is respected where approved

Placement violations shall produce Error findings where mandatory.

---

# 82. Architecture Document Placement

Architecture documents shall be validated against the approved architecture repository structure.

Expected approved locations include architecture package and ADR areas defined by AEP-002-02 and the actual baselined repository structure.

The validator shall not invent new directories.

It shall not move documents.

---

# 83. Prompt Document Placement

Prompt documents shall be validated only against approved prompt locations.

This rule shall not validate prompt content or rendering behavior.

Prompt Management Service remains IWP-010.

---

# 84. Guide, Policy, and Reference Placement

Guide, Policy, and Reference documents shall be validated only where approved repository locations and classification rules are explicitly defined.

If exact placement is not defined, the implementation shall not invent a mandatory path.

The document may remain classified through approved metadata or be marked Unknown according to deterministic rules.

---

# 85. Repository Type Compatibility

Repository placement rules may vary for:

* Platform
* CLI
* Example
* Extension
* Plugin

Validators shall obtain normalized repository type from Repository Service.

They shall not infer repository type independently.

A rule shall declare applicable repository types.

Unknown repository type shall produce deterministic warning, skip, or error behavior only as defined by approved configuration and repository contracts.

---

# 86. Repository Boundary

Repository boundary enforcement belongs to Repository Service.

Validators shall rely on Repository Service to reject unsafe paths.

A path rejection is an execution error or invalid request, not an ordinary placement finding.

Validators shall not reimplement traversal or symlink security.

---

# 87. Category and Location Consistency

Where approved architecture associates a category with one or more locations, the validator shall verify consistency.

Examples of deterministic mismatches may include:

* an ADR in a prompt directory
* a prompt template in an architecture package directory
* a policy document under an implementation source package where policies are governed elsewhere

Only approved mappings shall be enforced.

---

# 88. Inventory Inclusion

A repository validator may verify that a governed document discovered through approved locations is included exactly once in the document inventory.

It shall report internal inventory inconsistency as an execution error rather than an artifact finding where the repository contents are not at fault.

---

# 89. Document Category Validators

Category validators shall determine or verify category using approved evidence.

They may compare:

* identifier prefix
* repository location
* metadata
* filename convention

Where evidence conflicts, produce an Error finding rather than silently selecting one source.

Classification precedence shall be documented.

---

# 90. Architecture Category

Architecture classification may include:

* Architecture Package documents
* Architecture Reference Package documents
* Architecture Decision Records

The implementation shall preserve the specific document type where models support it while exposing the broader Architecture category.

---

# 91. Specification Category

Specifications shall be classified only where approved identifier, location, or metadata conventions define them.

Architecture documents that contain the word “Specification” in their title shall not automatically be reclassified away from Architecture.

Classification shall use structural identity, not title keywords alone.

---

# 92. Prompt Category

Prompt classification shall rely on approved prompt location or metadata.

The validator shall not parse or validate prompt template semantics.

---

# 93. Guide Category

Guide classification shall rely on approved repository structure or metadata.

The validator shall not judge guide completeness or writing quality.

---

# 94. Policy Category

Policy classification shall rely on approved repository structure or metadata.

The validator shall not determine whether a policy is legally sufficient.

---

# 95. Reference Category

Reference documents shall be classified through approved repository structure or metadata.

The validator shall not validate factual accuracy of reference content.

---

# 96. Validator Execution Phases

Validators shall declare one approved execution phase.

Expected mappings are:

Category	Phase
Naming	Core Validation
Metadata	Core Validation
Structure	Core Validation
References	Reference Validation
Dependencies	Dependency Validation
Repository	Core Validation or Discovery, according to rule purpose

A validator shall not execute before its required analysis inputs exist.

---

# 97. Validator Prerequisites

Each validator shall declare or document its prerequisites.

Examples:

* naming validator requires normalized path
* metadata validator requires extracted metadata
* structure validator requires parsed headings
* reference validator requires reference index
* dependency validator requires dependency graph
* placement validator requires repository metadata and category

Missing prerequisites caused by an earlier Critical finding shall result in deterministic skip behavior.

Missing prerequisites caused by internal engine failure shall result in execution error behavior.

---

# 98. Validator Independence

Validators shall not invoke one another.

Shared information shall be supplied by:

* execution context
* normalized document
* analysis snapshot
* reference index
* dependency graph

A validator shall not depend on another validator’s finding collection.

---

# 99. Validator Thread Safety

Every validator shall declare whether it is thread-safe.

Mandatory validators should be stateless and thread-safe.

A validator shall not retain request-specific mutable state in instance fields.

Permitted shared state is limited to immutable configuration and immutable metadata.

Thread-safe validators may execute concurrently where the engine permits.

---

# 100. Stateless Validator Design

Validators should be implemented as stateless components.

They may contain:

* immutable metadata
* compiled approved patterns
* immutable configuration
* safe parser references

They shall not contain:

* mutable current target
* mutable finding lists
* request counters affecting semantics
* repository handles
* open file handles
* active execution context
* mutable caches outside approved framework caching

---

# 101. Parser and Pattern Reuse

Compiled patterns and parser helpers may be reused where:

* they are immutable
* they are thread-safe
* they do not retain document content
* they do not change semantics between requests

Do not compile patterns from unvalidated user content.

---

# 102. Validator Configuration

Validator configuration shall come through the approved Validation Framework configuration snapshot.

Configuration may include only architecture-authorized behavior such as:

* enabled optional validators
* approved naming patterns
* approved metadata vocabularies
* approved governed locations
* approved repository-type applicability
* approved severity overrides where permitted

Mandatory architectural validation shall not be disabled through ordinary configuration.

---

# 103. Configurable Naming Rules

Naming patterns may be configured only where architecture explicitly delegates them to Configuration Service.

Configured patterns shall:

* be validated during initialization
* be deterministic
* avoid unsafe catastrophic regular expressions
* be immutable during runtime
* be logged only in safe form where appropriate

If approved naming standards are fixed, configuration shall not override them.

---

# 104. Finding Ordering

Validators shall return findings in deterministic order.

For multiple findings from one validator, order by logical source occurrence:

1. repository-relative path
2. line number
3. column number
4. canonical field or section order
5. stable message/finding code

The Validation Engine will apply global canonical ordering.

---

# 105. Multiple Findings

Validators should report all independent findings in one execution where safe.

Examples:

* report all missing metadata fields
* report all unresolved references
* report all invalid dependency declarations
* report all duplicate identifiers

Do not stop after the first ordinary Error finding.

Stop only where continued validation is unsafe or a Critical condition exists.

---

# 106. Finding Consolidation

Findings may be consolidated when doing so preserves all relevant source information.

Examples:

* one duplicate-identifier finding listing all conflicting paths
* one dependency-cycle warning listing the canonical cycle

Do not consolidate unrelated failures merely to reduce result count.

The consolidation policy shall be deterministic.

---

# 107. No Automatic Remediation

Validators may include safe remediation guidance.

They shall not:

* rewrite filenames
* insert metadata
* create approval sections
* repair references
* remove dependencies
* break cycles
* move documents
* modify repository contents

Remediation guidance shall describe the requirement, not execute it.

---

# 108. Security Requirements

Validators shall:

* treat document content as untrusted data
* avoid code execution
* avoid shell execution
* avoid dynamic imports
* avoid remote access
* avoid unsafe deserialization
* avoid external entity expansion
* avoid arbitrary template evaluation
* avoid direct filesystem access
* use Repository Service-mediated content
* sanitize finding messages
* avoid echoing secrets
* remain read-only

---

# 109. Embedded Code Blocks

Code blocks in Markdown shall be treated as content.

Validators shall not:

* execute code blocks
* import code examples
* interpret shell commands
* validate runtime correctness
* follow URLs in code blocks
* mistake example identifiers for references unless approved extraction syntax says otherwise

Reference and dependency extraction should avoid code blocks unless architecture explicitly includes them.

---

# 110. Comments and Examples

Identifiers appearing only inside:

* fenced code blocks
* inline examples
* explanatory examples
* HTML comments

shall not automatically be treated as live references or dependencies.

The extraction policy shall explicitly define which Markdown regions are considered governed declarations.

This behavior shall be tested against AEP-002-11 examples.

---

# 111. Sensitive Values

A validator detecting a secret-like value shall not reproduce the value in a finding.

Secret scanning itself is not required by IWP-008 unless approved elsewhere.

If a parser error includes content, sanitize it before reporting.

---

# 112. Resource Safety

Validators shall handle:

* large document inventories
* large reference sets
* large dependency graphs
* malformed Markdown
* deeply nested headings
* repeated metadata fields
* long filenames

without uncontrolled resource use.

Tests shall prefer operation-count assertions over fragile timing thresholds.

---

# 113. Regex Safety

Regular expressions shall:

* be anchored where appropriate
* avoid catastrophic backtracking
* operate on bounded target strings
* be compiled once where safe
* be covered by malformed-input tests

Do not accept arbitrary user-supplied regex without initialization validation.

---

# 114. Error Handling

A validator shall distinguish:

* normal non-compliance finding
* unsupported target skip
* missing prerequisite skip
* internal validator failure

Normal invalid content shall not raise an implementation exception.

Unexpected exceptions shall be encapsulated by the Validation Engine.

Validators may raise only approved internal exceptions where the engine contract requires it.

---

# 115. Unsupported Target Behavior

If a validator receives an unsupported target due to an engine defect:

* it shall not guess
* it shall return or raise the approved internal contract error
* the engine shall classify it as Internal Validation Error

Under normal operation, the engine shall filter unsupported validators before invocation.

---

# 116. Parse Failure Behavior

A parse failure shall be handled according to its nature:

* document is readable but lacks required structure: finding
* document syntax prevents safe normalized representation: Critical finding or execution error according to the parser contract
* parser implementation crashes unexpectedly: Internal Validation Error
* repository read fails: Repository Failure

The distinction shall be tested.

---

# 117. Minimum Validator Classes

The implementation may provide cohesive validators logically equivalent to:

DocumentFilenameValidator
RequiredMetadataValidator
DocumentIdentifierValidator
DocumentIdentifierUniquenessValidator
DocumentStructureValidator
CrossReferenceValidator
DependencyDeclarationValidator
DependencyCycleValidator
RepositoryPlacementValidator

Cursor may split or consolidate these classes where:

* every required rule remains independently identifiable
* rule metadata remains clear
* test coverage remains focused
* no oversized validator becomes difficult to reason about

Do not create one monolithic validator for all behavior.

---

# 118. Expected Package Layout

Cursor shall inspect and follow the actual repository structure.

The expected logical layout is:

src/
└── vibe/
    └── validation/
        └── validators/
            ├── __init__.py
            ├── base.py
            ├── naming.py
            ├── metadata.py
            ├── structure.py
            ├── references.py
            ├── dependencies.py
            ├── repository.py
            └── classification.py

Supporting internal modules may include:

src/vibe/validation/
├── extraction.py
├── parsing.py
├── inventory.py
├── references.py
└── dependency_graph.py

Not every listed file is mandatory.

Modules may be consolidated where they remain focused and consistent with repository conventions.

---

# 119. Public Exports

The vibe.validation public package may export:

* approved validator protocol
* approved rule metadata model
* approved extension-facing validator interface where architecture intends future extension

Concrete built-in validators should remain internal unless architecture explicitly requires public construction.

Do not export:

* parser internals
* mutable indexes
* graph internals
* registry mutation methods
* compiled regex objects
* internal extraction helpers

---

# 120. Rule Catalog Documentation

The implementation shall document the built-in rule catalog.

Documentation shall include:

* rule identifier
* rule name
* category
* default severity
* mandatory status
* supported targets
* supported scopes
* concise purpose

Documentation shall not duplicate full architecture documents.

The rule catalog shall match list_rules() output.

---

# 121. Unit Testing Requirements

Each validator shall have focused unit tests.

Naming Tests

* valid approved architecture filename
* valid approved ADR filename
* invalid prefix
* malformed numeric sequence
* missing descriptive suffix where required
* invalid extension
* filename/identifier mismatch
* case behavior
* no automatic rename

Metadata Tests

* all required fields present
* each required field missing
* empty field values
* valid identifier
* invalid identifier
* duplicate identifier
* conflicting duplicate metadata
* valid platform
* invalid platform
* valid status
* invalid status
* owner missing
* architecture package mismatch

Structure Tests

* valid title
* missing title
* duplicate primary title where prohibited
* valid Document Control
* missing Document Control
* numbered sections valid
* malformed numbering
* approval section present
* approval section missing
* malformed Markdown handling

Reference Tests

* valid existing reference
* missing reference target
* invalid reference identifier
* duplicate references permitted
* duplicate document identifiers affect resolution
* references in code blocks ignored where required
* deterministic finding order

Dependency Tests

* valid dependencies
* invalid dependency identifier
* missing dependency target
* duplicate dependency declaration
* no cycle
* self-cycle
* two-node cycle
* multi-node cycle
* deterministic canonical cycle reporting
* disconnected graph

Repository Tests

* valid governed placement
* invalid placement
* category/location mismatch
* repository-type-specific applicability
* unknown repository type
* normalized repository-relative path
* no direct filesystem access

---

# 122. Validator Protocol Tests

Tests shall verify:

* metadata completeness
* stable unique identifier
* valid category
* valid severity
* valid phase
* immutable metadata
* deterministic execution
* standard outcome type
* standard finding type
* no artifact mutation
* no instance request-state retention
* thread-safety declaration

---

# 123. Registry Integration Tests

Tests shall verify:

* all mandatory validators register
* duplicate rule identifier fails
* incomplete metadata fails
* unsupported category fails
* unsupported severity fails
* invalid target declaration fails
* registry snapshot is immutable
* deterministic rule listing
* built-in rule catalog matches documentation

---

# 124. Document Discovery Integration Tests

Using temporary repositories, tests shall verify:

* recursive governed-document discovery
* deterministic ordering
* architecture document discovery
* prompt discovery
* guide discovery where approved
* policy discovery where approved
* reference discovery where approved
* hidden-file behavior
* ignored-file behavior
* .git exclusion
* duplicate path prevention
* unsupported files skipped
* symlink boundaries enforced by Repository Service

---

# 125. End-to-End Validator Integration Tests

Integration tests shall validate representative repositories containing:

* one fully valid architecture document
* one invalid filename
* one missing metadata field
* one duplicate identifier
* one missing approval section
* one broken cross-reference
* one missing dependency
* one dependency cycle
* one misplaced governed document

The resulting validation output shall be deterministic.

---

# 126. Baseline Architecture Corpus Tests

Where permitted by repository test conventions, the approved architecture corpus may be used as a non-destructive validation fixture.

Such tests shall:

* operate on a copied or read-only fixture
* avoid modifying the authoritative files
* not assume the checked-out developer path
* produce stable results
* document any known approved legacy variations
* not weaken rules solely to force a clean result

If the approved architecture corpus exposes a convention conflict, Cursor shall report it rather than suppressing the rule.

---

# 127. Determinism Tests

Each validator shall be executed repeatedly against identical normalized inputs.

Tests shall compare:

* finding count
* finding codes
* severities
* messages
* source locations
* ordering

Naturally variable execution duration shall not be part of semantic equality.

---

# 128. Concurrency Tests

Thread-safe validators shall be tested under concurrent execution.

Tests shall prove:

* no state leakage
* no finding cross-contamination
* no parser corruption
* no mutation of shared metadata
* identical semantic results
* no deadlocks

Stateless validators should be reusable across concurrent requests.

---

# 129. Security Tests

Tests shall prove:

* code blocks are not executed
* Python files are not imported
* shell commands are not invoked
* external links are not fetched
* malicious Markdown does not escape parsing boundaries
* unsafe paths remain blocked by Repository Service
* secrets are not echoed in findings
* full document contents are not logged
* validators cannot write artifacts

---

# 130. Negative Tests

Every rule shall include negative tests for malformed inputs.

Examples include:

* null or missing normalized values
* whitespace-only metadata
* invalid Unicode control characters
* malformed headings
* duplicate fields
* malformed identifiers
* extreme but bounded input lengths
* invalid graph nodes
* incomplete source locations

Validators shall fail safely and deterministically.

---

# 131. Performance Tests

Performance tests shall verify behavior rather than fragile timing.

Tests should confirm:

* shared parsed representation is reused
* validators do not reread artifacts independently
* reference validation uses an index
* dependency validation uses a graph representation
* duplicate-identifier lookup is indexed
* no unbounded recursive scanning occurs
* cycle detection terminates deterministically

---

# 132. Coding Standards

Implementation shall comply with ARP-001-05.

Requirements include:

* complete type hints
* docstrings for public protocols and extension-facing APIs
* immutable metadata
* focused validators
* explicit exceptions
* no wildcard imports
* no global mutable finding collections
* no import-time repository access
* no import-time validation
* no dynamic imports from validated repositories
* no shell execution
* deterministic collection ordering
* repository formatting conventions
* repository linting conventions
* repository static-analysis conventions

---

# 133. Dependency Requirements

Use only approved dependencies.

Prefer the Python standard library for:

* regular expressions
* immutable data
* graph traversal
* sorting
* Unicode handling
* path-string inspection
* collections

A Markdown parser dependency may be used only if:

1. already approved or present
2. necessary for deterministic structural parsing
3. safe from code execution
4. compatible with supported Python versions
5. covered by tests
6. encapsulated behind an internal parser abstraction

Do not add:

* AI libraries
* natural-language-processing libraries
* remote link checkers
* graph databases
* workflow engines
* repository-hosting SDKs
* shell wrappers
* document rewriting libraries

---

# 134. Backward Compatibility

The validator implementation shall preserve all baseline behavior.

It shall not:

* change Repository Service APIs
* change Configuration Service APIs
* change Logging Service APIs
* change Service Registry behavior
* change Bootstrap behavior
* change lifecycle contracts
* change CLI framework APIs
* introduce import-time side effects
* change approved architecture files
* modify ADR-001

All prior regression tests shall continue to pass.

---

# 135. Functional Acceptance Criteria

This companion specification is functionally complete only when:

* the validator protocol exists
* validator metadata is immutable
* all mandatory rule categories are represented
* governed documents are classified deterministically
* filenames are validated
* required metadata is validated
* document identifiers are validated
* document identifier uniqueness is validated
* required document structure is validated
* cross-references are validated
* duplicate references remain permitted
* dependencies are validated
* dependency cycles generate warnings
* repository placement is validated
* findings use the standard model
* findings use approved severities
* source locations are preserved where available
* validators remain read-only
* validators remain deterministic
* validators remain thread-safe where declared
* all validator tests pass

---

# 136. Architectural Acceptance Criteria

The validators are architecturally conformant only when:

* AEP-002-11 is satisfied
* ARP-002-08 is satisfied
* Validation Service remains the single public entry point
* validators execute only through the Validation Engine
* validators are not public platform services
* no artifact modification occurs
* no repository management is duplicated
* no direct filesystem access occurs
* no direct Git access occurs
* no AI inference occurs
* no CLI dependency exists
* rule identifiers are unique
* rule outcomes are deterministic
* findings use standard models
* severities remain consistent
* duplicate identifiers fail validation
* missing metadata fails validation
* missing references fail validation
* dependency cycles produce warnings
* future validators can implement the same protocol

---

# 137. Engineering Acceptance Criteria

The validator implementation is technically complete only when:

* type checking passes
* linting passes
* formatting checks pass
* validator metadata is validated
* validator registration is deterministic
* unit tests pass
* integration tests pass
* determinism tests pass
* concurrency tests pass
* security tests pass
* baseline regression tests pass
* package build passes where configured
* no artifact contents leak into logs
* no secrets leak into findings
* no test repositories remain
* no generated caches are committed
* no unapproved dependencies are added

---

# 138. Deliverables

This companion specification shall deliver:

* validator protocol
* validator metadata integration
* built-in validator registry entries
* governed-document classification
* filename validators
* metadata validators
* document identifier validators
* uniqueness validator
* structure validators
* cross-reference validators
* dependency validators
* cycle detector
* repository-placement validators
* safe Markdown parsing integration
* normalized extraction helpers where required
* validator unit tests
* validator integration tests
* determinism tests
* concurrency tests
* security tests
* built-in rule catalog documentation

Completion of this companion alone does not complete IWP-008.

---

# 139. Files Expected to Change

Expected additions or modifications may include:

src/vibe/validation/validators/__init__.py
src/vibe/validation/validators/base.py
src/vibe/validation/validators/naming.py
src/vibe/validation/validators/metadata.py
src/vibe/validation/validators/structure.py
src/vibe/validation/validators/references.py
src/vibe/validation/validators/dependencies.py
src/vibe/validation/validators/repository.py
src/vibe/validation/validators/classification.py
src/vibe/validation/extraction.py
src/vibe/validation/parsing.py
src/vibe/validation/inventory.py
src/vibe/validation/dependency_graph.py
src/vibe/validation/registry.py
src/vibe/validation/protocols.py
src/vibe/validation/errors.py

Expected tests may include:

tests/unit/validation/validators/test_naming.py
tests/unit/validation/validators/test_metadata.py
tests/unit/validation/validators/test_structure.py
tests/unit/validation/validators/test_references.py
tests/unit/validation/validators/test_dependencies.py
tests/unit/validation/validators/test_repository.py
tests/unit/validation/validators/test_classification.py
tests/unit/validation/test_document_extraction.py
tests/unit/validation/test_dependency_graph.py
tests/unit/validation/test_validator_registry.py
tests/integration/validation/test_document_discovery.py
tests/integration/validation/test_builtin_validators.py
tests/integration/validation/test_reference_validation.py
tests/integration/validation/test_dependency_validation.py
tests/integration/validation/test_repository_placement_validation.py

Exact paths shall follow the current repository conventions.

---

# 140. Companion Specification Dependencies

This document relies on:

* IWP-008-01 for the public Validation Service boundary
* IWP-008-02 for execution, scheduling, aggregation, caching, and lifecycle phases
* IWP-008-04 for validator metadata, target, finding, outcome, severity, source-location, and document models
* IWP-008-05 for CLI consumption of findings
* IWP-008-06 for complete testing, acceptance, quality gates, and completion reporting

Cursor shall not finalize validator signatures before reviewing IWP-008-04.

Cursor shall not implement CLI formatting inside validators.

---

# 141. Implementation Guidance for Cursor

When implementing the complete IWP-008 package, Cursor shall:

1. Review IWP-008-00 through IWP-008-06.
2. Inspect the approved architecture documents.
3. Inspect the current 86-vibe-cli baseline.
4. Confirm exact approved filename and identifier conventions.
5. Confirm approved metadata labels and vocabularies.
6. Confirm approved governed-document locations.
7. Confirm Repository Service discovery and read APIs.
8. Implement public models before validator signatures.
9. Implement normalized document extraction.
10. Implement immutable document inventory and indexes.
11. Implement the validator protocol.
12. Implement rule metadata and registry integrity checks.
13. Implement naming validators.
14. Implement metadata validators.
15. Implement structure validators.
16. Implement reference validators.
17. Implement dependency validators.
18. Implement repository-placement validators.
19. Register every mandatory validator.
20. Add focused tests for every rule.
21. Add integration, determinism, concurrency, and security tests.
22. Run all repository quality gates.
23. Stop and report any architecture ambiguity.
24. Do not begin IWP-010.

---

# 142. Definition of Done for IWP-008-03

IWP-008-03 is complete when:

* the approved validator protocol is implemented
* every validator has stable unique metadata
* validators are registered deterministically
* mandatory validators cannot be silently omitted
* document discovery inputs are classified deterministically
* filename prefix is validated
* numeric sequence is validated
* descriptive suffix is validated where required
* file extension is validated
* filename and document identifier consistency is validated
* required metadata fields are validated
* document identifier validity is validated
* duplicate document identifiers fail validation
* required title structure is validated
* Document Control is validated
* numbered sections are validated
* approval section is validated
* valid references resolve
* missing references fail validation
* invalid reference identifiers fail validation
* duplicate references remain permitted
* dependency identifiers are validated
* missing dependencies fail validation
* duplicate dependency declarations are handled according to approved policy
* dependency cycles produce warnings
* governed-document placement is validated
* repository type comes from Repository Service
* all findings use standard immutable models
* findings are deterministic
* validators remain read-only
* validators do not execute repository content
* validator unit tests pass
* integration tests pass
* determinism tests pass
* concurrency tests pass
* security tests pass
* baseline regression tests pass
* no architectural drift is introduced

Completion of IWP-008-03 alone does not authorize approval or merge of IWP-008.

The complete work package requires successful implementation of all companion specifications.

---

# 143. Next Companion Specification

The next companion specification is:

IWP-008-04 – Validation Models Implementation Specification

Do not begin implementation from this document alone.

Cursor shall receive and review the complete IWP-008 specification set before implementation begins.

---
