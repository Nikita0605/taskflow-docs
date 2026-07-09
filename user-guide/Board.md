# Board

| Document Information | |
|----------------------|------------------------------------------------|
| Product | TaskFlow |
| Version | 1.0 |
| Document Type | User Guide |
| Audience | Developers, QA Engineers, Scrum Masters, Project Managers |
| Owner | Product Documentation Team |
| Status | Draft |
| Last Updated | July 2026 |

---

# Board

The Board provides a visual representation of work as it progresses through the project workflow. It enables teams to monitor implementation status, manage work items, identify delivery risks, and coordinate development activities throughout the sprint.

Each card on the board represents a work item, such as a User Story, Task, or Bug. As work progresses, cards move across workflow columns, providing a real-time view of project execution.

The board automatically reflects updates made by project members, ensuring that sprint progress, work distribution, and workflow status remain synchronized across the project.

---

# Board Layout

The board is organized into workflow columns that represent different stages of implementation.

A typical Scrum workflow includes:

```text
Backlog
    │
    ▼
To Do
    │
    ▼
In Progress
    │
    ▼
Code Review
    │
    ▼
Testing
    │
    ▼
Done
```

Each column contains work items currently assigned to that stage of the workflow. Additional columns can be introduced to support organization-specific development processes.

Project Administrators can customize workflow states without affecting historical sprint data.

---

# Work Item Cards

Each work item is displayed as an individual card containing the information required to track implementation progress.

A card may include:

| Property | Description |
|----------|-------------|
| Work Item ID | Unique identifier for the work item. |
| Title | Summary of the work. |
| Assignee | Current owner of the work item. |
| Priority | Business or technical priority. |
| Story Points | Estimated implementation effort. |
| Labels | Categorization tags. |
| Status | Current workflow state. |
| Sprint | Associated sprint. |

Selecting a card opens the work item where additional information, comments, linked items, attachments, activity history, and development details are available.

---

# Moving Work Through the Workflow

As implementation progresses, work items move between workflow states.

TaskFlow supports drag-and-drop workflow transitions directly from the board. Status changes update the associated work item and are immediately reflected across sprint reports, dashboards, and project metrics.

Workflow transitions may be controlled by project rules. For example, a task cannot move from **In Progress** directly to **Done** if mandatory review or testing stages have not been completed.

These validation rules help maintain a consistent development process across the project.

---

# Filter the Board

Large projects often contain hundreds of active work items.

TaskFlow provides filtering options that allow teams to focus on a specific subset of work without modifying the underlying board.

Available filters include:

| Filter | Description |
|---------|-------------|
| Sprint | Display work assigned to a specific sprint. |
| Assignee | Display work owned by a selected team member. |
| Status | Display work in selected workflow states. |
| Priority | Display work based on business priority. |
| Labels | Display categorized work items. |
| Epic | Display work associated with a specific Epic. |

Filters can be combined to create focused views for sprint planning, stand-up meetings, and release preparation.

---

# Work in Progress Limits

TaskFlow supports configurable Work in Progress (WIP) limits for workflow columns.

A WIP limit defines the maximum number of work items that should remain in a workflow stage at one time. When the configured limit is exceeded, the affected column is highlighted, allowing the team to identify bottlenecks before they impact sprint delivery.

WIP limits encourage teams to complete existing work before starting additional tasks, improving workflow efficiency and reducing context switching.

---

# Automation Rules

Routine workflow activities can be automated using board rules.

Automation rules evaluate predefined conditions and perform actions without manual intervention.

Common automation scenarios include:

| Trigger | Action |
|----------|--------|
| Pull Request merged | Move linked Task to **Done** |
| Build failed | Reopen associated Bug |
| Sprint started | Move selected work items to **In Progress** |
| Work item assigned | Notify the assignee |
| Sprint completed | Return incomplete work to the Product Backlog |

Automation reduces repetitive administrative work while ensuring workflow consistency.

---

# Development Integration

When source control integration is enabled, TaskFlow links work items with development activities.

Developers can access implementation information directly from the board without switching to external tools.

Development information may include:

| Development Activity | Description |
|----------------------|-------------|
| Commits | Source code changes linked to the work item. |
| Pull Requests | Code review requests associated with implementation. |
| Build Status | Current CI pipeline status. |
| Deployment Status | Latest deployment associated with the work item. |
| Branches | Source control branches created for implementation. |

These integrations improve traceability between project planning and software delivery.

---

# Board Metrics

The board continuously collects workflow data that contributes to project reporting.

Common metrics include:

| Metric | Description |
|---------|-------------|
| Cycle Time | Time taken for a work item to move from **In Progress** to **Done**. |
| Lead Time | Total duration from work item creation to completion. |
| Throughput | Number of completed work items during a selected period. |
| Blocked Work | Work items waiting on dependencies or approvals. |
| Velocity | Story Points completed during previous sprints. |

These metrics help teams evaluate delivery performance, identify workflow bottlenecks, and improve sprint planning accuracy.

---

# Permissions

Board operations are controlled through Role-Based Access Control (RBAC).

Depending on assigned permissions, users may be able to:

- Create work items.
- Update workflow status.
- Assign work.
- Configure workflow columns.
- Manage automation rules.
- Customize board settings.

Administrative actions are restricted to authorized users.

---

# Activity History

TaskFlow records significant board activity for every work item.

The activity history includes workflow transitions, assignment changes, comments, automation events, field updates, and linked development activities.

Maintaining a complete activity history supports auditing, troubleshooting, and project reporting throughout the software development lifecycle.

---

# Related Topics

- Sprint
- User Stories
- Tasks
- Bugs
- Dashboard
- Reports
