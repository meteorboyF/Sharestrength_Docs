# Tasks API

## List Tasks

Get all open tasks.

```http
GET /api/v1/tasks
```

### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| page | integer | Page number |
| per_page | integer | Items per page (default: 15) |
| status | string | Filter by status |
| urgency | string | Filter by urgency level |

### Response

```json
{
    "success": true,
    "data": [
        {
            "id": 1,
            "title": "Grocery Shopping Assistance",
            "description": "Need help with weekly grocery shopping",
            "location": "Downtown area",
            "budget": 50.00,
            "urgency_level": "medium",
            "status": "open",
            "scheduled_date": "2024-01-15",
            "required_skills": ["transportation", "shopping"],
            "creator": {
                "id": 1,
                "name": "John Doe"
            },
            "created_at": "2024-01-10T10:00:00Z"
        }
    ],
    "meta": {
        "current_page": 1,
        "last_page": 3,
        "per_page": 15,
        "total": 42
    }
}
```

## Get My Tasks

Get tasks created by the authenticated PWD user.

```http
GET /api/v1/my-tasks
Authorization: Bearer {token}
```

## Get Single Task

```http
GET /api/v1/tasks/{id}
```

### Response

```json
{
    "success": true,
    "data": {
        "id": 1,
        "title": "Grocery Shopping Assistance",
        "description": "Need help with weekly grocery shopping at local supermarket",
        "location": "Downtown area",
        "budget": 50.00,
        "urgency_level": "medium",
        "status": "open",
        "scheduled_date": "2024-01-15",
        "required_skills": ["transportation", "shopping"],
        "creator": {
            "id": 1,
            "name": "John Doe"
        },
        "caregiver": null,
        "applications_count": 3,
        "created_at": "2024-01-10T10:00:00Z"
    }
}
```

## Create Task

Create a new task (PWD users only).

```http
POST /api/v1/tasks
Authorization: Bearer {token}
Content-Type: application/json

{
    "title": "Grocery Shopping Assistance",
    "description": "Need help with weekly grocery shopping",
    "location": "Downtown area",
    "budget": 50.00,
    "urgency_level": "medium",
    "scheduled_date": "2024-01-15",
    "required_skills": ["transportation", "shopping"]
}
```

### Parameters

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| title | string | Yes | Task title |
| description | string | Yes | Detailed description |
| location | string | No | Task location |
| budget | decimal | No | Payment amount |
| urgency_level | string | No | low, medium, high |
| scheduled_date | date | No | When task should be done |
| required_skills | array | No | Skills needed |

### Response

```json
{
    "success": true,
    "data": {
        "id": 1,
        "title": "Grocery Shopping Assistance",
        "status": "open"
    },
    "message": "Task created successfully"
}
```

## Update Task

Update an existing task (creator only).

```http
PUT /api/v1/tasks/{id}
Authorization: Bearer {token}
Content-Type: application/json

{
    "title": "Updated Task Title",
    "budget": 75.00
}
```

## Delete Task

Delete a task (creator only, must be open status).

```http
DELETE /api/v1/tasks/{id}
Authorization: Bearer {token}
```

## Start Task

Mark a task as in progress (assigned HelpMate only).

```http
PUT /api/v1/tasks/{id}/start
Authorization: Bearer {token}
```

### Response

```json
{
    "success": true,
    "data": {
        "id": 1,
        "status": "in_progress",
        "started_at": "2024-01-15T09:00:00Z"
    },
    "message": "Task started"
}
```

## Complete Task

Mark a task as completed (assigned HelpMate only).

```http
PUT /api/v1/tasks/{id}/complete
Authorization: Bearer {token}
```

### Response

```json
{
    "success": true,
    "data": {
        "id": 1,
        "status": "completed",
        "completed_at": "2024-01-15T12:00:00Z"
    },
    "message": "Task completed"
}
```

## Repost Task

Repost a cancelled or expired task.

```http
POST /api/v1/tasks/{id}/repost
Authorization: Bearer {token}
```

## Match Caregivers

Get suggested HelpMates for a task.

```http
GET /api/v1/tasks/{id}/match
Authorization: Bearer {token}
```

### Response

```json
{
    "success": true,
    "data": [
        {
            "id": 1,
            "name": "Jane Smith",
            "skills": ["transportation", "shopping", "caregiving"],
            "rating": 4.8,
            "match_score": 0.95
        }
    ]
}
```

## Task Status Values

| Status | Description |
|--------|-------------|
| open | Task is available for applications |
| requested | Applications received, pending review |
| accepted | HelpMate assigned |
| in_progress | Task being worked on |
| completed | Task finished |
| cancelled | Task cancelled by creator |
