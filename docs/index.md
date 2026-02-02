# ShareStrength

**Connecting People With Disabilities with HelpMates**

ShareStrength is a comprehensive Laravel-based web application designed to connect People With Disabilities (PWD) with HelpMates (caregivers) for task-based assistance.

## Overview

The platform enables:

- **PWD Users** to post tasks they need help with
- **HelpMates** to browse and apply for tasks
- **Secure messaging** between users
- **Payment tracking** for completed services
- **Accessibility features** including TTS, STT, and high contrast modes

## Problem Definition

Individuals with disabilities frequently experience a heavy reliance on family members or friends for daily assistance due to a lack of specialized, accessible support services. Despite recent technological advancements, there is still a notable absence of centralized and reliable platforms dedicated to the diverse needs of this community 

This lack of structured support leads to several core issues:
**Reduced Autonomy**: Individuals with disabilities face limited independence in their daily lives
**Caregiver Burden**: Families and informal caregivers often face significant emotional and logistical challenges
**Employment Gaps**: There are limited professional opportunities for those who wish to provide specialized assistance in this sector

## Motivation and Solution

The primary motivation for ShareStrength is to leverage modern technology to build an inclusive and empowering ecosystem that bridges these gaps. The platform is designed to provide seamless, dignified access to services and resources that foster independence.

Our solution focuses on these key objectives:
**Reducing Dependency**: By connecting users to verified HelpMates, we reduce the total reliance on personal or informal networks.
**Promoting Dignity**: Easy access to tailored support services allows users to maintain greater autonomy.
**Professional Opportunity**: The platform provides structured employment for individuals dedicated to assisting persons with special needs.
**Community Connection**: We aim to build a supportive community that shares resources and peer support.


## Technology Stack

| Component | Technology |
|-----------|------------|
| Backend | Laravel 12 (PHP 8.2+) |
| Frontend | Blade + Livewire 4 |
| Styling | Tailwind CSS 4 |
| Database | MySQL |
| Authentication | Laravel Sanctum |
| AI Integration | Google Gemini API |
| Vector Database | Qdrant (RAG system) |

## Quick Links

- [Getting Started](getting-started.md) - Installation and setup guide
- [Features](features.md) - Complete feature overview
- [API Reference](api/index.md) - REST API documentation
- [Database Models](database/models.md) - Data structure reference

## Project Structure

```
backend/
├── app/
│   ├── Http/Controllers/    # API and web controllers
│   ├── Livewire/            # Reactive UI components
│   ├── Models/              # Eloquent models
│   └── Services/            # Business logic (RAG, Gemini)
├── routes/
│   ├── api.php              # API routes (v1)
│   └── web.php              # Web routes (Livewire)
├── database/
│   └── migrations/          # Schema definitions
├── resources/
│   └── views/               # Blade templates
└── config/                  # Configuration files
```

## User Types

ShareStrength supports three distinct user roles:

1. **PWD (People With Disabilities)** - Service requesters who post tasks
2. **HelpMate** - Caregivers who apply for and complete tasks
3. **Admin** - System administrators who manage resources and users
