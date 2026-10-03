# TaskForge

**TaskForge is a full-stack project and issue management platform inspired by modern Agile tools such as Jira.**

The project is being built from scratch with the goal of creating a realistic, production-oriented application while exploring the complete lifecycle of a modern software system — from database design and backend development to frontend development, automated testing, CI/CD, containerization, and Kubernetes.

> **The goal is not to build a copy of Jira, but to understand how a real-world application is designed, built, tested, secured, deployed, and maintained.**

---

## 🎯 Project Goal

TaskForge is designed as a practical software engineering project focused on building a scalable and maintainable project management platform.

The application allows teams to:

* create and manage projects
* manage project members and roles
* create and assign issues
* track issue status and priority
* communicate through comments
* track changes and activity
* manage access through authentication and authorization

The project is intentionally developed incrementally, with each technology introduced to solve a specific problem rather than being added simply for the sake of using more tools.

---

# 🏗️ Architecture

The planned architecture consists of several major components:

```text
                         ┌──────────────────┐
                         │      React       │
                         │    Frontend      │
                         └────────┬─────────┘
                                  │
                                  │ REST API
                                  ▼
                         ┌──────────────────┐
                         │   Spring Boot    │
                         │     Backend      │
                         └───────┬──────────┘
                                 │
                  ┌──────────────┼──────────────┐
                  │              │              │
                  ▼              ▼              ▼
           ┌────────────┐  ┌───────────┐  ┌────────────┐
           │ PostgreSQL │  │   Kafka   │  │   Redis*   │
           │  Database  │  │ Messaging │  │   Cache    │
           └────────────┘  └───────────┘  └────────────┘

                     * Optional future component
```

Infrastructure will later be extended with:

```text
GitHub Actions
      ↓
     CI/CD
      ↓
   Docker
      ↓
 Kubernetes
      ↓
Prometheus + Grafana
```

---

# 🔐 Authentication & Authorization

TaskForge uses **Spring Security and JWT-based authentication**.

The authentication flow is designed around:

```text
User
 │
 ├── Register
 │
 └── Login
       │
       ▼
   JWT Tokens
       │
       ▼
Authenticated API Requests
       │
       ▼
Spring Security
       │
       ▼
Authorization
```

The system will support:

* user registration
* secure password hashing
* login
* access tokens
* refresh tokens
* authentication
* authorization
* role-based access
* project-level permissions

The goal is to distinguish between **authentication** ("Who are you?") and **authorization** ("What are you allowed to do?").

---

# 📁 Project Management

Users can belong to multiple projects.

Each project can contain multiple members with different roles.

Example:

```text
Project: TaskForge

├── Nikola
│   └── ADMIN
│
├── Ivan
│   └── DEVELOPER
│
└── Ana
    └── VIEWER
```

Project membership and roles are used to determine which operations a user is allowed to perform.

---

# 📝 Issue Management

Issues are the core work items inside a project.

An issue can contain information such as:

* title
* description
* status
* priority
* reporter
* assignee
* due date
* project
* creation date
* modification date

Example lifecycle:

```text
TODO
  ↓
IN_PROGRESS
  ↓
IN_REVIEW
  ↓
DONE
```

The exact workflow and business rules will evolve during development.

---

# 💬 Collaboration

TaskForge will support collaboration through:

* comments
* issue assignments
* activity tracking
* issue history
* audit information

The goal is to make changes to important project data traceable rather than simply overwriting the previous state.

---

# 📨 Event-Driven Architecture

TaskForge will explore event-driven communication using **Apache Kafka**.

Kafka will not be introduced simply because it is a popular technology.

It will be used where asynchronous communication provides a meaningful architectural benefit.

Potential events include:

```text
IssueCreated
IssueUpdated
IssueAssigned
IssueStatusChanged
CommentAdded
```

Example:

```text
User changes issue status
          │
          ▼
     Spring Boot
          │
          ▼
    Kafka Event
          │
     ┌────┴─────┐
     ▼          ▼
   Audit    Notification
```

This allows additional consumers to react to events without tightly coupling every component to the original operation.

---

# 🗄️ Database

TaskForge uses **PostgreSQL** as its relational database.

The planned domain includes entities such as:

```text
User
 │
 ├── ProjectMember
 │        │
 │        └── Project
 │              │
 │              └── Issue
 │                    │
 │                    └── Comment
 │
 └── RefreshToken
```

Additional entities such as issue history and audit records may be introduced as the application evolves.

Database schema changes will be managed using **Flyway migrations**.

This allows database changes to be:

* version controlled
* reproducible
* tracked
* applied consistently across environments

---

# 🧪 Testing

Testing is an important part of the project.

The planned testing strategy includes:

### Unit Tests

Testing individual components in isolation.

### Integration Tests

Testing interactions between application components.

### Testcontainers

Using real infrastructure components inside disposable containers for integration testing.

Potential test infrastructure:

```text
JUnit
  │
  ▼
Testcontainers
  │
  ├── PostgreSQL
  │
  └── Kafka
```

The goal is to test realistic application behavior instead of relying exclusively on mocks.

---

# 📚 API Documentation

The backend will expose REST APIs documented using **OpenAPI / Swagger**.

The API documentation will provide information about:

* available endpoints
* request parameters
* request bodies
* response models
* authentication requirements
* possible errors

---

# 🖥️ Frontend

The frontend will be developed using:

* React
* TypeScript

The frontend will communicate with the Spring Boot backend through REST APIs.

Planned areas include:

* authentication
* dashboard
* project management
* issue management
* project members
* comments
* filtering
* sorting
* pagination

The frontend will be developed after the core backend has been completed.

---

# 🔄 CI/CD

After the application reaches a stable full-stack state, CI/CD will be introduced using **GitHub Actions**.

The pipeline will gradually automate:

```text
Git Push
   ↓
Build
   ↓
Tests
   ↓
Code Quality Checks
   ↓
Docker Image
   ↓
Deployment
```

The exact deployment strategy will evolve as the project progresses.

---

# 🐳 Docker

Docker will be introduced after the core application is functional.

The goal is to containerize the application and supporting infrastructure.

Potential containers:

```text
TaskForge Backend
TaskForge Frontend
PostgreSQL
Kafka
```

Docker Compose may be used for local development and multi-container environments.

---

# ☸️ Kubernetes

Kubernetes will be introduced as a later stage of the project.

The goal is to explore:

* container orchestration
* deployments
* services
* configuration
* secrets
* health checks
* scaling
* persistent storage

Kubernetes is not required for the basic application to function. It is introduced to explore how the application could be deployed and managed in a cloud-native environment.

---

# 📊 Monitoring & Observability

As the infrastructure evolves, TaskForge will introduce monitoring using:

* Prometheus
* Grafana

Potential metrics include:

* HTTP request count
* response time
* error rate
* JVM metrics
* memory usage
* CPU usage
* database connections
* Kafka-related metrics

The goal is to understand not only whether the application works, but also how it behaves while running.

---

# 🛠️ Technology Stack

## Backend

* Java
* Spring Boot
* Spring Security
* Spring Data JPA
* Hibernate
* PostgreSQL
* Flyway
* JWT
* MapStruct
* Bean Validation
* OpenAPI / Swagger

## Testing

* JUnit
* Mockito
* Spring Boot Test
* Testcontainers

## Messaging

* Apache Kafka

## Frontend

* React
* TypeScript

## DevOps & Infrastructure

* Git
* GitHub
* GitHub Actions
* Docker
* Docker Compose
* Kubernetes

## Monitoring

* Prometheus
* Grafana

## Code Quality

* SonarQube / SonarCloud

---

# 📂 Repository Structure

```text
taskforge/
│
├── backend/
│   ├── src/
│   ├── pom.xml
│   └── README.md
│
├── frontend/
│   ├── src/
│   ├── package.json
│   └── README.md
│
├── docs/
│   ├── architecture/
│   ├── database/
│   └── api/
│
├── infrastructure/
│   ├── docker/
│   └── kubernetes/
│
├── .github/
│   └── workflows/
│
├── docker-compose.yml
├── .gitignore
└── README.md
```

---

# 🗺️ Development Roadmap

## Phase 1 — Backend

Build the core backend using Spring Boot and PostgreSQL.

Focus areas:

* domain modeling
* database design
* REST API
* authentication
* authorization
* JWT
* project management
* project members
* issue management
* comments
* validation
* exception handling
* pagination and filtering
* testing
* API documentation
* Kafka integration

---

## Phase 2 — Frontend

Build the React and TypeScript frontend.

Focus areas:

* authentication
* dashboard
* projects
* issues
* comments
* project members
* filtering
* pagination
* API integration

---

## Phase 3 — CI/CD

Introduce automated development workflows.

Focus areas:

* automated builds
* automated tests
* code quality
* GitHub Actions
* Docker image creation
* deployment pipeline

---

## Phase 4 — Docker

Containerize the application and supporting services.

---

## Phase 5 — Kubernetes

Explore container orchestration, deployment and scaling.

---

## Phase 6 — Observability

Introduce:

* Prometheus
* Grafana
* application metrics
* infrastructure monitoring

---

# 📈 Future Improvements

Potential future features include:

* Kanban boards
* Sprint management
* advanced search
* file attachments
* email notifications
* real-time notifications
* WebSockets
* activity feeds
* reporting and analytics
* Redis caching
* cloud deployment

These features will only be introduced when they provide a meaningful learning or architectural benefit.

---

# 🎓 Learning Objective

TaskForge is also a practical learning project.

The objective is not simply to use as many technologies as possible.

Each technology is introduced to solve a specific problem.

For example:

| Technology      | Problem it addresses             |
| --------------- | -------------------------------- |
| Spring Boot     | Backend application development  |
| PostgreSQL      | Persistent relational data       |
| Spring Security | Authentication & authorization   |
| JWT             | Stateless authentication         |
| Flyway          | Database versioning              |
| Kafka           | Asynchronous event communication |
| Testcontainers  | Realistic integration testing    |
| React           | User interface                   |
| GitHub Actions  | Automation                       |
| Docker          | Containerization                 |
| Kubernetes      | Container orchestration          |
| Prometheus      | Metrics collection               |
| Grafana         | Monitoring and visualization     |

The project is designed to build both **practical development skills and the ability to explain architectural decisions**.

---

# 🚧 Project Status

**Currently in development.**

TaskForge is being built incrementally, starting with the Spring Boot backend.

The development process focuses on understanding each technology and architectural decision rather than simply implementing features.

---

## 👨‍💻 Development Philosophy

> **Build it. Understand it. Explain it. Improve it.**

The goal of TaskForge is to create a project that can be understood from the database layer all the way to deployment and infrastructure.

Every major architectural decision should have a clear reason behind it.
