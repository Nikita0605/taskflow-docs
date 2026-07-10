# Error Handling

## Overview

Error handling is a core part of application behavior, not an implementation detail. Every error must be classified, handled consistently, and provide enough information for diagnosis without exposing internal implementation.

The objective is not to eliminate errors, but to ensure that failures are predictable, recoverable where appropriate, and observable during production.

---

# EH-001 — Classify Errors Before Handling Them

## Standard

Every failure must belong to one of the following categories.

| Category       | Description                                       | Recoverable |
| -------------- | ------------------------------------------------- | ----------- |
| Validation     | Invalid input or request format.                  | Yes         |
| Business       | A business rule prevents the requested operation. | No          |
| Network        | Communication with an external service failed.    | Yes         |
| Infrastructure | Database, storage, or platform dependency failed. | Depends     |
| Unexpected     | An unhandled runtime failure.                     | No          |

Different error categories require different handling strategies. Do not process all failures using the same implementation path.

---

# EH-002 — Handle Errors at the Correct Boundary

## Standard

Handle an error only if the current layer can recover from it.

A component that cannot recover from a failure must allow the error to propagate to the next boundary.

For example:

```text
Repository
    │
    ▼
Service
    │
    ▼
Cubit
    │
    ▼
UI
```

A repository should not display messages.

A widget should not determine database retry behavior.

Each layer owns only the error handling responsibilities appropriate to that layer.

---

# EH-003 — Preserve Error Context

## Standard

Do not replace an error with a generic exception that removes diagnostic information.

**Preferred**

```dart
throw WorkItemNotFoundException(
  workItemId: id,
);
```

**Avoid**

```dart
throw Exception('Failed');
```

The error type should describe what failed. Additional context should help engineers identify the failing operation without requiring reproduction.

---

# EH-004 — Separate User Messages from Diagnostic Information

## Standard

User-facing messages and diagnostic information serve different purposes and must remain independent.

A user message explains what happened and, where possible, what action to take.

Diagnostic information supports troubleshooting and may include identifiers, stack traces, request metadata, or service responses.

Do not expose internal implementation details directly to users.

---

# EH-005 — Do Not Ignore Exceptions

## Standard

Every exception must result in one of the following actions.

* Recover.
* Retry.
* Translate into a domain-specific failure.
* Log and propagate.

Empty catch blocks are not permitted.

**Avoid**

```dart
try {
  await repository.save(item);
} catch (_) {}
```

Silently ignoring failures produces inconsistent application state and prevents diagnosis.

---

# EH-006 — Retry Only Transient Failures

## Standard

Retries are appropriate only for temporary failures.

Examples include:

* Network interruption
* Temporary service unavailability
* Timeout

Retries must not be used for:

* Validation failures
* Authorization failures
* Business rule violations
* Programming errors

Repeated execution cannot resolve deterministic failures.

---

# EH-007 — Keep Business Rules Out of Exception Handling

## Standard

Exceptions communicate failure.

They must not become part of normal business flow.

**Avoid**

```dart
try {
  completeSprint();
} catch (_) {
  createSprint();
}
```

Business decisions should be represented explicitly through application logic instead of exception handling.

---

# EH-008 — Log Once

## Standard

An error should be logged at the boundary responsible for handling it.

Logging the same failure multiple times creates duplicate records and complicates root cause analysis.

Lower layers may enrich an error with additional context, but logging responsibility belongs to the component that determines the final outcome.

---

# EH-009 — Fail Safely

## Standard

When recovery is not possible, leave the application in a valid state.

Avoid partially completed operations.

Business state should remain unchanged when an operation cannot complete successfully.

Where atomic execution cannot be guaranteed, implement explicit rollback or compensation logic.

---

# Review Checklist

During code review, verify the following.

| Question                                                          | Expected Outcome                        |
| ----------------------------------------------------------------- | --------------------------------------- |
| Is the failure classified correctly?                              | Appropriate handling strategy selected. |
| Can this layer recover from the error?                            | If not, propagate it.                   |
| Does the error preserve diagnostic context?                       | Yes.                                    |
| Is the user message independent of implementation details?        | Yes.                                    |
| Are retries limited to transient failures?                        | Yes.                                    |
| Is the same error logged more than once?                          | No.                                     |
| Can the operation leave the application in an inconsistent state? | No.                                     |

---

# Common Violations

| Violation                                  | Why it is a problem                                                          |
| ------------------------------------------ | ---------------------------------------------------------------------------- |
| Catching every exception with `Exception`  | Removes valuable diagnostic information.                                     |
| Returning `null` to indicate failure       | Hides the actual cause and complicates error handling.                       |
| Using exceptions to control business logic | Makes application behavior difficult to understand and maintain.             |
| Logging the same exception multiple times  | Produces duplicate telemetry and obscures the root cause.                    |
| Displaying raw exception messages to users | Exposes implementation details and creates an inconsistent user experience.  |
| Ignoring exceptions                        | Leaves failures undetected and may result in inconsistent application state. |

