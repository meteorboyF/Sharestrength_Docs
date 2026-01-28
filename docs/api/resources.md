# Resources API

The Resource Library provides accessible content in various formats.

## List Resources

Get all resources (public).

```http
GET /api/v1/resources
```

### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| category_id | integer | Filter by category |
| type | string | Filter by resource type |
| page | integer | Page number |

### Response

```json
{
    "success": true,
    "data": [
        {
            "id": 1,
            "title": "Introduction to Accessibility",
            "description": "A comprehensive guide to digital accessibility",
            "type": "accessible_pdf",
            "file_url": "/storage/resources/intro-accessibility.pdf",
            "file_size": 2456789,
            "duration": null,
            "category": {
                "id": 1,
                "name": "Guides"
            },
            "is_featured": true,
            "download_count": 150,
            "created_at": "2024-01-05T10:00:00Z"
        }
    ]
}
```

## Get Featured Resources

Get highlighted resources.

```http
GET /api/v1/resources/featured
```

### Response

```json
{
    "success": true,
    "data": [
        {
            "id": 1,
            "title": "Introduction to Accessibility",
            "type": "accessible_pdf",
            "is_featured": true
        }
    ]
}
```

## Search Resources

Search resources by keyword.

```http
GET /api/v1/resources/search?q=accessibility
```

### Query Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| q | string | Search query |
| type | string | Filter by type |
| category_id | integer | Filter by category |

## Get Single Resource

```http
GET /api/v1/resources/{id}
```

### Response

```json
{
    "success": true,
    "data": {
        "id": 1,
        "title": "Introduction to Accessibility",
        "description": "A comprehensive guide to digital accessibility covering WCAG guidelines and best practices.",
        "type": "accessible_pdf",
        "file_url": "/storage/resources/intro-accessibility.pdf",
        "file_size": 2456789,
        "duration": null,
        "category": {
            "id": 1,
            "name": "Guides"
        },
        "uploader": {
            "id": 1,
            "name": "Admin"
        },
        "is_featured": true,
        "download_count": 150,
        "created_at": "2024-01-05T10:00:00Z"
    }
}
```

## Download Resource

Download a resource file (increments download count).

```http
GET /api/v1/resources/{id}/download
```

Returns the file as a download.

## Resource Types

| Type | Description |
|------|-------------|
| audiobook | Audio version of text content |
| sign_language_video | Video with sign language |
| braille | Braille-formatted document |
| large_print | Large text document |
| accessible_pdf | Tagged, accessible PDF |

---

## Admin Endpoints

The following endpoints require admin authentication.

## Create Resource

```http
POST /api/v1/resources
Authorization: Bearer {admin_token}
Content-Type: multipart/form-data

title: Introduction to Accessibility
description: A comprehensive guide...
type: accessible_pdf
category_id: 1
is_featured: true
file: [upload file]
```

### Parameters

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| title | string | Yes | Resource title |
| description | string | Yes | Resource description |
| type | string | Yes | Resource type |
| category_id | integer | Yes | Category ID |
| is_featured | boolean | No | Featured flag |
| file | file | Yes | Resource file |
| duration | integer | No | Duration in seconds (audio/video) |

### Response

```json
{
    "success": true,
    "data": {
        "id": 2,
        "title": "Introduction to Accessibility"
    },
    "message": "Resource created successfully"
}
```

## Update Resource

```http
PUT /api/v1/resources/{id}
Authorization: Bearer {admin_token}
Content-Type: application/json

{
    "title": "Updated Title",
    "is_featured": false
}
```

## Delete Resource

```http
DELETE /api/v1/resources/{id}
Authorization: Bearer {admin_token}
```

---

## Categories API

## List Categories

```http
GET /api/v1/resources/categories
```

### Response

```json
{
    "success": true,
    "data": [
        {
            "id": 1,
            "name": "Guides",
            "resources_count": 15
        },
        {
            "id": 2,
            "name": "Tutorials",
            "resources_count": 8
        }
    ]
}
```

## Create Category (Admin)

```http
POST /api/v1/categories
Authorization: Bearer {admin_token}
Content-Type: application/json

{
    "name": "New Category"
}
```

## Update Category (Admin)

```http
PUT /api/v1/categories/{id}
Authorization: Bearer {admin_token}
Content-Type: application/json

{
    "name": "Updated Category Name"
}
```

## Delete Category (Admin)

```http
DELETE /api/v1/categories/{id}
Authorization: Bearer {admin_token}
```
