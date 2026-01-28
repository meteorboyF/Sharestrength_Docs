# Messaging API

## List Conversations

Get all conversations for the authenticated user.

```http
GET /api/v1/conversations
Authorization: Bearer {token}
```

### Response

```json
{
    "success": true,
    "data": [
        {
            "id": 1,
            "participant": {
                "id": 2,
                "name": "Jane Smith",
                "type": "helper"
            },
            "task": {
                "id": 1,
                "title": "Grocery Shopping"
            },
            "last_message": {
                "content": "I can help with that!",
                "created_at": "2024-01-10T15:30:00Z"
            },
            "unread_count": 2
        }
    ]
}
```

## Get Conversation

Get a specific conversation with messages.

```http
GET /api/v1/conversations/{id}
Authorization: Bearer {token}
```

### Response

```json
{
    "success": true,
    "data": {
        "id": 1,
        "participant": {
            "id": 2,
            "name": "Jane Smith",
            "type": "helper"
        },
        "task": {
            "id": 1,
            "title": "Grocery Shopping"
        },
        "messages": [
            {
                "id": 1,
                "content": "Hi, I'm interested in helping with your task.",
                "sender_id": 2,
                "sender_type": "helper",
                "is_read": true,
                "created_at": "2024-01-10T15:00:00Z"
            },
            {
                "id": 2,
                "content": "Great! Do you have experience with grocery shopping?",
                "sender_id": 1,
                "sender_type": "user",
                "is_read": true,
                "created_at": "2024-01-10T15:15:00Z"
            }
        ]
    }
}
```

## Get or Create Conversation

Start a new conversation or get existing one.

```http
POST /api/v1/conversations/get-or-create
Authorization: Bearer {token}
Content-Type: application/json

{
    "participant_id": 2,
    "participant_type": "helper",
    "task_id": 1
}
```

### Parameters

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| participant_id | integer | Yes | Other user's ID |
| participant_type | string | Yes | `user` or `helper` |
| task_id | integer | No | Associated task ID |

### Response

```json
{
    "success": true,
    "data": {
        "id": 1,
        "is_new": false
    }
}
```

## List Messages

Get messages, optionally filtered by conversation.

```http
GET /api/v1/messages
Authorization: Bearer {token}
```

### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| conversation_id | integer | Filter by conversation |
| page | integer | Page number |

## Send Message

Send a new message.

```http
POST /api/v1/messages
Authorization: Bearer {token}
Content-Type: application/json

{
    "conversation_id": 1,
    "content": "Hello, I can help with your task!"
}
```

### Parameters

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| conversation_id | integer | Yes | Target conversation |
| content | string | Yes | Message content |

### Response

```json
{
    "success": true,
    "data": {
        "id": 5,
        "conversation_id": 1,
        "content": "Hello, I can help with your task!",
        "sender_id": 2,
        "sender_type": "helper",
        "is_read": false,
        "created_at": "2024-01-10T16:00:00Z"
    },
    "message": "Message sent"
}
```

## Delete Message

Delete a message (sender only).

```http
DELETE /api/v1/messages/{id}
Authorization: Bearer {token}
```

## Mark Message as Read

Mark a single message as read.

```http
PATCH /api/v1/messages/{id}/read
Authorization: Bearer {token}
```

## Mark Conversation as Read

Mark all messages in a conversation as read.

```http
PATCH /api/v1/conversations/{id}/read
Authorization: Bearer {token}
```

### Response

```json
{
    "success": true,
    "message": "Conversation marked as read"
}
```

## Polymorphic Users

Messages support both PWD users and HelpMates through polymorphic relationships:

| Type | Model | Description |
|------|-------|-------------|
| user | User | PWD user |
| helper | Helper | HelpMate user |

The `sender_type` and `receiver_type` fields indicate the user type.
