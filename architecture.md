# Todo App Architecture

## Overview
This is a full-stack todo application with a FastAPI backend and Next.js frontend. It implements multi-user task management with JWT-based authentication and user isolation.

## Backend Architecture

### Tech Stack
- **Framework**: FastAPI
- **Database**: PostgreSQL (via SQLModel)
- **Authentication**: JWT tokens with Better Auth
- **ORM**: SQLModel (SQLAlchemy + Pydantic)

### Key Components

#### 1. Main Application (`main.py`)
- Sets up the FastAPI application
- Configures CORS middleware
- Includes the tasks router
- Implements a global authentication middleware

#### 2. Routes (`routes/tasks.py`)
- Implements 6 main endpoints for task management:
  - `GET /api/{user_id}/tasks` - List all tasks for a user
  - `POST /api/{user_id}/tasks` - Create a new task
  - `GET /api/{user_id}/tasks/{id}` - Get a specific task
  - `PUT /api/{user_id}/tasks/{id}` - Update a specific task
  - `DELETE /api/{user_id}/tasks/{id}` - Delete a specific task
  - `PATCH /api/{user_id}/tasks/{id}/complete` - Toggle task completion status
- Implements user ID validation and authentication checks
- Enforces user isolation (users can only access their own tasks)

#### 3. Models (`models/task.py`)
- Defines four Pydantic models:
  - `TaskBase`: Base class with common fields
  - `Task`: Database model with SQLAlchemy table definition
  - `TaskCreate`: Model for creating new tasks
  - `TaskUpdate`: Model for updating tasks
  - `TaskRead`: Model for reading tasks (includes ID and timestamps)
- Implements field validation (title length, description length, user ID format)

#### 4. Services (`services/task_service.py`)
- Contains business logic for task operations:
  - `create_task`: Creates a new task for a user
  - `get_tasks_by_user`: Retrieves all tasks for a specific user
  - `get_task_by_id_and_user`: Gets a specific task by ID and user ID
  - `update_task_by_id_and_user`: Updates a specific task
  - `delete_task_by_id_and_user`: Deletes a specific task
  - `toggle_task_completion`: Toggles the completion status of a task

#### 5. Database (`database.py`)
- Configures the database connection using SQLModel
- Sets up connection pooling
- Provides a session generator for dependency injection

#### 6. Authentication
- Uses JWT-based authentication with Better Auth
- Implements user isolation to prevent cross-user access
- Validates user ID in token matches user ID in URL

## Frontend Architecture

### Tech Stack
- **Framework**: Next.js 14+ with App Router
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **Authentication**: Better Auth

### Key Components

#### 1. Pages
- `page.tsx`: Home page that redirects to dashboard or signin based on auth status
- `dashboard/page.tsx`: Main dashboard showing user's tasks with CRUD functionality
- `signin/page.tsx`: Signin page
- `signup/page.tsx`: Signup page

#### 2. Components
- `TaskForm`: Form for creating new tasks
- `TaskEditForm`: Form for editing existing tasks
- `TaskItem`: Individual task display with edit/delete/toggle functionality
- `TaskList`: List of tasks
- `LoadingSpinner`: Loading indicator component
- `ErrorMessage`: Error display component

#### 3. Libraries
- `auth-context.tsx`: Authentication context provider
- `auth-storage.ts`: Utilities for managing auth tokens in localStorage
- `api-client.ts`: Centralized API client with JWT injection

#### 4. Models
- `task.ts`: TypeScript interfaces for Task, TaskCreate, and TaskUpdate

## Security Features
- JWT-based authentication
- User isolation (each user can only access their own tasks)
- Input validation and sanitization
- Secure token storage in localStorage

## API Design
- RESTful API design
- Proper HTTP status codes
- Consistent error handling
- Input validation at multiple levels

## Testing
- Comprehensive test suite covering all major functionality
- Tests for CRUD operations, authentication, and user isolation
- Health check endpoint