# Tasks

| Document Information | |
|----------------------|------------------------------------------------|
| Product | TaskFlow |
| Version | 1.0 |
| Document Type | User Guide |
| Audience | Developers, QA Engineers, Product Owners, Scrum Masters |
| Owner | Product Documentation Team |
| Status | Draft |
| Last Updated | July 2026 |

---

# Tasks

Tasks represent the implementation work required to complete a User Story, Bug, or other work item. They are assigned to individual team members and tracked throughout the development lifecycle.

Each task belongs to a project and can be associated with a sprint, parent work item, release, or development activity. TaskFlow records task progress, assignments, work logs, comments, and status changes to provide complete visibility into ongoing work.

---

# Create a Task

Create a task when work needs to be assigned to an individual contributor.

To create a task:

1. Open the required project.
2. Navigate to **Backlog**, **Sprint**, or **User Story**.
3. Select **Create** > **Task**.
4. Complete the required fields.
5. Select **Save**.

The task is immediately available for assignment and planning.

---

# Task Fields

Complete the following information when creating a task.

| Field | Description |
|---------|-------------|
| Title | Short description of the work to be completed. |
| Description | Technical or functional details required to complete the task. |
| Assignee | Team member responsible for the task. |
| Reporter | User who created the task. |
| Priority | Indicates the urgency of the work item. |
| Status | Current workflow state of the task. |
| Sprint | Sprint assigned to the task. |
| Parent | Associated User Story or Bug. |
| Labels | Categories used for filtering and reporting. |
| Due Date | Expected completion date. |
| Attachments | Supporting documents, designs, or files. |

Depending on your project configuration, additional custom fields may also be available.

---

# Assign Tasks

Each task should have a single owner.

Assigning work ensures accountability and allows project dashboards to accurately report workload, sprint progress, and team capacity.

Task ownership can be updated at any time without affecting the associated User Story or Sprint.

---

# Track Progress

Update the task status as work progresses.

TaskFlow records every workflow transition, allowing project managers and Scrum Masters to monitor implementation progress throughout the sprint.

Depending on project configuration, task updates may automatically refresh sprint boards, dashboards, reports, and team activity feeds.

---

# Log Work

TaskFlow supports work logging for teams that track implementation effort.

Work logs can be used to:

- Record time spent on development.
- Compare estimated effort with actual effort.
- Support sprint reporting.
- Review engineering effort after release.

Logged work contributes to project reporting but does not change Story Point estimates.

---

# Manage Dependencies

Tasks often depend on other work items.

TaskFlow allows dependencies to be created between tasks, User Stories, and Bugs to help teams identify blocked work and coordinate implementation.

When a dependent task cannot proceed, its status can be updated to indicate that progress is blocked until the required work is completed.

---

# Development Activity

Tasks can be linked with development activity throughout the implementation process.

Depending on repository integration, a task may display:

| Activity | Description |
|----------|-------------|
| Commits | Source code changes associated with the task. |
| Pull Requests | Code reviews related to the implementation. |
| Builds | CI build status for the completed work. |
| Deployments | Deployment history linked to the task. |

These links improve traceability between project planning and software delivery.

---

# Activity History

TaskFlow maintains a complete history of task updates.

The activity history records changes such as assignments, status transitions, comments, field updates, attachments, and workflow actions. This information provides an audit trail for project collaboration and troubleshooting.

---

# Best Practices

Assign tasks to one owner to avoid ambiguity during implementation.

Keep task descriptions focused on the required work rather than business requirements, which should remain in the associated User Story.

Update task status as work progresses instead of waiting until implementation is complete. Accurate status information improves sprint reporting and helps identify delivery risks earlier.

---

# Related Topics

- User Stories
- Sprint
- Board
- Bugs
- Releases
