# Features

ShareStrength provides a comprehensive set of features for connecting PWD users with HelpMates.

## Task Management

The core functionality of ShareStrength revolves around task-based assistance.

### Task Lifecycle

```
open → requested → accepted → in_progress → completed/cancelled
```

### Task Properties

- **Title & Description** - What needs to be done
- **Location** - Where the task takes place
- **Budget** - Payment offered for the task
- **Required Skills** - Skills needed to complete the task
- **Urgency Level** - How urgent the task is
- **Scheduled Date** - When the task should be completed

### Task Actions

| Action | Who Can Perform | Description |
|--------|-----------------|-------------|
| Create | PWD Users | Post a new task |
| Apply | HelpMates | Apply to complete a task |
| Accept | PWD Users | Accept an application |
| Start | Assigned HelpMate | Begin working on the task |
| Complete | Assigned HelpMate | Mark task as done |
| Cancel | Task Creator | Cancel an open task |
| Repost | Task Creator | Repost a cancelled/expired task |

## Messaging System

Real-time communication between users.

- **Conversations** - Thread-based messaging
- **Polymorphic Users** - Supports both PWD and HelpMate participants
- **Task Association** - Messages can be linked to specific tasks
- **Read Tracking** - Unread message indicators

## Payment System

Track payments between service requesters and providers.

- **Payment Status** - Pending, Paid, Failed
- **Payment History** - Complete transaction log
- **Analytics** - Payment insights and summaries
- **Payer/Payee Tracking** - Clear record of who pays whom

## Marketplace

Built-in shop for accessibility products and services.

- **Product Listings** - Browse available products
- **Shopping Cart** - Add items before checkout
- **Order Management** - Track order status
- **Order States** - Pending → Paid → Shipped → Delivered/Cancelled

## Resource Library

Curated collection of accessible resources.

### Resource Types

- Audiobooks
- Sign language videos
- Braille materials
- Large print documents
- Accessible PDFs

### Features

- **Categorization** - Organized by resource categories
- **Featured Resources** - Highlighted important resources
- **Search** - Find resources by keyword
- **Download Tracking** - Monitor resource usage
- **Admin Management** - Full CRUD for administrators

## Community

Social features for user engagement.

- **Posts** - Share updates and experiences
- **Comments** - Engage with community posts
- **Moderation** - Status: Active, Flagged, Hidden

## Trusted Contacts

Emergency contact management for PWD users.

- **Add Contacts** - Register trusted individuals
- **Verification** - Contact verification system
- **Relationship Tracking** - Define contact relationships

## AI Chatbot

Intelligent assistant powered by Google Gemini.

- **Conversational AI** - Natural language interactions
- **RAG Integration** - Context-aware responses using Qdrant
- **Session History** - Persistent conversation memory
- **Streaming Responses** - Real-time response generation

## Profile Management

User profile customization.

- **Photo Uploads** - Profile pictures
- **Profile Information** - Personal details
- **Skills (HelpMates)** - List of capabilities
- **Reviews (HelpMates)** - Rating and feedback system
