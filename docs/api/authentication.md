# Authentication API

## Register

Create a new user account.

### PWD Registration

```http
POST /api/v1/register
Content-Type: application/json

{
    "name": "John Doe",
    "email": "john@example.com",
    "password": "password123",
    "password_confirmation": "password123",
    "account_type": "pwd"
}
```

### HelpMate Registration

```http
POST /api/v1/register
Content-Type: application/json

{
    "name": "Jane Smith",
    "email": "jane@example.com",
    "password": "password123",
    "password_confirmation": "password123",
    "account_type": "helpmate",
    "skills": ["caregiving", "transportation", "household"]
}
```

### Response

```json
{
    "success": true,
    "data": {
        "user": {
            "id": 1,
            "name": "John Doe",
            "email": "john@example.com"
        },
        "token": "1|abc123..."
    },
    "message": "Registration successful"
}
```

## Login

Authenticate and receive an access token.

```http
POST /api/v1/login
Content-Type: application/json

{
    "email": "john@example.com",
    "password": "password123",
    "account_type": "pwd"
}
```

### Parameters

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| email | string | Yes | User email |
| password | string | Yes | User password |
| account_type | string | Yes | `pwd` or `helpmate` |

### Response

```json
{
    "success": true,
    "data": {
        "user": {
            "id": 1,
            "name": "John Doe",
            "email": "john@example.com",
            "account_type": "pwd"
        },
        "token": "1|abc123..."
    }
}
```

## Logout

Revoke the current access token.

```http
POST /api/v1/logout
Authorization: Bearer {token}
```

### Response

```json
{
    "success": true,
    "message": "Logged out successfully"
}
```

## Get Current User

Retrieve the authenticated user's information.

```http
GET /api/v1/me
Authorization: Bearer {token}
```

### Response

```json
{
    "success": true,
    "data": {
        "id": 1,
        "name": "John Doe",
        "email": "john@example.com",
        "account_type": "pwd",
        "profile_photo": "/storage/photos/user1.jpg",
        "accessibility_settings": {
            "font_size": "medium",
            "tts_enabled": false,
            "stt_enabled": false,
            "high_contrast": false
        }
    }
}
```

## Update Profile

Update the authenticated user's profile.

### PWD Profile Update

```http
PUT /api/v1/profile
Authorization: Bearer {token}
Content-Type: application/json

{
    "name": "John Updated",
    "phone": "555-1234",
    "address": "123 Main St"
}
```

### HelpMate Profile Update

```http
PUT /api/v1/helper/profile
Authorization: Bearer {token}
Content-Type: application/json

{
    "name": "Jane Updated",
    "skills": ["caregiving", "cooking", "transportation"],
    "bio": "Experienced caregiver with 5 years experience"
}
```

## Upload Profile Photo

```http
POST /api/v1/profile/photo
Authorization: Bearer {token}
Content-Type: multipart/form-data

photo: [file]
```

### Response

```json
{
    "success": true,
    "data": {
        "photo_url": "/storage/photos/user1_new.jpg"
    }
}
```

## Error Responses

### Invalid Credentials

```json
{
    "success": false,
    "message": "Invalid credentials"
}
```

**Status Code:** 401

### Validation Errors

```json
{
    "success": false,
    "message": "Validation failed",
    "errors": {
        "email": ["The email has already been taken."],
        "password": ["The password must be at least 8 characters."]
    }
}
```

**Status Code:** 422
