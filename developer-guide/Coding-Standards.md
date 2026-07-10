# Coding Standards

## Overview

This document defines the engineering standards for developing and maintaining TaskFlow.

The standards focus on code organization, ownership, implementation consistency, and long-term maintainability. They are intended to support engineering decisions during development and code review.

Formatting conventions are enforced through automated tooling and are not covered in this document.

---

# Standard 01 – Feature Ownership

## Objective

Ensure that every business capability has a single implementation owner.

## Standard

Organize code by feature instead of technical layers.

Each feature owns its:

* UI
* State management
* Business logic
* Repository contracts
* Models specific to the feature

A feature should remain independently maintainable without requiring implementation changes in unrelated features.

## Implementation

A feature module should contain everything required to implement its business capability.

```text
features/
└── work_item/
    ├── presentation/
    ├── application/
    ├── domain/
    ├── data/
    └── widgets/
```

Shared functionality should be moved only when multiple features require the same implementation.

## Review Questions

* Does the feature own its business logic?
* Is functionality duplicated across multiple features?
* Can the feature be modified without changing another feature?

---

# Standard 02 – Widget Responsibility

## Objective

Keep widgets focused on rendering and user interaction.

## Standard

Widgets must not implement business workflows.

A widget may:

* Render application state.
* Collect user input.
* Dispatch user actions.
* Display operation results.

A widget must not:

* Execute business rules.
* Access persistence directly.
* Call external services.
* Coordinate multiple business operations.

## Preferred

```dart
ElevatedButton(
  onPressed: () {
    context.read<WorkItemCubit>().create(request);
  },
  child: const Text('Create'),
)
```

## Avoid

```dart
onPressed: () async {
  final response = await api.createWorkItem(request);
  await database.save(response);
  notificationService.send(response);
}
```

## Why

Widgets rebuild frequently and should remain presentation-focused. Keeping business workflows outside the UI improves testability, reduces coupling, and keeps rendering logic predictable.

---

# Standard 03 – State Management

## Objective

Maintain a single source of truth for application state.

## Standard

State changes must originate from the owning state management component.

Widgets observe state but do not own business state.

Avoid duplicating the same business data across multiple state objects.

## Preferred

```dart
Cubit
   │
   ▼
Repository
   │
   ▼
State
   │
   ▼
Widget
```

## Avoid

```text
Widget

↓

Local variables

↓

Repository

↓

Another Widget State
```

Independent copies of the same business data eventually become inconsistent.

---

# Standard 04 – Repository Responsibilities

## Objective

Separate business workflows from data access.

## Standard

Repositories manage data retrieval and persistence.

Repositories must not:

* Perform navigation.
* Update UI state.
* Display dialogs.
* Execute presentation logic.

Repositories should return data or operation results. Business decisions remain with the calling feature.

## Review Questions

* Is the repository responsible only for data operations?
* Does the repository expose implementation details?
* Can the repository be reused without the UI?

---

# Standard 05 – Asynchronous Operations

## Objective

Keep asynchronous workflows predictable and safe.

## Standard

Every asynchronous operation must complete in one of the following states:

* Success
* Failure
* Cancellation

Always handle failures explicitly.

Avoid leaving asynchronous operations without completion handling.

Do not use `BuildContext` after an `await` unless the widget is still mounted.

## Preferred

```dart
if (!context.mounted) return;

Navigator.of(context).pop();
```

## Why

Widgets can be removed from the widget tree while an asynchronous operation is still running. Accessing an invalid `BuildContext` can result in runtime exceptions.

---

# Code Review Expectations

Every implementation should answer the following questions before approval.

| Review Area     | Verification                                         |
| --------------- | ---------------------------------------------------- |
| Ownership       | Does the correct feature own the implementation?     |
| Widget Design   | Is business logic outside the widget?                |
| State           | Is there a single source of truth?                   |
| Repository      | Does it manage data only?                            |
| Async           | Are failures and widget lifecycle handled correctly? |
| Maintainability | Does the implementation follow existing patterns?    |

Meeting these standards is part of the definition of done for all production code.
