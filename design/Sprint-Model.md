# Sprint Model

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

The Sprint Model defines how TaskFlow organizes and manages iterative development.

It establishes the structure of a sprint, its lifecycle, planning boundaries, membership, capacity, and relationship with project work items.

The Sprint Model provides a consistent framework for planning, executing, monitoring, and completing development iterations.

---

# 2. Sprint Architecture

A sprint is a time-boxed iteration within a project.

Each sprint groups a set of work items that the team plans to complete during a defined period. Sprints help organize development activities into manageable iterations while providing visibility into progress and delivery.

A sprint always belongs to a single project.

```text
Project
    │
    ▼
Sprint
    ├── Sprint Goal
    ├── Work Items
    ├── Team Members
    ├── Capacity
    └── Metrics
```

---

# 3. Sprint Components

A sprint consists of the following components.

| Component | Description |
|-----------|-------------|
| Sprint Goal | Objective to be achieved during the sprint. |
| Work Items | Planned work assigned to the sprint. |
| Team Members | Users participating in the sprint. |
| Capacity | Available effort for the sprint duration. |
| Timeline | Start and end dates of the sprint. |
| Metrics | Measurements collected during sprint execution. |

Each component contributes to sprint planning and execution.

---

# 4. Sprint Metadata

Every sprint maintains a standard set of metadata.

| Property | Description |
|----------|-------------|
| Sprint ID | Unique system-generated identifier. |
| Sprint Name | Display name of the sprint. |
| Sprint Goal | High-level objective. |
| Start Date | Date the sprint begins. |
| End Date | Date the sprint ends. |
| Duration | Planned sprint length. |
| Status | Current lifecycle state. |
| Project | Project to which the sprint belongs. |

---

# 5. Sprint Lifecycle

Every sprint progresses through a defined lifecycle.

```text
Planned
    │
    ▼
Active
    │
    ▼
Completed
```

- **Planned** – The sprint is being prepared and work is assigned.
- **Active** – Development work is in progress.
- **Completed** – Sprint execution has finished and no additional work may be added.

---

# 6. Sprint Planning

Sprint planning determines the work scheduled for the iteration.

Planning activities include:

- Defining the sprint goal.
- Selecting work items from the backlog.
- Estimating planned effort.
- Assigning work to team members.
- Reviewing team capacity.

Sprint planning establishes the scope of work before the sprint becomes active.

---

# 7. Capacity Model

Capacity represents the amount of work the team is expected to complete during the sprint.

Capacity planning considers:

| Factor | Description |
|--------|-------------|
| Team Size | Number of participating members. |
| Working Days | Available development days. |
| Individual Availability | Planned leave or reduced availability. |
| Estimated Effort | Total planned effort for selected work items. |

Capacity planning supports realistic sprint commitments.

---

# 8. Sprint Metrics

Sprint metrics measure planning accuracy and execution progress.

Common sprint metrics include:

| Metric | Description |
|--------|-------------|
| Planned Work | Total work committed at sprint start. |
| Completed Work | Total work completed during the sprint. |
| Remaining Work | Work not yet completed. |
| Sprint Velocity | Amount of completed work delivered. |
| Completion Rate | Percentage of planned work completed. |

Metric definitions remain consistent across all projects.

---

# 9. Relationships

The Sprint Model defines the relationship between a sprint and other platform entities.

| Entity | Relationship |
|--------|--------------|
| Project | Every sprint belongs to one project. |
| Work Item | Work items may be assigned to a sprint. |
| Board | Sprint progress is visualized on project boards. |
| Team | Team members participate in sprint activities. |
| Release | Completed sprint work may contribute to a release. |

---

# 10. Sprint Constraints

| ID | Constraint |
|----|------------|
| SP-001 | Every sprint belongs to one project. |
| SP-002 | A project may contain multiple sprints. |
| SP-003 | A sprint has one active lifecycle state. |
| SP-004 | Only active work items may be assigned to a sprint. |
| SP-005 | Completed sprints are read-only. |
| SP-006 | Sprint dates cannot overlap for the same iteration sequence. |

---

# 11. Related Documents

| Document | Purpose |
|----------|---------|
| Project Model | Defines the project that owns the sprint. |
| Work Item Model | Defines work items planned within a sprint. |
| Workflow Model | Defines workflow behavior during sprint execution. |
| Board Model | Defines visualization of sprint work. |
| Release Model | Defines how completed sprint work contributes to releases. |
