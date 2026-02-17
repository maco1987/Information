# ADD-ON REQUIREMENTS (High-Impact Testing Enhancements)

## A) Observability & Test Oracles (MANDATORY)

- For each Atom/UC, define the **test oracle**: what signals/logs/traces determine Pass/Fail.
- Define **timestamping & correlation** requirements for E2E evidence (especially HIL/Vehicle).
- If the oracle signals/logs are not specified in the source → mark as [OPEN] and request:
  - signal names (or placeholders), sampling rates, time sync method, log triggers.

## B) Fault Injection Matrix (MANDATORY)

Create a structured fault matrix and map each fault to test levels:

- Signal lost / timeout
- Signal stuck / frozen value
- CRC/counter errors (if mentioned)
- Bus-off / communication dropout
- ECU reset / watchdog reset
- Sensor implausibility
- Actuator not available / no response

For each fault:

- Expected reaction: [FACT|EVID] if stated, else [OPEN]
- Detection mechanism (diag/logging): [FACT|EVID] else [OPEN]
- Recommended test level(s): [DERIVATION]
- Required evidence artifacts: logs/traces + timestamps

## C) Concurrency / Arbitration / Feature Interaction (MANDATORY)

- Identify any interaction points where other functions may conflict (shared actuators, shared HMI, shared states).
- If not stated: produce an [OPEN] list + risk statement.
- Provide a "priority/override/inhibition" checklist to clarify.

## D) Release Gates & Test KPIs (MANDATORY)

Define measurable exit criteria and KPIs:

- Requirement coverage %, UC coverage %, state/mode coverage %
- Fault injection coverage (defined set vs executed)
- Defect leakage rate, flaky test rate, CI runtime budget

If targets are not specified: propose as [DERIVATION], not as facts.

## E) Variant Coverage Strategy (MANDATORY)

- Build a variant coverage matrix: Variant × UC × Tests.
- If many variants exist: propose pairwise selection as [DERIVATION].
- Split execution: CI smoke vs nightly regression vs release campaign.

## F) Reproducibility & Test Data Management (MANDATORY)

- Define required test data sets, seeds, calibration versions, environment preconditions.
- Identify non-determinism risks and mitigation (flaky tests).
- If not in source: list [OPEN] items needed for reproducibility.

## G) Power/Reset/Startup/Sleep-Wake Coverage (MANDATORY)

- Add tests for power cycles, resets, startup timing, shutdown, sleep/wake transitions.
- Any timing values must be [FACT|EVID] or [OPEN] (never guess).

## H) Evidence Pack Definition (MANDATORY)

Define an auditable evidence pack structure:

- Test report summary
- Raw logs/traces
- Configuration (SW version, calibration, environment)
- Traceability snapshot (Req↔Test↔Evidence)

Include naming conventions and retention expectations as [DERIVATION] if not specified.

## I) Requirements Quality Review (MANDATORY)

Before generating final test cases, run a "requirements quality gate":

- Ambiguity check
- Testability check (measurable acceptance criteria)
- Missing units/ranges/timing check
- Missing modes/states check

Output: a prioritized [OPEN] list and rewrite suggestions as [DERIVATION] only.
