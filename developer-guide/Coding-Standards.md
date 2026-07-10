# Coding Standards

## Keep business logic out of widgets

Widgets are responsible for rendering the current application state and forwarding user actions. They should not coordinate business workflows or make decisions that affect application behavior.

When business logic is implemented inside a widget, it becomes tied to the widget lifecycle. The same workflow cannot be reused by another screen, tested independently, or executed outside the UI. As the application grows, widgets become difficult to understand because rendering, state updates, validation, and data access are implemented in the same place.

Instead, treat a widget as an entry point. Collect the user's intent, then delegate the operation to the feature that owns the business process.

### Recommended

The widget forwards the request.

```dart
ElevatedButton(
  onPressed: () {
    context.read<WorkItemCubit>().create(request);
  },
  child: const Text('Create'),
)
```

The feature owns the workflow.

```dart
Future<void> create(CreateWorkItemRequest request) async {
  emit(const WorkItemLoading());

  final validation = validator.validate(request);

  if (!validation.isValid) {
    emit(ValidationFailed(validation.errors));
    return;
  }

  final workItem = await repository.create(request);

  emit(WorkItemCreated(workItem));
}
```

The widget does not need to know:

* how validation works,
* where data is stored,
* whether the request is cached,
* whether analytics are recorded,
* or whether additional events are published.

Its responsibility ends after forwarding the user's action.

### Avoid

```dart
onPressed: () async {
  if (titleController.text.isEmpty) {
    showError();
    return;
  }

  final response = await api.createWorkItem(...);

  await database.save(response);

  notificationService.send(...);

  analytics.track(...);

  Navigator.pop(context);
}
```

Although this implementation works, the widget now owns validation, networking, persistence, analytics, notifications, and navigation. Any future change to the workflow requires modifying the UI, increasing coupling between presentation and business logic.

### Review guidance

During code review, ask the following questions:

* Can another screen reuse this workflow without copying code?
* Would the business process continue to work if the UI changed completely?
* Is the widget making business decisions instead of forwarding user intent?
* Does the widget know more than it needs to?

If the answer to any of these questions is **Yes**, move the business workflow into the owning feature before approving the change.
