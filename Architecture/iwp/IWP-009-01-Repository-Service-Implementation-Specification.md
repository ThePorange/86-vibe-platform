# IWP-009-01 – Repository Service Implementation Specification

## Document Control

| Item | Value |
|---|---|
| Document ID | IWP-009-01 |
| Document Name | Repository Service Implementation Specification |
| Implementation Package | IWP-009 – Repository Service |
| Platform | 86-vibe |
| Implementation Repository | `86-vibe-cli` |
| Architecture Repository | `86-vibe-platform` |
| Status | Implementation Ready |
| Owner | Chief Systems Architect |
| Implementation Agent | Cursor |
| Depends On | IWP-001 – Repository Foundation; IWP-002 – Configuration Service; IWP-003 – Logging Service; IWP-004 – Bootstrap Service; IWP-005 – Service Registry; IWP-006 – Service Lifecycle Manager; IWP-007 – CLI Framework |
| Sequencing Authority | ADR-001 – Implement Repository Service Before Validation Framework |
| Supersedes | None |

---

# 1. Purpose

This document defines the complete implementation requirements for:

**IWP-009 – Repository Service**

The Repository Service is the authoritative platform service for discovering, opening, inspecting, validating, reading, writing, listing, refreshing, and reporting the status of repositories and repository-managed artifacts.
This work package shall implement the approved Repository Service contract without redesigning the architecture.
The implementation shall provide a single governed boundary between platform consumers and repository storage.
Cursor shall treat this document as the complete implementation prompt.
Nothing required for implementation shall exist outside this Markdown document except:
- the approved architecture repository
- the current `86-vibe-cli` implementation baseline
- the approved ADR identified in this document
- this implementation specification

---

# 2. Implementation Objective

Implement the Repository Service in the `86-vibe-cli` repository.

The implementation shall provide:
- the public `vibe.repository` package
- the public `RepositoryService`
- repository discovery
- explicit repository opening
- repository structure validation
- repository classification
- normalized repository metadata
- normalized repository status
- deterministic artifact existence checks
- deterministic artifact reads
- governed artifact writes
- deterministic directory and file listing
- architecture-document discovery metadata
- read-only Git inspection
- repository refresh
- metadata and validation-result caching
- cache invalidation
- repository-boundary enforcement
- path normalization
- traversal prevention
- repository-specific error classification
- lifecycle integration
- Service Registry integration
- Service Lifecycle Manager integration
- CLI integration for approved repository commands
- unit tests
- integration tests
- concurrency tests
- security tests
- regression tests

The implementation shall conform to:
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
- ARP-002-09 – Bootstrap Service Interface Contract
- ARP-002-10 – Service Lifecycle Manager Interface Contract
- ARP-002-11 – Service Registry Interface Contract
- ARP-002-16 – Platform Service Contract Compliance Matrix

---

# 3. Sequencing Decision

ADR-001 reverses the implementation order of IWP-008 and IWP-009.

The approved implementation sequence is now:
1. IWP-001 – Repository Foundation
2. IWP-002 – Configuration Service
3. IWP-003 – Logging Service
4. IWP-004 – Bootstrap Service
5. IWP-005 – Service Registry
6. IWP-006 – Service Lifecycle Manager
7. IWP-007 – CLI Framework
8. IWP-009 – Repository Service
9. IWP-008 – Validation Framework
10. IWP-010 – Prompt Management Service
11. IWP-011 – AI Provider Layer
12. IWP-012 – MCP Client Service
13. IWP-013 – Health Monitoring Service
14. IWP-014 – Event Bus Service
15. IWP-015 – Plugin Manager Service
16. IWP-016 – Platform Context Service
17. IWP-017 – End-to-End Platform Integration
This work package shall not renumber IWP-009.
This work package shall not implement IWP-008.
The implementation and completion report shall reference ADR-001 when describing why IWP-009 was implemented before IWP-008.

---

# 4. Current Implementation Baseline

The following work packages have already been built, approved, committed, and merged:
- IWP-001 – Repository Foundation
- IWP-002 – Configuration Service
- IWP-003 – Logging Service
- IWP-004 – Bootstrap Service
- IWP-005 – Service Registry
- IWP-006 – Service Lifecycle Manager
- IWP-007 – CLI Framework

These implementations form the immutable implementation baseline for IWP-009.

Cursor shall:
1. Inspect the current `86-vibe-cli` repository before writing code.
2. Identify the actual package, test, lifecycle, configuration, logging, registry, CLI, exception, and result-model conventions.
3. Preserve all existing public APIs.
4. Preserve all existing dependency directions.
5. Use the Configuration Service through its approved public interface.
6. Use the Logging Service through its approved public interface.
7. Register the Repository Service through the existing Service Registry mechanism.
8. integrate Repository Service lifecycle behavior through the existing Bootstrap Service and Service Lifecycle Manager.
9. replace the IWP-007 deferred repository-command behavior with real Repository Service command handlers.
10. preserve deferred behavior for services that remain unimplemented.
11. avoid modifying previous work unless correcting a defect that directly blocks IWP-009.
12. document any required baseline defect correction in the completion report.
Cursor shall not assume that example class names or physical files in this document override established repository conventions.
Where the committed baseline provides an approved equivalent abstraction, Cursor shall extend that abstraction rather than create a parallel implementation.

---

# 5. Strict Scope

This work package implements only:

**IWP-009 – Repository Service**

The scope includes:
- Repository Service public package
- Repository Service public class
- repository configuration consumption
- repository discovery
- repository opening
- repository root detection
- repository structure validation
- repository classification
- repository metadata generation
- repository status generation
- repository health reporting
- read-only Git inspection
- deterministic file discovery
- deterministic directory listing
- artifact existence checks
- artifact reading
- governed artifact writing
- path validation
- repository-boundary enforcement
- architecture-document discovery metadata
- cache behavior
- cache invalidation
- lifecycle behavior
- service registration
- CLI repository commands
- repository diagnostics used by `86vibe doctor`
- tests and documentation required for this increment

---

# 6. Explicitly Out of Scope

The following work packages remain out of scope:
- IWP-008 – Validation Framework
- IWP-010 – Prompt Management Service
- IWP-011 – AI Provider Layer
- IWP-012 – MCP Client Service
- IWP-013 – Health Monitoring Service
- IWP-014 – Event Bus Service
- IWP-015 – Plugin Manager Service
- IWP-016 – Platform Context Service
- IWP-017 – End-to-End Platform Integration

This work package shall not implement:
- architecture-document semantic validation
- architecture-document approval rules
- prompt loading or rendering
- AI inference
- AI-provider selection
- MCP server communication
- event publication infrastructure
- plugin discovery or loading
- platform-context aggregation
- remote repository synchronization
- Git commits
- Git pushes
- Git pulls
- Git merges
- branch creation
- tag creation
- repository cloning
- repository hosting-provider APIs
- multi-repository workspace orchestration
- repository analytics
- repository health scoring
- automated remediation of repository problems

Repository structure checks performed by this service shall remain repository checks.

They shall not become document-validation rules assigned to IWP-008.

---

# 7. Architectural Non-Negotiables

## 7.1 No Architecture Redesign

Cursor shall not:
- redesign the Repository Service contract
- change the approved package dependency graph
- create an alternative repository service
- create a second repository abstraction
- expose raw filesystem implementation details to consumers
- expose Git-library implementation details to consumers
- bypass Configuration Service
- bypass Logging Service
- bypass Bootstrap Service
- bypass Service Registry
- bypass Service Lifecycle Manager
- implement Validation Framework behavior
- make the Repository Service depend on CLI
- make the Repository Service depend on Validation Framework
- make the Repository Service depend on Prompt Management
- make the Repository Service depend on AI Provider
- make the Repository Service depend on MCP Client
- add provider-specific repository-hosting behavior
- add remote Git write behavior
- change approved public service contracts
- modify approved architecture documents
- modify ADR-001
- silently reinterpret an architectural ambiguity

## 7.2 Repository Boundary

All managed artifact operations shall remain within the active repository root.

The implementation shall not permit:
- `..` traversal outside the root
- absolute paths outside the root
- symlink-based escape from the root
- platform-specific path tricks that escape the root
- writes to `.git`
- writes to repository paths excluded by the approved service policy
- accidental access to sibling repositories
- access to arbitrary user-home files

## 7.3 Consumer Boundary

Consumers shall interact with repositories through the Repository Service.

The implementation shall not place repository filesystem logic in:
- CLI command handlers
- validation code
- bootstrap code
- service registry code
- prompt code
- AI code
- MCP code
- generic utility modules unrelated to repository access

## 7.4 Stop Conditions

Cursor shall stop and report an architectural ambiguity if:
1. The committed baseline conflicts with ARP-002-07.
2. The Configuration Service cannot provide repository-related configuration through a stable public interface.
3. The Logging Service cannot be consumed through a stable public interface.
4. The Repository Service cannot be registered without changing the approved Service Registry contract.
5. The lifecycle dependency order cannot express Configuration and Logging before Repository Service.
6. The CLI repository command group differs materially from the approved CLI contract.
7. Repository writes cannot be implemented safely within the approved repository boundary.
8. Required repository classification cannot be determined from approved architecture or existing configuration.
9. Implementing a requirement would require Validation Framework behavior.
10. Implementing a requirement would require a new architectural dependency.
11. A required public interface cannot be preserved without an ADR.
12. Existing implementation code would need broad unrelated refactoring.

Cursor shall not invent a solution to any stop condition.

---

# 8. Service Identity

The implementation shall conform to ARP-002-07.
| Property | Required Value |
|---|---|
| Service Name | Repository Service |
| Public Package | `vibe.repository` |
| Primary Class | `RepositoryService` |
| Lifetime | Singleton |
| Thread Safety | Required |
| Public | Yes |
| Mandatory Dependencies | Configuration Service, Logging Service |
| Forbidden Dependencies | AI Provider Service, Prompt Management Service, MCP Client Service, Validation Framework, CLI |

Use the canonical service identifier established by the Service Registry baseline.

Do not create a second identifier if an approved repository-service identifier or identifier convention already exists.

---

# 9. Design Principles

The implementation shall preserve the following principles.

## 9.1 Git First

Git is the authoritative source of repository history and state.

Repository inspection shall use Git semantics where Git information is required.

## 9.2 Hosting-Provider Independence

The service shall not depend on GitHub, GitLab, Bitbucket, Azure DevOps, or another hosting provider.

## 9.3 Read First

Inspection shall be read-only by default.
No Git write operation is authorized.
Artifact writes are permitted only through the approved `write(path)` service operation and shall be governed by the requirements in this document.

## 9.4 Common Access Layer

Platform components shall use the Repository Service rather than implement direct repository access.

## 9.5 Determinism

The same repository state and request shall produce the same normalized result.

## 9.6 Encapsulation

Raw filesystem, subprocess, and Git-library exceptions shall not cross the public service boundary.

## 9.7 Secure Failure

Invalid paths, inaccessible repositories, malformed repository state, and permission failures shall produce controlled repository errors.

## 9.8 Minimal Side Effects

Discovery, status, metadata, list, exists, read, refresh, and health operations shall not modify repository contents.

## 9.9 Architecture Compatibility

This increment shall establish the Repository Service required by the future Validation Framework and Prompt Management Service without implementing either consumer.

---

# 10. Required Public Interface

The Repository Service shall expose operations logically equivalent to:
```python
initialize()
open(repository)
status()
exists(path)
read(path)
write(path, content)
list(path)
metadata()
refresh()
health()
shutdown()

Equivalent Python signatures are permitted where needed for:

* complete type safety
* explicit request or result models
* existing lifecycle protocols
* existing synchronous or asynchronous conventions
* existing repository conventions

Observable behavior shall remain consistent with ARP-002-07.

The public API shall be explicit and discoverable.

Do not hide required operations behind undocumented dynamic dispatch.

⸻

# 11. Public API Requirements

## 11.1 Construction

Construction shall accept mandatory dependencies through the dependency-injection mechanism established by the implementation baseline.

The constructor shall not:

* discover a repository
* traverse the filesystem
* invoke Git
* write artifacts
* publish service availability
* create uncontrolled global state

Lifecycle work shall occur through the approved initialization lifecycle.

## 11.2 Initialization

initialize() shall:

1. validate service lifecycle state
2. load repository-related configuration
3. resolve the configured or active repository candidate
4. discover the repository root
5. validate repository accessibility
6. validate required repository structure
7. classify the repository
8. load normalized Git metadata
9. load normalized platform metadata
10. populate repository caches
11. transition to the approved ready or available state
12. make the service available through the Service Registry
13. log the initialization outcome

Initialization shall be idempotent according to existing lifecycle conventions.

Repeated initialization shall not:

* create duplicate registry entries
* duplicate handlers
* corrupt caches
* cause uncontrolled repeated traversal
* change repository contents

## 11.3 Explicit Open

open(repository) shall allow an approved repository location or repository descriptor to become the active repository.

It shall:

* normalize the supplied location
* validate existence
* validate accessibility
* discover the actual Git root where applicable
* enforce repository-boundary rules
* validate the supported repository structure
* load metadata
* update the active repository atomically
* invalidate stale caches
* return a normalized result or metadata model

An unsuccessful open() shall not leave the service in a partially switched state.

If the service already has an active repository, a failed open shall preserve the previously valid active repository unless the approved baseline requires a different lifecycle behavior.

## 11.4 Status

status() shall return normalized repository status.

Status shall not expose mutable internal state.

## 11.5 Exists

exists(path) shall:

* accept a repository-relative path
* normalize and validate the path
* enforce the repository boundary
* return a deterministic Boolean result
* avoid exposing filesystem exceptions

Existence checks shall not follow an unsafe path outside the repository.

## 11.6 Read

read(path) shall:

* accept a repository-relative artifact path
* enforce the repository boundary
* verify that the target is a permitted regular file
* reject directory reads
* reject paths escaping through symlinks
* reject inaccessible artifacts with a classified error
* return deterministic content
* avoid logging artifact contents
* avoid exposing raw filesystem exceptions

The implementation shall use the repository’s established text-encoding convention.

Where no existing convention exists, UTF-8 shall be used for platform-managed text artifacts.

Binary artifact support shall not be added unless already required by the approved baseline.

## 11.7 Write

write(path, content) shall:

* accept a repository-relative path
* enforce the repository boundary
* reject writes outside the active repository
* reject writes into .git
* respect filesystem permissions
* validate parent paths
* avoid implicit repository creation
* avoid Git staging or commits
* use deterministic encoding for text content
* prevent partial-file corruption
* invalidate affected caches after success
* return a normalized write result or approved equivalent
* encapsulate implementation-specific failures

Where supported by the baseline and standard library, writes should use an atomic replacement pattern.

A write shall not be reported as successful unless the complete requested content has been persisted.

This operation authorizes repository artifact persistence only.

It does not authorize Git write operations.

## 11.8 List

list(path) shall:

* accept a repository-relative path
* enforce the repository boundary
* support listing a directory
* return normalized repository-relative entries
* sort results deterministically
* apply approved hidden-file and ignored-file behavior
* avoid returning .git internals
* avoid exposing paths outside the repository
* return immutable or read-only result data

The physical return model shall follow current repository conventions.

## 11.9 Metadata

metadata() shall return normalized immutable metadata for the active repository.

It shall not require consumers to interpret raw Git output.

## 11.10 Refresh

refresh() shall:

* re-read repository and Git state
* invalidate stale metadata
* invalidate stale directory-validation results
* preserve service integrity
* update the active metadata atomically
* return deterministic refreshed state
* log refresh success or failure

A failed refresh shall not expose partially updated metadata.

## 11.11 Health

health() shall integrate with the health model already established by previous IWPs.

Repository health shall reflect at least:

* lifecycle initialization state
* active repository availability
* repository accessibility
* metadata availability
* last refresh outcome
* read or write capability where applicable

Do not create a parallel health framework.

IWP-013 remains responsible for the future Health Monitoring Service.

## 11.12 Shutdown

shutdown() shall:

* stop accepting lifecycle-dependent operations as required by the baseline
* release repository resources
* clear sensitive in-memory transient data where applicable
* invalidate internal caches
* transition lifecycle state correctly
* avoid modifying repository contents
* complete safely when called more than once if existing lifecycle conventions require idempotence

⸻

# 12. Repository Configuration

The Repository Service shall obtain configuration through the Configuration Service.

It shall not read configuration files directly.

Cursor shall inspect the committed configuration schema and extend it only through the established configuration conventions.

Repository configuration may include architecture-approved settings such as:

* explicitly configured repository path
* repository discovery starting path
* required directory settings
* hidden-file behavior
* ignored-file behavior
* read-only mode
* permitted write behavior
* cache settings

Do not add configuration solely for speculative future functionality.

Defaults shall be deterministic and documented.

Environment-variable behavior shall use the Configuration Service rather than direct environment access from Repository Service code.

Sensitive configuration shall not be logged.

⸻

# 13. Repository Discovery

Repository discovery shall identify the active repository.

The discovery implementation shall support the approved active-repository behavior using the current baseline and configuration model.

Discovery shall verify:

* candidate path existence
* candidate path accessibility
* Git repository presence where required
* Git repository root
* required repository structure
* available repository metadata

Discovery shall fail with a classified error if no valid active repository can be established.

Discovery shall not scan unrelated portions of the filesystem.

Discovery shall begin only from:

* an explicitly configured repository path
* an explicitly supplied open() path
* an approved current-working-directory discovery point
* another starting point already established by the approved baseline

Parent traversal used to discover a Git root shall terminate deterministically.

Repository discovery shall not follow repository links into unrelated filesystem trees.

⸻

#14. Repository Root Detection

The service shall normalize the repository root to an absolute canonical path internally.

Public results shall use the path representation established by the codebase.

Repository root detection shall:

* handle invocation from the repository root
* handle invocation from a descendant directory
* detect absence of a Git repository
* avoid selecting a parent repository incorrectly
* operate consistently across supported operating systems
* handle inaccessible .git state securely
* avoid exposing raw Git commands or filesystem implementation details

Worktree-compatible Git-root discovery may be supported where it follows standard Git behavior and requires no new architecture.

⸻

#15. Repository Validation

Repository validation in this work package means validating that a repository is structurally usable by the platform.

It shall include:

* repository existence
* repository accessibility
* Git repository presence where required
* required directories
* required files
* platform configuration presence where required
* architecture directory presence where required
* prompts directory presence where required
* Python project configuration presence where required
* supported repository classification
* repository-boundary validity

Repository validation shall produce a normalized result.

It shall distinguish:

* valid
* warning
* invalid
* unknown

Validation shall not perform semantic validation of architecture documents.

Semantic document validation belongs to IWP-008.

⸻

# 16. Required Repository Structures

AEP-002-10 identifies the minimum platform repository structure as:

docs/
architecture/
prompts/
src/
tests/

Cursor shall also inspect AEP-002-02 and the current repository baseline for repository-specific structures.

Validation rules shall recognize the approved repository types:

* Platform
* CLI
* Example
* Extension
* Plugin

The implementation shall not require every repository type to contain directories that apply only to another repository type.

Repository-type-specific validation shall be based on approved architecture and existing repository conventions.

Do not invent speculative structures for Extension or Plugin repositories.

Where complete validation requirements for a future repository type are not yet defined:

* classify the repository where deterministically possible
* report a warning where permitted by AEP-002-10
* do not fabricate mandatory structure
* do not block unrelated supported operations unless required by the current command

⸻

# 17. Repository Classification

The service shall classify repositories using the approved repository types.

The normalized enum or equivalent shall include:

* PLATFORM
* CLI
* EXAMPLE
* EXTENSION
* PLUGIN
* UNKNOWN

Names may follow established enum naming conventions.

Classification shall use deterministic repository evidence such as:

* approved metadata
* known directory structure
* project configuration
* package identity
* repository markers established by architecture

Classification shall not rely only on a remote origin URL.

Unknown repository types shall generate a warning but shall not automatically prevent operation unless the requested operation requires a known type.

The reason for classification shall be testable.

⸻

3 18. Repository Metadata Model

The service shall expose normalized immutable repository metadata.

Minimum metadata shall include:

Field	Requirement
Repository Identifier	Stable normalized identifier
Repository Name	Human-readable repository name
Repository Root	Normalized absolute root path or approved path representation
Repository Type	Approved repository classification
Current Branch	Active branch, where available
Default Branch	Default branch, where deterministically available
Git Status	Clean or modified normalized status
Latest Commit	Normalized latest commit identity and approved metadata
Current Tag	Current tag where available
Repository Version	Platform or project version where available
Platform Version	Current platform version where applicable
Capabilities	Immutable set of supported repository capabilities
Read-Only Status	Whether writes are permitted
Initialization State	Normalized service/repository initialization state

Optional values shall be represented explicitly.

Do not use ambiguous empty strings where None, an enum, or another explicit absence model is appropriate.

Metadata consumers shall not be able to mutate service state by modifying returned data.

⸻

# 19. Repository Status Model

The Repository Service shall expose a normalized status model.

The status model shall include logical information for:

* initialized
* available
* synchronized
* read-only status
* repository validity
* repository cleanliness
* health
* warnings
* active repository identity

The repository-validation classification shall include:

Status	Meaning
Valid	Repository passes required structural validation
Warning	Repository is usable but has one or more non-fatal issues
Invalid	Repository cannot safely satisfy the requested platform operation
Unknown	Validation has not completed or cannot be determined

The physical Python model shall be immutable where practical.

Status output shall be deterministic.

⸻

# 20. Repository Capabilities

Repository capabilities shall describe operations that the active repository can support.

Capabilities may include approved logical values such as:

* metadata inspection
* Git inspection
* artifact reading
* artifact writing
* recursive listing
* architecture-document discovery

Capability names shall use an enum or another typed immutable representation.

Do not introduce capabilities for:

* remote synchronization
* cloning
* committing
* pushing
* branch creation
* prompt rendering
* AI execution
* validation semantics

⸻

# 21. Artifact Paths

Public artifact paths shall be repository-relative.

The service shall centralize path resolution.

A single internal path-policy component or equivalent cohesive implementation shall:

1. reject invalid input
2. reject null-byte input
3. reject unsafe absolute paths
4. normalize separators
5. resolve the target against the active repository root
6. resolve symlinks safely
7. verify that the resolved target remains within the root
8. reject .git internals for artifact operations
9. return the normalized internal path
10. produce a normalized repository-relative path for public results

Do not duplicate path-security logic across exists, read, write, and list.

⸻

# 22. Path Traversal Prevention

Automated tests shall prove rejection of at least:

../outside.txt
../../outside.txt
/path/outside/repository
C:\outside\repository
repository/../../outside.txt
symlink-to-outside/secret.txt
.git/config
.git/objects/...

Platform-specific cases shall run only on supported platforms where appropriate.

The implementation shall not rely on string-prefix checks alone.

Path containment shall be based on normalized path semantics.

⸻

# 23. Symlink Policy

Symlinks shall not be allowed to escape the active repository root.

For repository reads and lists:

* a symlink resolving inside the repository may be handled according to standard filesystem behavior
* a symlink resolving outside the repository shall be rejected

For writes:

* writes through a symlink shall be rejected unless the established baseline explicitly permits a secure internal-only symlink policy
* existing symlinks shall not be overwritten accidentally
* parent-directory symlink escape shall be detected

Tests shall cover symlink behavior on supported operating systems.

Tests may skip symlink cases only where the environment cannot create symlinks, and the reason shall be explicit.

⸻

# 24. File Discovery

The service shall provide deterministic file discovery through the approved list operation and any cohesive internal helper required by AEP-002-10.

Supported behavior shall include:

* recursive discovery where requested by the public or internal approved interface
* file filtering
* extension filtering
* directory exclusion
* hidden-file handling
* ignored-file handling
* deterministic ordering

Do not add an unrelated general-purpose filesystem framework.

Discovery shall operate only inside the active repository.

⸻

# 25. Deterministic Ordering

All repository entry collections shall have deterministic ordering.

Unless the repository baseline establishes another approved convention:

* normalize repository-relative paths
* sort lexicographically by normalized path
* use stable case behavior appropriate to the supported operating system while preserving deterministic output

Do not return raw filesystem iteration order.

Deterministic ordering applies to:

* directory listings
* recursive file discovery
* architecture-document inventories
* warning collections
* validation issue collections
* capability collections where serialized

⸻

# 26. Hidden Files

Hidden-file handling shall be explicit.

By default:

* .git shall always be excluded from artifact discovery and listing
* operating-system metadata files shall not affect repository classification
* hidden files shall not be returned unless approved configuration or the existing contract requests them
* hidden directories shall not be traversed by default

Do not expose Git internals through repository artifact APIs.

⸻

# 27. Git-Ignored Files

The Repository Service shall not inspect ignored files unless explicitly requested through an approved option or existing configuration.

Ignored-file behavior shall:

* use Git semantics where available
* remain provider independent
* avoid parsing repository contents unnecessarily
* fail securely if ignore status cannot be determined
* remain deterministic

Do not implement a separate speculative ignore-pattern language when Git already defines the repository behavior.

A repository without Git ignore rules shall still operate predictably.

⸻

# 28. Architecture-Document Discovery

The Repository Service shall discover architecture documents without semantically validating them.

Discovered architecture-document metadata shall include, where deterministically available:

* document identifier
* document title
* architecture package
* document status
* repository-relative file path

Discovery shall:

* search only approved architecture locations
* return deterministic ordering
* avoid reading unrelated repository files
* avoid modifying documents
* tolerate a document whose metadata cannot be fully extracted by returning an explicit incomplete result or warning
* avoid applying IWP-008 validation rules

Metadata extraction shall remain minimal and structural.

Do not implement:

* document compliance evaluation
* cross-document dependency validation
* approval-policy validation
* architecture quality scoring
* automatic document correction

⸻

# 29. Git Integration

The Repository Service shall provide read-only Git inspection.

Supported operations shall include normalized access to:

* repository root
* current branch
* default branch where deterministically available
* working-tree cleanliness
* latest commit
* current tag
* repository status

Git integration shall not:

* stage files
* create commits
* amend commits
* create branches
* switch branches
* merge
* rebase
* tag
* push
* pull
* fetch unless architecture explicitly requires it
* change Git configuration
* contact a remote hosting provider

⸻

# 30. Git Implementation Boundary

Cursor may use:

* an architecture-approved existing dependency
* an existing Git abstraction in the implementation baseline
* a narrowly encapsulated standard-library subprocess integration

Before adding a Git dependency, Cursor shall:

1. inspect pyproject.toml
2. inspect architecture-approved dependencies
3. reuse an existing approved dependency where present
4. avoid adding a large Git framework solely for simple read-only operations
5. ensure implementation-specific exceptions remain internal
6. preserve supported Python versions and packaging rules

Raw Git command output shall not be returned directly from public methods.

Subprocess invocations, where used, shall:

* avoid shell execution
* pass arguments as a sequence
* set the repository working directory explicitly
* use bounded execution
* capture output safely
* normalize encoding
* avoid logging sensitive command output
* map errors into repository exceptions

⸻

# 31. Current Branch

Current branch behavior shall distinguish:

* a named branch
* detached HEAD
* unavailable branch information
* non-Git repository failure

Detached HEAD shall not be misreported as a normal branch.

Use an explicit model or normalized representation.

⸻

# 32. Default Branch

Default branch shall be reported only when it can be determined locally and safely.

The implementation shall not require network access.

Permitted local evidence may include:

* locally configured symbolic references
* approved repository metadata
* a deterministic local fallback established by the existing baseline

Do not guess that every repository uses main or master.

If the default branch cannot be determined, return an explicit unavailable value.

⸻

# 33. Repository Cleanliness

Repository cleanliness shall distinguish at least:

* clean
* modified
* unavailable
* unknown

Modified state shall account for tracked and untracked working-tree changes according to standard Git status semantics.

Do not expose the contents of changed files.

⸻

# 34. Latest Commit

Latest-commit metadata shall be immutable and normalized.

It may include:

* full commit identifier
* abbreviated commit identifier
* author name where approved
* commit timestamp
* subject line

Do not expose the complete commit body unless required by the approved contract.

Do not log commit-message content at normal log levels.

⸻

# 35. Current Tag

Current-tag reporting shall be local and read-only.

Where no exact tag applies, return an explicit absence value.

Do not contact remote repositories.

⸻

# 36. Platform and Repository Version

The Repository Service shall derive version information only from approved repository metadata sources.

Examples may include:

* installed package metadata
* approved project configuration
* an approved version file
* Git tag information where established by architecture

Do not create a new versioning authority.

Version precedence shall follow AEP-001-10 and existing implementation conventions.

⸻

# 37. Artifact Reading

Artifact reads shall be deterministic and secure.

The implementation shall:

* validate lifecycle readiness
* require an active repository
* normalize the requested path
* enforce repository containment
* reject directories
* reject unsupported artifact types where necessary
* use deterministic text decoding
* classify missing artifacts separately from read failures
* avoid modifying access metadata deliberately
* avoid retaining artifact contents in metadata caches unless explicitly needed
* avoid logging contents

If artifact size limits already exist in the architecture or baseline, preserve them.

Do not invent arbitrary limits that change the approved contract.

⸻

# 38. Artifact Writing

Artifact writing is part of the approved Repository Service contract.

The implementation shall:

* validate lifecycle readiness
* require an active repository
* enforce read-only status
* normalize the destination path
* enforce repository containment
* reject writes to .git
* reject unsafe symlink targets
* validate or create parent directories only where permitted by approved repository behavior
* preserve deterministic encoding
* avoid partial writes
* avoid changing file permissions unnecessarily
* invalidate relevant caches
* log operation metadata without logging content
* return normalized success information
* map permission errors to Access Denied
* map persistence errors to Write Failure

The implementation shall not:

* stage the written file
* commit the written file
* modify architecture documents automatically
* perform content validation assigned to another service
* write outside the active repository
* silently overwrite a directory
* silently replace a symlink

If the approved baseline has no explicit overwrite-control option, preserve the logical behavior of write(path, content) while documenting the implemented deterministic behavior.

⸻

# 39. Atomic Write Behavior

Where feasible using the standard library and supported filesystems, writes shall:

1. create a temporary file in the target directory
2. write the complete content
3. flush and close the temporary file
4. replace the target atomically
5. remove temporary artifacts after failure
6. invalidate caches only after successful replacement

Temporary filenames shall not be exposed publicly.

Tests shall verify that failed writes do not report success and do not leave uncontrolled temporary files.

Do not introduce transactional repository architecture beyond this file-safety requirement.

⸻

# 40. Repository Cache

AEP-002-10 requires caching of repository metadata and directory-validation results.

The implementation shall cache, where appropriate:

* normalized repository metadata
* repository classification
* directory-validation results
* Git metadata that is safe to reuse
* architecture-document discovery metadata where beneficial

The cache shall:

* remain internal
* be thread-safe
* not expose mutable references
* not change public semantics
* be invalidated on open()
* be invalidated on refresh()
* be invalidated after successful writes where relevant
* be cleared on shutdown
* avoid retaining artifact contents unnecessarily

Do not introduce a distributed cache or external cache dependency.

⸻

# 41. Cache Invalidation

Repository state can change outside the running process.

The implementation shall provide deterministic explicit refresh through refresh().

Within this work package:

* refresh() is the authoritative explicit cache invalidation mechanism
* successful writes shall invalidate affected cache entries
* open() shall invalidate all previous-repository cache entries
* shutdown shall clear all entries
* failed operations shall not publish partially refreshed entries

Automatic filesystem watching is out of scope.

Do not introduce a file-watcher service.

⸻

# 42. Thread Safety

The Repository Service shall be thread-safe.

Concurrent access shall:

* preserve active-repository integrity
* preserve cache integrity
* avoid data races
* avoid deadlocks
* prevent consumers from observing partial refresh state
* prevent consumers from observing partial repository switches
* prevent write corruption
* preserve deterministic results for a stable repository state

Use the synchronization primitives and conventions already established by the implementation baseline.

Do not hold locks while performing avoidable logging or external callbacks.

Do not return mutable cache entries.

⸻

# 43. Lifecycle Contract

The Repository Service shall initialize after:

1. Configuration Service
2. Logging Service

The required initialization sequence is:

1. construct service
2. inject approved dependencies
3. load repository configuration
4. discover repository
5. validate repository structure
6. load repository metadata
7. register or publish service availability
8. transition to the approved ready state

Shutdown shall release repository resources and clear caches.

The Repository Service shall not initialize before its mandatory dependencies are ready.

⸻

# 44. Service Registry Integration

The Repository Service shall be registered through the Service Registry created by IWP-005.

Integration shall:

* use the canonical service identifier
* register one singleton instance
* declare Configuration and Logging dependencies
* avoid duplicate registration
* preserve deterministic resolution
* expose the approved public service interface
* integrate with existing registry diagnostics
* avoid registering internal helpers as public platform services

Cursor shall not create a new service locator.

⸻

# 45. Service Lifecycle Manager Integration

The Repository Service shall integrate with IWP-006.

Lifecycle integration shall include:

* dependency declaration
* initialization
* readiness
* health
* refresh behavior where lifecycle-visible
* shutdown
* failure state
* deterministic ordering

A repository-discovery or validation failure during initialization shall produce the lifecycle failure behavior already defined by the baseline.

Do not conceal initialization failure by marking an unusable repository service as ready.

Where CLI commands can operate in a limited diagnostic state, follow the existing lifecycle and CLI contracts rather than inventing a new state.

⸻

# 46. Bootstrap Integration

The Bootstrap Service shall include the Repository Service in platform startup.

Bootstrap integration shall:

* preserve all existing startup behavior
* initialize Configuration and Logging first
* make the Repository Service available through the Service Registry
* allow lifecycle state inspection
* shut the Repository Service down safely
* avoid initializing future services
* avoid implementing Validation Framework registration

Do not duplicate bootstrap logic inside Repository Service code.

⸻

# 47. Logging Requirements

All Repository Service logging shall use the Logging Service.

The Repository Service shall log:

* initialization start and completion
* repository discovery outcome
* repository validation outcome
* repository classification
* repository refresh
* repository open or switch
* artifact write success or failure
* path-policy rejection at an appropriate level
* Git inspection failures
* lifecycle failures
* shutdown

Normal high-frequency operations should avoid excessive logging.

Routine exists, read, and list operations should not produce noisy informational logs.

Diagnostic logging may include:

* normalized repository-relative paths
* result counts
* repository type
* elapsed operation information where supported
* normalized error categories

Logging shall not include:

* artifact contents
* passwords
* API keys
* access tokens
* private keys
* authorization headers
* unredacted secrets
* complete environment variables
* arbitrary Git configuration
* file contents discovered during inspection

Absolute paths shall follow the platform’s approved redaction and diagnostic conventions.

⸻

# 48. Error Contract

The implementation shall provide a Repository Service exception hierarchy consistent with existing platform exception conventions.

Required logical error categories are:

Error Type	Description
Repository Not Found	Repository unavailable or no repository can be discovered
Invalid Repository	Repository structure or required metadata is invalid
Artifact Not Found	Requested repository artifact does not exist
Access Denied	Filesystem or repository permissions prohibit the operation
Read Failure	Artifact exists but cannot be read successfully
Write Failure	Artifact cannot be persisted successfully
Repository Failure	Internal repository operation failed
Invalid Repository Path	Requested path is malformed or escapes the repository boundary
Repository Not Initialized	Operation requires an initialized active repository

The exact class hierarchy shall follow established implementation conventions.

Implementation-specific exceptions shall remain encapsulated.

Do not expose directly:

* OSError
* PermissionError
* FileNotFoundError
* subprocess exceptions
* third-party Git exceptions
* parser exceptions
* internal cache exceptions

Exception chaining may be retained internally for diagnostics while public error messages remain safe.

⸻

# 49. Error Mapping

The implementation shall map failures deterministically.

Examples:

Failure	Public Classification
Configured repository path absent	Repository Not Found
Candidate is not a supported repository	Invalid Repository
Requested artifact absent	Artifact Not Found
Path escapes repository root	Invalid Repository Path or Access Denied according to established error conventions
Read permission denied	Access Denied
Write permission denied	Access Denied
File decoding fails	Read Failure
Atomic replacement fails	Write Failure
Git inspection unexpectedly fails	Repository Failure
Operation occurs before initialization	Repository Not Initialized

Do not collapse every failure into a generic runtime exception.

⸻

# 50. Health Behavior

Repository Service health shall use the health-result model established by prior IWPs.

At minimum, health evaluation shall consider:

* service lifecycle state
* active repository presence
* repository-root accessibility
* metadata-cache integrity
* last discovery outcome
* last refresh outcome

Health details shall be concise and safe.

Health shall not read or log artifact contents.

The future IWP-013 Health Monitoring Service may aggregate this result but shall not be implemented now.

⸻

# 51. CLI Integration

IWP-007 established the CLI command framework and deferred repository commands.

IWP-009 shall activate the approved Repository Service commands:

86vibe repo status
86vibe repo validate
86vibe repo docs
86vibe doctor

Future commands remain out of scope unless already implemented as deferred shells:

86vibe repo inspect
86vibe repo inventory
86vibe repo summary

Do not implement future commands merely because a shell exists.

Repository command handlers shall remain thin.

They shall:

* resolve Repository Service through the approved application context or Service Registry
* verify service readiness
* call public Repository Service operations
* render normalized results
* map errors through the existing CLI error mapper
* preserve text and JSON output conventions
* preserve stable exit codes
* avoid direct Git access
* avoid direct filesystem access
* avoid repository business logic

⸻

# 52. 86vibe repo status

The status command shall report the active repository’s normalized state.

Human-readable output shall include, where available:

* repository name
* repository type
* repository root using approved path-display conventions
* current branch
* default branch
* Git cleanliness
* current version
* read-only state
* validation state
* health state
* warnings

JSON output shall:

* be valid JSON
* contain stable field names
* contain no ANSI codes
* serialize enums predictably
* use explicit nulls for unavailable optional values
* avoid stringified Python objects
* avoid sensitive values

A successful status lookup shall return the existing CLI success exit code.

Repository errors shall map through the existing CLI exit-code contract.

⸻

# 53. 86vibe repo validate

The validate command shall execute repository structural validation.

It shall report:

* overall status
* repository classification
* passed checks
* warnings
* failed checks
* actionable issue messages

The command shall not perform architecture-document semantic validation.

Human-readable output shall make the distinction clear.

JSON output shall use deterministic ordering for checks and issues.

Exit behavior shall follow the existing CLI contract.

At minimum:

* valid repository: success
* warning-only repository: behavior shall follow the approved CLI and repository status contracts
* invalid repository: validation-failure exit classification
* internal repository failure: runtime or internal-platform failure according to the existing mapper

Do not invent a new exit code.

⸻

# 54. 86vibe repo docs

The docs command shall list discovered architecture documents.

Output shall include, where available:

* document identifier
* title
* package
* status
* repository-relative path

Ordering shall be deterministic.

The command shall not:

* validate document content
* approve documents
* modify documents
* infer architecture compliance
* generate missing metadata
* rewrite headings

Incomplete metadata shall be displayed safely and explicitly.

⸻

# 55. 86vibe doctor

The existing doctor command shall incorporate Repository Service diagnostics.

This work package shall add repository checks without turning doctor into the future Health Monitoring Service.

Repository diagnostics shall include:

* Repository Service registration
* lifecycle readiness
* repository discovery
* repository accessibility
* repository classification
* required structure
* Git inspection availability
* repository health

The doctor command shall continue to include any diagnostics already implemented by prior IWPs.

Do not remove or duplicate existing doctor checks.

⸻

# 56. CLI Deferred-Command Transition

IWP-007 may currently report repository commands as unavailable pending IWP-009.

IWP-009 shall:

* replace the deferred handlers for the four approved operational commands
* preserve command names and help
* preserve command-group structure
* preserve aliases already approved
* preserve output renderer usage
* preserve CLI context usage
* preserve error mapping
* preserve JSON behavior
* preserve diagnostic mode
* preserve confirmation behavior where applicable

Do not rebuild the CLI framework.

Do not alter unrelated deferred command groups.

⸻

# 57. Public Models

The Repository Service shall expose typed models sufficient to represent:

* repository type
* repository validation status
* Git cleanliness
* repository capabilities
* repository metadata
* repository status
* repository validation result
* validation issue or check result
* architecture-document metadata
* artifact write result where required
* latest-commit metadata where required

Models shall:

* use complete type hints
* be immutable where practical
* validate invariants
* avoid exposing internal implementation objects
* support deterministic serialization by the CLI
* use explicit optional types
* use enums for bounded classifications

Do not create overlapping models where an existing platform model already satisfies the requirement.

⸻

# 58. Internal Components

The physical implementation shall follow current repository conventions.

A cohesive implementation may include internal components logically equivalent to:

* repository path policy
* repository discovery
* repository classifier
* structure validator
* Git inspector
* artifact store
* architecture-document discoverer
* metadata cache

These are internal implementation concerns.

They shall not become public platform services unless the approved architecture already defines them as such.

Avoid unnecessary abstraction layers.

Each component shall have a clear, testable responsibility.

⸻

# 59. Expected Package Layout

Cursor shall inspect and follow the actual package structure.

The expected logical layout is:

src/
└── vibe/
    └── repository/
        ├── __init__.py
        ├── service.py
        ├── models.py
        ├── errors.py
        ├── protocols.py
        ├── discovery.py
        ├── classification.py
        ├── validation.py
        ├── paths.py
        ├── artifacts.py
        ├── git.py
        ├── documents.py
        └── cache.py

Not every listed file is mandatory.

Cursor may consolidate files where that better matches established repository conventions and keeps modules focused.

The public package shall expose only the approved public API.

Internal implementation symbols shall not be exported unnecessarily.

Potential integration changes may be required in:

src/vibe/bootstrap/
src/vibe/registry/
src/vibe/lifecycle/
src/vibe/cli/
src/vibe/config/

Modify those packages only where required to integrate the Repository Service.

Do not move or rename existing public modules.

⸻

# 60. Expected Test Layout

Follow existing test conventions.

The expected logical test layout is:

tests/
├── unit/
│   └── repository/
│       ├── test_service.py
│       ├── test_discovery.py
│       ├── test_classification.py
│       ├── test_validation.py
│       ├── test_paths.py
│       ├── test_artifacts.py
│       ├── test_git.py
│       ├── test_documents.py
│       ├── test_cache.py
│       ├── test_models.py
│       ├── test_errors.py
│       ├── test_lifecycle.py
│       └── test_thread_safety.py
│
└── integration/
    ├── repository/
    │   ├── test_repository_service.py
    │   ├── test_repository_discovery.py
    │   ├── test_repository_artifacts.py
    │   ├── test_repository_git_state.py
    │   ├── test_repository_refresh.py
    │   └── test_repository_security.py
    │
    └── cli/
        ├── test_repo_status.py
        ├── test_repo_validate.py
        ├── test_repo_docs.py
        └── test_doctor_repository.py

Use the repository’s actual naming and fixture conventions where they differ.

⸻

# 61. Representative Repository Fixtures

Tests shall use representative temporary repositories for:

* Platform repository
* CLI repository
* Example repository
* unknown repository
* invalid repository
* non-Git directory
* inaccessible repository where the environment permits
* clean Git repository
* modified Git repository
* detached-HEAD repository
* tagged repository
* repository containing hidden files
* repository containing ignored files
* repository containing architecture documents
* repository containing malformed document metadata
* repository containing an internal symlink
* repository containing a symlink escaping the root where supported
* read-only repository where the environment permits

Fixtures shall:

* be isolated
* be deterministic
* avoid network access
* avoid dependence on the developer’s real repositories
* clean themselves up
* avoid user-level Git configuration where possible
* set local Git identity when test commits are required

Do not run destructive tests against the checked-out implementation repository.

⸻

# 62. Unit Testing Requirements

Unit tests shall verify at minimum:

62.1 Construction and Dependencies

* mandatory dependencies are required
* constructor has no repository side effects
* public service identity is correct
* singleton registration behavior is preserved

## 62.2 Initialization

* configured repository initialization
* current-directory discovery where approved
* initialization order
* successful readiness transition
* failed discovery behavior
* failed validation behavior
* idempotent initialization
* no duplicate service registration

## 62.3 Discovery

* root invocation
* descendant-directory invocation
* no repository found
* inaccessible repository
* deterministic repository selection
* no unrelated filesystem scan

## 62.4 Classification

* Platform classification
* CLI classification
* Example classification
* unknown classification
* deterministic classification
* warning behavior for unknown types

## 62.5 Validation

* required directories present
* required directory missing
* required file missing
* warning-only result
* invalid result
* unknown result
* repository-type-specific rules
* no semantic document validation

## 62.6 Metadata

* required fields
* optional fields
* immutability
* current branch
* detached HEAD
* default branch unavailable
* clean and modified status
* latest commit
* current tag
* version metadata
* capability set

## 62.7 Paths

* valid repository-relative path
* path normalization
* traversal rejection
* absolute-path rejection
* .git rejection
* symlink escape rejection
* platform-specific path handling
* null-byte rejection where applicable

## 62.8 Artifacts

* exists true and false
* text read
* missing artifact
* directory read rejection
* write success
* overwrite behavior
* write permission failure
* read-only mode
* parent path behavior
* failed-write cleanup
* deterministic encoding
* cache invalidation after write

## 62.9 Listing

* root listing
* subdirectory listing
* deterministic sorting
* hidden files
* ignored files
* .git exclusion
* recursive filtering
* extension filtering where exposed

## 62.10 Architecture Documents

* metadata extraction
* deterministic ordering
* incomplete metadata
* malformed metadata
* no semantic validation
* no document mutation

## 62.11 Refresh and Cache

* cache reuse
* explicit refresh
* state change after refresh
* failed refresh atomicity
* open invalidation
* write invalidation
* shutdown clearing
* no mutable cache leakage

## 62.12 Errors

* every required logical error category
* implementation exception encapsulation
* safe error messages
* exception chaining where used

## 62.13 Logging

* initialization logs
* discovery logs
* validation logs
* refresh logs
* failure logs
* no artifact content logged
* no secret values logged

## 62.14 Shutdown

* normal shutdown
* repeated shutdown behavior
* cache clearing
* post-shutdown operation behavior
* no repository modifications

⸻

# 63. Integration Testing Requirements

Integration tests shall verify:

* Configuration Service to Repository Service integration
* Logging Service to Repository Service integration
* Bootstrap Service registration
* Service Registry resolution
* Service Lifecycle Manager dependency ordering
* platform startup with a valid repository
* startup failure with an invalid required repository
* repository status through the CLI
* repository validation through the CLI
* architecture-document discovery through the CLI
* repository diagnostics through 86vibe doctor
* text output
* JSON output
* stable exit-code mapping
* safe platform shutdown
* baseline services remain operational

Integration tests shall use actual temporary Git repositories.

Network access shall not be required.

⸻

# 64. Concurrency Testing Requirements

Tests shall verify concurrent:

* metadata reads
* status reads
* existence checks
* artifact reads
* directory listings
* refresh operations
* read operations during refresh
* writes to distinct files
* attempted conflicting writes where behavior is defined
* shutdown interaction according to existing lifecycle rules

Tests shall prove:

* no data corruption
* no deadlock
* no mutable-state leakage
* no partially published metadata
* no partially switched active repository
* deterministic final state

Avoid timing-dependent tests where synchronization primitives can provide deterministic coordination.

⸻

# 65. Security Testing Requirements

Tests shall verify:

* repository-root containment
* parent traversal prevention
* absolute-path escape prevention
* symlink escape prevention
* .git access prevention
* .git write prevention
* permission handling
* no content leakage in logs
* no secret leakage in errors
* ignored-file behavior
* hidden-file behavior
* failed writes do not corrupt targets
* temporary write files are cleaned up
* subprocess use does not invoke a shell
* repository paths are handled safely across supported platforms

Security tests shall not access real user files.

⸻

# 66. Performance Requirements

The implementation shall:

* minimize unnecessary filesystem traversal
* avoid repeatedly invoking Git for unchanged cached metadata
* avoid reading artifact contents during status and validation
* avoid scanning .git
* avoid scanning ignored directories by default
* reuse metadata where practical
* use explicit refresh for complete cache invalidation
* avoid unbounded memory growth

Tests shall not enforce fragile wall-clock thresholds.

Where performance behavior is tested, verify operation counts or cache reuse rather than machine-specific timing.

⸻

# 67. Dependency Requirements

Use only architecture-approved dependencies.

Before adding any dependency:

1. inspect pyproject.toml
2. inspect existing lock or dependency-management files
3. confirm the dependency is necessary
4. reuse an existing approved dependency where available
5. preserve Python-version support
6. preserve packaging conventions
7. avoid overlapping libraries

Prefer the Python standard library for:

* paths
* filesystem access
* immutable data models where appropriate
* synchronization
* subprocess execution
* temporary files

Do not add:

* a repository-hosting SDK
* a GitHub SDK
* a GitLab SDK
* a cloud storage SDK
* a remote synchronization library
* a third-party dependency-injection container
* a file-watching framework
* a distributed cache
* a database
* a validation framework belonging to IWP-008

A Git library may be added only if it is already architecture-approved or clearly required after inspecting the baseline.

⸻

# 68. Python Standards

Implementation shall comply with ARP-001-05.

Requirements include:

* Python 3.11 or the supported version defined by the baseline
* complete type hints
* public docstrings
* explicit return types
* enums for bounded classifications
* immutable result models where practical
* descriptive names
* focused modules
* focused functions
* explicit exception handling
* no wildcard imports
* no mutable public globals
* no hidden service locator
* no import-time repository discovery
* no import-time Git execution
* deterministic behavior
* repository formatting conventions
* repository linting conventions
* repository static-analysis conventions

Public docstrings shall describe:

* purpose
* parameters
* return values
* raised public exceptions
* lifecycle requirements
* thread-safety behavior where relevant
* path-boundary behavior where relevant

⸻

# 69. Documentation Requirements

Update repository documentation only where required to describe:

* Repository Service purpose
* public Repository Service API
* supported repository types
* repository command usage
* repository validation versus document validation
* repository path security
* artifact write limitations
* read-only Git behavior
* refresh behavior

Command help shall document:

86vibe repo status
86vibe repo validate
86vibe repo docs
86vibe doctor

Do not document future commands as implemented.

Do not document remote repository functionality.

Do not duplicate approved architecture documents inside implementation documentation.

⸻

# 70. Backward Compatibility

The implementation shall preserve all public behavior from IWP-001 through IWP-007.

In particular:

* existing imports shall continue to work
* existing service identifiers shall remain stable
* existing lifecycle APIs shall remain stable
* existing bootstrap APIs shall remain stable
* existing CLI command names shall remain stable
* existing CLI output and error abstractions shall remain stable
* existing exit-code meanings shall remain stable
* unrelated deferred commands shall remain deferred
* existing tests shall continue to pass

Any defect correction to baseline work shall be:

* minimal
* directly required for IWP-009
* covered by regression tests
* identified explicitly in the completion report

⸻

# 71. Repository Quality Gates

Before completion, Cursor shall run all repository quality gates established by IWP-001.

At minimum, run the repository-approved equivalent of:

ruff check .
ruff format --check .
mypy .
pytest

Also run packaging or build checks defined by pyproject.toml.

Where configured, verify:

python -m build

No quality-gate failures are permitted.

Do not suppress warnings or tests merely to obtain a passing result.

⸻

# 72. Regression Requirements

All tests from IWP-001 through IWP-007 shall continue to pass.

This includes tests for:

* Repository Foundation
* Configuration Service
* Logging Service
* Bootstrap Service
* Service Registry
* Service Lifecycle Manager
* CLI Framework

Repository Service integration shall not introduce import-time side effects that break direct service tests.

CLI integration shall not alter unrelated command behavior.

⸻

# 73. Functional Acceptance Criteria

IWP-009 is functionally complete only when:

* vibe.repository exists
* RepositoryService exists
* the service is injectable
* the service is registered as a singleton
* Configuration Service dependency is integrated
* Logging Service dependency is integrated
* lifecycle dependency order is correct
* bootstrap integration is complete
* repository discovery works
* explicit repository opening works
* repository-root detection works
* repository validation works
* repository classification works
* metadata generation works
* status generation works
* capabilities are exposed
* read-only Git inspection works
* artifact existence checks work
* artifact reads work
* governed artifact writes work
* deterministic listing works
* hidden-file behavior works
* ignored-file behavior works
* architecture-document discovery works
* path normalization works
* traversal prevention works
* symlink-boundary enforcement works
* caching works
* cache invalidation works
* refresh works
* health works
* shutdown works
* 86vibe repo status works
* 86vibe repo validate works
* 86vibe repo docs works
* 86vibe doctor includes repository diagnostics
* text output works
* JSON output works
* stable error and exit-code mapping works

⸻

# 74. Architectural Acceptance Criteria

IWP-009 is architecturally complete only when:

* ADR-001 sequencing is observed
* no architecture redesign is introduced
* ARP-002-07 is satisfied
* AEP-002-10 is satisfied
* ARP-002-16 is satisfied
* repository access is centralized
* consumers do not receive raw filesystem objects
* consumers do not receive raw Git implementation objects
* Configuration and Logging are the only mandatory platform-service dependencies
* no dependency on CLI exists from Repository Service
* no dependency on Validation Framework exists
* no dependency on Prompt Management exists
* no dependency on AI Provider exists
* no dependency on MCP Client exists
* no repository-hosting-provider dependency is introduced
* no remote Git operation is introduced
* no Git write operation is introduced
* no Validation Framework behavior is introduced
* no direct repository access is added to CLI handlers
* repository boundaries are enforced
* metadata semantics are deterministic
* lifecycle behavior is preserved
* service registration is deterministic
* public package and primary class names are preserved

⸻

# 75. Engineering Acceptance Criteria

IWP-009 is technically complete only when:

* public APIs are fully typed
* public APIs have docstrings
* public errors are documented
* result models are immutable where practical
* deterministic ordering is tested
* path security is tested
* concurrency is tested
* cache behavior is tested
* failure atomicity is tested
* unit tests pass
* integration tests pass
* CLI tests pass
* security tests pass
* baseline tests pass
* linting passes
* formatting checks pass
* static typing passes
* package build passes where configured
* no secrets or artifact contents appear in logs
* no generated test repositories remain after tests
* no local environment files are committed

⸻

# 76. Deliverables

This implementation package shall deliver:

* Repository Service package
* Repository Service public class
* repository public models
* repository public exceptions
* repository path policy
* repository discovery
* repository classification
* repository structure validation
* repository metadata
* repository status
* repository capabilities
* read-only Git inspection
* artifact existence checks
* artifact reads
* governed artifact writes
* deterministic listing and discovery
* architecture-document discovery metadata
* repository cache
* refresh behavior
* health behavior
* lifecycle integration
* Service Registry integration
* Bootstrap integration
* CLI repository command integration
* doctor-command repository diagnostics
* unit tests
* integration tests
* CLI tests
* concurrency tests
* security tests
* required documentation updates

No unrelated platform functionality shall be delivered.

⸻

# 77. Files Expected to Change

Exact files depend on the committed baseline.

Expected additions or modifications include:

src/vibe/repository/
tests/unit/repository/
tests/integration/repository/
tests/integration/cli/

Potential integration updates include:

src/vibe/bootstrap/
src/vibe/registry/
src/vibe/lifecycle/
src/vibe/cli/
src/vibe/config/
pyproject.toml
README.md
docs/

Only update integration packages where required by this specification.

Do not modify:

* Validation Framework production code
* Prompt Management production code
* AI Provider production code
* MCP Client production code
* Health Monitoring production code
* Event Bus production code
* Plugin Manager production code
* Platform Context production code
* Example Repository production code
* approved architecture documents

⸻

# 78. Commit Guidance

Create one focused implementation commit.

Suggested commit message:

feat(repository): implement IWP-009 Repository Service

Do not combine unrelated refactoring.

Do not include:

* generated caches
* temporary repositories
* .86vibe local state
* log files
* IDE metadata
* developer-specific configuration
* virtual environments
* test-generated Git configuration
* build artifacts unless required by repository policy

⸻

# 79. Completion Report

Upon completion, Cursor shall provide the following report exactly as a clearly structured Markdown response.

IWP-009 Completion Report

Implementation Summary
----------------------
Brief description of the completed Repository Service.

Sequencing Compliance
---------------------
- ADR-001 – Implement Repository Service Before Validation Framework: PASS
- IWP-008 behavior introduced: NO

Architecture Compliance
-----------------------
- AEP-002-10 Repository Services Specification: PASS
- ARP-002-07 Repository Service Interface Contract: PASS
- AEP-002-02 Repository Directory Structure: PASS
- AEP-002-16 Platform Bootstrap Sequence Specification: PASS
- AEP-002-17 Service Lifecycle & Dependency Management Specification: PASS
- ARP-001-03 Package Dependency Matrix: PASS
- ARP-001-05 Engineering Coding Standards: PASS
- ARP-002-16 Platform Service Contract Compliance Matrix: PASS

Public Interface
----------------
Implemented Operations:
- initialize
- open
- status
- exists
- read
- write
- list
- metadata
- refresh
- health
- shutdown

Public Models:
- ...

Public Errors:
- ...

Repository Capabilities
-----------------------
Repository Types:
- Platform: PASS
- CLI: PASS
- Example: PASS
- Extension: Supported classification or documented approved limitation
- Plugin: Supported classification or documented approved limitation
- Unknown: PASS

Git Inspection:
- Repository root: PASS
- Current branch: PASS
- Detached HEAD: PASS
- Default branch: PASS or explicit unavailable behavior
- Repository cleanliness: PASS
- Latest commit: PASS
- Current tag: PASS
- No Git write operations: PASS

Artifact Operations:
- Exists: PASS
- Read: PASS
- Write: PASS
- List: PASS
- Deterministic ordering: PASS
- Hidden-file handling: PASS
- Ignored-file handling: PASS

Security Verification
---------------------
- Repository boundary enforcement: PASS
- Parent traversal prevention: PASS
- Absolute-path escape prevention: PASS
- Symlink escape prevention: PASS
- .git access prevention: PASS
- .git write prevention: PASS
- Permission failure handling: PASS
- Artifact contents excluded from logs: PASS
- Sensitive values excluded from logs and errors: PASS
- Atomic write behavior: PASS

Lifecycle Integration
---------------------
- Configuration dependency: PASS
- Logging dependency: PASS
- Service Registry registration: PASS
- Service Lifecycle Manager integration: PASS
- Bootstrap integration: PASS
- Singleton lifetime: PASS
- Thread safety: PASS
- Safe shutdown: PASS

CLI Integration
---------------
- 86vibe repo status: PASS
- 86vibe repo validate: PASS
- 86vibe repo docs: PASS
- 86vibe doctor repository diagnostics: PASS
- Text output: PASS
- JSON output: PASS
- Stable exit-code mapping: PASS
- Unrelated deferred commands preserved: PASS

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
- Passed: ...
- Failed: 0

Integration Tests:
- Passed: ...
- Failed: 0

CLI Tests:
- Passed: ...
- Failed: 0

Concurrency Tests:
- Passed: ...
- Failed: 0

Security Tests:
- Passed: ...
- Failed: 0

Regression Tests:
- Passed: ...
- Failed: 0

Quality Checks
--------------
- Ruff: PASS
- Formatting: PASS
- MyPy: PASS
- PyTest: PASS
- Package Build: PASS or NOT CONFIGURED

Baseline Defect Corrections
---------------------------
- None

or provide, for every correction:
- affected baseline component
- defect
- minimum correction applied
- regression test added
- reason correction was required for IWP-009

Implementation Notes
--------------------
Document only implementation choices permitted by the approved architecture.

Known Limitations
-----------------
List only architecture-approved deferred capabilities.
Do not describe required IWP-009 behavior as a limitation.
Do not describe future functionality as implemented.

Ready For Review
----------------
YES

⸻

# 80. Definition of Done

IWP-009 is complete when:

* all functional acceptance criteria are satisfied
* all architectural acceptance criteria are satisfied
* all engineering acceptance criteria are satisfied
* ADR-001 is observed
* Repository Service is registered and lifecycle managed
* Repository Service initializes after Configuration and Logging
* repository discovery is deterministic
* repository validation is deterministic
* repository classification is deterministic
* repository metadata is normalized
* repository status is normalized
* Git inspection is read-only
* artifact access is governed
* artifact writes are safe
* repository boundaries are enforced
* traversal attempts are rejected
* symlink escapes are rejected
* metadata caching works
* refresh invalidates stale state
* thread safety is demonstrated
* CLI repository commands are operational
* doctor diagnostics include Repository Service
* no Validation Framework behavior is introduced
* all unit tests pass
* all integration tests pass
* all CLI tests pass
* all security tests pass
* all concurrency tests pass
* all baseline tests pass
* all quality gates pass
* packaging checks pass where configured
* implementation introduces no architectural drift
* implementation is ready for review and merge into 86-vibe-cli

No functionality beyond IWP-009 shall be implemented.

⸻

# 81. Cursor Execution Instructions

Before writing code, Cursor shall:

1. Inspect the current 86-vibe-cli implementation baseline.
2. Review ADR-001.
3. Review every governing architecture document referenced by this specification.
4. Confirm the current physical package structure.
5. Confirm the Configuration Service public API.
6. Confirm the Logging Service public API.
7. Confirm the Bootstrap Service public API.
8. Confirm the Service Registry public API and canonical service identifiers.
9. Confirm the Service Lifecycle Manager public API and lifecycle state model.
10. Confirm the CLI application context, output renderer, error mapper, exit codes, and deferred repository commands.
11. Inspect pyproject.toml and existing dependencies.
12. Identify the minimum files required for IWP-009.
13. Implement the Repository Service strictly within this specification.
14. Activate only the approved repository CLI commands.
15. Add all required tests.
16. Run all repository quality gates.
17. Run the complete regression suite.
18. Run packaging checks where configured.
19. Produce the required completion report.
20. Stop.

Do not begin IWP-008.

Do not begin IWP-010.

Do not modify the approved architecture.

Do not implement future repository capabilities.

⸻

:::