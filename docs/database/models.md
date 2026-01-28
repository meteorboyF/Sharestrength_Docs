# Database Models

ShareStrength uses Eloquent ORM with the following models.

## Entity Relationship Diagram

```
┌─────────────┐       ┌─────────────┐       ┌─────────────┐
│    User     │       │    Task     │       │   Helper    │
│   (PWD)     │───────│             │───────│ (HelpMate)  │
└─────────────┘       └─────────────┘       └─────────────┘
      │                     │                     │
      │                     │                     │
      ▼                     ▼                     ▼
┌─────────────┐       ┌─────────────┐       ┌─────────────┐
│   Order     │       │   Message   │       │ Application │
└─────────────┘       └─────────────┘       └─────────────┘
      │                     │
      ▼                     ▼
┌─────────────┐       ┌─────────────┐
│  OrderItem  │       │Conversation │
└─────────────┘       └─────────────┘
```

## Core Models

### User (PWD)

People With Disabilities who request services.

| Column | Type | Description |
|--------|------|-------------|
| id | bigint | Primary key |
| name | string | Full name |
| email | string | Email (unique) |
| password | string | Hashed password |
| phone | string | Phone number |
| address | text | Address |
| profile_photo | string | Photo path |
| is_active | boolean | Account status |
| created_at | timestamp | Created date |
| updated_at | timestamp | Updated date |

**Relationships:**

- `tasks()` - HasMany Task (as creator)
- `trustedContacts()` - HasMany TrustedContact
- `orders()` - HasMany Order
- `payments()` - HasMany Payment (as payer)
- `accessibilitySettings()` - HasOne AccessibilitySetting

### Helper (HelpMate)

Caregivers who provide services.

| Column | Type | Description |
|--------|------|-------------|
| id | bigint | Primary key |
| name | string | Full name |
| email | string | Email (unique) |
| password | string | Hashed password |
| skills | json | Array of skills |
| bio | text | Profile bio |
| profile_photo | string | Photo path |
| is_verified | boolean | Verification status |
| is_active | boolean | Account status |
| created_at | timestamp | Created date |
| updated_at | timestamp | Updated date |

**Relationships:**

- `applications()` - HasMany Application
- `assignedTasks()` - HasMany Task (as caregiver)
- `reviews()` - HasMany Review
- `payments()` - HasMany Payment (as payee)

### Admin

System administrators.

| Column | Type | Description |
|--------|------|-------------|
| id | bigint | Primary key |
| name | string | Full name |
| email | string | Email (unique) |
| password | string | Hashed password |
| created_at | timestamp | Created date |
| updated_at | timestamp | Updated date |

**Relationships:**

- `resources()` - HasMany Resource (as uploader)

### Task

Service requests posted by PWD users.

| Column | Type | Description |
|--------|------|-------------|
| id | bigint | Primary key |
| user_id | bigint | Creator (FK to users) |
| caregiver_id | bigint | Assigned helper (FK to helpers) |
| title | string | Task title |
| description | text | Task details |
| location | string | Task location |
| budget | decimal | Payment amount |
| urgency_level | string | low/medium/high |
| status | string | Task status |
| required_skills | json | Skills needed |
| scheduled_date | date | When task should be done |
| started_at | timestamp | When work began |
| completed_at | timestamp | When work finished |
| created_at | timestamp | Created date |
| updated_at | timestamp | Updated date |

**Relationships:**

- `creator()` - BelongsTo User
- `caregiver()` - BelongsTo Helper
- `applications()` - HasMany Application
- `messages()` - HasMany Message
- `payments()` - HasMany Payment

### Application

HelpMate applications for tasks.

| Column | Type | Description |
|--------|------|-------------|
| id | bigint | Primary key |
| task_id | bigint | FK to tasks |
| helper_id | bigint | FK to helpers |
| message | text | Application message |
| status | string | pending/accepted/rejected |
| created_at | timestamp | Created date |
| updated_at | timestamp | Updated date |

**Relationships:**

- `task()` - BelongsTo Task
- `helper()` - BelongsTo Helper
- `applicant()` - BelongsTo Helper (alias)

### Conversation

Message threads between users.

| Column | Type | Description |
|--------|------|-------------|
| id | bigint | Primary key |
| user_one_id | bigint | First participant ID |
| user_one_type | string | First participant type |
| user_two_id | bigint | Second participant ID |
| user_two_type | string | Second participant type |
| task_id | bigint | Associated task (optional) |
| created_at | timestamp | Created date |
| updated_at | timestamp | Updated date |

**Relationships:**

- `userOne()` - MorphTo (User or Helper)
- `userTwo()` - MorphTo (User or Helper)
- `messages()` - HasMany Message
- `task()` - BelongsTo Task

### Message

Individual messages in conversations.

| Column | Type | Description |
|--------|------|-------------|
| id | bigint | Primary key |
| conversation_id | bigint | FK to conversations |
| sender_id | bigint | Sender ID |
| sender_type | string | Sender type (user/helper) |
| receiver_id | bigint | Receiver ID |
| receiver_type | string | Receiver type |
| content | text | Message content |
| is_read | boolean | Read status |
| created_at | timestamp | Created date |
| updated_at | timestamp | Updated date |

**Relationships:**

- `conversation()` - BelongsTo Conversation
- `sender()` - MorphTo (User or Helper)
- `receiver()` - MorphTo (User or Helper)

### Payment

Payment records between users.

| Column | Type | Description |
|--------|------|-------------|
| id | bigint | Primary key |
| task_id | bigint | FK to tasks |
| payer_id | bigint | FK to users |
| payee_id | bigint | FK to helpers |
| amount | decimal | Payment amount |
| status | string | pending/paid/failed |
| paid_at | timestamp | When payment completed |
| created_at | timestamp | Created date |
| updated_at | timestamp | Updated date |

**Relationships:**

- `task()` - BelongsTo Task
- `payer()` - BelongsTo User
- `payee()` - BelongsTo Helper

### Resource

Accessible content in the library.

| Column | Type | Description |
|--------|------|-------------|
| id | bigint | Primary key |
| title | string | Resource title |
| description | text | Resource description |
| type | string | Resource type |
| file_path | string | File location |
| file_size | bigint | File size in bytes |
| duration | integer | Duration (audio/video) |
| category_id | bigint | FK to resource_categories |
| uploaded_by | bigint | FK to admins |
| is_featured | boolean | Featured flag |
| download_count | integer | Download counter |
| created_at | timestamp | Created date |
| updated_at | timestamp | Updated date |

**Relationships:**

- `category()` - BelongsTo ResourceCategory
- `uploader()` - BelongsTo Admin

### Order

Shop orders.

| Column | Type | Description |
|--------|------|-------------|
| id | bigint | Primary key |
| user_id | bigint | FK to users |
| status | string | Order status |
| total | decimal | Total amount |
| shipping_address | json | Delivery address |
| created_at | timestamp | Created date |
| updated_at | timestamp | Updated date |

**Relationships:**

- `user()` - BelongsTo User
- `items()` - HasMany OrderItem

### AccessibilitySetting

User accessibility preferences.

| Column | Type | Description |
|--------|------|-------------|
| id | bigint | Primary key |
| user_id | bigint | FK to users |
| font_size | string | Text size preference |
| tts_enabled | boolean | Text-to-speech |
| stt_enabled | boolean | Speech-to-text |
| high_contrast | boolean | High contrast mode |
| created_at | timestamp | Created date |
| updated_at | timestamp | Updated date |

**Relationships:**

- `user()` - BelongsTo User
