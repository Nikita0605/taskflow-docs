# Pagination

## Overview

Pagination limits the number of resources returned in a single API response. Instead of returning an entire dataset, the API divides the results into smaller pages that can be retrieved sequentially.

This approach improves response time, reduces network usage, lowers server resource consumption, and provides a predictable experience for client applications consuming large collections of data. Endpoints that return lists of projects, work items, users, or other collections support pagination to ensure consistent performance regardless of dataset size.

---

## Why Pagination Is Required

As the volume of application data increases, returning every resource in a single response becomes inefficient. Large payloads increase response size, consume additional bandwidth, and require more memory for both the server and the client.

Pagination minimizes these issues by allowing clients to request only the subset of data required for the current operation. Smaller responses are processed more efficiently, resulting in improved application responsiveness and reduced infrastructure overhead.

Applications should retrieve only the records necessary for the current user interaction rather than requesting complete datasets.

---

## Pagination Model

TaskFlow supports **offset-based pagination** for collection endpoints. Clients specify the number of records to return and the starting position within the result set using query parameters.

**Example Request**

```http
GET /api/projects?page=2&pageSize=25
Authorization: Bearer <access_token>
```

The API returns the requested page together with pagination metadata that enables the client to navigate the remaining results.

**Example Response**

```json
{
  "page": 2,
  "pageSize": 25,
  "totalRecords": 142,
  "totalPages": 6,
  "data": [
    {
      "projectId": "PJT-101",
      "projectName": "TaskFlow Mobile"
    }
  ]
}
```

Providing pagination metadata allows client applications to build navigation controls without performing additional calculations.

---

## Pagination Parameters

Collection endpoints support a consistent set of pagination parameters.

| Parameter    | Description                                                            |
| ------------ | ---------------------------------------------------------------------- |
| **page**     | Specifies the page number to retrieve.                                 |
| **pageSize** | Specifies the maximum number of records returned in a single response. |

When pagination parameters are omitted, the API applies default values configured by the service.

Client applications should avoid requesting excessively large page sizes because doing so increases response time and memory consumption while reducing the benefits of pagination.

---

## Sorting and Filtering

Pagination is most effective when combined with sorting and filtering.

Filtering reduces the dataset before pagination is applied, allowing the API to return only records that satisfy the requested criteria. Sorting establishes a predictable order for the returned data, ensuring that consecutive pages remain consistent as users navigate through the result set.

**Example**

```http
GET /api/projects?page=1&pageSize=20&status=Active&sortBy=createdAt&order=desc
Authorization: Bearer <access_token>
```

Applying sorting before pagination ensures that records appear in a stable and repeatable sequence across multiple requests.

---

## Pagination Response Metadata

In addition to the requested resources, paginated responses include metadata describing the current page and the complete result set.

Typical pagination metadata includes the current page number, page size, total number of available records, and the total number of pages.

This information enables client applications to determine whether additional pages exist and provides sufficient information to implement paging controls without additional API requests.

---

## Performance Considerations

Choosing an appropriate page size is essential for maintaining API performance.

Very small page sizes increase the number of API requests required to retrieve a dataset, while excessively large page sizes increase response size and processing time. Client applications should balance these considerations based on the expected usage scenario.

Endpoints should continue to return predictable response times regardless of dataset size by enforcing reasonable limits on the maximum number of records returned per request.

---

## Best Practices

Client applications should request only the data required for the current operation and avoid retrieving complete datasets when paginated endpoints are available.

Pagination should be combined with filtering and sorting whenever possible to minimize unnecessary data transfer and improve result relevance.

Applications should rely on the pagination metadata returned by the API rather than calculating navigation values independently. This ensures that paging behavior remains consistent even if the underlying dataset changes.

When processing multiple pages, clients should preserve the selected sorting criteria throughout the session to maintain a consistent ordering of returned resources.

