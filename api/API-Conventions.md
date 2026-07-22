# API Conventions

## Overview

Consistent API design improves usability, simplifies client development, and reduces integration errors. Rather than requiring developers to interpret different request patterns for each endpoint, TaskFlow APIs follow a common set of conventions for resource naming, HTTP methods, request structure, response formatting, and error reporting.

These conventions apply to all public API endpoints and should be followed whenever new services are introduced or existing endpoints are modified. Maintaining a consistent API contract allows client applications to interact with different services using predictable patterns.

---

## Resource-Oriented Endpoints

TaskFlow APIs expose business resources rather than implementation details. Endpoint paths should represent the resource being accessed and remain independent of internal application architecture.

Resource names use plural nouns and lowercase characters separated by hyphens where required.

**Examples**

```text
GET    /projects
GET    /projects/{projectId}
POST   /projects
PATCH  /projects/{projectId}
DELETE /projects/{projectId}
```

Endpoints should describe *what* is being accessed rather than *how* the operation is implemented. Avoid verbs within resource paths whenever the requested operation can be represented by an appropriate HTTP method.

---

## HTTP Method Usage

HTTP methods define the intended operation performed on a resource. Each method should be used consistently throughout the API to preserve predictable behavior.

| Method | Purpose                                                |
| ------ | ------------------------------------------------------ |
| GET    | Retrieve one or more resources without modifying data. |
| POST   | Create a new resource.                                 |
| PUT    | Replace an existing resource.                          |
| PATCH  | Apply partial updates to an existing resource.         |
| DELETE | Remove a resource.                                     |

Selecting the appropriate HTTP method improves readability and allows client applications to interact with the API using standard REST principles.

---

## Request Structure

Requests should contain only the information required to complete the requested operation. Resource identifiers are passed through the endpoint path, while optional filtering, sorting, and pagination values are supplied as query parameters.

Data required to create or update resources is provided in the request body using JSON.

Maintaining a consistent request structure simplifies validation, reduces ambiguity, and improves interoperability between services.

---

## Response Structure

Successful responses should return a consistent JSON representation of the requested resource. When additional information is required, metadata should accompany the primary response without changing the overall response structure.

Error responses should follow the standardized error model described in **Error-Handling.md**, allowing client applications to process failures consistently across all endpoints.

A predictable response format reduces client-side parsing logic and improves long-term maintainability.

---

## Naming Conventions

Field names should use a consistent naming style throughout the API.

Use descriptive property names that clearly represent the associated business concept. Abbreviations, ambiguous terminology, and implementation-specific names should be avoided.

Examples of recommended naming practices include:

* Use `projectId` instead of `id`.
* Use `createdAt` instead of `creation_date`.
* Use `assignedUser` instead of `usr`.

Consistency in naming improves readability and minimizes misunderstandings during integration.

---

## HTTP Status Codes

HTTP status codes communicate the outcome of every request and should accurately reflect the result of the performed operation.

Successful requests return standard success codes, while client and server failures return the appropriate error status.

Common response codes include:

* **200 OK** – Request completed successfully.
* **201 Created** – Resource created successfully.
* **204 No Content** – Request completed without returning content.
* **400 Bad Request** – Invalid request data.
* **401 Unauthorized** – Authentication required or failed.
* **403 Forbidden** – Authenticated user lacks permission.
* **404 Not Found** – Requested resource does not exist.
* **500 Internal Server Error** – Unexpected server failure.

Detailed error information is documented separately in **Error-Handling.md**.

---

## Version Compatibility

API changes should preserve backward compatibility whenever possible. Existing client integrations should continue functioning without modification unless a breaking change has been explicitly introduced.

When incompatible changes become necessary, a new API version should be introduced rather than modifying the behavior of existing endpoints. Maintaining version compatibility reduces integration risk and provides consumers with sufficient time to adopt updated interfaces.

---

## Documentation Principles

Every endpoint should follow a consistent documentation structure to improve discoverability and simplify maintenance.

Endpoint documentation should clearly identify:

* the purpose of the endpoint
* supported HTTP methods
* required request parameters
* request body, when applicable
* response structure
* supported HTTP status codes
* possible error responses
* example requests and responses

Applying the same documentation format across all endpoints enables developers to understand new APIs quickly without adapting to different documentation styles.

---

## Best Practices

Design endpoints around business resources rather than implementation details.

Use HTTP methods consistently to represent resource operations.

Keep request and response structures predictable across all endpoints.

Return meaningful HTTP status codes that accurately describe request outcomes.

Maintain backward compatibility whenever introducing API enhancements.

Document every endpoint using a consistent structure and terminology.

