# User Stories

| Document Information | |
|----------------------|------------------------------------------------|
| Product | TaskFlow |
| Version | 1.0 |
| Document Type | User Guide |
| Audience | Product Owners, Scrum Masters, Developers, QA Engineers |
| Owner | Product Documentation Team |
| Status | Draft |
| Last Updated | July 2026 |

---

# User Stories

User Stories capture functional requirements from the perspective of the end user. In TaskFlow, they are used to plan features, organize sprint work, and track development from implementation through release.

A User Story belongs to a project and is managed through the project backlog until it is scheduled for a sprint. During development, stories can be linked to tasks, bugs, releases, and documentation, providing complete traceability across the software delivery lifecycle.

---

# Create a User Story

Create a User Story when a new feature, enhancement, or business requirement has been approved for development.

To create a User Story:

1. Open the required project.
2. Select **Backlog**.
3. Click **New User Story**.
4. Complete the required fields.
5. Select **Create**.

The story is added to the Product Backlog and is available for backlog refinement and sprint planning.

---

# User Story Fields

When creating or editing a User Story, TaskFlow displays the following information.

| Field | Description |
|---------|-------------|
| Title | A short summary of the requirement. |
| Description | Detailed business requirement or feature description. |
| Epic | Parent feature or business initiative. |
| Priority | Business priority assigned to the story. |
| Story Points | Relative effort estimate used during sprint planning. |
| Sprint | Sprint assigned to the story. |
| Assignee | Team member responsible for implementation. |
| Reporter | User who created the story. |
| Labels | Tags used for categorization and filtering. |
| Acceptance Criteria | Conditions that must be satisfied before the story can be completed. |
| Attachments | Supporting files such as mockups, specifications, or design documents. |

Complete these fields before moving the story into an active sprint.

---

# Estimation

Before a User Story is selected for a sprint, the team estimates the effort required to complete it.

TaskFlow supports Story Point estimation to help Scrum teams measure effort, plan sprint capacity, and monitor team velocity across multiple iterations.

Story Point estimates are stored with the User Story and remain available for reporting after the sprint is completed.

---

# Acceptance Criteria

Acceptance Criteria define the expected behaviour of a User Story.

These criteria are used by developers during implementation and by QA engineers during validation. A story should not be marked as complete until every acceptance criterion has been satisfied.

For example:

| ID | Acceptance Criterion |
|----|----------------------|
| AC-01 | Users can request a password reset using a registered email address. |
| AC-02 | Password reset links expire after 30 minutes. |
| AC-03 | Users receive a confirmation after successfully changing their password. |

Well-defined acceptance criteria reduce ambiguity and improve testing consistency.

---

# Linking Related Work

User Stories rarely exist on their own.

TaskFlow allows you to create relationships between User Stories and other project artifacts, making it easier to understand dependencies and track implementation progress.

A User Story can be linked to:

| Work Item | Purpose |
|------------|---------|
| Epic | Groups related User Stories under a larger business objective. |
| Task | Represents implementation work required to complete the story. |
| Bug | Records defects discovered during development or testing. |
| Sprint | Identifies the sprint in which the story is planned. |
| Release | Tracks the product version that delivers the completed feature. |

Maintaining these relationships improves traceability throughout the development lifecycle.

---

# Search and Filter User Stories

As projects grow, locating specific User Stories becomes increasingly important.

TaskFlow provides filtering and search options based on common project attributes, including:

- Status
- Sprint
- Epic
- Assignee
- Priority
- Labels
- Reporter

Filters can be combined to create focused views for sprint planning, backlog refinement, or release preparation.

---

# User Story Actions

The actions available for a User Story depend on your project permissions.

Common actions include:

| Action | Description |
|---------|-------------|
| Edit | Update story information. |
| Assign | Change ownership of the story. |
| Move | Transfer the story to another sprint or backlog. |
| Link | Associate the story with other work items. |
| Watch | Receive notifications when the story changes. |
| Archive | Remove completed or obsolete stories from active views. |

---

# Best Practices

Write User Stories that describe a single business outcome rather than multiple unrelated requirements.

Keep acceptance criteria specific and measurable so they can be verified during testing. Estimate stories only after the team has reviewed the requirements, and avoid assigning stories to a sprint before backlog refinement is complete.

Review linked work items regularly to ensure dependencies remain accurate throughout development.

---

# Related Topics

- Backlog
- Sprint
- Task
- Bug
- Epic
- Release
