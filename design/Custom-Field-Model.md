# Custom Field Model

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

The Custom Field Model defines how TaskFlow extends the standard work item schema through user-defined fields.

Custom fields allow organizations to capture project-specific information without modifying the platform's core data model. This model defines field types, scope, validation, visibility, and lifecycle rules for custom fields.

---

# 2. Field Model

A custom field represents a configurable attribute that can be associated with one or more work item types.

Custom fields supplement the standard work item properties and participate in data entry, filtering, searching, reporting, and automation.

A custom field is defined once and reused wherever it is applicable.

---

# 3. Field Types

TaskFlow supports multiple field types to accommodate different data requirements.

| Field Type | Description |
|------------|-------------|
| Single Line Text | Stores short text values. |
| Multi Line Text | Stores formatted or extended text. |
| Number | Stores integer or decimal values. |
| Date | Stores calendar dates. |
| Date and Time | Stores date and time values. |
| Boolean | Stores true or false values. |
| Single Select | Allows selection of one predefined option. |
| Multi Select | Allows selection of multiple predefined options. |
| User | References a platform user. |
| Label | References one or more project labels. |

Each field stores values according to its configured data type.

---

# 4. Field Definition

Every custom field is defined using a common set of properties.

| Property | Description |
|----------|-------------|
| Field Name | Display name shown to users. |
| Field Key | System identifier used internally. |
| Field Type | Data type assigned to the field. |
| Description | Optional explanation of the field's purpose. |
| Default Value | Value assigned during work item creation when applicable. |
| Required | Indicates whether a value must be provided. |
| Status | Active or inactive state of the field. |

Field definitions remain independent of individual work items.

---

# 5. Scope

Custom fields are assigned within a defined scope.

Supported scopes include:

| Scope | Description |
|--------|-------------|
| Workspace | Available to all projects within the workspace. |
| Project | Available only within the selected project. |
| Work Item Type | Available only for specific work item types. |

Scope determines where a field can be used but does not affect its behavior.

---

# 6. Field Assignment

Custom fields are associated with one or more work item types.

Examples include:

| Work Item Type | Custom Fields |
|----------------|---------------|
| Bug | Severity, Environment |
| User Story | Story Points, Business Value |
| Task | Estimated Hours |
| Epic | Target Release |

Field assignment controls availability during work item creation and editing.

---

# 7. Validation Rules

Validation rules ensure that field values meet defined requirements.

Supported validation includes:

- Required values
- Minimum and maximum numeric values
- Minimum and maximum text length
- Allowed option values
- Date restrictions
- Unique value constraints

Validation is performed before changes are committed.

---

# 8. Visibility

Field visibility controls where a custom field is presented.

A field may be visible in:

- Work item forms
- Backlog views
- Board cards
- Search filters
- Reports
- Exported datasets

Visibility settings affect presentation only and do not alter stored data.

---

# 9. Field Lifecycle

Custom fields progress through the following lifecycle.

```text
 Created
    │
    ▼
  Active
    │
    ▼
 Inactive
```

**Created**  
The field definition has been created but is not yet available for use.

**Active**  
The field is available for data entry and reporting.

**Inactive**  
The field is retained for historical data but cannot be assigned to new work items.

Removing a field definition does not remove values stored in existing records unless explicitly configured.

---

# 10. Constraints

| ID | Constraint |
|----|------------|
| CF-001 | Every custom field has a unique field key within its scope. |
| CF-002 | Every custom field has exactly one field type. |
| CF-003 | Validation rules are enforced before data is persisted. |
| CF-004 | A field may be assigned to multiple work item types. |
| CF-005 | Inactive fields cannot be assigned to new work items. |
| CF-006 | Field values must conform to the configured data type. |

---

# 11. Relationships

| Document | Purpose |
|----------|---------|
| Work Item Model | Defines entities that use custom fields. |
| Project Model | Defines project-level field scope. |
| Board Model | Defines field visibility on board cards. |
| Permission Model | Defines access to create and manage custom fields. |
| Reporting Model | Defines use of custom fields in reports and analytics. |
