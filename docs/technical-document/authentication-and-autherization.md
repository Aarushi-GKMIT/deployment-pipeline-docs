# Authentication and Authorization

## Overview
This document describes the authentication and authorization mechanisms implemented in the project. The system uses JWT-based authentication with role-based access control (RBAC) to secure endpoints and features.

---

## Authentication

### Authentication Methods

The system supports two authentication methods:

1. **Email/Password Authentication**
2. **GitHub Token Authentication**

### Signup Process

#### User Registration
Users can register with the following information:

- **Name**: Full name of the user
- **Email**: Valid email address (unique)
- **Password**: Secure password (hashed before storage)
- **Role**: User role (`ADMIN` or `USER`)

#### Signup Flow
```mermaid
sequenceDiagram
    participant Client
    participant API
    participant Database
    participant JWT

    Client->>API: POST /auth/signup
    Note over Client,API: {name, email, password, role}
    API->>API: Validate input data
    API->>API: Hash password
    API->>Database: Create user record
    Database-->>API: User created
    API->>JWT: Generate JWT token
    JWT-->>API: Return token
    API-->>Client: 201 Created {token, user}
```

### Login Process

#### User Login
Users authenticate using:

- **Email**: Registered email address
- **Password**: User password

#### Login Flow
```mermaid
sequenceDiagram
    participant Client
    participant API
    participant Database
    participant JWT

    Client->>API: POST /auth/login
    Note over Client,API: {email, password}
    API->>Database: Find user by email
    Database-->>API: User data
    API->>API: Verify password hash
    alt Password Valid
        API->>JWT: Generate JWT token
        JWT-->>API: Return token
        API-->>Client: 200 OK {token, user}
    else Password Invalid
        API-->>Client: 401 Unauthorized
    end
```

---

### Complete Authentication Flow
```mermaid
sequenceDiagram
    participant Client
    participant API
    participant Auth Middleware
    participant Database

    Client->>API: Request with JWT
    API->>Auth Middleware: Validate token
    Auth Middleware->>Auth Middleware: Verify signature
    Auth Middleware->>Auth Middleware: Check expiration
    
    alt Token Valid
        Auth Middleware->>Database: Get user data
        Database-->>Auth Middleware: User data
        Auth Middleware->>API: Attach user to request
        API->>API: Process request
        API-->>Client: Response
    else Token Invalid/Expired
        Auth Middleware-->>Client: 401 Unauthorized
    end
```

---

### GitHub Token Authentication

Users can authenticate using GitHub personal access tokens for project creation and management.

#### GitHub Auth Flow
```mermaid
sequenceDiagram
    participant Client
    participant API
    participant GitHub

    Client->>API: Request with GitHub Token
    Note over Client,API: Header: Authorization: Bearer {gitToken}
    API->>GitHub: Validate token
    GitHub-->>API: Token valid/invalid
    alt Token Valid
        API->>API: Process request
        API-->>Client: 200 OK with data
    else Token Invalid
        API-->>Client: 401 Unauthorized
    end
```



---


## Authorization

### Role-Based Access Control (RBAC)

The system implements role-based access control with two primary roles:

1. **ADMIN**: Full access to all features
2. **USER**: Limited access based on permissions

### Admin Flow


#### Admin Capabilities

After signup with role `ADMIN`, administrators can:

1. **Create Projects**
    <br>
        Project name
   <br>
        Git URL
   <br>
        Git token

2. **Deploy Projects**
    <br>
        Single button deployment
    <br>
        Full deployment control

3. **Grant Permissions**
    <br>
        View list of all users
    <br>
        Grant/revoke access to specific users
    <br>
        Manage user permissions per project



#### Admin Authorization Flow
```mermaid
sequenceDiagram
    participant Admin
    participant API
    participant Database

    Admin->>API: Create Project
    Note over Admin,API: {name, gitURL, gitToken}
    API->>API: Verify JWT (role=ADMIN)
    API->>Database: Create project
    Database-->>API: Project created
    
    Admin->>API: Grant permission to user
    Note over Admin,API: {userId, projectId}
    API->>API: Verify JWT (role=ADMIN)
    API->>Database: Create permission record
    Database-->>API: Permission granted
    API-->>Admin: Success
```

### User Flow

#### User Capabilities

After signup with role `USER`, regular users have:

1. **Limited Access**
<br>
   Can only access projects they're granted permission to
<br>
   Cannot create new projects
<br>
   Cannot grant permissions to others

2. **Permission-Based Actions**
<br>
   **With Permission**: Can deploy assigned projects
<br>
   **Without Permission**: Receive unauthorized access error

#### User Authorization Flow
```mermaid
sequenceDiagram
    participant User
    participant API
    participant Database

    User->>API: Access/Deploy Project
    Note over User,API: projectId in request
    API->>API: Verify JWT (role=USER)
    API->>Database: Check permission
    Note over API,Database: WHERE userId AND projectId
    
    alt Has Permission
        Database-->>API: Permission found
        API->>API: Process deployment
        API-->>User: 200 OK - Deploy initiated
    else No Permission
        Database-->>API: Permission not found
        API-->>User: 403 Forbidden - Unauthorized Access
    end
```

---



