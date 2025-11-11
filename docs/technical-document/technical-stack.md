# Technical Stack

This project is built using a modern and stable technology stack to ensure scalability, maintainability, and ease of deployment. Below are the choices made for each layer of the application:

## Frontend
- **React**  
  - **Version:** 18.x (Stable)  
  - **Reason:** React provides a component-based architecture and reactive UI, making it ideal for building dynamic and responsive web applications. Vite is used as the build tool for faster development and optimized production builds.

## Backend
- **Node.js + Express**  
  - **Version:** Node.js 20.x (LTS), Express 4.x (Stable)  
  - **Reason:** Node.js allows building scalable server-side applications using JavaScript. Express is lightweight and provides a flexible framework for handling APIs and routing.

## Database
- **Prisma ORM with PostgreSQL**  
  - **Version:** Prisma 5.x, PostgreSQL 16.x  
  - **Reason:** Prisma provides a type-safe ORM for database operations, reducing errors and improving development speed. PostgreSQL is a reliable relational database with strong consistency and support for complex queries.

## Authentication & Authorization
- **JWT (JSON Web Token)**  
  - **Version:** 9.x (Stable)  
  - **Reason:** JWT allows stateless authentication, making it easy to manage sessions securely for users and support role-based access (Admin/User).

## Cloud Services
- **AWS ECS (Elastic Container Service)**  
  - **Reason:** Hosts Docker containers and scales applications automatically.  
- **AWS ECR (Elastic Container Registry)**  
  - **Reason:** Stores Docker images securely and integrates with ECS for deployments.  
- **AWS S3 (Simple Storage Service)**  
  - **Reason:** Stores static files (HTML, CSS, JS) and serves them efficiently via a reverse proxy.  

## Other Tools & Libraries
- **Bcrypt** for password hashing (ensures secure storage of user credentials)  
- **Zod** for input validation (ensures backend receives validated and structured data)  
- **Cors** middleware for handling cross-origin requests  
- **dotenv** for managing environment variables securely  

---

This tech stack is chosen for its stability, support, and ease of integration with AWS for containerized deployments.
