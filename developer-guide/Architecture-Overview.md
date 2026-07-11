# Architecture Overview

## Introduction

TaskFlow is organized around business capabilities rather than technical layers.

Each feature owns the code required to implement its functionality, including presentation, state management, business logic, data access, and feature-specific models. Keeping these responsibilities together makes the application easier to understand, reduces cross-feature dependencies, and establishes clear ownership throughout the codebase.

The architecture is designed to support continuous development. New features should integrate into the existing structure without requiring large-scale refactoring or changes to unrelated areas of the application.

---

## Design Philosophy

The project structure reflects the business domain instead of the Flutter framework.

Business capabilities such as Projects, Work Items, Boards, Sprints, and Notifications evolve independently. Organizing the application around these capabilities keeps implementation localized and makes feature ownership obvious during development and maintenance.

A developer working on Sprint planning should not need to understand how Notifications are implemented. Likewise, changing Work Item validation should not require modifications to Board management.

The architecture favors clear ownership over centralized implementation.

---

## Why Feature-Based Architecture

Flutter applications are commonly organized by technical layers.

```text
lib/
├── screens/
├── widgets/
├── models/
├── repositories/
└── services/
```

While this approach works well for smaller applications, feature implementation gradually becomes distributed across multiple directories. Understanding a single workflow often requires navigating large portions of the project, making ownership difficult to identify.

TaskFlow instead groups implementation by business capability.

```text
lib/
├── core/
├── features/
│   ├── project/
│   ├── sprint/
│   ├── work_item/
│   ├── board/
│   └── notification/
├── shared/
└── main.dart
```

Each feature contains everything required to implement its functionality. Developers should be able to develop, test, review, and maintain a feature without depending on unrelated parts of the application.

This organization improves navigation, simplifies code reviews, and keeps changes localized.

---

## Feature Ownership

Every business capability has a single implementation owner.

For example, the Work Item feature owns operations such as creating work items, assigning members, changing status, updating priorities, and enforcing workflow rules.

Business rules should always be implemented by the feature that owns the business process—not by another feature that happens to use the same data.

Clear ownership answers three important questions immediately.

* Where should new functionality be implemented?
* Where should defects be fixed?
* Which team is responsible for the behavior?

If ownership cannot be identified quickly, the architecture should be reconsidered before additional implementation is added.

---

## Shared Modules

Not every component belongs to a business feature.

Shared modules provide reusable technical capabilities that support the application as a whole.

Typical examples include:

* application configuration
* networking
* logging
* localization
* reusable UI components
* theme management

Shared modules should remain framework or infrastructure focused.

Business workflows should not move into shared modules simply because multiple features require similar behavior. Shared abstractions should emerge from repeated implementation patterns rather than anticipated reuse.

---

## Feature Communication

Features should remain independent while still collaborating when necessary.

Communication should occur through well-defined contracts instead of direct implementation dependencies.

```text
Project
     │
     ▼
Public Contract
     │
     ▼
Notification
```

A feature may request functionality from another feature, but it should never depend on the internal classes, state objects, or implementation details of that feature.

Keeping communication contract-based allows features to evolve independently without introducing hidden coupling.

---

## Designing New Features

Before creating a new class, determine which business capability owns the requirement.

For example, adding recurring work items extends the Work Item feature because recurring behavior changes how work items are created and managed.

Creating a new shared service simply because the implementation might be reused in the future often introduces unnecessary abstractions.

A useful design question is:

> Does this implementation represent a business capability or a reusable technical capability?

The answer should determine where the implementation belongs.

---

## Architectural Constraints

The following constraints apply throughout the application.

| Constraint                                                             | Reason                                                         |
| ---------------------------------------------------------------------- | -------------------------------------------------------------- |
| Business rules remain inside the owning feature.                       | Preserves ownership and prevents duplicated behavior.          |
| Shared modules do not implement business workflows.                    | Keeps shared code reusable across all features.                |
| Features communicate through stable contracts.                         | Prevents direct implementation dependencies.                   |
| Business logic is not implemented inside widgets.                      | Separates presentation from application behavior.              |
| New abstractions should solve existing problems, not anticipated ones. | Reduces unnecessary complexity and premature design decisions. |

These constraints establish a consistent development model and should be validated during architecture and code reviews.

---

## Architectural Testability

The architecture is designed to support feature-level testing.

Each feature should be testable in isolation by verifying its business rules, state management, and data access without requiring unrelated features to participate in the test.

Keeping dependencies explicit and ownership localized reduces the amount of setup required for unit, widget, and integration tests while improving confidence in feature-specific changes.

---

## Growing the Application

As the application evolves, new functionality should extend existing business capabilities before introducing new architectural concepts.

Most requirements should affect a single feature. When implementation regularly spans multiple unrelated features, it often indicates that ownership boundaries should be reviewed rather than introducing additional shared components.

This approach allows the architecture to scale as both the codebase and development team grow while preserving the principles of feature ownership, predictable dependencies, and localized change.

---

## Architecture at a Glance

```text
                     ┌──────────────────────────┐
                     │        Shared            │
                     │──────────────────────────│
                     │ Theme                    │
                     │ Networking               │
                     │ Logging                  │
                     │ Localization             │
                     │ Reusable Components      │
                     └───────────┬──────────────┘
                                 │
─────────────────────────────────┼─────────────────────────────────
                                 │
      Project      Sprint      Work Item      Board      Notification
          │            │            │             │               │
          ├────────────┼────────────┼─────────────┼───────────────┤
          │            │            │             │
     Presentation  State  Business Logic  Repository  Data Sources
```

The architecture promotes feature independence while allowing features to share common technical capabilities through the shared layer. Business behavior remains inside the feature that owns it, ensuring changes remain localized and responsibilities stay clear.
