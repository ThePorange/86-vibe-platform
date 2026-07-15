# 13. Engine Configuration

The Health Monitoring Engine SHALL obtain all runtime configuration exclusively through the Configuration Service.

Configuration SHALL include architecture-defined settings for:

- provider execution timeout;
- readiness evaluation behaviour;
- liveness evaluation behaviour;
- health aggregation behaviour;
- evaluation scheduling parameters where defined by architecture;
- diagnostic logging configuration.

Configuration SHALL:

- be validated before use;
- remain immutable during an evaluation cycle;
- support runtime refresh where defined by architecture.

Configuration SHALL NOT:

- be hard coded;
- bypass the Configuration Service;
- duplicate configuration management logic.

---

# 14. Engine Logging

The Health Monitoring Engine SHALL integrate exclusively with the Logging Service.

Logging SHALL include:

- evaluation start;
- evaluation completion;
- provider execution;
- provider timeout;
- provider failure;
- readiness evaluation;
- liveness evaluation;
- aggregation completion;
- unexpected exceptions.

Logging SHALL remain deterministic and structured.

Sensitive information SHALL NOT be logged.

---

# 15. Error Handling

The engine SHALL implement deterministic error handling.

The engine SHALL detect and process:

- provider execution failures;
- timeout failures;
- invalid provider responses;
- aggregation failures;
- configuration failures;
- unexpected runtime exceptions.

The engine SHALL:

- isolate provider failures;
- continue processing where permitted by architecture;
- record diagnostic information;
- preserve evaluation consistency.

The engine SHALL NOT terminate platform execution unless required by the approved architecture.

---

# 16. Thread Safety

The Health Monitoring Engine SHALL support concurrent execution.

Implementation SHALL ensure:

- thread-safe evaluation processing;
- thread-safe provider execution;
- thread-safe aggregation;
- immutable evaluation results;
- deterministic concurrent behaviour.

Shared mutable state SHALL be protected using approved synchronization mechanisms.

Race conditions SHALL NOT be introduced.

---

# 17. Performance Requirements

The Health Monitoring Engine SHALL perform health evaluation efficiently.

Implementation SHALL:

- minimise provider execution overhead;
- minimise aggregation latency;
- avoid unnecessary object allocation;
- avoid repeated provider discovery;
- minimise locking duration;
- preserve deterministic execution ordering.

Performance optimisations SHALL NOT alter architectural behaviour.

---

# 18. Internal State Management

The engine SHALL maintain only architecture-defined internal state.

Internal state SHALL include:

- active evaluation context;
- provider execution results;
- aggregated health state;
- readiness calculation;
- liveness calculation.

Intermediate processing state SHALL remain internal to the engine.

State SHALL NOT persist beyond the architecture-defined lifecycle.

---

# 19. Build Requirements

Implementation SHALL:

- compile successfully;
- satisfy platform linting standards;
- satisfy static analysis requirements;
- satisfy formatting requirements;
- introduce no compiler warnings attributable to this implementation.

The implementation SHALL integrate into the existing platform build without modification of previous work packages.

---

# 20. Acceptance Criteria

The implementation SHALL be accepted when:

- provider execution operates correctly;
- readiness calculation is deterministic;
- liveness calculation is deterministic;
- aggregation produces architecture-compliant results;
- timeout handling functions correctly;
- failure isolation behaves correctly;
- configuration integration complies with architecture;
- logging integration complies with architecture;
- concurrency requirements are satisfied;
- automated tests pass successfully.

No architectural deviations SHALL be introduced.

---

# 21. Part Completion

This concludes **IWP-013-02 — Health Monitoring Engine Implementation Specification — Part 1B**.

The next document in the approved implementation sequence is:

**IWP-013-03 — Health Monitoring Models Implementation Specification — Part 1A**

---

# END OF FILE
