# Coding Standards

## Introduction

Code consistency is not about making the codebase look uniform. It is about making implementation predictable.

A developer should be able to open any feature, understand how it is organized, and implement new functionality without introducing a different development style. Consistent code reduces review effort, simplifies debugging, and allows features to evolve independently.

This document defines the engineering standards followed throughout the TaskFlow codebase. These standards focus on implementation decisions rather than formatting conventions.

---

# 1. Organize code by feature

TaskFlow organizes source code around business capabilities instead of technical layers.

Each feature owns everything required to implement its functionality, including presentation, state management, business logic, repositories, and feature-specific models.

```text
lib/
└── features/
    ├── project/
    ├── sprint/
    ├── work_item/
    └── notification/
```

This structure provides three important benefits.

First, related code remains together. A developer working on Work Items should not navigate multiple directories to understand one workflow.

Second, changes remain localized. Most feature requests affect a single business capability instead of multiple unrelated folders.

Finally, ownership becomes obvious. When a defect is reported in Sprint planning, there should be exactly one feature responsible for implementing that behavior.

### Don't organize by technical layers

```text
lib/
├── screens/
├── widgets/
├── models/
├── repositories/
├── services/
└── utils/
```

Although this structure appears clean initially, it becomes difficult to maintain as the application grows. A single feature eventually spans multiple directories, increasing navigation cost and making ownership unclear.

During code review, ask a simple question:

> Can I understand the entire feature without leaving its directory?

If the answer is no, the implementation should be reconsidered.

---

# 2. Keep widgets focused on presentation

Widgets describe the user interface. They should not describe how the application works.

A widget has four responsibilities.

* Display application state.
* Collect user input.
* Trigger user actions.
* Render feedback.

Everything else belongs somewhere else.

For example, consider creating a Work Item.

A widget should trigger the operation.

```dart
ElevatedButton(
  onPressed: () {
    context.read<WorkItemCubit>().create(request);
  },
  child: const Text('Create'),
)
```

The widget does not validate business rules, send HTTP requests, write to local storage, publish analytics, or determine workflow transitions.

Those responsibilities belong to the feature implementation.

When widgets begin coordinating business workflows, two problems appear immediately.

The same workflow cannot be reused by another screen, and every business change requires modifying presentation code.

Widgets change frequently. Business rules should not.

---

# 3. Write business logic once

Business rules should have a single implementation.

Suppose a Work Item cannot move to **Done** unless every subtask has been completed.

That rule should exist in exactly one place.

Not in the Board screen.

Not in the Details screen.

Not in an API helper.

Not inside a widget callback.

Every duplicate implementation increases the risk of inconsistent application behavior.

When reviewing a pull request, look for repeated validation, repeated calculations, or repeated workflow decisions. These usually indicate that business logic has started leaking outside its owning feature.

A useful rule is:

> If changing one business rule requires modifications in multiple files, the implementation probably has the wrong ownership boundary.

---

# 4. Build small widgets

Large widgets are difficult to understand because they mix layout, interaction, conditional rendering, and state updates into a single class.

Split a widget when it represents a different responsibility—not because it reaches an arbitrary line count.

For example:

Instead of

```text
ProjectScreen
```

Prefer

```text
ProjectScreen
 ├── ProjectHeader
 ├── ProjectSummary
 ├── SprintList
 ├── MemberList
 └── ActivityTimeline
```

This makes rebuild boundaries explicit and allows individual components to evolve independently.

When extracting a widget, avoid passing unnecessary objects. A widget should receive only the data required to render itself.

---

# 5. Design for change

Application code changes continuously.

The goal is not to reduce change but to reduce the number of places that change.

When implementing a feature, ask the following questions before writing code.

* If the workflow changes tomorrow, which files will I modify?
* Can another feature reuse this implementation?
* Is this behavior specific to this feature or shared across the application?
* Am I solving today's requirement or introducing unnecessary abstraction?

Good implementations isolate future changes instead of spreading them across the codebase.

Developers rarely regret writing simple code.

They frequently regret writing clever code too early.
