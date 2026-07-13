# IWP-008-05 – Validation CLI Integration Implementation Specification
## Part 1A – CLI Architecture, Commands, Execution Flow and Integration

---

# Document Control

| Item | Value |
|------|-------|
| Document ID | IWP-008-05 |
| Document Name | Validation CLI Integration Implementation Specification |
| Part | 1A |
| Implementation Package | IWP-008 |
| Status | Implementation Ready |
| Owner | Chief Systems Architect |
| Repository | 86-vibe-cli |
| Depends On | IWP-001 through IWP-007, IWP-009, IWP-008-00 through IWP-008-04 |
| Sequencing Authority | ADR-001 |

---

# 1. Purpose

This specification defines the implementation of the Validation Framework integration into the existing CLI Framework delivered by IWP-007.

The Validation CLI layer provides the public command interface used to invoke validation operations while remaining independent of validation implementation details.

The CLI layer shall:

- expose Validation Service functionality
- construct immutable ValidationRequest models
- invoke Validation Service
- render ValidationResponse objects
- return deterministic exit codes
- integrate with Logging Service
- integrate with Configuration Service
- integrate with Repository Service
- conform to the existing CLI Framework

The CLI shall not perform validation itself.

---

# 2. Scope

Part 1A defines:

- CLI architecture
- package structure
- command registration
- command hierarchy
- argument models
- option models
- command execution flow
- Validation Service interaction
- Repository Service interaction
- Configuration integration
- Logging integration
- request construction
- execution lifecycle
- cancellation
- progress reporting architecture
- exit code architecture
- public command contracts

Rendering, JSON formatting, testing and acceptance are defined in Part 1B.

---

# 3. Governing Specifications

Implementation shall conform to:

- AP-001
- AP-002
- ARP-001
- ARP-002
- IWP-007 CLI Framework
- IWP-008-00
- IWP-008-01
- IWP-008-02
- IWP-008-03
- IWP-008-04

No interface defined by those documents may be modified.

---

# 4. Architectural Position

The CLI shall sit above the Validation Service.

```text
CLI Framework
      │
      ▼
Validation CLI Commands
      │
      ▼
Validation Service
      │
      ▼
Validation Engine
      │
      ▼
Validators
```

No validator shall ever be invoked directly by CLI code.

---

# 5. Responsibilities

The Validation CLI is responsible for:

- parsing user input
- validating CLI arguments
- locating repositories
- creating ValidationRequest models
- invoking Validation Service
- rendering ValidationResponse
- returning exit codes

The Validation CLI is not responsible for:

- validation logic
- repository traversal
- document parsing
- rule execution
- finding generation
- result aggregation

---

# 6. Package Structure

The implementation shall reside beneath:

```text
src/
    cli/
        validation/
```

Minimum modules:

```text
validation_command.py
validation_parser.py
validation_options.py
validation_request_factory.py
validation_runner.py
validation_progress.py
validation_exit_codes.py
```

Rendering modules are defined in Part 1B.

---

# 7. Command Registration

The Validation command shall register with the existing CLI Framework.

Root command:

```text
86-vibe validate
```

Registration shall use the IWP-007 command registry.

Registration shall occur during CLI bootstrap.

---

# 8. Command Hierarchy

Minimum hierarchy:

```text
validate

validate repository

validate document
```

Future commands may be added.

No existing commands shall be modified.

---

# 9. Public Commands

Initial commands:

```text
86-vibe validate repository

86-vibe validate document
```

Aliases shall not be introduced without architecture approval.

---

# 10. Command Responsibilities

Repository command shall:

- locate repository
- construct RepositoryValidationTarget
- invoke Validation Service

Document command shall:

- locate document
- construct DocumentValidationTarget
- invoke Validation Service

---

# 11. Command Lifecycle

Every command shall follow:

```text
Parse

↓

Validate CLI Arguments

↓

Resolve Repository

↓

Construct ValidationRequest

↓

Invoke Validation Service

↓

Receive ValidationResponse

↓

Render Output

↓

Return Exit Code
```

This sequence is mandatory.

---

# 12. Dependency Injection

Command handlers shall receive:

- Validation Service
- Repository Service
- Configuration Service
- Logging Service

Handlers shall not instantiate services directly.

---

# 13. Validation Request Factory

The CLI shall not construct ValidationRequest objects inline.

A dedicated factory shall perform:

- target creation
- option conversion
- rule selection
- execution options
- metadata population

The factory shall return immutable ValidationRequest models.

---

# 14. Repository Resolution

Repository resolution shall use Repository Service.

The CLI shall never inspect Git directly.

Repository lookup shall support:

- current directory
- explicit path

Failure shall return deterministic errors.

---

# 15. Document Resolution

Document lookup shall use Repository Service.

Absolute filesystem paths shall not become ValidationTargets.

Repository-relative paths shall be normalized.

---

# 16. Argument Parsing

Arguments shall be parsed by the CLI Framework.

Validation CLI shall receive typed argument models.

String parsing inside handlers is prohibited.

---

# 17. Required Arguments

Repository validation:

```text
validate repository
```

Document validation:

```text
validate document <path>
```

Document path is mandatory.

---

# 18. Common Options

Supported options:

```text
--strict

--json

--rules

--categories

--fail-fast

--parallel

--timeout

--incremental

--verbose

--quiet
```

Option semantics shall match ValidationRequest models.

---

# 19. Rule Selection Options

Rule selection shall support:

```text
--rules

ruleA,ruleB
```

Categories:

```text
--categories naming,metadata
```

Mandatory rules shall remain enabled.

---

# 20. Execution Options

Execution options shall populate:

ValidationExecutionOptions

Supported:

- strategy
- timeout
- concurrency
- fail-fast
- cache policy

---

# 21. Validation Modes

Supported:

```text
standard

strict
```

Mode selection shall populate ValidationMode.

---

# 22. Request Metadata

CLI metadata shall include:

- invocation timestamp
- CLI version
- command name

Sensitive information shall not be included.

---

# 23. Logging Integration

Logging shall use Logging Service.

CLI shall never configure loggers directly.

Validation commands shall emit:

- command start
- request creation
- completion
- failure

---

# 24. Configuration Integration

Configuration shall use Configuration Service.

CLI shall not read configuration files directly.

Configuration shall include:

- default mode
- default concurrency
- default timeout
- output preferences

---

# 25. Validation Service Invocation

CLI shall invoke only:

```text
ValidationService.validate(request)
```

No engine methods shall be visible.

---

# 26. Exception Handling

The CLI shall catch only:

- Validation Service exceptions
- Repository Service exceptions
- Configuration exceptions
- CLI argument exceptions

Unexpected exceptions shall propagate to the global CLI handler.

---

# 27. Cancellation

Keyboard interrupt shall trigger cancellation.

Cancellation shall propagate using ValidationCancellationContext.

Forced process termination shall be avoided.

---

# 28. Progress Architecture

Progress reporting shall remain independent of validation logic.

The CLI shall subscribe to progress events.

Validators shall never emit CLI output.

---

# 29. Progress Events

Supported events:

- request accepted
- repository loaded
- inventory complete
- analysis complete
- rule execution started
- rule execution complete
- aggregation complete
- response complete

---

# 30. Progress Subscriber

Progress shall use an observer interface.

Validation Engine shall remain unaware of terminal rendering.

---

# 31. Output Separation

Execution shall return ValidationResponse.

Rendering shall occur afterwards.

Rendering shall not modify ValidationResponse.

---

# 32. Exit Code Architecture

Exit codes shall be represented by an enumeration.

Handlers shall not return numeric literals.

Mappings are defined in Part 1B.

---

# 33. Command Result Model

Internal command execution shall return:

```text
ValidationCommandResult
```

Containing:

- response
- elapsed time
- exit code

---

# 34. Error Categories

CLI errors shall distinguish:

- argument errors
- configuration errors
- repository errors
- validation errors
- execution errors

---

# 35. Performance Requirements

CLI overhead shall remain negligible compared to validation execution.

Request construction shall avoid unnecessary allocations.

---

# 36. Thread Safety

Handlers shall remain stateless.

Shared mutable state is prohibited.

---

# 37. Stop Conditions

Cursor shall stop implementation if:

- Validation Service interface changes
- Repository Service contract changes
- CLI Framework contracts change
- required models are unavailable
- architecture conflicts exist

---

# 38. Public API

The Validation CLI package shall expose only approved command registration interfaces.

Internal factories and helpers shall remain private.

---

# 39. Part 1A Completion

Part 1A defines:

- CLI architecture
- command hierarchy
- request construction
- Validation Service integration
- Repository Service integration
- Configuration integration
- Logging integration
- execution lifecycle
- progress architecture
- cancellation
- exit code architecture
- command contracts

Rendering, JSON output, formatting, testing, acceptance criteria and deliverables are defined in Part 1B.
