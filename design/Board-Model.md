# Board Model

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

The Board Model defines how TaskFlow visualizes work items throughout their lifecycle.

It establishes the structure of project boards, column configuration, card representation, filtering behavior, and visualization rules used to monitor and manage project work.

The Board Model provides a consistent visual representation of work without changing the underlying workflow definitions.

---

# 2. Board Architecture

A board provides a visual representation of work items within a project.

Boards organize work into configurable columns that reflect workflow progress. They allow teams to monitor work distribution, identify bottlenecks, and manage ongoing activities without modifying the underlying work item or workflow models.

A project may contain multiple boards configured for different planning or reporting needs.

```text
Project
    │
    ▼
Board
    ├── Columns
    ├── Cards
    ├── Swimlanes
    ├── Filters
    └── Views
```

---

# 3. Board Components

A board consists of the following components.

| Component | Description |
|-----------|-------------|
| Columns | Visual grouping of work items. |
| Cards | Visual representation of individual work items. |
| Swimlanes | Horizontal grouping used to organize cards. |
| Filters | Rules that determine visible work items. |
| Views | Alternative board presentations based on selected criteria. |

Each component contributes to the visualization of project work.

---

# 4. Column Model

Columns define how work is presented on the board.

Columns are independent of workflow definitions.

Each column may represent:

- One workflow state
- Multiple workflow states
- A filtered subset of workflow states

Column configuration determines presentation only and does not modify workflow behavior.

---

# 5. Column Mapping

Board columns are mapped to workflow states.

Example:

| Board Column | Workflow States |
|--------------|-----------------|
| To Do | Draft, Ready |
| Development | In Progress, Code Review |
| Validation | Testing |
| Completed | Done |

Multiple workflow states may be represented within a single board column.

---

# 6. Card Model

Each work item is displayed as a board card.

Cards present key information without exposing the complete work item.

Typical card information includes:

| Property | Description |
|----------|-------------|
| Identifier | Unique work item reference. |
| Title | Work item summary. |
| Assignee | Current owner. |
| Priority | Business priority. |
| Labels | Classification tags. |
| Status | Current workflow state. |

Card presentation may vary according to work item type.

---

# 7. Swimlane Model

Swimlanes organize cards horizontally across the board.

Swimlanes may be configured using project-defined criteria such as:

- Assignee
- Epic
- Priority
- Team
- Custom Field

Swimlanes improve work organization without changing board structure.

---

# 8. Board Filtering

Boards support configurable filtering.

Filters determine which work items appear on the board.

Typical filtering criteria include:

- Sprint
- Assignee
- Labels
- Work Item Type
- Priority
- Workflow State

Filtering affects presentation only.

---

# 9. Work-in-Progress Limits

Boards may define Work-in-Progress (WIP) limits for selected columns.

A WIP limit specifies the recommended maximum number of work items allowed within a column.

WIP limits provide visibility into workload distribution but do not prevent work item movement unless enforced by project configuration.

---

# 10. Board Constraints

| ID | Constraint |
|----|------------|
| BM-001 | Every board belongs to one project. |
| BM-002 | Every board contains at least one column. |
| BM-003 | Board columns do not define workflow behavior. |
| BM-004 | Cards represent existing work items. |
| BM-005 | Filtering affects presentation only. |
| BM-006 | Column mappings must reference valid workflow states. |

---

# 11. Related Documents

| Document | Purpose |
|----------|---------|
| Project Model | Defines project ownership of boards. |
| Work Item Model | Defines entities represented as cards. |
| Workflow Model | Defines workflow states mapped to board columns. |
| Sprint Model | Defines sprint visualization on boards. |
