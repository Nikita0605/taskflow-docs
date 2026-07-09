# Work Item Model

| Document Information | |
|----------------------|------------------------------------------------|
| Product | TaskFlow |
| Version | 1.0 |
| Document Type | Architecture Specification |
| Audience | Solution Architects, Software Engineers, Product Managers, Technical Writers |
| Owner | Product Documentation Team |
| Status | Draft |
| Last Updated | July 2026 |

---

# 1. Purpose

The Work Item Model defines the structure of all work items managed within TaskFlow.

It specifies the supported work item types, their common attributes, ownership, relationships, lifecycle, and extension model. The objective is to establish a consistent representation of work across planning, development, testing, reporting, automation, and release management.

All TaskFlow modules that create, manage, or reference work items must conform to this model.

---

# 2. Work Item Architecture

A work item is the primary entity used to represent and track work within TaskFlow.

Every work item inherits a common set of system properties and participates in a shared lifecycle. Individual work item types extend the base model with additional fields, workflows, validation rules, and business behaviour.

The platform treats all work items as first-class entities, allowing them to participate in search, reporting, notifications, automation, permissions, and integrations.

---

# 3. Supported Work Item Types

TaskFlow supports the following work item types.

| Work Item | Purpose |
|-----------|---------|
| Epic | Represents a large business initiative that spans multiple user stories. |
| User Story | Represents a functional requirement that delivers business value. |
| Task | Represents implementation work required to complete a user story. |
| Subtask | Represents a smaller unit of work within a task. |
| Bug | Represents a defect identified during development, testing, or production. |
| Spike | Represents research or technical investigation required before implementation. |

Each work item type defines its own workflow while inheriting the common work item model.

---

# 4. Hierarchy Model

TaskFlow supports hierarchical relationships between planning and implementation work items.

```text
Epic
│
└── User Story
      │
      └── Task
             │
             └── Subtask
```

Hierarchy represents ownership.

Each child work item belongs to a single parent.

Hierarchy is used for:

- Progress calculation
- Roll-up reporting
- Sprint planning
- Capacity planning
- Work decomposition

Bugs are not part of the hierarchy because they can be reported independently or linked to existing work items.

---

# 5. Common Properties

All work items inherit the following properties.

| Property | Description |
|-----------|-------------|
| Work Item ID | Unique system-generated identifier. |
| Title | Short description of the work item. |
| Description | Detailed information about the work. |
| Status | Current workflow state. |
| Priority | Business priority assigned to the work item. |
| Assignee | User responsible for implementation. |
| Reporter | User who created the work item. |
| Labels | Classification tags. |
| Created Date | Timestamp when the work item was created. |
| Updated Date | Timestamp of the most recent update. |

Individual work item types may define additional fields without modifying the base model.

---

# 6. Identity Model

Every work item is uniquely identified within a project.

TaskFlow generates immutable identifiers using a type-specific prefix.

| Work Item | Identifier |
|-----------|------------|
| Epic | EP-001 |
| User Story | US-001 |
| Task | TASK-001 |
| Subtask | SUB-001 |
| Bug | BUG-001 |
| Spike | SPK-001 |

Identifiers are never reused.

---

# 7. Relationship Model

In addition to hierarchy, work items may be connected through logical relationships.

These relationships do not establish ownership.

| Relationship | Description |
|--------------|-------------|
| Blocks | Prevents another work item from progressing. |
| Blocked By | Indicates that another work item must be completed first. |
| Relates To | Associates two work items without creating a dependency. |
| Duplicates | Identifies duplicate work items. |
| Duplicated By | Indicates another work item represents the same issue. |
| Implements | Associates implementation work with a user story. |
| Tests | Associates a test activity with implementation work. |
| Fixes | Associates development work with a reported defect. |

Relationship integrity is maintained automatically by the platform.

---

# 8. Ownership Model

Every work item belongs to exactly one project.

```text
Workspace
    │
    ▼
Project
    │
    ▼
Work Item
```

A work item cannot exist outside a project.

Ownership determines permissions, reporting boundaries, workflow configuration, and notification scope.

---

# 9. Lifecycle Model

Every work item progresses through a lifecycle.

The base lifecycle consists of four logical states.

```text
Created
    │
    ▼
Active
    │
    ▼
Completed
    │
    ▼
Archived
```

Each work item type extends this lifecycle with its own workflow.

For example:

- User Stories introduce **Ready** and **In Review**.
- Tasks introduce **Code Review** and **Testing**.
- Bugs introduce **Triaged**, **Resolved**, and **Verified**.

---

# 10. Release Association

A release is not a work item hierarchy.

Instead, releases reference completed work items that are delivered together.

```text
Release 2.1

├── Epic EP-03
├── User Story US-142
├── User Story US-146
├── Bug BUG-18
└── Task TASK-287
```

A single release may include work from multiple epics, user stories, and defects.

Similarly, an epic may span multiple releases.

---

# 11. Dependency Model

Dependencies define execution order between work items.

TaskFlow evaluates dependencies during sprint planning, workflow transitions, release validation, and project reporting.

Dependency information is used to identify blocked work, calculate delivery risk, and visualize implementation sequencing.

---

# 12. Extensibility

The Work Item Model supports custom work item types.

Custom types inherit:

- Identity management
- Activity history
- Permissions
- Search indexing
- Notifications
- Automation support
- Reporting
- Audit history

Extensions may introduce custom fields, workflows, validation rules, and business logic without modifying the base work item architecture.

---

# 13. Model Constraints

The following constraints apply to all work items.

| ID | Constraint |
|----|------------|
| WM-001 | Every work item belongs to one project. |
| WM-002 | Every work item has one unique identifier. |
| WM-003 | A child work item can have only one parent. |
| WM-004 | Circular parent-child relationships are not permitted. |
| WM-005 | Relationship references must point to existing work items. |
| WM-006 | Archived work items remain available for reporting and audit history. |
| WM-007 | Every work item must have one active workflow state. |

---

# 14. Related Documents

| Document | Purpose |
|----------|---------|
| Product Model | Defines the conceptual structure of TaskFlow. |
| Navigation Model | Defines how work items are accessed. |
| Workflow Model | Defines workflow behaviour for each work item type. |
| Permission Model | Defines authorization rules for work item operations. |
| Integration Model | Defines external integrations involving work items. |
