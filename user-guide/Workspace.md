# Workspace

| Document Information | |
|----------------------|------------------------------------------------|
| Product | TaskFlow |
| Version | 1.0 |
| Document Type | User Guide |
| Audience | All Users |
| Owner | Product Documentation Team |
| Status | Draft |
| Last Updated | July 2026 |

---

# 1. About Workspaces

## Overview

A Workspace is the top-level container in TaskFlow. It provides a shared environment where teams collaborate, manage projects, track software delivery, and maintain documentation.

Every project, team member, dashboard, report, and wiki page belongs to a workspace. Think of it as the digital home for your organization or department.

For example, a company named **Nova Hub** might create a workspace called **Nova Hub**. Within that workspace, separate projects can be created for products such as a customer portal, mobile application, or internal HR system.

A user must be a member of a workspace before they can access its projects or resources.

---

## Workspace Hierarchy

A workspace organizes everything required to manage software development.

```text
Workspace
│
├── Members
├── Teams
├── Projects
│   ├── Backlog
│   ├── Sprint
│   ├── Board
│   ├── User Stories
│   ├── Tasks
│   ├── Bugs
│   └── Releases
│
├── Dashboards
├── Reports
├── Wiki
└── Settings
```

This hierarchy keeps related work together and makes it easier to manage access, collaboration, and reporting.

> **Note**
>
> A member can belong to multiple workspaces if they work across different organizations or departments.

---

# 2. Create a Workspace

Creating a workspace takes only a few minutes.

To create a new workspace:

1. Sign in to TaskFlow.
2. Select **Create Workspace**.
3. Enter the workspace name.
4. Choose the organization's time zone.
5. Select the default language.
6. Select **Create**.

After the workspace is created, you'll be redirected to the workspace home page.

> **Tip**
>
> Use a meaningful workspace name such as **Nova Hub**, **Mobile Platform Team**, or **Customer Success**. Avoid temporary names like **Test Workspace** or **Demo**.

---

# 3. Workspace Components

A workspace brings together the people, projects, and resources required to deliver software.

| Component | Description |
|----------|-------------|
| Members | Users who have access to the workspace |
| Teams | Groups of members working together |
| Projects | Individual software products or initiatives |
| Dashboards | Real-time project insights |
| Reports | Historical project data and analytics |
| Wiki | Shared documentation and knowledge |
| Settings | Workspace configuration and administration |

Each component serves a different purpose while contributing to a single collaborative environment.

---

# 4. Workspace Settings

Workspace settings allow administrators to configure and maintain the workspace.

Common settings include:

- Workspace name
- Time zone
- Language
- Branding
- Member management
- Roles and permissions
- Notification preferences
- Integrations

Some settings are available only to Workspace Administrators.

> **Best Practice**
>
> Review workspace settings before inviting team members. Configuring permissions and notification preferences early helps avoid unnecessary changes later.

---

# 5. Manage Members

Members must be added to a workspace before they can participate in projects.

To invite a new member:

1. Open the workspace.
2. Select **Members**.
3. Select **Invite Member**.
4. Enter the user's email address.
5. Assign an appropriate role.
6. Send the invitation.

The invited user receives an email with instructions for joining the workspace.

Workspace Administrators can also update member roles, deactivate accounts, or remove members when access is no longer required.

---

# 6. Workspace Roles

Every member is assigned a role that determines what they can do within the workspace.

Typical roles include:

| Role | Description |
|------|-------------|
| Workspace Administrator | Manages workspace settings, members, and permissions |
| Project Manager | Creates and manages projects |
| Scrum Master | Plans and manages sprints |
| Developer | Works on assigned tasks and user stories |
| QA Engineer | Reports and verifies bugs |
| Technical Writer | Creates and maintains documentation |
| Viewer | Read-only access |

Roles help maintain security while ensuring users have the access they need to perform their responsibilities.

---

# 7. Manage Projects

Projects are created inside a workspace.

Each project has its own backlog, sprint plan, board, releases, documentation, and reports.

A single workspace can contain multiple projects, allowing different teams to work independently while remaining part of the same organization.

For example, the **Nova Hub** workspace might contain the following projects:

- Customer Portal
- Mobile Application
- Internal HR System
- Public Website

This structure keeps work organized without requiring separate workspaces for every initiative.

---

# 8. Workspace Dashboard

The Workspace Dashboard provides an overview of activity across all projects.

Depending on your role, the dashboard may display:

- Active projects
- Sprint progress
- Team workload
- Open bugs
- Recent deployments
- Upcoming releases
- Recent activity

The dashboard helps administrators and managers understand the overall health of the workspace without opening individual projects.

---

# 9. Best Practices

The following recommendations can help you manage your workspace effectively.

- Create one workspace for each organization or business unit.
- Use projects to separate products or initiatives.
- Assign roles based on responsibilities rather than individuals.
- Remove inactive members regularly.
- Review permissions periodically.
- Keep workspace documentation up to date.
- Use consistent naming conventions for projects and teams.

Following these practices helps maintain an organized and secure workspace as your organization grows.

---

# 10. Troubleshooting

| Issue | Possible Solution |
|-------|-------------------|
| Unable to create a workspace | Verify that your account has permission to create workspaces. |
| Invitation email not received | Ask the user to check their spam folder or resend the invitation. |
| Unable to access a project | Confirm that the user has been added to the workspace and assigned the appropriate role. |
| Missing workspace settings | Verify that you are signed in as a Workspace Administrator. |

---

# 11. Related Documentation

For more information, see the following guides:

- Project
- Teams
- Roles and Permissions
- User Management
- Dashboard
- Reports
- Administrator Guide
