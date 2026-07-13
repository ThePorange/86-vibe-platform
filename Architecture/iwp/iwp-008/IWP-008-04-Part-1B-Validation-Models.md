# 91. Normalized Document Models

The Validation Framework shall define immutable normalized document models that represent the canonical analysis form consumed by validators.

The normalized document model shall be independent of:

- Markdown parser implementation
- Repository Service implementation
- CLI rendering
- serialization format
- validator implementation
- reporting components

The primary normalized document model shall be:

```text
NormalizedDocument
```

Supporting immutable models shall include:

- DocumentHeader
- DocumentSection
- DocumentParagraph
- DocumentTable
- DocumentList
- DocumentCodeBlock
- DocumentReference
- DocumentMetadata
- DocumentAnchor
- DocumentFragment

The normalized document shall represent semantic structure rather than parser implementation details.

---

# 92. Normalized Document Contract

Every normalized document shall expose:

| Field | Type | Required |
|---|---|---:|
| document_id | DocumentId | Yes |
| target_id | ValidationTargetId | Yes |
| repository_path | Repository-relative path | Yes |
| metadata | DocumentMetadata | Yes |
| fragments | Immutable tuple of DocumentFragment | Yes |
| anchors | Immutable tuple of DocumentAnchor | Yes |
| references | Immutable tuple of DocumentReference | Yes |
| checksum | ContentDigest | Yes |

Construction shall reject:

- duplicate anchors
- malformed section hierarchy
- invalid metadata
- parser-owned mutable objects
- cyclic fragment structures

The normalized document shall not expose parser nodes.

---

# 93. Document Metadata Model

The metadata model shall contain canonical immutable metadata extracted from governed documents.

Minimum fields:

- document identifier
- document title
- document version
- owner
- approval status
- document type
- creation timestamp
- last modification timestamp

Metadata extraction belongs to the parser.

Validation belongs to validators.

The metadata model shall not infer missing values.

---

# 94. Document Fragment Hierarchy

The normalized fragment hierarchy shall represent logical document structure.

Fragment kinds shall include:

- heading
- paragraph
- table
- ordered list
- unordered list
- code block
- quote
- admonition
- horizontal rule

Fragments shall preserve canonical ordering.

Each fragment shall possess:

- fragment identifier
- parent identifier
- fragment kind
- source location
- immutable child collection

Cycles are prohibited.

---

# 95. Document Section Model

Sections shall represent the logical hierarchy.

Each section shall include:

- section number
- heading text
- hierarchy level
- parent section
- child sections
- fragment range
- anchor

Hierarchy shall satisfy:

- exactly one root
- increasing depth by one level
- no skipped hierarchy
- no cycles

---

# 96. Document Inventory Models

Repository validation requires immutable inventories.

Inventory models shall include:

```text
RepositoryInventory
DocumentInventoryEntry
```

RepositoryInventory shall expose:

- repository identifier
- repository fingerprint
- immutable document collection

Each inventory entry shall expose:

- document identifier
- repository path
- checksum
- document type
- parser status

Inventory construction shall not perform validation.

---

# 97. Metadata Index

Validation Engine shall construct immutable metadata indexes.

The metadata index shall provide deterministic lookup by:

- document id
- title
- owner
- status
- version

Duplicate keys shall be represented explicitly.

Silent overwrite is prohibited.

---

# 98. Document Identifier Index

A dedicated immutable identifier index shall provide:

- lookup by DocumentId
- duplicate detection
- canonical ordering

Duplicate document identifiers shall be retained until validators determine the finding.

---

# 99. Reference Models

Reference models represent logical document references.

Reference kinds:

- document reference
- section reference
- ADR reference
- AEP reference
- ARP reference
- IWP reference
- URL reference

Each reference shall expose:

- reference id
- source document
- source fragment
- destination identifier
- source location

References shall remain unresolved until analysis.

---

# 100. Cross Reference Index

CrossReferenceIndex shall provide immutable mappings between:

- source document
- destination document
- destination section
- destination anchor

Resolution status shall remain explicit.

Missing references shall not be discarded.

---

# 101. Dependency Graph Models

Dependency analysis requires immutable graph models.

Primary models:

```text
DependencyGraph
DependencyNode
DependencyEdge
```

Edges shall identify:

- source
- destination
- dependency type

Graphs shall reject:

- duplicate edges
- self loops where prohibited
- malformed identifiers

Cycles shall remain represented.

Validators determine whether cycles are acceptable.

---

# 102. Source Location Models

Every finding shall identify its source.

Primary model:

```text
SourceLocation
```

Fields:

- repository path
- line
- column
- end line
- end column
- fragment identifier

Locations shall be immutable.

Line numbers shall be one-based.

---

# 103. Source Range

SourceRange represents contiguous regions.

Required fields:

- start location
- end location

The range shall satisfy:

- start <= end
- same document

---

# 104. Validation Finding Identifier

Each finding shall possess:

```text
ValidationFindingId
```

Identifiers shall be opaque UUIDs.

Finding identifiers shall remain stable throughout aggregation.

---

# 105. Validation Severity Enumeration

Severity shall be represented by:

```text
INFO
WARNING
ERROR
CRITICAL
```

Canonical values:

- info
- warning
- error
- critical

Ordering shall be deterministic.

---

# 106. Validation Finding Model

ValidationFinding shall expose:

- finding id
- rule id
- validator id
- severity
- category
- source location
- message
- remediation
- metadata

Findings shall be immutable.

---

# 107. Rule Outcome Model

Each validator invocation shall produce:

```text
RuleOutcome
```

Fields:

- rule execution id
- status
- findings
- elapsed duration
- diagnostics

Outcomes shall not contain mutable collections.

---

# 108. Validation Summary

ValidationSummary shall expose:

- total rules
- executed rules
- skipped rules
- info count
- warning count
- error count
- critical count

Values shall be internally consistent.

---

# 109. Validation Result

ValidationResult represents the completed engine output.

Fields:

- result id
- execution id
- target id
- summary
- findings
- rule outcomes
- timings
- diagnostics

The result shall be immutable.

---

# 110. Validation Response

ValidationResponse represents the Validation Service contract.

Fields:

- request id
- execution id
- result
- response timestamp

The response shall contain no engine internals.

---

# 111. Diagnostic Models

Diagnostic models shall represent implementation diagnostics.

Diagnostics shall never affect validation outcome.

Each diagnostic shall expose:

- code
- message
- severity
- timestamp

---

# 112. Serialization Envelope

Every serialized response shall be wrapped in:

```text
ValidationEnvelope
```

Fields:

- contract version
- timestamp
- payload

Envelope ordering shall remain deterministic.

---

# 113. Builders

Builder classes shall exist for:

- ValidationResult
- ValidationSummary
- RuleOutcome
- ValidationResponse

Builders shall remain internal.

Public callers shall receive immutable models only.

---

# 114. Factory Classes

Factories shall exist for:

- Identifier creation
- Model normalization
- Serialization

Factories shall not perform validation logic.

---

# 115. Compatibility Contracts

All public models defined in IWP-008-04 shall remain binary and serialization compatible throughout Model Contract Version 1.

Breaking changes require:

- architecture approval
- version increment
- migration strategy
- regression testing

---

# 116. Public Package Exports

The package shall export only approved public models.

Internal builders, factories and helper models shall remain private.

---

# 117. Testing Requirements

Testing shall verify:

- immutability
- equality
- hashing
- serialization
- deserialization
- identifier validation
- enum validation
- deterministic ordering
- invariant enforcement

100% model coverage is required.

---

# 118. Acceptance Criteria

Implementation shall be accepted only when:

- all immutable models compile
- all contracts conform to approved architecture
- deterministic serialization verified
- all identifiers validated
- all invariants tested
- no prohibited dependencies exist
- no mutable public state exists
- full integration passes IWP-008-06 acceptance tests

---

# 119. Deliverables

The completed implementation shall include:

- immutable model package
- enumerations
- identifier models
- request models
- response models
- execution models
- finding models
- summary models
- serialization support
- builders
- factories
- unit tests
- integration tests
- package exports
- documentation

---

# 120. Part 1B Completion

Part 1B completes the Validation Models specification covering:

- normalized document models
- inventories
- indexes
- references
- dependency models
- source location models
- finding models
- response models
- serialization
- builders
- factories
- testing
- acceptance
- deliverables

The Validation Models Implementation Specification (IWP-008-04) shall not be considered complete until all companion implementation packages defined by IWP-008 have been implemented, tested, reviewed, approved and merged into the implementation baseline.
