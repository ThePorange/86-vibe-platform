Filename: IWP-007-01-CLI-Framework.md

IWP-007-01 – CLI Framework Implementation Specification

Document Control

Item	Value
Document ID	IWP-007-01
Document Name	CLI Framework Implementation Specification
Implementation Package	IWP-007 – CLI Framework
Platform	86-vibe
Implementation Repository	86-vibe-cli
Architecture Repository	86-vibe-platform
Status	Implementation Ready
Owner	Chief Systems Architect
Implementation Agent	Cursor
Depends On	IWP-001 – Repository FoundationIWP-002 – Configuration ServiceIWP-003 – Logging ServiceIWP-004 – Bootstrap ServiceIWP-005 – Service RegistryIWP-006 – Service Lifecycle Manager
Supersedes	None

⸻

1. Purpose

This document defines the complete implementation requirements for IWP-007 – CLI Framework.

The CLI Framework is the process-level entry point through which users invoke the 86-vibe platform.

This work package shall implement the executable framework, command registration model, application lifecycle, argument handling, output abstraction, error mapping, exit-code behavior, help system, version command, and framework-level tests.

The CLI shall remain thin.

Business logic shall remain in platform services.

This work package shall not prematurely implement the business behavior assigned to later implementation work packages.

Cursor shall treat this document as the complete implementation prompt.

Nothing required for implementation shall exist outside this Markdown document except:

* the approved architecture repository
* the existing 86-vibe-cli implementation baseline
* this implementation specification

⸻

2. Implementation Objective

Implement the CLI Framework in the 86-vibe-cli repository.

The implementation shall provide:

* the installed 86vibe executable
* the CLIApplication process-level application
* Typer-based command routing
* deterministic command registration
* platform bootstrap integration
* lifecycle manager integration
* service readiness validation
* structured terminal output
* stable exit codes
* user-facing error formatting
* diagnostic-mode behavior
* command context and dependency access
* top-level help
* command-group help
* version output
* framework-level command shells for approved future commands
* unit and integration tests

The implementation shall conform to:

* AEP-002-04 – CLI Specification
* ARP-002-01 – CLI Service Interface Contract
* AEP-002-13 – Build & Packaging Specification
* AEP-002-15 – Local Development Workflow Specification
* AEP-002-16 – Platform Bootstrap Sequence Specification
* AEP-002-17 – Service Lifecycle & Dependency Management Specification
* AEP-002-18 – Platform Reference Implementation Specification
* ARP-002-16 – Platform Service Contract Compliance Matrix
* ARP-001-03 – Package Dependency Matrix
* ARP-001-05 – Engineering Coding Standards

⸻

3. Current Implementation Baseline

The following implementation work packages have already been built, approved, committed, and are immutable:

* IWP-001 – Repository Foundation
* IWP-002 – Configuration Service
* IWP-003 – Logging Service
* IWP-004 – Bootstrap Service
* IWP-005 – Service Registry
* IWP-006 – Service Lifecycle Manager

Cursor shall:

1. Inspect the current 86-vibe-cli repository before writing code.
2. Preserve repository, package, typing, testing, formatting, and dependency conventions.
3. Use the public interfaces already implemented by previous IWPs.
4. Initialize the platform through the Bootstrap Service.
5. Resolve platform services through approved Bootstrap or Service Registry interfaces.
6. Use the Service Lifecycle Manager to validate service readiness where required.
7. Avoid modifying previous work unless correcting a defect that directly blocks IWP-007.
8. Document any such defect correction in the completion report.

⸻

4. Strict Scope

This work package implements only:

IWP-007 – CLI Framework

The scope includes:

* CLI package and executable entry point
* CLI application lifecycle
* Typer application setup
* root options
* command and command-group registration
* invocation context
* output rendering abstraction
* exception classification
* exit-code mapping
* bootstrap startup and shutdown integration
* framework-level service access
* help output
* version command
* command shells for architecturally approved commands
* tests for framework behavior

This work package does not implement the complete business behavior of:

* Validation Framework
* Repository Service
* Prompt Management Service
* AI Provider Layer
* MCP Client Service
* Health Monitoring Service
* Event Bus Service
* Plugin Manager Service
* Platform Context Service

⸻

5. Explicitly Out of Scope

The following implementation work packages remain out of scope:

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

Do not implement production business logic belonging to those packages.

Minimal command shells and test doubles are permitted only where required to establish the approved CLI surface.

⸻

6. Architectural Non-Negotiables

6.1 No Architecture Redesign

Cursor shall not:

* redesign the approved CLI structure
* replace Typer with another framework
* create a second platform bootstrap path
* read configuration files directly
* use Python logging directly from CLI commands
* instantiate future service implementations
* embed repository, validation, prompt, AI, or MCP business logic in commands
* create hidden mutable global application state
* create a global service locator
* bypass the Service Registry
* bypass lifecycle readiness checks
* change approved public command names without an ADR
* change stable exit-code meanings without an ADR
* expose stack traces by default
* expose secrets in terminal output

6.2 Stop Conditions

Cursor shall stop and report an ambiguity instead of inventing a solution if:

1. The committed implementation baseline conflicts with the approved CLI contracts.
2. The Bootstrap Service cannot be invoked through a stable public interface.
3. The Service Registry cannot expose services needed by the CLI.
4. The Lifecycle Manager cannot expose platform or service readiness.
5. The installed command cannot be configured without changing the approved packaging structure.
6. Typer cannot be introduced under the existing dependency policy.
7. Rich cannot be introduced where needed under the existing dependency policy.
8. A command requires business behavior belonging to a future IWP.
9. Existing public service exceptions cannot be mapped deterministically.
10. Implementation would require changing an approved service contract.

Do not silently resolve architectural ambiguity.

⸻

7. Service Identity

The CLI shall conform to ARP-002-01.

Property	Required Value
Service Name	CLI
Public Package	vibe.cli
Primary Class	CLIApplication
Installed Command	86vibe
Lifetime	Process Lifetime
Thread Safety	Required
Public	Yes

AEP-002-04 also refers to the Python package name vibe_cli.

The current implementation baseline shall determine the physical package layout.

The implementation shall expose the approved public package vibe.cli.

If packaging compatibility requires vibe_cli as a thin entry-point package, it may delegate to vibe.cli, but it shall not contain a second CLI implementation.

⸻

8. CLI Design Principles

The implementation shall preserve the architecture-defined design principles.

8.1 Architecture-First Execution

Commands shall operate through approved platform services and architecture contracts.

8.2 Human-Governed Workflows

The CLI shall not approve, overwrite, or rebaseline architecture without explicit human action.

8.3 Git-First Behavior

Material file changes produced by later commands shall remain visible through Git.

IWP-007 shall not implement those file-changing workflows.

8.4 Vendor-Neutral Execution

The CLI framework shall not import concrete AI provider implementations.

8.5 Local-First Operation

The CLI shall function from a local repository checkout without requiring hosted platform services.

8.6 Safe Defaults

Destructive commands introduced in future work packages shall require confirmation.

The framework shall provide reusable confirmation support but shall not implement destructive operations.

⸻

9. Technology Requirements

The CLI shall be implemented using:

* Python 3.11 or later
* Typer
* Click only indirectly through Typer or where Typer exposes Click primitives
* Rich for formatted terminal output where useful
* pyproject.toml for packaging and entry-point configuration
* pytest for tests
* Ruff for linting and formatting
* the repository’s configured type checker

Use existing dependency versions and conventions where already established.

Do not add another CLI framework.

Do not add a third-party dependency-injection framework.

⸻

10. Installed Executable

The package shall install a single executable:

86vibe

The following shall work after installation:

86vibe --help
86vibe version

The entry point shall be declared through the repository’s approved pyproject.toml configuration.

Expected conceptual configuration:

[project.scripts]
86vibe = "vibe.cli.main:main"

Use the actual module path created by the implementation.

Do not add multiple competing executable entry points.

⸻

11. CLIApplication

Implement the primary process-level application class:

class CLIApplication:
    ...

The class shall own:

* Typer application construction
* command registration
* root callback behavior
* bootstrap invocation
* command execution context
* exception mapping
* shutdown coordination
* exit-code determination

The class shall not own:

* configuration parsing
* logging implementation
* service construction outside Bootstrap
* repository business logic
* validation business logic
* prompt business logic
* AI business logic
* MCP business logic

⸻

12. Public Application Interface

The CLIApplication shall expose behavior equivalent to:

class CLIApplication:
    def __init__(
        self,
        bootstrap_service: BootstrapService | None = None,
    ) -> None:
        ...
    def create_app(self) -> typer.Typer:
        ...
    def run(
        self,
        args: Sequence[str] | None = None,
    ) -> int:
        ...
    def shutdown(self) -> None:
        ...

The actual interface shall follow the approved architecture and existing repository conventions.

The executable entry-point function shall be small and equivalent to:

def main() -> None:
    application = CLIApplication()
    exit_code = application.run()
    raise SystemExit(exit_code)

Avoid placing application logic in the entry-point function.

⸻

13. Process Lifecycle

Every CLI invocation shall execute independently.

The process lifecycle shall be deterministic:

Receive arguments
Validate root syntax
Determine requested command
Construct CLI application
Initialize Bootstrap Service when required
Obtain Configuration Service
Obtain Logging Service
Obtain Service Registry
Obtain Lifecycle Manager
Validate required service readiness
Execute command handler
Map result or exception to exit code
Shut down platform services
Terminate process

The CLI shall not rely on state retained from another invocation.

⸻

14. Bootstrap Timing

The CLI architecture requires platform initialization before command execution.

However, commands such as --help and invalid syntax shall not require full platform startup where Typer can resolve them safely before command execution.

The implementation shall follow these rules:

* root help shall not require platform bootstrap
* command help shall not require platform bootstrap
* shell completion shall not require platform bootstrap
* argument parsing failures shall not require platform bootstrap
* version should avoid full bootstrap unless version metadata requires platform services
* operational commands shall initialize Bootstrap before business execution

This preserves the startup contract while avoiding side effects for help and parsing.

⸻

15. Command Context

Implement an invocation-scoped command context.

Expected immutable or controlled model:

@dataclass
class CLIContext:
    bootstrap: BootstrapService
    configuration: ConfigurationService
    logging: LoggingService
    registry: ServiceRegistry
    lifecycle: ServiceLifecycleManager
    output: OutputRenderer
    diagnostic: bool = False
    machine_readable: bool = False

The context shall:

* exist only for the current invocation
* expose approved service interfaces
* not expose mutable registry internals
* not contain raw secrets
* not be stored in a module-level mutable global

Typer context storage may hold this invocation-scoped object.

⸻

16. Dependency Access

Commands shall obtain dependencies through CLIContext.

Commands shall not:

* instantiate services
* read configuration files
* inspect environment variables directly
* access Service Registry internals
* import concrete future service implementations
* use module-level singleton service references

Future service-specific commands shall resolve those services through approved registry interfaces.

⸻

17. Root Application

The root command shall use the installed name:

86vibe

The root application shall provide:

* platform description
* top-level help
* stable command listing
* global diagnostic option
* global machine-readable output option where supported
* version callback or version command
* no-arguments help behavior

Invoking:

86vibe

shall display concise help and terminate successfully unless the approved Typer convention in the repository requires another deterministic behavior.

⸻

18. Root Options

The root application shall support framework-level options equivalent to:

--help
--version
--diagnostic
--output <format>

--help shall display help.

--version may be implemented as an eager root option in addition to the required version command.

--diagnostic shall enable diagnostic error output.

--output shall support approved output modes.

Minimum modes:

text
json

Commands that do not yet support machine-readable output shall fail clearly rather than emit malformed JSON.

⸻

19. Approved Command Surface

The architecture documents define overlapping command terminology.

The CLI Framework shall expose a compatible superset without duplicating business logic.

The approved top-level command surface shall include:

86vibe init
86vibe doctor
86vibe config
86vibe arch
86vibe prompt
86vibe ai
86vibe mcp
86vibe repo
86vibe validate
86vibe build
86vibe providers
86vibe prompts
86vibe repository
86vibe version

Where both singular and plural or abbreviated and full command names exist, one shall be canonical and the other shall be a thin alias.

Required aliases:

prompt      -> prompts
repo        -> repository
ai providers -> providers

Aliases shall delegate to the same handler or command group.

Do not implement duplicate business logic.

⸻

20. Command Implementation Scope

IWP-007 shall fully implement framework behavior and only the commands whose behavior can be completed without future services.

20.1 Fully Implemented in IWP-007

The following shall be operational:

86vibe --help
86vibe --version
86vibe version

The root framework shall also support:

* invalid-command handling
* invalid-option handling
* diagnostic mode
* deterministic exit codes
* help for all registered command shells

20.2 Framework Shells in IWP-007

The following command names and groups shall exist with help output, but their business operations shall be implemented in their governing future IWPs:

init
doctor
config
arch
validate
build
prompt
prompts
ai
providers
mcp
repo
repository

A deferred command shall:

* display meaningful help
* identify the governing future service or work package
* return a deterministic runtime or unavailable exit code if directly executed
* not pretend the operation succeeded
* not implement future business logic
* not modify files

⸻

21. Command Groups and Subcommands

The framework shall register the architecture-approved nested command structure.

21.1 Configuration Group

86vibe config show
86vibe config validate
86vibe config set
86vibe config unset

The group and subcommand help shall exist.

Full mutation behavior shall not be added if it is absent from the approved Configuration Service public API.

21.2 Architecture Group

86vibe arch list
86vibe arch validate
86vibe arch status
86vibe arch baseline

These commands shall remain shells until the required Repository and Validation services exist.

The CLI shall never approve architecture.

21.3 Prompt Groups

Canonical group:

86vibe prompts

Compatibility alias:

86vibe prompt

Approved subcommands:

list
show
validate
render

These shall remain shells until IWP-010.

21.4 AI Group

86vibe ai providers
86vibe ai models
86vibe ai plan
86vibe ai implement

These shall remain shells until IWP-011.

21.5 MCP Group

86vibe mcp list
86vibe mcp validate
86vibe mcp doctor

These shall remain shells until IWP-012.

21.6 Repository Groups

Canonical group:

86vibe repository

Compatibility alias:

86vibe repo

Approved subcommands:

status
validate
docs

These shall remain shells until IWP-009.

⸻

22. Deferred Command Behavior

A deferred command shall produce a concise actionable message.

Example:

This command is not available in the current implementation baseline.
Required service: Repository Service.

The command shall return:

5 – Runtime Failure

unless the approved error mapping identifies a more precise stable code.

Deferred commands shall not return success.

Deferred commands shall not emit stack traces.

Deferred commands shall log invocation and unavailability after Bootstrap is initialized, where applicable.

⸻

23. Version Command

The version command shall be fully implemented.

It shall display:

* CLI or platform version
* architecture version where available
* Python version
* build identifier where available
* platform name

Human-readable example:

86-vibe CLI: 0.1.0
Platform: 86-vibe
Architecture: 1.0.0
Python: 3.11.x
Build: development

The exact values shall come from approved package metadata, constants, or generated build metadata.

Do not hard-code a false Git commit hash.

When a build identifier is unavailable, use a deterministic value such as:

development

⸻

24. Machine-Readable Version Output

The version command shall support JSON output.

Example:

{
  "architecture_version": "1.0.0",
  "build": "development",
  "cli_version": "0.1.0",
  "platform": "86-vibe",
  "python_version": "3.11.9"
}

JSON key ordering shall be deterministic.

JSON output shall contain no Rich markup or ANSI control codes.

⸻

25. Exit Code Contract

The CLI shall implement the stable exit codes defined by ARP-002-01.

Code	Meaning
0	Success
1	General Error
2	Invalid Arguments
3	Configuration Error
4	Validation Failure
5	Runtime Failure
6	External Service Failure
7	Internal Platform Error

Additionally, user interruption shall preserve the conventional architecture-defined code:

Code	Meaning
130	User Cancelled or Interrupted

These codes are the canonical framework mapping.

The narrower historical codes in AEP-002-04 shall map into this canonical contract:

* repository failures map to Validation Failure or Runtime Failure according to context
* AI provider and MCP external failures map to External Service Failure
* user cancellation maps to 130

Do not create additional public exit codes in this work package.

⸻

26. Exit Code Model

Implement an enum equivalent to:

class ExitCode(IntEnum):
    SUCCESS = 0
    GENERAL_ERROR = 1
    INVALID_ARGUMENTS = 2
    CONFIGURATION_ERROR = 3
    VALIDATION_FAILURE = 4
    RUNTIME_FAILURE = 5
    EXTERNAL_SERVICE_FAILURE = 6
    INTERNAL_PLATFORM_ERROR = 7
    USER_CANCELLED = 130

Commands shall not use arbitrary integer literals.

⸻

27. Error Categories

The framework shall classify user-visible failures into stable categories.

Expected categories:

CONFIG_ERROR
REPO_ERROR
VALIDATION_ERROR
AI_PROVIDER_ERROR
MCP_ERROR
USER_CANCELLED
RUNTIME_ERROR
INTERNAL_ERROR

Categories shall map deterministically to exit codes.

Example mappings:

Category	Exit Code
CONFIG_ERROR	3
VALIDATION_ERROR	4
REPO_ERROR	4 or 5 according to failure type
AI_PROVIDER_ERROR	6
MCP_ERROR	6
USER_CANCELLED	130
RUNTIME_ERROR	5
INTERNAL_ERROR	7

⸻

28. Error Mapping

The CLI shall map known platform exceptions to stable user-facing failures.

At minimum, map exceptions from:

* Configuration Service
* Logging Service
* Bootstrap Service
* Service Registry
* Service Lifecycle Manager
* Typer or Click argument parsing

Known failures shall not be reported as internal errors.

Unexpected unclassified exceptions shall:

* be logged
* return exit code 7
* display a concise internal-platform error
* include a stack trace only in diagnostic mode

⸻



# 29. User-Facing Error Format

User-facing errors shall be concise, deterministic, and actionable.

Human-readable errors shall include:

- error category
- concise summary
- corrective action where known
- correlation identifier where available and useful

Example:

```text
Configuration error: Unable to load project configuration.

Check that .86vibe/config.yaml exists and contains valid YAML.
```

Errors shall not include:

- Python object representations
- internal service instances
- raw stack traces by default
- secrets
- authentication values
- mutable internal state

---

# 30. Diagnostic Mode

Diagnostic mode shall be enabled through:

```text
--diagnostic
```

Diagnostic mode may include:

- exception type
- exception cause chain
- stack trace
- correlation identifier
- service lifecycle state
- command invocation details
- platform version metadata

Diagnostic mode shall still redact secrets.

Diagnostic mode shall not change command behavior or exit-code mapping.

---

# 31. Output Renderer

Implement a reusable CLI output abstraction.

Expected public interface:

```python
class OutputRenderer:
    def info(self, message: str) -> None:
        ...

    def success(self, message: str) -> None:
        ...

    def warning(self, message: str) -> None:
        ...

    def error(self, message: str) -> None:
        ...

    def table(
        self,
        columns: Sequence[str],
        rows: Sequence[Sequence[object]],
    ) -> None:
        ...

    def json(self, value: object) -> None:
        ...
```

The exact API may differ according to repository conventions, provided equivalent behavior exists.

The renderer shall centralize:

- Rich console usage
- plain-text fallback
- JSON serialization
- ANSI suppression
- output-mode handling

Commands shall not directly construct Rich Console instances.

---

# 32. Output Modes

The framework shall support at least:

```text
text
json
```

## 32.1 Text Mode

Text mode shall:

- use concise human-readable output
- use Rich formatting where configured
- avoid excessive decoration
- remain readable without color

## 32.2 JSON Mode

JSON mode shall:

- emit valid JSON
- emit no ANSI codes
- use deterministic key ordering
- avoid trailing commentary
- return one complete JSON value for a command result
- serialize dataclasses, enums, tuples, and supported primitive values deterministically

Errors in JSON mode shall also be valid JSON.

Example:

```json
{
  "error": {
    "category": "CONFIG_ERROR",
    "message": "Unable to load configuration"
  },
  "success": false
}
```

---

# 33. Color Behavior

Color shall be used only in human-readable mode.

Color shall be disabled when:

- JSON output is selected
- the terminal does not support color
- standard no-color conventions are present
- tests require deterministic plain output

The implementation shall respect the conventional environment variable:

```text
NO_COLOR
```

The CLI shall not depend on color to convey meaning.

---

# 34. Standard Streams

The CLI shall use streams consistently.

## Standard Output

Use standard output for:

- successful command results
- help
- version output
- machine-readable successful output

## Standard Error

Use standard error for:

- errors
- warnings where appropriate
- diagnostic stack traces
- invalid invocation messages

JSON error output shall be written to standard error unless repository conventions define a single machine-readable stream contract.

Tests shall verify stream behavior.

---

# 35. Logging Integration

The CLI shall use the Logging Service from IWP-003.

The CLI shall obtain its logger through approved platform interfaces.

Preferred logger name:

```text
cli
```

Command-specific child names may be used:

```text
cli.version
cli.repository.status
cli.validate
```

The CLI shall log:

- process invocation
- selected command
- bootstrap success or failure
- command start
- command completion
- exit code
- known platform failure
- unexpected failure
- shutdown completion or failure

The CLI shall not log:

- raw secrets
- authentication tokens
- complete sensitive configuration
- arbitrary command argument values known to be sensitive

---

# 36. Correlation Integration

Each operational CLI invocation shall establish a correlation identifier through the Logging Service when supported.

The correlation identifier shall:

- be unique per invocation
- remain stable throughout the command
- appear in diagnostic logs
- be cleared during shutdown
- not leak into later invocations in the same test process

The identifier need not be displayed to users for successful commands.

For failures, it may be displayed as a support reference.

---

# 37. Bootstrap Integration

Operational command execution shall initialize the platform through Bootstrap.

The CLI shall not reconstruct individual platform services.

Expected conceptual flow:

```python
bootstrap = BootstrapService(...)
bootstrap.initialize()

configuration = bootstrap.configuration_service
logging_service = bootstrap.logging_service
registry = bootstrap.service_registry
lifecycle = bootstrap.lifecycle_manager
```

Use actual public accessors from the committed baseline.

If Bootstrap exposes a result object rather than direct properties, use that object.

Do not modify Bootstrap merely to make CLI code shorter.

---

# 38. Service Registry Integration

The CLI shall use the Service Registry for approved service resolution.

The CLI shall not:

- access registry internals
- maintain a duplicate service map
- instantiate absent services
- silently continue when a required service is missing

Missing required services shall produce a deterministic runtime or internal-platform error according to the failure cause.

Future command groups shall resolve their service dependencies through the registry when those services are implemented.

---

# 39. Lifecycle Manager Integration

Before executing an operational command, the CLI shall verify that required platform services are ready.

At minimum, the framework shall verify readiness of:

- Configuration Service
- Logging Service
- Service Registry
- Service Lifecycle Manager

Bootstrap readiness shall be derived from successful Bootstrap completion or its public status.

If a required service is:

- missing
- failed
- stopping
- stopped
- not ready

the command shall not execute.

The CLI shall report a deterministic platform-readiness failure.

---

# 40. Command Dependency Declaration

Each command or command group shall declare the service identifiers it requires.

Expected conceptual model:

```python
@dataclass(frozen=True)
class CommandRequirements:
    required_services: tuple[str, ...] = ()
    bootstrap_required: bool = True
    supports_json: bool = False
    destructive: bool = False
```

The framework shall use these declarations to:

- determine whether Bootstrap is needed
- validate service readiness
- validate output-mode compatibility
- enforce confirmation policy for future destructive commands

Do not hard-code readiness checks separately inside every command.

---

# 41. Command Registration Model

Command registration shall be centralized and deterministic.

Expected logical registration flow:

```python
register_core_commands(app)
register_config_commands(app)
register_architecture_commands(app)
register_repository_commands(app)
register_validation_commands(app)
register_prompt_commands(app)
register_ai_commands(app)
register_mcp_commands(app)
```

IWP-007 shall register framework shells for future command groups.

The registration model shall allow later IWPs to replace shell handlers with operational handlers without restructuring the root application.

---

# 42. No Dynamic Plugin Registration Yet

The CLI Framework shall not implement dynamic command discovery or plugin-based command registration.

Plugin command integration belongs to IWP-015.

For IWP-007:

- command registration shall be explicit
- command registration order shall be deterministic
- no filesystem scanning shall occur
- no Python entry-point plugin discovery shall occur

The design shall remain extendable for future plugin integration without implementing it now.

---

# 43. Command Naming and Help

Command names shall use lowercase kebab-case where multiple words are required.

Examples:

```text
service-status
architecture-version
```

Existing architecture-defined single-word commands shall remain unchanged.

Help text shall be:

- concise
- action-oriented
- deterministic
- free from implementation detail
- clear about unavailable future capabilities

Each command group and subcommand shall include help text.

---

# 44. Help Output Requirements

Root help shall include:

- platform summary
- global options
- registered commands
- concise command descriptions

Command-group help shall include:

- group purpose
- subcommands
- aliases where relevant
- current availability where deferred

Help output shall not require repository state.

Help output shall not initialize platform services.

Help output shall return exit code 0.

---

# 45. Version Metadata Source

Version information shall be retrieved using the repository’s approved package metadata mechanism.

Preferred standard-library approach:

```python
from importlib.metadata import PackageNotFoundError, version
```

Development checkouts shall have a deterministic fallback.

The framework shall not execute Git commands solely to render version output unless the architecture or existing build process already provides that behavior.

Architecture version may be sourced from:

- an approved constant
- generated package metadata
- architecture manifest already present in the repository

Do not parse arbitrary architecture documents at runtime for basic version output.

---

# 46. Confirmation Support

The framework shall provide a reusable confirmation helper for future destructive commands.

Expected behavior:

```python
confirm_action(
    message: str,
    *,
    default: bool = False,
) -> bool
```

Rules:

- default shall be safe
- non-interactive execution shall not silently approve
- cancellation shall map to exit code 130 or a deterministic unsuccessful result according to invocation context
- JSON mode shall not prompt interactively unless explicitly supported

No destructive operations are implemented in IWP-007.

---

# 47. Non-Interactive Execution

The CLI shall behave predictably in automation.

Non-interactive requirements:

- no unexpected prompts
- deterministic exit codes
- ANSI-free JSON output
- no progress animation in JSON mode
- stable error structure
- help and version usable without Bootstrap
- deferred commands fail clearly

Where terminal interactivity is required in future, commands shall expose explicit flags rather than infer approval.

---

# 48. Progress Output

The CLI framework may provide a reusable progress abstraction for future long-running operations.

IWP-007 shall not introduce background workers.

Progress behavior shall:

- be disabled in JSON mode
- be testable
- use deterministic completion behavior
- avoid corrupting redirected output
- avoid hiding errors

Do not add decorative progress output where no long-running operation exists.

---

# 49. Interruption Handling

The CLI shall handle:

- `KeyboardInterrupt`
- Typer aborts
- Click aborts
- explicit user cancellation

Interruption shall:

1. log cancellation where logging is available
2. initiate safe Bootstrap shutdown
3. return exit code 130
4. avoid displaying a stack trace unless diagnostic mode requires it
5. preserve terminal usability

---

# 50. Shutdown Behavior

The CLI shall always attempt platform shutdown after operational command execution.

Shutdown shall occur for:

- successful command completion
- known command failure
- unexpected exception
- user interruption

The CLI shall invoke the approved Bootstrap shutdown path.

The CLI shall not independently shut down Registry or Lifecycle Manager services unless Bootstrap contract explicitly delegates that behavior.

If shutdown fails after a successful command:

- log the shutdown failure
- return an error exit code appropriate to the failure
- do not report full success

If shutdown fails after an existing command failure:

- preserve the primary command failure category
- log the secondary shutdown failure
- expose the shutdown issue in diagnostic mode

---

# 51. Framework Exception Hierarchy

Create CLI-specific exceptions where needed.

Expected hierarchy:

```text
CLIError

├── CLIInitializationError

├── CLICommandError

├── CLIOutputError

├── CLIServiceUnavailableError

├── CLIUnsupportedOperationError

└── CLIInternalError
```

If IWP-001 defines a common platform base exception, `CLIError` shall inherit from it.

Do not wrap known platform exceptions unnecessarily if the error mapper can classify them directly.

---

# 52. Error Mapper

Implement a centralized error mapper.

Expected conceptual result:

```python
@dataclass(frozen=True)
class CLIErrorResult:
    category: CLIErrorCategory
    exit_code: ExitCode
    message: str
    detail: str | None = None
    correlation_id: str | None = None
```

The mapper shall accept an exception and produce a deterministic result.

The mapper shall not print output directly.

Output rendering and error classification shall remain separate.

---

# 53. Command Result Model

Command handlers shall return a stable result model or domain value suitable for rendering.

Expected framework result model:

```python
@dataclass(frozen=True)
class CommandResult:
    success: bool
    exit_code: ExitCode
    message: str | None = None
    data: object | None = None
```

A successful handler shall not call `SystemExit` directly.

The root CLI application shall own process exit behavior.

Typer parsing internals may still raise framework-specific exits, which shall be handled consistently in tests.

---

# 54. Internal Package Layout

Follow the package conventions established by IWP-001.

Expected logical layout:

```text
src/
└── vibe/
    └── cli/
        __init__.py
        application.py
        main.py
        context.py
        commands/
            __init__.py
            core.py
            config.py
            architecture.py
            repository.py
            validation.py
            prompts.py
            ai.py
            mcp.py
        output/
            __init__.py
            renderer.py
            serializers.py
        errors.py
        exit_codes.py
        requirements.py
        confirmation.py
        version.py
```

Use the actual current package structure where it differs.

Do not create parallel CLI implementations.

---

# 55. Internal Module Responsibilities

## application.py

Responsible for:

- `CLIApplication`
- Typer root app construction
- root callback
- command registration
- invocation lifecycle
- Bootstrap integration
- exception handling
- shutdown coordination

## main.py

Responsible only for:

- installed entry point
- creating `CLIApplication`
- terminating with the returned exit code

## context.py

Responsible for:

- invocation-scoped `CLIContext`
- safe service access
- output-mode state
- diagnostic-mode state

## commands/

Responsible for:

- Typer command declarations
- help text
- translating CLI inputs into service calls
- rendering or returning command results

Command modules shall not contain service implementation logic.

## output/

Responsible for:

- human-readable rendering
- JSON rendering
- supported serialization
- output stream selection

## errors.py

Responsible for:

- CLI exception hierarchy
- error categories
- centralized error mapping

## exit_codes.py

Responsible for:

- stable `ExitCode` enum

## requirements.py

Responsible for:

- command dependency declarations
- readiness validation inputs

## confirmation.py

Responsible for:

- safe confirmation helper

## version.py

Responsible for:

- version metadata retrieval
- version result model

---

56. Public Package Exports

The vibe.cli package shall export only stable public framework symbols.

Expected exports:

from vibe.cli.application import CLIApplication
from vibe.cli.context import CLIContext
from vibe.cli.errors import (
    CLIError,
    CLICommandError,
    CLIInitializationError,
    CLIInternalError,
    CLIOutputError,
    CLIServiceUnavailableError,
    CLIUnsupportedOperationError,
)
from vibe.cli.exit_codes import ExitCode
from vibe.cli.output.renderer import OutputRenderer
from vibe.cli.requirements import CommandRequirements

Do not export:

* internal Typer command registration helpers
* mutable application state
* private serializers
* test utilities
* internal error-mapping tables

⸻

57. Root Callback Behavior

The Typer root callback shall process framework-level options before operational command execution.

The callback shall support:

* diagnostic mode
* output mode
* version display where implemented as an eager option
* no-command help behavior
* invocation context initialization

The root callback shall not perform full platform bootstrap merely to display:

* help
* version
* shell completion
* argument errors

Operational command handlers shall request platform initialization through the shared application execution mechanism.

⸻

58. Command Execution Wrapper

Implement a centralized command execution wrapper.

All operational command handlers shall execute through this wrapper rather than duplicating:

* Bootstrap initialization
* context creation
* readiness checks
* logging
* exception mapping
* rendering
* shutdown

Expected conceptual interface:

def execute_command(
    handler: Callable[[CLIContext], CommandResult],
    *,
    requirements: CommandRequirements,
) -> int:
    ...

The exact form may differ according to Typer integration requirements.

The wrapper shall:

1. establish invocation correlation
2. initialize Bootstrap where required
3. construct CLIContext
4. validate required service readiness
5. execute the command handler
6. render the result
7. map errors
8. shut down Bootstrap
9. return the final exit code

⸻

59. Command Handler Rules

Command handlers shall:

* accept parsed and typed CLI parameters
* obtain services through CLIContext
* call approved public service APIs
* return CommandResult or an approved equivalent
* avoid direct process termination
* avoid direct service construction
* avoid direct configuration parsing
* avoid direct log-handler access
* avoid business logic belonging to services
* avoid writing raw output outside the renderer abstraction

Handlers shall remain small and independently testable.

⸻

60. Deferred Command Implementation

Deferred commands shall use a shared implementation helper.

Expected conceptual behavior:

def unavailable_command(
    *,
    command_name: str,
    required_service: str,
    governing_iwp: str,
) -> CommandResult:
    ...

The result shall clearly identify:

* the command invoked
* the required future service
* the governing future IWP
* that no operation was performed

Example human-readable output:

Command unavailable: repository status
Required service: Repository Service
Planned implementation: IWP-009
No repository changes were made.

Deferred handlers shall not duplicate this formatting independently.

⸻

61. Core Command Registration

The core command module shall register:

version
init
doctor
build

Only version shall be fully operational in IWP-007.

The others shall be deterministic command shells unless their framework-only behavior can be completed without implementing future service responsibilities.

init shall not create repository structures in IWP-007.

doctor shall not perform health diagnostics in IWP-007.

build shall not invoke implementation or AI workflows in IWP-007.

⸻

62. Configuration Command Group

The configuration command module shall register:

config show
config validate
config set
config unset

Because Configuration Service already exists, Cursor shall inspect its approved public API.

Only functionality directly supported by the committed Configuration Service contract may be exposed operationally.

The CLI shall not add configuration mutation capabilities to the Configuration Service merely to satisfy a command shell.

Where the public service supports read-only export:

* config show may be implemented using the approved export API
* secret values shall be redacted
* JSON output may be supported

Where validation is already an approved Configuration Service operation:

* config validate may invoke that operation

config set and config unset shall remain unavailable unless the Configuration Service contract explicitly supports persistent mutation.

⸻

63. Architecture Command Group

The architecture command module shall register:

arch list
arch validate
arch status
arch baseline

All architecture operations shall remain deferred until the Repository and Validation services are available.

The CLI shall not:

* scan architecture files directly
* parse architecture packages directly
* modify approved architecture
* create architecture baselines
* approve architecture changes

arch baseline shall explicitly state that human approval and the appropriate future service are required.

⸻

64. Validation Commands

The validation command module shall register:

validate

The command shell shall explain that the Validation Framework is required.

The command shall not perform ad hoc validation using CLI code.

The governing package is:

IWP-008 – Validation Framework

⸻

65. Repository Command Groups

The repository command module shall register both:

repository
repo

repository is canonical.

repo is a compatibility alias.

Both shall expose:

status
validate
docs

Alias behavior shall use the same underlying handlers.

The CLI shall not independently inspect Git, filesystem structure, or architecture documents in IWP-007.

The governing package is:

IWP-009 – Repository Service

⸻

66. Prompt Command Groups

The prompt command module shall register both:

prompts
prompt

prompts is canonical.

prompt is a compatibility alias.

Approved subcommands:

list
show
validate
render

All shall remain deferred until:

IWP-010 – Prompt Management Service

The CLI shall not load, parse, render, or validate prompt files directly.

⸻

67. AI Command Groups

The AI command module shall register:

ai providers
ai models
ai plan
ai implement
providers

The top-level providers command shall be a compatibility alias for the relevant AI provider listing behavior.

All AI commands shall remain deferred until:

IWP-011 – AI Provider Layer

The CLI shall not:

* call external AI APIs
* inspect provider credentials directly
* select models independently
* implement retry behavior
* implement provider adapters

⸻

68. MCP Command Group

The MCP command module shall register:

mcp list
mcp validate
mcp doctor

All MCP commands shall remain deferred until:

IWP-012 – MCP Client Service

The CLI shall not:

* create MCP network connections
* inspect MCP server processes
* parse MCP configuration independently
* implement transport behavior

⸻

69. CLI Configuration Consumption

CLI-specific settings shall be obtained through the Configuration Service.

Potential approved settings may include:

cli:
  output: text
  color: true
  diagnostic: false

If these settings are absent, use deterministic framework defaults.

Command-line arguments shall override configuration for the current invocation only.

The CLI shall not persist root-option values back to configuration.

⸻

70. Sensitive Configuration Output

Where config show is operational, output shall be redacted.

The CLI shall reuse approved redaction behavior from the Logging Service where publicly available.

If the Logging Service redactor is not a public contract, implement a CLI output sanitizer using the same approved sensitive-key rules without importing private logging internals.

Sensitive keys include, at minimum:

password
passwd
pwd
secret
token
api_key
apikey
authorization
auth
private_key
certificate
credential

Redaction shall be recursive and case-insensitive.

⸻

71. Serialization Requirements

JSON serialization shall support:

* primitive scalar values
* dictionaries with string keys
* lists
* tuples
* immutable dataclasses
* enums
* timezone-aware datetime values
* paths where necessary

Datetime values shall be rendered as ISO-8601 strings.

Enums shall be rendered using stable values or names according to the approved public contract.

Unsupported values shall raise CLIOutputError.

Do not silently serialize arbitrary service instances.

⸻

72. Deterministic Output

Framework output shall be deterministic.

This includes:

* JSON key ordering
* service-list ordering
* command-list ordering where controlled
* table-row ordering
* error formatting
* version field ordering
* deferred-command wording

Do not depend on unordered collection iteration.

⸻

73. Service Readiness Validation

Implement a reusable readiness validator.

For every declared required service, validate:

1. the service is present in Service Registry
2. the service has a lifecycle record
3. the service is in READY
4. the service is not health-failed where health information exists

A service in DEGRADED may be allowed only where the command requirement explicitly permits degraded operation and the architecture allows it.

The default shall require READY.

Readiness failure shall raise CLIServiceUnavailableError.

⸻

74. Application State

The CLI application may maintain process-lifetime internal state required for one invocation.

Allowed internal state includes:

* Typer application instance
* injected Bootstrap Service
* current invocation context
* shutdown flag
* current correlation identifier

State shall:

* be protected where concurrent access is possible
* be cleared after invocation
* not leak between repeated test executions
* not be exposed as mutable public state

⸻

75. Thread Safety

ARP-002-01 requires thread safety.

The CLI shall ensure that:

* application initialization is protected
* command registration is not repeated unsafely
* invocation state is not shared across concurrent invocations
* output rendering remains consistent
* shutdown is not executed concurrently more than once per invocation

The CLI is primarily process-oriented, but its public classes shall not contain unsafe unprotected mutable state.

Use locks only where necessary.

Do not overcomplicate the design with asynchronous execution.

⸻

76. Testing Strategy

Cursor shall implement:

* unit tests
* CLI invocation tests
* integration tests
* packaging entry-point tests where practical

Tests shall use Typer’s approved testing utilities.

Preferred approach:

from typer.testing import CliRunner

Tests shall not invoke the developer’s installed global 86vibe command.

Tests shall execute the repository application directly or through the built package in isolated integration tests.

⸻

77. Unit Testing Requirements

Minimum unit test coverage shall include the following.

77.1 Exit Codes

Verify every ExitCode value:

* success
* general error
* invalid arguments
* configuration error
* validation failure
* runtime failure
* external service failure
* internal platform error
* user cancellation

77.2 Error Mapping

Verify mapping for:

* Configuration Service exceptions
* Bootstrap exceptions
* Service Registry exceptions
* Lifecycle Manager exceptions
* invalid arguments
* user cancellation
* unknown exceptions

77.3 Output Renderer

Verify:

* informational output
* success output
* warning output
* error output
* table output
* JSON output
* ANSI suppression
* deterministic JSON ordering
* unsupported serialization failure

77.4 Command Requirements

Verify:

* Bootstrap-required commands
* Bootstrap-free commands
* required service declarations
* JSON support declarations
* destructive declarations

77.5 Readiness Validation

Verify:

* required service ready
* missing service
* failed service
* stopped service
* degraded service
* unsupported lifecycle state

77.6 Version Metadata

Verify:

* installed package metadata
* development fallback
* Python version
* architecture version where available
* deterministic build fallback
* JSON representation

77.7 Confirmation

Verify:

* explicit approval
* explicit rejection
* safe default
* non-interactive behavior
* JSON-mode behavior
* cancellation behavior

77.8 Deferred Commands

Verify:

* nonzero exit code
* required service message
* governing IWP message
* no file mutation
* deterministic output

⸻

78. CLI Invocation Testing Requirements

Using CliRunner, verify:

86vibe
86vibe --help
86vibe --version
86vibe version
86vibe version --output json

Also verify help for:

config
arch
repository
repo
prompts
prompt
ai
mcp

Verify:

* unknown command behavior
* unknown option behavior
* missing required argument behavior
* diagnostic-mode behavior
* JSON error behavior
* exit codes
* standard output versus standard error

⸻

79. Bootstrap Integration Tests

Integration tests shall use the real committed platform services.

Verify:

1. an operational command initializes Bootstrap
2. the CLI obtains Configuration Service
3. the CLI obtains Logging Service
4. the CLI obtains Service Registry
5. the CLI obtains Lifecycle Manager
6. readiness validation succeeds
7. command execution completes
8. Bootstrap shutdown is called
9. no service remains in an invalid state

Use temporary directories and temporary configuration where required.

Do not write to the developer’s real .86vibe directory.

⸻

80. Failure Integration Tests

Integration tests shall verify:

* Configuration initialization failure
* Logging initialization failure
* Bootstrap failure
* missing Service Registry entry
* lifecycle service not ready
* command handler failure
* output-rendering failure
* Bootstrap shutdown failure
* user interruption

Each failure shall produce:

* expected exit code
* expected user-facing category
* no unredacted secrets
* deterministic stream output
* appropriate log behavior where available

⸻

81. Entry-Point Testing

Where practical, verify the configured package script resolves successfully.

The test shall confirm that the configured entry-point target:

* imports successfully
* invokes the CLI application
* terminates with the expected exit code

Do not require a system-wide installation.

Use repository packaging or metadata tools where available.

⸻

82. Help Snapshot Stability

Help output is part of the user-facing contract.

Tests shall verify the presence of:

* root description
* required global options
* required command names
* command-group subcommands
* aliases

Avoid brittle full-screen snapshots where Typer version formatting may differ.

Assert stable semantic elements rather than terminal-width-dependent whitespace.

⸻

83. Test Isolation

Every CLI test shall isolate:

* environment variables
* current working directory
* configuration paths
* log directories
* invocation context
* correlation identifiers
* Bootstrap instances
* registry state
* lifecycle state

Tests shall not depend on execution order.

Tests shall not leave files or mutable global state behind.

⸻

84. Test Directory Layout

Follow the repository conventions established by IWP-001.

Expected logical layout:

tests/
├── unit/
│   └── cli/
│       test_application.py
│       test_context.py
│       test_errors.py
│       test_exit_codes.py
│       test_output_renderer.py
│       test_requirements.py
│       test_confirmation.py
│       test_version.py
│       test_deferred_commands.py
│
└── integration/
    └── cli/
        test_cli_invocation.py
        test_cli_bootstrap.py
        test_cli_failures.py
        test_cli_entry_point.py

Follow current repository naming conventions where they differ.

⸻

85. Documentation Requirements

Every public CLI API shall include docstrings describing:

* purpose
* parameters
* returns
* raised exceptions
* lifecycle behavior
* output behavior
* thread-safety expectations where relevant

Command help shall serve as user-facing documentation.

Update repository documentation only where necessary to describe:

* installing the CLI
* invoking 86vibe
* displaying help
* displaying version information
* understanding deferred commands

Do not document future command behavior as already available.

⸻

86. Dependency Requirements

Use only architecture-approved dependencies.

Expected CLI dependencies:

* Typer
* Rich
* Click transitively through Typer
* existing platform dependencies

Before adding dependencies:

1. inspect pyproject.toml
2. reuse existing versions or constraints
3. avoid duplicate dependency declarations
4. preserve supported Python version
5. preserve packaging conventions

Do not add:

* argparse-based parallel framework
* Fire
* Cement
* third-party DI containers
* custom shell frameworks

⸻

87. Python Standards

Implementation shall comply with ARP-001-05.

Requirements include:

* Python 3.11+
* complete type hints
* public docstrings
* immutable result models where appropriate
* enums for exit codes and error categories
* descriptive naming
* no wildcard imports
* no hidden mutable globals
* deterministic behavior
* explicit exception handling
* small focused handlers
* repository formatting conventions

⸻

88. Security Requirements

The CLI shall never expose:

* passwords
* API keys
* access tokens
* bearer tokens
* private keys
* certificates
* decrypted secrets
* authorization headers
* raw sensitive configuration
* unredacted command arguments known to contain credentials

Security requirements apply to:

* terminal output
* JSON output
* diagnostic output
* logs
* exception messages
* completion reports generated by command execution

Diagnostic mode does not disable redaction.

⸻

89. Repository Quality Gates

Before completion, Cursor shall run all repository quality gates established by IWP-001.

At minimum, run the equivalent of:

ruff check .
ruff format --check .
mypy .
pytest

Also run any packaging or build checks defined in pyproject.toml.

Where available, verify:

python -m build

or the repository’s approved equivalent.

No quality-gate failures are permitted.

⸻

90. Regression Requirements

All tests from the committed implementation baseline shall continue to pass.

This includes tests for:

* Repository Foundation
* Configuration Service
* Logging Service
* Bootstrap Service
* Service Registry
* Service Lifecycle Manager

The CLI implementation shall not introduce initialization side effects that break direct service tests.

⸻

91. Acceptance Criteria

IWP-007 shall be complete only when all criteria below are satisfied.

91.1 Functional Acceptance Criteria

* 86vibe executable configured.
* CLIApplication implemented.
* Typer root application implemented.
* Root help implemented.
* Version command implemented.
* Version root option implemented where supported.
* Text output implemented.
* JSON output implemented.
* Diagnostic mode implemented.
* Stable exit codes implemented.
* Centralized error mapping implemented.
* CLI context implemented.
* Output renderer implemented.
* Command requirements implemented.
* Service readiness validation implemented.
* Bootstrap integration implemented.
* Service Registry integration implemented.
* Lifecycle Manager integration implemented.
* Command groups registered.
* Compatibility aliases registered.
* Deferred commands fail deterministically.
* Safe shutdown implemented.
* User interruption handled.

91.2 Architectural Acceptance Criteria

* No architecture redesign introduced.
* Typer used as approved.
* CLI remains a thin adapter.
* No future service business logic implemented.
* No duplicate Bootstrap path introduced.
* No duplicate Service Registry introduced.
* No direct configuration-file parsing introduced.
* No direct Python logging usage introduced in commands.
* No dynamic plugin registration introduced.
* Public command names preserved.
* Exit-code contract preserved.
* Package dependency rules preserved.

91.3 Engineering Acceptance Criteria

* Public APIs fully typed.
* Public APIs documented.
* Unit tests implemented.
* CLI invocation tests implemented.
* Integration tests implemented.
* Entry-point configuration verified.
* JSON output is valid and deterministic.
* Sensitive values are redacted.
* Existing IWP-001 through IWP-006 tests pass.
* Repository quality gates pass.
* Package build passes where configured.

⸻

92. Deliverables

This implementation package shall deliver:

* CLI application package
* installed 86vibe entry point
* Typer application
* command context
* command registration modules
* root options
* version command
* deferred command shells
* command aliases
* output renderer
* JSON serializer
* exit-code enum
* CLI exception hierarchy
* error mapper
* command requirement model
* readiness validation
* confirmation helper
* Bootstrap integration
* Service Registry integration
* Lifecycle Manager integration
* unit tests
* integration tests
* packaging updates
* concise documentation updates

No unrelated platform functionality shall be delivered.

⸻

93. Files Expected to Change

Exact files depend on the committed baseline.

Expected additions or modifications include:

src/vibe/cli/
tests/unit/cli/
tests/integration/cli/
pyproject.toml

Potential updates may include:

src/vibe/__init__.py
README.md
docs/

Only update these where required by repository conventions or acceptance criteria.

Do not modify future service packages.

⸻

94. Commit Guidance

Create one focused implementation commit.

Suggested commit message:

feat(cli): implement IWP-007 CLI Framework

Do not combine unrelated refactoring.

Do not include:

* generated caches
* local .86vibe data
* log files
* IDE metadata
* local virtual environments
* developer-specific configuration

⸻

95. Completion Report

Upon completion, Cursor shall provide the following report.

IWP-007 Completion Report
Implementation Summary
----------------------
Brief description of the completed CLI Framework.
Architecture Compliance
-----------------------
- ARP-002-01 CLI Service Interface Contract: PASS
- AEP-002-04 CLI Specification: PASS
- AEP-002-13 Build & Packaging Specification: PASS
- AEP-002-16 Platform Bootstrap Sequence Specification: PASS
- AEP-002-17 Service Lifecycle & Dependency Management Specification: PASS
- ARP-002-16 Platform Service Contract Compliance Matrix: PASS
- ARP-001-05 Engineering Coding Standards: PASS
Command Surface
---------------
Operational Commands:
- ...
Deferred Command Groups:
- ...
Aliases:
- ...
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
CLI Invocation Tests:
- Passed
Integration Tests:
- Passed
Entry-Point Verification:
- Passed
Quality Checks:
- Ruff: PASS
- Formatting: PASS
- MyPy: PASS
- PyTest: PASS
- Package Build: PASS or NOT CONFIGURED
Exit-Code Verification
----------------------
- Success: PASS
- Invalid Arguments: PASS
- Configuration Error: PASS
- Validation Failure: PASS
- Runtime Failure: PASS
- External Service Failure: PASS
- Internal Platform Error: PASS
- User Cancellation: PASS
Security Verification
---------------------
- Sensitive output redaction: PASS
- JSON output contains no ANSI codes: PASS
- Diagnostic mode preserves redaction: PASS
Implementation Notes
--------------------
Document any implementation assumptions explicitly permitted by the approved architecture.
Known Limitations
-----------------
List deferred command behavior only. Do not describe planned functionality as implemented.
Ready For Review
----------------
YES

⸻

96. Definition of Done

This implementation package is complete when:

* all functional acceptance criteria are satisfied
* all architectural acceptance criteria are satisfied
* all engineering acceptance criteria are satisfied
* the 86vibe entry point resolves
* root help works
* version output works in text and JSON modes
* invalid invocations return stable exit codes
* operational framework paths initialize and shut down the platform safely
* deferred commands fail clearly without modifying files
* all unit tests pass
* all CLI invocation tests pass
* all integration tests pass
* all baseline tests pass
* all repository quality gates pass
* packaging checks pass where configured
* implementation introduces no architectural drift
* implementation is ready for review and merge into 86-vibe-cli

No functionality beyond the scope of IWP-007 shall be implemented.

⸻

97. Cursor Execution Instructions

Before writing code, Cursor shall:

1. Inspect the current implementation baseline from IWP-001 through IWP-006.
2. Review every governing architecture document referenced by this specification.
3. Confirm the current package structure.
4. Confirm the Bootstrap Service public API.
5. Confirm the Service Registry public API and canonical service identifiers.
6. Confirm the Service Lifecycle Manager public API and readiness states.
7. Confirm the Configuration and Logging service APIs.
8. Inspect pyproject.toml and existing dependencies.
9. Implement the CLI Framework strictly within this specification.
10. Run all repository quality, test, entry-point, and packaging checks.
11. Produce the required completion report.
12. Stop.

Do not begin IWP-008.

⸻
