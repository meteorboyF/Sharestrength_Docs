# Architecture

ShareStrength follows a modern Laravel architecture with Livewire for reactive UI components.

## System Overview

```
┌─────────────────────────────────────────────────────────┐
│                      Client Layer                        │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────┐  │
│  │   Browser   │  │  Mobile App │  │   API Clients   │  │
│  └─────────────┘  └─────────────┘  └─────────────────┘  │
└─────────────────────────────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────┐
│                   Application Layer                      │
│  ┌─────────────────────────────────────────────────┐    │
│  │              Laravel 12 Backend                  │    │
│  │  ┌───────────┐  ┌───────────┐  ┌─────────────┐  │    │
│  │  │ Livewire  │  │    API    │  │    Auth     │  │    │
│  │  │Components │  │Controllers│  │  (Sanctum)  │  │    │
│  │  └───────────┘  └───────────┘  └─────────────┘  │    │
│  └─────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────┐
│                    Service Layer                         │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────────┐  │
│  │   Gemini    │  │   Qdrant    │  │     Queue       │  │
│  │   Service   │  │   Service   │  │    Workers      │  │
│  └─────────────┘  └─────────────┘  └─────────────────┘  │
└─────────────────────────────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────┐
│                     Data Layer                           │
│  ┌─────────────────────────────────────────────────┐    │
│  │                    MySQL                         │    │
│  │  Users, Helpers, Tasks, Messages, Payments...   │    │
│  └─────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────┘
```

## Directory Structure

```
backend/
├── app/
│   ├── Http/
│   │   ├── Controllers/        # Request handlers
│   │   │   ├── AuthController.php
│   │   │   ├── TaskController.php
│   │   │   ├── MessageController.php
│   │   │   └── ...
│   │   ├── Middleware/         # Request filters
│   │   │   └── AdminMiddleware.php
│   │   └── Requests/           # Form validation
│   │
│   ├── Livewire/               # Reactive components
│   │   ├── Dashboards/
│   │   │   ├── UserDashboard.php
│   │   │   ├── HelpMateDashboard.php
│   │   │   └── AdminDashboard.php
│   │   ├── Tasks/
│   │   │   ├── PostTask.php
│   │   │   ├── BrowseTasks.php
│   │   │   └── TaskStatus.php
│   │   ├── Shop/
│   │   │   ├── Marketplace.php
│   │   │   ├── Cart.php
│   │   │   └── Checkout.php
│   │   └── ...
│   │
│   ├── Models/                 # Eloquent models
│   │   ├── User.php
│   │   ├── Helper.php
│   │   ├── Task.php
│   │   ├── Message.php
│   │   └── ...
│   │
│   └── Services/               # Business logic
│       ├── GeminiService.php
│       ├── QdrantService.php
│       └── RagService.php
│
├── routes/
│   ├── api.php                 # REST API routes
│   └── web.php                 # Web/Livewire routes
│
├── database/
│   ├── migrations/             # Schema changes
│   └── seeders/                # Sample data
│
├── resources/
│   ├── views/
│   │   ├── livewire/           # Component views
│   │   └── components/         # Reusable blade components
│   ├── css/                    # Tailwind styles
│   └── js/                     # JavaScript
│
└── config/                     # Configuration files
```

## Authentication Flow

ShareStrength uses Laravel Sanctum for authentication with three separate guards:

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  PWD Guard  │     │HelpMate Guard│    │ Admin Guard │
│    (pwd)    │     │  (helpmate)  │    │   (admin)   │
└─────────────┘     └─────────────┘     └─────────────┘
       │                   │                   │
       └───────────────────┼───────────────────┘
                           │
                           ▼
                  ┌─────────────────┐
                  │ Sanctum Tokens  │
                  │  (API Auth)     │
                  └─────────────────┘
```

## Request Lifecycle

1. **Request** - HTTP request arrives
2. **Middleware** - Authentication and authorization checks
3. **Controller/Livewire** - Business logic execution
4. **Model** - Database operations via Eloquent
5. **Response** - JSON (API) or HTML (Livewire)

## Database Sessions

ShareStrength uses database-backed sessions for:

- User session management
- Queue job storage
- Cache storage
- Failed job tracking

## AI Integration

The chatbot uses a RAG (Retrieval Augmented Generation) architecture:

```
User Query → Embedding (Gemini) → Vector Search (Qdrant) → Context → Response (Gemini)
```

Components:

- **GeminiService** - Handles API calls to Google Gemini
- **QdrantService** - Manages vector database operations
- **RagService** - Orchestrates the RAG pipeline
