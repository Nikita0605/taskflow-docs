# Error Handling

## Overview

TaskFlow APIs communicate request failures using standard HTTP status codes and a consistent error response model. Every unsuccessful request returns an appropriate HTTP status together with a structured response body describing the reason for the failure.

A predictable error model enables client applications to distinguish between client-side validation errors, authentication failures, authorization failures, business rule violations, and unexpected server errors. Applications should always evaluate the HTTP status code before processing the response body.

Error responses are designed to provide sufficient information for troubleshooting while preventing the disclosure of internal implementation details.

---

## Error Response Model

Every API error follows a standardized response structure regardless of the endpoint that generated the failure. Using a common response model simplifies client-side error handling and allows applications to process failures consistently across the API.

A typical error response contains:

* A machine-readable error identifier.
* A descriptive error message.
* The corresponding HTTP status code.
* A unique request identifier for troubleshooting.

**Example Response**

```json
{
  "error": "ValidationError",
  "message": "The 'projectName' field is required.",
  "status": 400,
  "requestId": "8d9f8a52-3c1b-45f4-a0ef-5ab17f8e2f61"
}
```

The `requestId` uniquely identifies the request processed by the API and should be included when reporting issues to support teams.

---

## HTTP Status Codes

TaskFlow uses standard HTTP status codes to communicate the outcome of every request.

| Status Code                   | Description                                                  | Client Action                                         |
| ----------------------------- | ------------------------------------------------------------ | ----------------------------------------------------- |
| **200 OK**                    | Request completed successfully.                              | Process the response.                                 |
| **201 Created**               | Resource created successfully.                               | Continue with the returned resource.                  |
| **204 No Content**            | Operation completed successfully without a response body.    | No additional processing required.                    |
| **400 Bad Request**           | Invalid request syntax or malformed payload.                 | Correct the request before retrying.                  |
| **401 Unauthorized**          | Authentication failed or access token is missing or expired. | Re-authenticate and submit the request again.         |
| **403 Forbidden**             | Authentication succeeded, but the client lacks permission.   | Do not retry until permissions change.                |
| **404 Not Found**             | Requested resource does not exist.                           | Verify the requested endpoint or resource identifier. |
| **409 Conflict**              | Request conflicts with the current resource state.           | Resolve the conflict before retrying.                 |
| **422 Unprocessable Entity**  | Business validation failed.                                  | Correct the submitted data.                           |
| **429 Too Many Requests**     | API rate limit exceeded.                                     | Retry after the period specified by the server.       |
| **500 Internal Server Error** | Unexpected server failure.                                   | Retry if appropriate and log the failure.             |
| **503 Service Unavailable**   | Service temporarily unavailable.                             | Retry after a delay.                                  |

---

## Validation Errors

Validation occurs before business operations are executed. Requests that do not satisfy the API contract are rejected immediately without modifying application data.

Validation failures commonly result from missing required fields, invalid property values, unsupported data formats, or business rule violations.

**Request**

```http
POST /api/projects
Content-Type: application/json
Authorization: Bearer <access_token>

{
  "projectName": ""
}
```

**Response**

```http
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "error": "ValidationError",
  "message": "The 'projectName' field is required.",
  "status": 400,
  "requestId": "8d9f8a52-3c1b-45f4-a0ef-5ab17f8e2f61"
}
```

Because the request payload violates the endpoint validation rules, the resource is not created and the request is terminated before entering the business processing pipeline.

---

## Authentication and Authorization Errors

Authentication and authorization failures represent different stages of request processing and should be handled differently.

Authentication failures occur when the API cannot establish a trusted identity for the client. These requests return **401 Unauthorized** and require the client to obtain valid authentication credentials before retrying.

Authorization failures occur after successful authentication when the authenticated user does not have permission to perform the requested operation. These requests return **403 Forbidden** and should not be retried until the required permissions have been granted.

Distinguishing these two failure types allows client applications to implement appropriate recovery strategies without unnecessary retries.

---

## Server Errors

Server errors indicate that the request reached the API successfully but could not be completed because of an unexpected application or infrastructure failure.

Unlike validation or authentication failures, server errors generally do not require modifications to the original request. Instead, applications should record the failure, preserve the request identifier, and retry the request only when appropriate.

Persistent server errors should be investigated using operational logs and monitoring systems.

---

## Retry Strategy

Not every error should be retried automatically. Client applications should determine retry behavior based on the returned HTTP status code.

| HTTP Status                   | Recommended Behavior                              |
| ----------------------------- | ------------------------------------------------- |
| **400 Bad Request**           | Do not retry. Correct the request.                |
| **401 Unauthorized**          | Re-authenticate before retrying.                  |
| **403 Forbidden**             | Do not retry until permissions change.            |
| **404 Not Found**             | Verify the resource identifier before retrying.   |
| **409 Conflict**              | Resolve the conflicting resource state.           |
| **422 Unprocessable Entity**  | Correct the validation errors.                    |
| **429 Too Many Requests**     | Respect the `Retry-After` header before retrying. |
| **500 Internal Server Error** | Retry using exponential backoff.                  |
| **503 Service Unavailable**   | Retry after a short delay.                        |

Repeatedly submitting failed requests without addressing the underlying cause increases unnecessary network traffic and does not improve the likelihood of success.

---

## Logging and Diagnostics

Applications should record sufficient diagnostic information to support troubleshooting while ensuring that sensitive information is never exposed.

For failed requests, diagnostic records should include the endpoint, HTTP method, response status code, timestamp, and the request identifier returned by the API.

**Example Diagnostic Record**

```text
Endpoint    : POST /api/projects
Method      : POST
Status Code : 400
Request ID  : 8d9f8a52-3c1b-45f4-a0ef-5ab17f8e2f61
Timestamp   : 2026-07-26T10:15:42Z
```

Authentication credentials, access tokens, passwords, session identifiers, and confidential request payloads should never be written to application logs.

---

## Error Handling Best Practices

Client applications should treat HTTP status codes as the primary indicator of request outcome and use the response body to obtain additional diagnostic information. Validation should be performed before submitting requests whenever possible to reduce unnecessary API traffic, while server-side validation remains the authoritative enforcement mechanism.

Applications should retry only transient failures such as **500 Internal Server Error**, **503 Service Unavailable**, or **429 Too Many Requests**. Authentication and validation failures should be corrected before the request is submitted again.

Diagnostic information should always include the request identifier returned by the API to simplify troubleshooting and correlate client-side failures with server-side logs. Sensitive information, including authentication credentials and access tokens, should never be stored in application logs or error reports.

