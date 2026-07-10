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

# 6. Follow naming conventions

Consistent naming reduces the time required to understand unfamiliar code. Names should describe purpose, not implementation details.

A developer should understand what a class, method, or variable does without opening its implementation.

## File naming

Use `snake_case` for file names.

Example:

```text
work_item_screen.dart
project_repository.dart
notification_service.dart
```

Avoid:

```text
WorkItemScreen.dart
workItemScreen.dart
workitem.dart
```

File names should represent the primary responsibility of the file.

---

## Class naming

Use `PascalCase` for classes, widgets, states, and models.

Example:

```dart
class WorkItemScreen {}

class ProjectRepository {}

class UserProfile {}
```

Avoid abbreviations unless they are commonly understood.

Prefer:

```dart
UserAuthenticationService
```

Instead of:

```dart
UserAuthSvc
```

---

## Variable and method naming

Use `camelCase` for variables, methods, and parameters.

Example:

```dart
final projectName = "TaskFlow";

void loadWorkItems() {}
```

Names should describe intent.

Prefer:

```dart
isUserAuthenticated
```

Instead of:

```dart
flag
```

Prefer:

```dart
completedTaskCount
```

Instead of:

```dart
count
```

---

## Boolean naming

Boolean values should clearly indicate a condition.

Use prefixes such as:

* `is`
* `has`
* `can`
* `should`

Example:

```dart
bool isLoading;
bool hasPermission;
bool canEdit;
```

Avoid:

```dart
bool loading;
bool permission;
```

---

# 7. Handle data, API, and state correctly

Application layers should have clear responsibilities.

A feature should follow the flow:

```text
Widget
   |
Cubit / State Management
   |
Use Case / Business Logic
   |
Repository
   |
Data Source / API
```

Each layer should communicate only with the layer directly responsible for it.

---

## Widgets should not access data sources directly

Avoid:

```dart
class WorkItemScreen extends StatelessWidget {

  Future<void> loadItems() async {
    await api.getWorkItems();
  }

}
```

The widget should only request data through the state layer.

Preferred:

```dart
context.read<WorkItemCubit>().loadWorkItems();
```

The widget displays state. It does not manage application workflows.

---

## Repository responsibilities

Repositories act as the boundary between business logic and external data sources.

Repositories are responsible for:

* Calling APIs.
* Reading local storage.
* Mapping responses into application models.
* Handling data retrieval failures.

Example:

```dart
class WorkItemRepository {

  Future<List<WorkItem>> getWorkItems() async {
    final response = await api.fetchWorkItems();

    return response.map(
      (item) => WorkItem.fromJson(item)
    ).toList();
  }

}
```

Repositories should not contain UI-related decisions.

---

## Handle errors consistently

Errors should be predictable across features.

Avoid:

```dart
catch(e) {
 print(e);
}
```

Prefer meaningful application errors.

Example:

```dart
throw NetworkException(
  "Unable to load work items"
);
```

The presentation layer should decide how errors are displayed.

Example:

```text
Repository:
"NetworkException"

Cubit:
"WorkItemLoadingFailed"

Widget:
Show error message
```

---

## Manage state explicitly

Every feature state should represent a clear application condition.

Avoid unclear states:

```dart
bool loading;
bool error;
```

Prefer meaningful states:

```text
Initial
Loading
Loaded
Empty
Failure
```

Example:

```dart
sealed class WorkItemState {}

class WorkItemLoading extends WorkItemState {}

class WorkItemLoaded extends WorkItemState {}

class WorkItemFailure extends WorkItemState {}
```

Explicit states make UI behavior predictable.

---

# 8. Maintain code quality

Readable code is easier to maintain, review, and extend.

The goal is not to write the shortest code. The goal is to write code where another developer can understand the intention quickly.

---

## Avoid duplicate logic

The same business rule should not exist in multiple places.

Avoid:

```dart
if(task.completed && task.subtasksCompleted){
   moveToDone();
}
```

inside multiple screens.

Prefer:

```dart
workItem.canMoveToDone();
```

A rule should have one owner.

---

## Keep functions focused

A function should perform one clear responsibility.

Avoid:

```dart
createProject()
```

performing:

* validation
* API calls
* analytics
* navigation
* UI updates

Prefer:

```text
validateProject()
createProject()
trackCreation()
navigateToProject()
```

Small responsibilities make code easier to test and modify.

---

## Avoid unnecessary abstraction

Not every repeated line requires a new class or framework.

Create abstractions when they provide:

* Reusability.
* Clear ownership.
* Easier maintenance.

Avoid creating abstractions only because they appear architecturally advanced.

Simple code is preferred over complicated solutions that solve problems that do not exist yet.

---

## Review checklist

Before submitting code, verify:

* Does this follow the feature-based structure?
* Is business logic outside widgets?
* Is duplicated logic removed?
* Are names clear and meaningful?
* Are errors handled consistently?
* Can another developer understand this feature without unnecessary navigation?

Code quality is a shared responsibility. Standards exist to make development predictable, not restrictive.

