# API Reference

ShareStrength provides a RESTful API for all functionality.

## Base URL

```
http://localhost:8000/api/v1
```

## Authentication

The API uses Laravel Sanctum for token-based authentication.

### Getting a Token

```http
POST /api/v1/login
Content-Type: application/json

{
    "email": "user@example.com",
    "password": "password",
    "account_type": "pwd"  // or "helpmate"
}
```

### Using the Token

Include the token in the Authorization header:

```http
GET /api/v1/tasks
Authorization: Bearer {your_token}
```

## Response Format

### Success Response

```json
{
    "success": true,
    "data": { ... },
    "message": "Operation successful"
}
```

### Error Response

```json
{
    "success": false,
    "message": "Error description",
    "errors": {
        "field": ["Validation error message"]
    }
}
```

## HTTP Status Codes

| Code | Description |
|------|-------------|
| 200 | Success |
| 201 | Created |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 422 | Validation Error |
| 429 | Too Many Requests |
| 500 | Server Error |

## Rate Limiting

| Endpoint Group | Limit |
|----------------|-------|
| General API | 60 requests/minute |
| Chatbot | 30 requests/minute |
| History | 60 requests/minute |

## API Sections

- [Authentication](authentication.md) - Login, register, logout
- [Tasks](tasks.md) - Task CRUD and lifecycle
- [Messaging](messaging.md) - Conversations and messages
- [Resources](resources.md) - Resource library
- [Payments](payments.md) - Payment tracking
- [Shop](shop.md) - Marketplace and orders

## Common Headers

```http
Content-Type: application/json
Accept: application/json
Authorization: Bearer {token}
```

## Pagination

List endpoints support pagination:

```http
GET /api/v1/tasks?page=1&per_page=15
```

Response includes pagination metadata:

```json
{
    "data": [...],
    "meta": {
        "current_page": 1,
        "last_page": 5,
        "per_page": 15,
        "total": 73
    }
}
```
