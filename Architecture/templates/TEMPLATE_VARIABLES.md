# 86-Vibe Implementation Template Variable Dictionary

| Item | Value |
|---|---|
| Library Version | 1.0.1 |
| Correction Type | Non-structural variable-dictionary erratum |
| Frozen Templates Reconciled | 12 |
| Unique Template Variables | 177 |
| Template Changes | None |

---

# 1. Purpose

This document defines every placeholder variable used by the frozen 86-vibe Implementation Specification templates.

Template Library Version 1.0 contained a dictionary defect: the twelve frozen templates used 177 unique variables, while the dictionary defined only 41. Version 1.0.1 corrects the dictionary only. No template structure, numbering, formatting, wording, or placeholder has been changed.

Variables SHALL be populated only from approved architecture and governance sources. No implementation behavior may be invented.

If the value of a required variable cannot be derived from the cited approved source for the current work package, generation SHALL stop and report the unresolved architectural or governance input.

---

# 2. Authoritative Resolution Sources

Variables SHALL be resolved from the following approved sources, using the most specific applicable document:

1. Applicable AP-001 product and governance architecture.
2. Applicable AP-002 platform and subsystem architecture.
3. Applicable ARP-001 readiness and engineering standards.
4. Applicable ARP-002 public service contract and any approved architecture-completion addendum.
5. Applicable approved ADR.
6. The approved IWP roadmap and sequencing authority for identifiers, titles, predecessors, and companion-document composition.

Implementation code and unapproved prior implementation documents SHALL NOT be used to create architectural values.

---

# 3. Variable Dictionary

## 3.1 Shared and Document-Control Variables

| Variable | Description | Source |
|---|---|---|
| {{INTERNAL_COMPONENTS}} | Internal components definition and requirements | Applicable ARP-002 service contract and approved service architecture addendum; applicable AP-002 specification |
| {{IWP_ID}} | Current Implementation Work Package identifier | Approved IWP roadmap and sequencing authority (AEP-001-12, AEP-001-16, applicable ADR) |
| {{MASTER_DOCUMENT_ID}} | Identifier of the parent master implementation specification | Approved IWP roadmap and sequencing authority (AEP-001-12, AEP-001-16, applicable ADR) |
| {{MASTER_DOCUMENT_TITLE}} | Title of the master implementation specification | Approved IWP roadmap and sequencing authority (AEP-001-12, AEP-001-16, applicable ADR) |
| {{MODELS_DOCUMENT_TITLE}} | Title of the models implementation specification | Approved IWP roadmap and sequencing authority (AEP-001-12, AEP-001-16, applicable ADR) |
| {{REPOSITORY_NAME}} | Repository in which the implementation is delivered | AEP-001-08; AEP-002-02 |
| {{SERVICE_DOCUMENT_TITLE}} | Title of the service implementation specification | Approved IWP roadmap and sequencing authority (AEP-001-12, AEP-001-16, applicable ADR) |
| {{SERVICE_NAME}} | Canonical name of the service being implemented | Applicable ARP-002 service contract and approved service architecture addendum; applicable AP-002 specification |
| {{WORK_PACKAGE}} | Full approved Implementation Work Package name | Approved IWP roadmap and sequencing authority (AEP-001-12, AEP-001-16, applicable ADR) |

## 3.2 Master Specification Variables

| Variable | Description | Source |
|---|---|---|
| {{ACCEPTANCE_CRITERIA}} | Approved acceptance criteria | Applicable AP-002 specification; applicable ARP-002 contract/addendum; AEP-001-18 |
| {{ADR_LIST}} | Applicable approved Architecture Decision Records | Approved ADR register and applicable ADR documents |
| {{ARCHITECTURE_DOCUMENT_LIST}} | Complete list of governing AP, ARP, and ADR documents | Applicable AP, ARP, and ADR documents; ARP-001-01 |
| {{AVAILABILITY_REQUIREMENTS}} | Mandatory availability requirements | Applicable AP-002 specification and ARP-002 service contract/addendum; AEP-001-14 |
| {{BOOTSTRAP_USAGE}} | Required bootstrap usage integration and usage rules | AEP-002-16; ARP-002-09; applicable ARP-002 service contract/addendum |
| {{CODING_STANDARDS}} | Mandatory engineering and implementation coding standards | ARP-001-05 |
| {{COMPANION_DOCUMENT_LIST}} | Approved companion implementation specifications within the work package | Approved IWP roadmap and sequencing authority (AEP-001-12, AEP-001-16, applicable ADR) |
| {{COMPLETION_REQUIREMENTS}} | Mandatory completion requirements | AEP-001-18; ARP-001-04; ARP-001-05; applicable ARP-002 contract |
| {{CONCURRENCY_REQUIREMENTS}} | Mandatory concurrency requirements | AEP-002-17; applicable ARP-002 service contract/addendum |
| {{CONFIGURATION_USAGE}} | Required configuration usage integration and usage rules | AEP-002-05; ARP-002-02; applicable ARP-002 service contract/addendum |
| {{DELIVERABLES}} | Deliverables definition and requirements | AEP-001-18; ARP-001-04; ARP-001-05; applicable ARP-002 contract |
| {{DEPENDENCY_LIST}} | Approved predecessor work packages and implementation dependencies | Approved IWP roadmap and sequencing authority (AEP-001-12, AEP-001-16, applicable ADR) |
| {{DOCUMENTATION_REQUIREMENTS}} | Mandatory documentation requirements | ARP-001-05; AEP-001-18; applicable AP-002 and ARP-002 documents |
| {{HEALTH_REQUIREMENTS}} | Mandatory health requirements | Applicable AP-002 specification and ARP-002 service contract/addendum; AEP-001-14 |
| {{IMPLEMENTATION_BASELINE}} | Previously completed and approved implementation baseline | Approved IWP roadmap and sequencing authority (AEP-001-12, AEP-001-16, applicable ADR) |
| {{IMPLEMENTATION_CONSTRAINTS}} | Implementation constraints definition and requirements | Applicable AP-002 specification and ARP-002 service contract/addendum |
| {{IMPLEMENTATION_NOTES}} | Implementation notes definition and requirements | Applicable AP-002 specification and ARP-002 service contract/addendum |
| {{IMPLEMENTATION_SCOPE}} | Implementation scope definition and requirements | Applicable AP-002 specification and ARP-002 service contract/addendum |
| {{IMPLEMENTATION_SEQUENCE}} | Implementation sequence definition and requirements | Approved IWP roadmap and sequencing authority (AEP-001-12, AEP-001-16, applicable ADR) |
| {{LIFECYCLE_REQUIREMENTS}} | Mandatory lifecycle requirements | AEP-002-16; AEP-002-17; applicable ARP-002 service contract/addendum |
| {{LOGGING_REQUIREMENTS}} | Mandatory logging requirements | AEP-002-06; ARP-002-03; applicable ARP-002 service contract/addendum |
| {{LOGGING_USAGE}} | Required logging usage integration and usage rules | AEP-002-06; ARP-002-03; applicable ARP-002 service contract/addendum |
| {{MAINTAINABILITY_REQUIREMENTS}} | Mandatory maintainability requirements | Applicable AP-002 specification and ARP-002 service contract/addendum; AEP-001-14 |
| {{OUT_OF_SCOPE}} | Out of scope definition and requirements | Applicable AP-002 specification and ARP-002 service contract/addendum |
| {{PRE_IMPLEMENTATION_CHECKLIST}} | Mandatory pre-implementation checks for the implementation agent | AEP-001-18; ARP-001-04; ARP-001-05; applicable ARP-002 contract |
| {{PROHIBITED_DEPENDENCIES}} | Prohibited dependencies definition and requirements | ARP-001-03; AEP-002-17; applicable ARP-002 service contract/addendum |
| {{REGISTRY_USAGE}} | Required registry usage integration and usage rules | AEP-002-17; ARP-002-11; applicable ARP-002 service contract/addendum |
| {{RELIABILITY_REQUIREMENTS}} | Mandatory reliability requirements | Applicable AP-002 specification and ARP-002 service contract/addendum; AEP-001-14 |
| {{SERVICE_DEPENDENCIES}} | Service dependencies definition and requirements | ARP-001-03; AEP-002-17; applicable ARP-002 service contract/addendum |
| {{SERVICE_EXCLUSIONS}} | Service exclusions definition and requirements | Applicable ARP-002 service contract and approved service architecture addendum; applicable AP-002 specification |
| {{SERVICE_OWNERSHIP}} | Service ownership definition and requirements | Applicable ARP-002 service contract and approved service architecture addendum; applicable AP-002 specification |
| {{TESTABILITY_REQUIREMENTS}} | Mandatory testability requirements | Applicable AP-002 specification and ARP-002 service contract/addendum; AEP-001-14 |

## 3.3 Service Specification Variables

| Variable | Description | Source |
|---|---|---|
| {{CONFIGURATION_ITEMS}} | Configuration items definition and requirements | Applicable ARP-002 service contract and approved service architecture addendum; applicable AP-002 specification |
| {{DEPENDENCY_MANAGEMENT}} | Dependency management definition and requirements | Applicable ARP-002 service contract and approved service architecture addendum; applicable AP-002 specification |
| {{DIAGNOSTIC_REQUIREMENTS}} | Mandatory diagnostic requirements | Applicable ARP-002 service contract and approved service architecture addendum; applicable AP-002 specification |
| {{ERROR_CATEGORIES}} | Error categories definition and requirements | Applicable ARP-002 service contract/addendum; ARP-001-05 |
| {{EXTENSION_POINTS}} | Extension points definition and requirements | Applicable ARP-002 service contract and approved service architecture addendum; applicable AP-002 specification |
| {{INJECTED_DEPENDENCIES}} | Injected dependencies definition and requirements | Applicable ARP-002 service contract and approved service architecture addendum; applicable AP-002 specification |
| {{PACKAGE_STRUCTURE}} | Package structure definition and requirements | Applicable ARP-002 service contract and approved service architecture addendum; applicable AP-002 specification |
| {{PERFORMANCE_REQUIREMENTS}} | Mandatory performance requirements | Applicable AP-002 specification and ARP-002 service contract/addendum; AEP-001-14 |
| {{PROCESSING_PIPELINE}} | Processing pipeline definition and requirements | Applicable ARP-002 service contract and approved service architecture addendum; applicable AP-002 specification |
| {{PUBLIC_INTERFACE}} | Public interface definition and requirements | Applicable ARP-002 service contract and approved service architecture addendum; applicable AP-002 specification |
| {{RECOVERY_REQUIREMENTS}} | Mandatory recovery requirements | Applicable ARP-002 service contract and approved service architecture addendum; applicable AP-002 specification |
| {{REGISTRATION_REQUIREMENTS}} | Mandatory registration requirements | Applicable ARP-002 service contract and approved service architecture addendum; applicable AP-002 specification |
| {{RESOURCE_CLEANUP}} | Resource cleanup definition and requirements | Applicable ARP-002 service contract and approved service architecture addendum; applicable AP-002 specification |
| {{RESOURCE_REQUIREMENTS}} | Mandatory resource requirements | AEP-002-16; AEP-002-17; applicable ARP-002 service contract/addendum |
| {{SECURITY_REQUIREMENTS}} | Mandatory security requirements | AEP-001-04; AEP-001-13; applicable AP-002 specification and ARP-002 service contract/addendum |
| {{SERVICE_ACCEPTANCE_CRITERIA}} | Approved service acceptance criteria | Applicable ARP-002 service contract and approved service architecture addendum; applicable AP-002 specification |
| {{SERVICE_CAPABILITIES}} | Service capabilities definition and requirements | Applicable ARP-002 service contract and approved service architecture addendum; applicable AP-002 specification |
| {{SERVICE_DOCUMENTATION}} | Service documentation definition and requirements | Applicable ARP-002 service contract and approved service architecture addendum; applicable AP-002 specification |
| {{SERVICE_RESPONSIBILITIES}} | Service responsibilities definition and requirements | Applicable ARP-002 service contract and approved service architecture addendum; applicable AP-002 specification |
| {{SERVICE_STATE}} | Service state definition and requirements | Applicable ARP-002 service contract and approved service architecture addendum; applicable AP-002 specification |
| {{SERVICE_TEST_REQUIREMENTS}} | Mandatory service test requirements | Applicable ARP-002 service contract and approved service architecture addendum; applicable AP-002 specification |
| {{THREAD_SAFETY_REQUIREMENTS}} | Mandatory thread safety requirements | AEP-002-17; applicable ARP-002 service contract/addendum |

## 3.4 Engine Specification Variables

| Variable | Description | Source |
|---|---|---|
| {{ENGINE_ACCEPTANCE_CRITERIA}} | Engine acceptance criteria requirements and constraints | Applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{ENGINE_CAPABILITIES}} | Engine capabilities requirements and constraints | Applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{ENGINE_CLASSES}} | Engine classes requirements and constraints | Applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{ENGINE_COMPONENTS}} | Engine components requirements and constraints | Applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{ENGINE_CONFIGURATION}} | Engine configuration requirements and constraints | Applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{ENGINE_COORDINATION}} | Engine coordination requirements and constraints | Applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{ENGINE_DEPENDENCIES}} | Engine dependencies requirements and constraints | Applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{ENGINE_DIAGNOSTICS}} | Engine diagnostics requirements and constraints | Applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{ENGINE_DOCUMENTATION}} | Engine documentation requirements and constraints | Applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{ENGINE_DOCUMENT_TITLE}} | Title of the engine implementation specification | Approved IWP roadmap and sequencing authority (AEP-001-12, AEP-001-16, applicable ADR) |
| {{ENGINE_ERROR_CATEGORIES}} | Engine error categories requirements and constraints | Applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{ENGINE_EXECUTION_FLOW}} | Engine execution flow requirements and constraints | Applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{ENGINE_EXTENSION_POINTS}} | Engine extension points requirements and constraints | Applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{ENGINE_IMPLEMENTATION_NOTES}} | Engine implementation notes requirements and constraints | Applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{ENGINE_INTERFACES}} | Engine interfaces requirements and constraints | Applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{ENGINE_LIFECYCLE}} | Engine lifecycle requirements and constraints | Applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{ENGINE_LOGGING}} | Engine logging requirements and constraints | Applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{ENGINE_MONITORING}} | Engine monitoring requirements and constraints | Applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{ENGINE_NAME}} | Canonical name of the internal processing engine | Applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{ENGINE_PACKAGE_STRUCTURE}} | Engine package structure requirements and constraints | Applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{ENGINE_PERFORMANCE}} | Engine performance requirements and constraints | Applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{ENGINE_RECOVERY}} | Engine recovery requirements and constraints | Applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{ENGINE_RESOURCE_MANAGEMENT}} | Engine resource management requirements and constraints | Applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{ENGINE_RESPONSIBILITIES}} | Engine responsibilities requirements and constraints | Applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{ENGINE_SCHEDULING}} | Engine scheduling requirements and constraints | Applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{ENGINE_STATE}} | Engine state requirements and constraints | Applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{ENGINE_TEST_REQUIREMENTS}} | Engine test requirements requirements and constraints | Applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{ENGINE_THREAD_SAFETY}} | Engine thread safety requirements and constraints | Applicable AP-002 subsystem specification and ARP-002 service contract/addendum |

## 3.5 Models Specification Variables

| Variable | Description | Source |
|---|---|---|
| {{INTERNAL_MODELS}} | Internal models definition and requirements | AEP-002-03; applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{MODEL_ACCEPTANCE_CRITERIA}} | Model acceptance criteria requirements and constraints | AEP-002-03; applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{MODEL_CLASSIFICATION}} | Model classification requirements and constraints | AEP-002-03; applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{MODEL_COLLECTIONS}} | Model collections requirements and constraints | AEP-002-03; applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{MODEL_COMPARISON}} | Model comparison requirements and constraints | AEP-002-03; applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{MODEL_COMPATIBILITY}} | Model compatibility requirements and constraints | AEP-002-03; applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{MODEL_DATA_INTEGRITY}} | Model data integrity requirements and constraints | AEP-002-03; applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{MODEL_DEFAULTS}} | Model defaults requirements and constraints | AEP-002-03; applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{MODEL_DESERIALIZATION}} | Model deserialization requirements and constraints | AEP-002-03; applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{MODEL_DOCUMENTATION}} | Model documentation requirements and constraints | AEP-002-03; applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{MODEL_DOCUMENTATION_REQUIREMENTS}} | Model documentation requirements requirements and constraints | AEP-002-03; applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{MODEL_ENUMS}} | Model enums requirements and constraints | AEP-002-03; applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{MODEL_ERROR_STRUCTURES}} | Model error structures requirements and constraints | AEP-002-03; applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{MODEL_FACTORIES}} | Model factories requirements and constraints | AEP-002-03; applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{MODEL_IMMUTABILITY}} | Model immutability requirements and constraints | AEP-002-03; applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{MODEL_IMPLEMENTATION_NOTES}} | Model implementation notes requirements and constraints | AEP-002-03; applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{MODEL_LIST}} | Model list requirements and constraints | AEP-002-03; applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{MODEL_MAPPING}} | Model mapping requirements and constraints | AEP-002-03; applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{MODEL_NAMING}} | Model naming requirements and constraints | AEP-002-03; applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{MODEL_PACKAGE_STRUCTURE}} | Model package structure requirements and constraints | AEP-002-03; applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{MODEL_RELATIONSHIPS}} | Model relationships requirements and constraints | AEP-002-03; applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{MODEL_RESPONSIBILITIES}} | Model responsibilities requirements and constraints | AEP-002-03; applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{MODEL_SERIALIZATION}} | Model serialization requirements and constraints | AEP-002-03; applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{MODEL_SET_NAME}} | Canonical name of the model set | AEP-002-03; applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{MODEL_TEST_REQUIREMENTS}} | Model test requirements requirements and constraints | AEP-002-03; applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{MODEL_TYPES}} | Model types requirements and constraints | AEP-002-03; applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{MODEL_VALIDATION}} | Model validation requirements and constraints | AEP-002-03; applicable AP-002 subsystem specification and ARP-002 service contract/addendum |
| {{PUBLIC_DATA_CONTRACTS}} | Public data contracts definition and requirements | AEP-002-03; applicable AP-002 subsystem specification and ARP-002 service contract/addendum |

## 3.6 CLI Specification Variables

| Variable | Description | Source |
|---|---|---|
| {{CLI_ACCEPTANCE_CRITERIA}} | CLI acceptance criteria requirements and constraints | AEP-002-04; ARP-002-01; applicable ARP-002 service contract/addendum |
| {{CLI_CAPABILITIES}} | CLI capabilities requirements and constraints | AEP-002-04; ARP-002-01; applicable ARP-002 service contract/addendum |
| {{CLI_COMMAND_STRUCTURE}} | CLI command structure requirements and constraints | AEP-002-04; ARP-002-01; applicable ARP-002 service contract/addendum |
| {{CLI_COMPONENTS}} | CLI components requirements and constraints | AEP-002-04; ARP-002-01; applicable ARP-002 service contract/addendum |
| {{CLI_CONFIGURATION}} | CLI configuration requirements and constraints | AEP-002-04; ARP-002-01; applicable ARP-002 service contract/addendum |
| {{CLI_CONTEXT}} | CLI context requirements and constraints | AEP-002-04; ARP-002-01; applicable ARP-002 service contract/addendum |
| {{CLI_DIAGNOSTICS}} | CLI diagnostics requirements and constraints | AEP-002-04; ARP-002-01; applicable ARP-002 service contract/addendum |
| {{CLI_DOCUMENTATION}} | CLI documentation requirements and constraints | AEP-002-04; ARP-002-01; applicable ARP-002 service contract/addendum |
| {{CLI_DOCUMENT_TITLE}} | Title of the CLI implementation specification | Approved IWP roadmap and sequencing authority (AEP-001-12, AEP-001-16, applicable ADR) |
| {{CLI_ERROR_HANDLING}} | CLI error handling requirements and constraints | AEP-002-04; ARP-002-01; applicable ARP-002 service contract/addendum |
| {{CLI_EXECUTION_FLOW}} | CLI execution flow requirements and constraints | AEP-002-04; ARP-002-01; applicable ARP-002 service contract/addendum |
| {{CLI_EXIT_CODES}} | CLI exit codes requirements and constraints | AEP-002-04; ARP-002-01; applicable ARP-002 service contract/addendum |
| {{CLI_EXTENSION_POINTS}} | CLI extension points requirements and constraints | AEP-002-04; ARP-002-01; applicable ARP-002 service contract/addendum |
| {{CLI_IMPLEMENTATION_NOTES}} | CLI implementation notes requirements and constraints | AEP-002-04; ARP-002-01; applicable ARP-002 service contract/addendum |
| {{CLI_INPUT_VALIDATION}} | CLI input validation requirements and constraints | AEP-002-04; ARP-002-01; applicable ARP-002 service contract/addendum |
| {{CLI_INTERACTIVE}} | CLI interactive requirements and constraints | AEP-002-04; ARP-002-01; applicable ARP-002 service contract/addendum |
| {{CLI_JSON_OUTPUT}} | CLI json output requirements and constraints | AEP-002-04; ARP-002-01; applicable ARP-002 service contract/addendum |
| {{CLI_LOGGING}} | CLI logging requirements and constraints | AEP-002-04; ARP-002-01; applicable ARP-002 service contract/addendum |
| {{CLI_OPTIONS}} | CLI options requirements and constraints | AEP-002-04; ARP-002-01; applicable ARP-002 service contract/addendum |
| {{CLI_OUTPUT_FORMATS}} | CLI output formats requirements and constraints | AEP-002-04; ARP-002-01; applicable ARP-002 service contract/addendum |
| {{CLI_OUTPUT_RENDERING}} | CLI output rendering requirements and constraints | AEP-002-04; ARP-002-01; applicable ARP-002 service contract/addendum |
| {{CLI_PACKAGE_STRUCTURE}} | CLI package structure requirements and constraints | AEP-002-04; ARP-002-01; applicable ARP-002 service contract/addendum |
| {{CLI_PROGRESS_REPORTING}} | CLI progress reporting requirements and constraints | AEP-002-04; ARP-002-01; applicable ARP-002 service contract/addendum |
| {{CLI_REGISTRATION}} | CLI registration requirements and constraints | AEP-002-04; ARP-002-01; applicable ARP-002 service contract/addendum |
| {{CLI_RESPONSIBILITIES}} | CLI responsibilities requirements and constraints | AEP-002-04; ARP-002-01; applicable ARP-002 service contract/addendum |
| {{CLI_SECURITY}} | CLI security requirements and constraints | AEP-002-04; ARP-002-01; applicable ARP-002 service contract/addendum |
| {{CLI_SERVICE_INVOCATION}} | CLI service invocation requirements and constraints | AEP-002-04; ARP-002-01; applicable ARP-002 service contract/addendum |
| {{CLI_TEST_REQUIREMENTS}} | CLI test requirements requirements and constraints | AEP-002-04; ARP-002-01; applicable ARP-002 service contract/addendum |
| {{CLI_USAGE}} | CLI usage requirements and constraints | AEP-002-04; ARP-002-01; applicable ARP-002 service contract/addendum |

## 3.7 Testing and Acceptance Variables

| Variable | Description | Source |
|---|---|---|
| {{ACCEPTANCE_TEST_SUITE}} | Required acceptance test suite | AEP-002-12; ARP-001-05; AEP-001-18; applicable ARP-002 contract/addendum acceptance criteria |
| {{BOUNDARY_TESTS}} | Required boundary tests | AEP-002-12; ARP-001-05; AEP-001-18; applicable ARP-002 contract/addendum acceptance criteria |
| {{CODE_QUALITY_VERIFICATION}} | Code quality verification definition and requirements | AEP-002-12; ARP-001-05; AEP-001-18; applicable ARP-002 contract/addendum acceptance criteria |
| {{COMPLETION_REPORT}} | Required implementation completion report contents | AEP-001-18; ARP-001-04; ARP-001-05; applicable ARP-002 contract |
| {{CONCURRENCY_TESTS}} | Required concurrency tests | AEP-002-12; ARP-001-05; AEP-001-18; applicable ARP-002 contract/addendum acceptance criteria |
| {{CONFIGURATION_TESTS}} | Required configuration tests | AEP-002-12; ARP-001-05; AEP-001-18; applicable ARP-002 contract/addendum acceptance criteria |
| {{DEFINITION_OF_DONE}} | Mandatory definition-of-done conditions | AEP-001-18; ARP-001-05 |
| {{DOCUMENTATION_VERIFICATION}} | Documentation verification definition and requirements | AEP-002-12; ARP-001-05; AEP-001-18; applicable ARP-002 contract/addendum acceptance criteria |
| {{END_TO_END_TESTS}} | Required end to end tests | AEP-002-12; ARP-001-05; AEP-001-18; applicable ARP-002 contract/addendum acceptance criteria |
| {{ERROR_TESTS}} | Required error tests | AEP-002-12; ARP-001-05; AEP-001-18; applicable ARP-002 contract/addendum acceptance criteria |
| {{FAILURE_SCENARIOS}} | Failure scenarios definition and requirements | AEP-002-12; ARP-001-05; AEP-001-18; applicable ARP-002 contract/addendum acceptance criteria |
| {{FINAL_ACCEPTANCE_CRITERIA}} | Approved final acceptance criteria | AEP-002-12; ARP-001-05; AEP-001-18; applicable ARP-002 contract/addendum acceptance criteria |
| {{FINAL_VALIDATION}} | Final validation activities required before approval | AEP-002-12; ARP-001-05; AEP-001-18; applicable ARP-002 contract/addendum acceptance criteria |
| {{INTEGRATION_TESTS}} | Required integration tests | AEP-002-12; ARP-001-05; AEP-001-18; applicable ARP-002 contract/addendum acceptance criteria |
| {{INTERFACE_VALIDATION}} | Interface validation definition and requirements | AEP-002-12; ARP-001-05; AEP-001-18; applicable ARP-002 contract/addendum acceptance criteria |
| {{MOCK_IMPLEMENTATIONS}} | Mock implementations definition and requirements | AEP-002-12; ARP-001-05; AEP-001-18; applicable ARP-002 contract/addendum acceptance criteria |
| {{PERFORMANCE_TESTS}} | Required performance tests | AEP-002-12; ARP-001-05; AEP-001-18; applicable ARP-002 contract/addendum acceptance criteria |
| {{RECOVERY_TESTS}} | Required recovery tests | AEP-002-12; ARP-001-05; AEP-001-18; applicable ARP-002 contract/addendum acceptance criteria |
| {{REGRESSION_TESTS}} | Required regression tests | AEP-002-12; ARP-001-05; AEP-001-18; applicable ARP-002 contract/addendum acceptance criteria |
| {{RESOURCE_VALIDATION}} | Resource validation definition and requirements | AEP-002-12; ARP-001-05; AEP-001-18; applicable ARP-002 contract/addendum acceptance criteria |
| {{SECURITY_TESTS}} | Required security tests | AEP-002-12; ARP-001-05; AEP-001-18; applicable ARP-002 contract/addendum acceptance criteria |
| {{TEST_ACCEPTANCE_CRITERIA}} | Testing acceptance criteria requirements and constraints | AEP-002-12; ARP-001-05; AEP-001-18; applicable ARP-002 contract/addendum acceptance criteria |
| {{TEST_ARCHITECTURE}} | Testing architecture requirements and constraints | AEP-002-12; ARP-001-05; AEP-001-18; applicable ARP-002 contract/addendum acceptance criteria |
| {{TEST_DATA}} | Testing data requirements and constraints | AEP-002-12; ARP-001-05; AEP-001-18; applicable ARP-002 contract/addendum acceptance criteria |
| {{TEST_DOCUMENT_TITLE}} | Title of the testing and acceptance implementation specification | Approved IWP roadmap and sequencing authority (AEP-001-12, AEP-001-16, applicable ADR) |
| {{TEST_IMPLEMENTATION_NOTES}} | Testing implementation notes requirements and constraints | AEP-002-12; ARP-001-05; AEP-001-18; applicable ARP-002 contract/addendum acceptance criteria |
| {{TEST_OBJECTIVES}} | Testing objectives requirements and constraints | AEP-002-12; ARP-001-05; AEP-001-18; applicable ARP-002 contract/addendum acceptance criteria |
| {{TEST_SCOPE}} | Testing scope requirements and constraints | AEP-002-12; ARP-001-05; AEP-001-18; applicable ARP-002 contract/addendum acceptance criteria |
| {{UNIT_TESTS}} | Required unit tests | AEP-002-12; ARP-001-05; AEP-001-18; applicable ARP-002 contract/addendum acceptance criteria |

---

# 4. Resolution Rules

1. Every placeholder present in a selected frozen template SHALL have exactly one dictionary entry in this document.
2. Every placeholder SHALL be replaced before an implementation specification is issued.
3. Values SHALL be derived from the most specific applicable approved source listed in the Source column.
4. A source mapping identifies where a value must be obtained; it does not authorize invention where the cited source is silent.
5. A variable may resolve to an explicit architectural statement that a capability is unsupported, prohibited, not applicable, caller-driven, or intentionally absent.
6. A generic template variable SHALL be resolved against the architecture of the current work package and document type.
7. If two approved sources conflict, the approved superseding ADR or later explicit architecture addendum SHALL govern.
8. If no approved source resolves a required value, document generation SHALL stop and report the exact variable and missing authority.

---

# 5. Reconciliation Result

- Frozen templates inspected: **12**
- Unique placeholders found: **177**
- Dictionary entries after correction: **177**
- Frozen templates modified: **0**
- Duplicate dictionary variables: **0**
- Undefined template placeholders after correction: **0**

The mandatory variable `{{CODING_STANDARDS}}` is defined as follows:

| Variable | Description | Source |
|---|---|---|
| {{CODING_STANDARDS}} | Mandatory engineering and implementation coding standards | ARP-001-05 |

---

# 6. Unresolved-Authority Assessment

No variable is intrinsically unresolvable at the Template Library level. Each variable now has an authoritative source category or specific governing document.

This does not guarantee that every future work package architecture contains a concrete value for every selected template variable. During generation, each variable SHALL still be resolved against the current approved AP, ARP, ADR, and roadmap baseline. If a cited source is silent for a required variable, generation SHALL stop and report that variable rather than inventing behavior.

For IWP-016, ARP-002-15 together with ARP-002-17 provides the service-specific architectural authority required by the frozen templates.

---

# 7. Template Integrity Statement

This correction is a non-structural dictionary erratum issued as Template Library Version 1.0.1.

The following frozen template properties remain unchanged:

- file set
- document structure
- section numbering
- heading hierarchy
- formatting
- wording
- placeholders
- document ordering

---

# 8. Future Dictionary Governance

A new template variable SHALL NOT be added unless it is introduced by:

- an approved architecture change;
- an approved ADR; or
- an approved new or revised frozen template.

Any future template release SHALL include an automated reconciliation confirming that the set of placeholders used by all frozen templates exactly equals the set of variables defined by this dictionary.

---

# END OF FILE
