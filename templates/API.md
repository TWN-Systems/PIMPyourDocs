# {SERVICE_NAME} API

---
title: "{SERVICE_NAME} API"
status: published
owner: "{TEAM_NAME}"
created: {YYYY-MM-DD}
updated: {YYYY-MM-DD}
tags: [api, {service}]
version: "1.0.0"
base_url: https://api.example.com/v1
---

## Overview

Brief description of the API and its purpose.

### Base URL

```
Production: https://api.example.com/v1
Staging:    https://api.staging.example.com/v1
```

### Versioning

This API uses URL-based versioning. The current version is `v1`.

---

## Authentication

All requests MUST include authentication.

### Bearer Token

```http
Authorization: Bearer {token}
```

### API Key (deprecated)

```http
X-API-Key: {api_key}
```

**Note:** API key authentication is deprecated and will be removed in v2.

---

## Common Headers

### Request Headers

| Header | Required | Description |
|--------|----------|-------------|
| `Authorization` | Yes | Bearer token |
| `Content-Type` | Yes* | `application/json` for POST/PUT/PATCH |
| `Accept` | No | `application/json` (default) |
| `X-Request-ID` | No | Client-provided request ID for tracing |

### Response Headers

| Header | Description |
|--------|-------------|
| `X-Request-ID` | Request ID (echoed or generated) |
| `X-RateLimit-Limit` | Rate limit ceiling |
| `X-RateLimit-Remaining` | Requests remaining |
| `X-RateLimit-Reset` | Unix timestamp when limit resets |

---

## Rate Limiting

| Tier | Limit | Window |
|------|-------|--------|
| Default | 100 requests | 1 minute |
| Premium | 1000 requests | 1 minute |

When rate limited, the API returns:

```http
HTTP/1.1 429 Too Many Requests
Retry-After: 30

{
  "error": {
    "code": "RATE_LIMITED",
    "message": "Rate limit exceeded. Retry after 30 seconds."
  }
}
```

---

## Error Handling

### Error Response Format

All errors follow this format:

```json
{
  "error": {
    "code": "ERROR_CODE",
    "message": "Human-readable message",
    "details": {}
  }
}
```

### Error Codes

| HTTP Status | Code | Description |
|-------------|------|-------------|
| 400 | `INVALID_REQUEST` | Request validation failed |
| 401 | `UNAUTHORIZED` | Missing or invalid authentication |
| 403 | `FORBIDDEN` | Insufficient permissions |
| 404 | `NOT_FOUND` | Resource not found |
| 409 | `CONFLICT` | Resource conflict |
| 422 | `UNPROCESSABLE` | Semantic validation failed |
| 429 | `RATE_LIMITED` | Rate limit exceeded |
| 500 | `INTERNAL_ERROR` | Server error |

---

## Pagination

List endpoints use cursor-based pagination.

### Request Parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `limit` | integer | 20 | Items per page (max: 100) |
| `cursor` | string | — | Cursor from previous response |

### Response Format

```json
{
  "data": [...],
  "pagination": {
    "has_more": true,
    "next_cursor": "eyJpZCI6MTAwfQ=="
  }
}
```

### Example

```bash
# First page
GET /api/v1/users?limit=20

# Next page
GET /api/v1/users?limit=20&cursor=eyJpZCI6MTAwfQ==
```

---

## Endpoints

### Health Check

Check API availability.

```http
GET /health
```

**Response:**

```json
{
  "status": "healthy",
  "version": "1.0.0",
  "timestamp": "2024-01-15T10:30:00Z"
}
```

---

### Resources

#### List Resources

```http
GET /api/v1/resources
```

**Query Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `status` | string | Filter by status |
| `created_after` | datetime | Filter by creation date |
| `sort` | string | Sort field (default: `created_at`) |
| `order` | string | `asc` or `desc` (default: `desc`) |

**Response:**

```json
{
  "data": [
    {
      "id": "res_123",
      "name": "Example Resource",
      "status": "active",
      "created_at": "2024-01-15T10:30:00Z",
      "updated_at": "2024-01-15T10:30:00Z"
    }
  ],
  "pagination": {
    "has_more": false,
    "next_cursor": null
  }
}
```

---

#### Get Resource

```http
GET /api/v1/resources/{id}
```

**Path Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `id` | string | Resource ID |

**Response:**

```json
{
  "data": {
    "id": "res_123",
    "name": "Example Resource",
    "status": "active",
    "metadata": {},
    "created_at": "2024-01-15T10:30:00Z",
    "updated_at": "2024-01-15T10:30:00Z"
  }
}
```

**Errors:**

| Status | Code | Condition |
|--------|------|-----------|
| 404 | `NOT_FOUND` | Resource does not exist |

---

#### Create Resource

```http
POST /api/v1/resources
```

**Request Body:**

```json
{
  "name": "New Resource",
  "metadata": {
    "key": "value"
  }
}
```

**Fields:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | Yes | Resource name (1-255 chars) |
| `metadata` | object | No | Arbitrary key-value pairs |

**Response:**

```http
HTTP/1.1 201 Created
Location: /api/v1/resources/res_456

{
  "data": {
    "id": "res_456",
    "name": "New Resource",
    "status": "active",
    "metadata": {"key": "value"},
    "created_at": "2024-01-15T10:30:00Z",
    "updated_at": "2024-01-15T10:30:00Z"
  }
}
```

**Errors:**

| Status | Code | Condition |
|--------|------|-----------|
| 400 | `INVALID_REQUEST` | Validation failed |
| 409 | `CONFLICT` | Resource with same name exists |

---

#### Update Resource

```http
PATCH /api/v1/resources/{id}
```

**Request Body:**

```json
{
  "name": "Updated Name",
  "status": "inactive"
}
```

**Fields:**

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | New name |
| `status` | string | `active` or `inactive` |
| `metadata` | object | Merged with existing metadata |

**Response:**

```json
{
  "data": {
    "id": "res_123",
    "name": "Updated Name",
    "status": "inactive",
    "metadata": {},
    "created_at": "2024-01-15T10:30:00Z",
    "updated_at": "2024-01-15T11:00:00Z"
  }
}
```

---

#### Delete Resource

```http
DELETE /api/v1/resources/{id}
```

**Response:**

```http
HTTP/1.1 204 No Content
```

**Errors:**

| Status | Code | Condition |
|--------|------|-----------|
| 404 | `NOT_FOUND` | Resource does not exist |
| 409 | `CONFLICT` | Resource has dependencies |

---

## Webhooks

### Webhook Payload

```json
{
  "id": "evt_123",
  "type": "resource.created",
  "created_at": "2024-01-15T10:30:00Z",
  "data": {
    "id": "res_456",
    "name": "New Resource"
  }
}
```

### Event Types

| Event | Description |
|-------|-------------|
| `resource.created` | Resource was created |
| `resource.updated` | Resource was updated |
| `resource.deleted` | Resource was deleted |

### Signature Verification

Webhooks include a signature header:

```http
X-Webhook-Signature: sha256=abc123...
```

Verify using HMAC-SHA256:

```python
import hmac
import hashlib

def verify_signature(payload, signature, secret):
    expected = hmac.new(
        secret.encode(),
        payload.encode(),
        hashlib.sha256
    ).hexdigest()
    return hmac.compare_digest(f"sha256={expected}", signature)
```

---

## SDK Examples

### cURL

```bash
curl -X GET "https://api.example.com/v1/resources" \
  -H "Authorization: Bearer ${TOKEN}" \
  -H "Accept: application/json"
```

### Python

```python
import httpx

client = httpx.Client(
    base_url="https://api.example.com/v1",
    headers={"Authorization": f"Bearer {token}"}
)

response = client.get("/resources")
resources = response.json()["data"]
```

### JavaScript

```javascript
const response = await fetch("https://api.example.com/v1/resources", {
  headers: {
    "Authorization": `Bearer ${token}`,
    "Accept": "application/json"
  }
});

const { data } = await response.json();
```

---

## Changelog

### v1.0.0 (2024-01-15)

- Initial release
- Resources CRUD endpoints
- Webhook support

---

*Generated from OpenAPI spec. See `openapi.yaml` for machine-readable version.*
