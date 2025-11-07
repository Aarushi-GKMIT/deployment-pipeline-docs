# Functional Use Cases

This section defines the main **functional behaviors** of the Deployment Pipeline.  
Each use case explains how different system components interact internally to deliver a key feature.

---

## 1. User Authentication and Authorization

**Goal:**  
Provide secure sign-up and login for users.

**Flow:**
1. User sends a sign-up or login request to the API server.
2. Server validates input and checks the database for existing users.
3. Passwords are hashed and stored securely.
4. On successful login, a JWT token is generated and returned.
5. Subsequent requests include this token for authorization.

**System Behavior:**
- Validations ensure unique emails and secure passwords.  
- Tokens are time-bound and refreshable.  
- Unauthorized access attempts return appropriate errors.

---

## 2. Project Management

**Goal:**  
Allow authenticated users to create and manage deployable projects linked to GitHub repositories.

**Flow:**
1. User provides project name and GitHub repo URL.  
2. API validates the URL format and ensures it’s accessible.  
3. A project entry is created in the database and linked to the user.  
4. Frontend displays the project dashboard with deployment options.

**System Behavior:**
- Ensures no duplicate project names for a single user.  
- Maintains relationship between users, projects, and deployments.  
- Prepares metadata for future deployment builds.

---

## 3. Build and Deployment Execution

**Goal:**  
Automate the build and deployment process when a user triggers a deployment.

**Flow:**
1. User triggers **Deploy** from the dashboard.  
2. API creates a deployment record and pushes a build task to ECS.  
3. The ECS container clones the GitHub repository from the provided URL.  
4. The build process (`npm install`, `npm run build`, etc.) runs inside the container.  
5. Build artifacts are uploaded to an S3 bucket.  
6. Deployment status (`queued`, `running`, `success`, `failed`) is updated in the database.

**System Behavior:**
- Uses AWS ECS for isolated build execution.  
- Logs are collected and streamed back to the backend.  
- On successful build, an accessible URL is generated via CloudFront or reverse proxy.

---

## 4. Reverse Proxy and File Streaming

**Goal:**  
Deliver the deployed React app to end users efficiently.

**Flow:**
1. User accesses the generated deployment URL.  
2. Reverse proxy receives the request and retrieves static assets from the S3 bucket.  
3. The files are streamed back to the client’s browser.



