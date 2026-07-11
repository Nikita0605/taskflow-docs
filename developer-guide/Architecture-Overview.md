# Architecture Overview

## Introduction

TaskFlow is organized around business capabilities rather than technical layers.

Instead of separating the application into directories such as *screens*, *widgets*, *models*, or *repositories*, each feature owns the implementation required to deliver its business functionality. This approach keeps related code together, establishes clear ownership, and limits the impact of change.

The architecture is designed to make everyday development predictable. A developer working on Sprint Management should spend most of their time inside the Sprint feature without understanding the internal implementation of Boards, Notifications, or Reporting.

The architecture should make the correct implementation obvious.

---

## Design Philosophy

The structure of an application should reflect how the business works, not how the framework is organized.

Projects, Work Items, Boards, Sprints, Notifications, and Reporting represent independent business capabilities. Each capability evolves at its own pace, has its own implementation, and solves a different problem.

Organizing the codebase around these capabilities makes ownership easier to understand. When a business rule changes, there is a single place to implement that change.

This approach also reduces unnecessary coordination between teams. Developers working on different features should rarely modify the same files unless the change intentionally affects multiple capabilities.

---

## Why Feature-Based Architecture

A common way to organize Flutter applications is by technical layers.

```text
lib/
├── screens/
├── widgets/
├── models/
├── repositories/
└── services/
```

This structure works well for small applications because similar file types are grouped together.

As the application grows, however, a single feature becomes distributed across multiple directories. Implementing or debugging one workflow often requires moving between unrelated parts of the project. Ownership becomes difficult to identify because no single location represents the complete feature.

TaskFlow avoids this problem by keeping implementation within the feature that owns the business capability.

```text
lib/
└── features/
    ├── project/
    ├── sprint/
    ├── work_item/
    ├── board/
    └── notification/
```

A feature contains everything required to implement its functionality. Developers should be able to understand, modify, test, and extend a feature without navigating the entire application.

The goal is not to eliminate layers, but to keep them together within the feature that owns them.

---

## Feature Ownership

Every business capability has a single implementation owner.

For example, the Work Item feature owns operations such as creating, updating, assigning, prioritizing, and completing work items. These rules should not be implemented by Boards, Projects, or Notifications, even if those features interact with work items.

Ownership is more than file organization. It determines where business rules are implemented, where defects are fixed, and where new functionality is introduced.

When ownership is unclear, the same rule often appears in multiple locations. Over time those implementations diverge, producing inconsistent behavior that becomes increasingly difficult to maintain.

A useful question during implementation is:

> **Which feature owns this business decision?**

The answer should determine where the code belongs.

---

## Shared Code

Not every implementation belongs to a feature.

Some functionality exists to support the application rather than a specific business capability. Examples include theme configuration, reusable UI components, networking infrastructure, logging, localization, and application-wide utilities.

Shared code should provide reusable technical capabilities. It should not become a collection of business logic extracted from multiple features.

Moving code into a shared module simply because it is used twice often creates unnecessary dependencies and weakens feature ownership.

Before introducing shared code, ask whether the implementation represents a reusable technical capability or a business rule that belongs to a specific feature.

If the answer is business behavior, it should remain with the owning feature.

---

## Adding New Functionality

The first implementation decision is not *how* to build a feature, but *where* it belongs.

Consider a requirement to introduce recurring work items.

The immediate temptation might be to create a new shared service because the functionality appears reusable. A better approach is to identify the business capability responsible for recurring work items.

If recurring behavior changes how work items are created and managed, the implementation belongs to the Work Item feature. If another feature later requires the same capability, shared abstractions can be introduced based on proven usage rather than anticipated requirements.

This approach keeps the architecture stable as requirements evolve.

Code should move into shared modules because the design requires it—not because future reuse is assumed.

---

## Architectural Trade-offs

Every architectural decision introduces constraints.

Feature-based organization reduces coupling and improves ownership, but it also encourages thoughtful boundaries between features. Developers may occasionally write similar implementations in different features before deciding whether a shared abstraction is justified.

This trade-off is intentional.

TaskFlow favors clear ownership over aggressive reuse because duplicated code can be refactored safely, while incorrect abstractions often become permanent architectural constraints.

The architecture is designed to evolve through real requirements instead of speculative design. Shared solutions should emerge from repeated patterns, not from assumptions about future development.
