# Work Item Model

| Document Information | |
|----------------------|------------------------------------------------|
| Product | TaskFlow |
| Version | 1.0 |
| Document Type | Architecture Specification |
| Audience | Solution Architects, Software Engineers, Technical Writers, Product Managers |
| Owner | Product Documentation Team |
| Status | Draft |
| Last Updated | July 2026 |

---

# 1. Purpose

The Work Item Model defines the standard representation of work within the TaskFlow platform.

It establishes the work item types supported by the product, the relationships between those types, and the common metadata used throughout planning, development, testing, reporting, and release management.

All planning modules, project workflows, reports, notifications, and integrations reference the Work Item Model.

---

# 2. Work Item Architecture

A work item represents a trackable unit of work.

Every work item shares a common structure regardless of its type. Specialized work items extend the base model by introducing additional properties and workflow behaviour without changing the underlying architecture.

This approach provides a consistent data model while allowing different work item types to support different business processes.

---

# 3. Work Item Hierarchy

TaskFlow organizes work items using a hierarchical model.

```text
Epic
│
├── User Story
│     │
│     ├── Task
│     ├── Bug
│     └── Subtask
│
└── Release
```

Each level represents a different planning granularity.

Higher-level work items define business objectives, while lower-level work items represent implementation and verification activities.

---

# 4. Work Item Types

TaskFlow supports multiple work item types.

| Work Item | Purpose |
|------------|---------|
| Epic | Represents a large business objective or initiative. |
| User Story | Captures a functional requirement from the user's perspective. |
| Task | Represents implementation work assigned to an individual contributor. |
| Bug | Records a software defect requiring investigation or resolution. |
| Subtask | Represents work that forms part of a parent task. |
| Release | Groups completed work items delivered in a product release. |

Each work item type defines its own workflow while inheriting the common work item structure.

---

# 5. Common Properties

All work items inherit a common set of properties.

| Property | Description |
|-----------|-------------|
| Work Item ID | System-generated unique identifier. |
| Title | Short description of the work item. |
| Description | Detailed information about the work. |
| Status | Current workflow state. |
| Priority | Business priority assigned to the work item. |
| Assignee | User responsible for completing the work. |
| Reporter | User who created the work item. |
| Labels | Classification tags. |
| Created Date | Date and time the work item was created. |
| Updated Date | Date and time of the most recent modification. |

Individual work item types may introduce additional properties without modifying the base model.

---

# 6. Identity Model

Every work item is uniquely identified within a project.

TaskFlow generates immutable identifiers using a type-specific prefix followed by a sequential number.

| Work Item | Identifier Format |
|------------|------------------|
| Epic | EP-001 |
| User Story | US-001 |
| Task | TASK-001 |
| Bug | BUG-001 |
| Release | REL-001 |

Identifiers are never reused, even after a work item is deleted or archived.

---

# 7. Relationship Model

Work items may be linked to establish planning, implementation, or dependency relationships.

TaskFlow supports the following relationship types.

| Relationship | Description |
|--------------|-------------|
| Parent | Defines hierarchical ownership. |
| Child | Represents work contained within a parent item. |
| Blocks | Prevents another work item from progressing. |
| Blocked By | Indicates a dependency on another work item. |
| Relates To | Associates work items without creating a dependency. |
| Duplicates | Identifies duplicate records. |
| Fixes | Associates implementation work with a reported defect. |
| Implements | Connects implementation tasks with business requirements. |

Relationship integrity is maintained automatically when linked work items are modified.

---

# 8. Ownership Model

Every work item belongs to a single project.

Ownership is resolved using the following hierarchy.

```text
Workspace
    │
    ▼
Project
    │
    ▼
Work Item
```

A work item cannot belong to multiple projects.

Moving work between projects creates a new project association while preserving activity history and audit records.

---

# 9. Lifecycle Model

Every work item progresses through a defined lifecycle.

The base lifecycle consists of four logical states.

```text
Open
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

Individual work item types extend this lifecycle by introducing additional workflow states appropriate to their business process.

---

# 10. Dependency Model

Dependencies define execution order between work items.

A dependency prevents implementation from progressing until the required work has been completed.

TaskFlow evaluates dependency relationships during sprint planning, workflow transitions, and release validation to identify blocked work.

Dependency information is also included in project reporting and board visualization.

---

# 11. Extensibility

The Work Item Model supports extension through custom work item types.

Custom types inherit the standard work item structure, including identity management, activity history, permissions, notifications, search indexing, and reporting.

Extensions may introduce additional properties, workflows, validation rules, and automation without changing the base model.

---

# 12. Model Constraints

The following constraints apply to all work items.

| ID | Constraint |
|----|------------|
| WM-001 | Every work item belongs to one project. |
| WM-002 | Every work item has a unique identifier. |
| WM-003 | Every work item has one active workflow state. |
| WM-004 | Parent-child relationships cannot form circular references. |
| WM-005 | Deleted work items retain audit history. |
| WM-006 | Relationship integrity must be maintained across all linked work items. |

These constraints ensure consistency across planning, execution, reporting, and integration services.

---

# 13. Related Documents

| Document | Purpose |
|----------|---------|
| Product Model | Defines the conceptual structure of the platform. |
| Navigation Model | Defines how work item features are accessed. |
| Workflow Model | Defines workflow behaviour for each work item type. |
| Permission Model | Defines access to work item operations. |
| Integration Model | Defines external interactions with work items. |
