# Data Modelling — Tables and ER Diagram

This section outlines the database schema used in the project, detailing each table, its fields, and their use cases. The schema is implemented using Prisma ORM with a PostgreSQL datasource.

---

## User Table

| Field | Type | Constraints | Description / Use Case |
|-------|------|--------------|-------------------------|
| `id` | String (UUID) | Primary Key | Unique identifier for each user. |
| `name` | String (Optional) |  | User’s full name for display. |
| `email` | String | Unique, Indexed | Used for authentication and identification. |
| `role` | String | User and Admin Role
| `password` | String |  | Securely stored hashed password. |
| `createdAt` | DateTime | Default: now() | Timestamp when the user was created. |
| `updatedAt` | DateTime | Auto-updated | Timestamp for last update. |
| `deletedAt` | DateTime | Auto-updated | Timestamp for last delete. |

---

## Project Table

| Field | Type | Constraints | Description / Use Case |
|-------|------|--------------|-------------------------|
| `id` | String (UUID) | Primary Key | Unique project identifier. |
| `name` | String |  | Project name displayed in UI. |
| `gitUrl` | String | Indexed | GitHub repository URL linked to the project. |
| `userId` | String | Foreign Key → `User.id` | Identifies the owner of the project. |
| `createdAt` | DateTime | Default: now() | Project creation timestamp. |
| `githubToken` | String (Optional) |  | Hashed token for GitHub integration, allowing deployments via repositories. |
| `updatedAt` | DateTime | Auto-updated | Timestamp for last update. |
| `deletedAt` | DateTime | Auto-updated | Timestamp for last delete. |

---

## Deployment Table

| Field | Type | Constraints | Description / Use Case |
|-------|------|--------------|-------------------------|
| `id` | String (UUID) | Primary Key | Unique deployment identifier. |
| `projectId` | String | Foreign Key → `Project.id` | Identifies the project this deployment belongs to. |
| `status` | String | Indexed, Default: `QUEUED` | Deployment lifecycle state (`QUEUED`, `RUNNING`, `COMPLETED`, `FAILED`). |
| `createdAt` | DateTime | Default: now() | Timestamp of deployment creation. |
| `updatedAt` | DateTime | Auto-updated | Timestamp for last update. |
| `deletedAt` | DateTime | Auto-updated | Timestamp for last delete. |

---

## Relationships Summary

- **User → Project**: One-to-Many (a user can own multiple projects).
- **Project → Deployment**: One-to-Many (a project can have multiple deployments).

---

## Entity Relationship Diagram

```mermaid
erDiagram
    USER {
        string id PK
        string name
        string email UK
        string role
        string password
        datetime createdAt
        datetime updatedAt
    }

    PROJECT {
        string id PK
        string name
        string git_url
        string user_id FK
        datetime createdAt
        datetime updatedAt
    }

    DEPLOYMENT {
        string id PK
        string project_id FK
        string status
        datetime createdAt
        datetime updatedAt
    }

    USER ||--o{ PROJECT : owns
    PROJECT ||--o{ DEPLOYMENT : has
```