# Architecture Overview

## Introduction

TaskFlow is organized around business capabilities rather than technical layers.

Instead of separating code into directories such as **screens**, **models**, **repositories**, and **services**, each feature owns the implementation required to deliver its functionality. This keeps related code together, reduces cross-feature dependencies, and makes ownership explicit.

The architecture is designed to support long-term maintainability. As the application grows, new functionality should extend an existing feature or introduce a new feature without requiring large-scale restructuring.

---

# Architectural Goals

The architecture follows four engineering goals.

### Keep features independent

Features should evolve without requiring implementation changes in unrelated areas of the application. A change to Sprint Management should not affect Project Management or Notifications unless there is a well-defined integration point.

Feature independence reduces merge conflicts, simplifies testing, and allows multiple developers to work in parallel.

---

### Keep ownership clear

Every piece of business functionality should have a single owner.

For example, the Work Item feature owns operations such as creating, updating, assigning, and changing the status of work items.

If developers need to update a business rule, they should immediately know where that implementation belongs.

Unclear ownership often leads to duplicated logic and inconsistent behavior.

---

### Keep dependencies predictable

Dependencies should always move toward shared capabilities instead of directly between features.

For example, if both Projects and Boards need date formatting, that functionality belongs in a shared utility rather than one feature depending on the other.

Predictable dependencies reduce coupling and simplify future refactoring.

---

### Optimize for change

Most software changes are incremental.

The architecture is designed so that common changes affect a single feature instead of multiple unrelated parts of the application.

Adding a new work item field, modifying sprint rules, or introducing a notification type should require changes within the owning feature instead of restructuring the application.

---

# Feature-Based Organization

The application is organized around business features.

```text id="qv2xw4"
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

Each feature contains everything required to implement its business capability.

Typical feature contents include presentation, state management, business logic, repositories, and feature-specific models.

Developers should spend most of their time working inside a single feature directory.

---

# Shared Components

Not every piece of code belongs to a feature.

Shared components provide functionality used across multiple features without owning business behavior.

Examples include:

* Theme configuration
* Reusable UI components
* Input validation helpers
* Date and time formatting
* Networking infrastructure
* Application configuration

Shared components should remain generic. Business-specific workflows should never be moved into shared modules simply because multiple features use them.

---

# Request Flow

Most user interactions follow the same execution path.

```text id="sl9jsu"
User Action
      │
      ▼
Widget
      │
      ▼
State Management
      │
      ▼
Repository
      │
      ▼
Data Source
      │
      ▼
Response
      │
      ▼
Updated State
      │
      ▼
Widget Rebuild
```

Each layer has a single responsibility.

Widgets collect user input.

State management coordinates the workflow.

Repositories provide data access.

Data sources communicate with local or remote systems.

Keeping these responsibilities separate improves testability and prevents implementation details from leaking into the user interface.

---

# Where New Code Belongs

Before creating a new class or file, identify the business capability it supports.

Ask the following questions.

* Does this solve a Project problem?
* Does this belong to Work Items?
* Is this specific to Sprint Management?
* Is this reusable across multiple features?

If the implementation belongs to one feature, place it inside that feature.

Only move code into **shared** when multiple features require the same implementation and the functionality no longer represents a business capability.

---

# Architectural Boundaries

Each feature owns its implementation.

Other features should interact through public interfaces rather than internal implementation details.

Avoid:

* Sharing feature-specific models across unrelated features.
* Importing internal classes from another feature.
* Implementing the same business rule in multiple features.
* Creating utility classes that contain feature-specific behavior.

Respecting these boundaries allows features to evolve independently without introducing hidden dependencies.

---

# Architecture in Practice

When implementing a new capability, think about ownership before implementation.

Adding a new Project workflow should extend the Project feature.

Changing Sprint planning rules should remain within the Sprint feature.

Improving shared date formatting belongs in the shared module.

The goal is not to eliminate dependencies entirely, but to make them intentional, predictable, and easy to understand.
