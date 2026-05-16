# README — Database Architecture Documentation

## AI Interview & Learning Platform — MySQL Database Design

---

# 1. Overview

This document describes the database architecture of the **AI Interview & Learning Platform**.

The system is designed to support:

* AI-powered interview simulation
* CV analysis & OCR
* Personalized learning paths
* Gamification
* Community interaction
* RAG (Retrieval-Augmented Generation)
* AI prompt management
* Monitoring & analytics
* Admin management

The architecture is separated into multiple domains for scalability and maintainability.

---

# 2. Database Technology Stack

| Component         | Technology                                                  |
| ----------------- | ----------------------------------------------------------- |
| Database          | MySQL                                                       |
| ORM / Query Layer | SQLAlchemy / Prisma / TypeORM (depending on implementation) |
| Cache             | Redis                                                       |
| Vector Database   | Weaviate                                                    |
| Object Storage    | MinIO                                                       |
| Monitoring        | Prometheus + Grafana                                        |
| AI Observability  | Langfuse                                                    |

---

# 3. Database Domains

The database is divided into the following modules:

| Domain                           | Purpose                                |
| -------------------------------- | -------------------------------------- |
| Authentication & User Management | User accounts and permissions          |
| CV & Skill Analysis              | Resume processing and skill extraction |
| AI Interview System              | AI-generated interviews and reports    |
| Learning System                  | Courses, lessons, quizzes, progress    |
| Community System                 | Forums, comments, notes                |
| RAG & AI System                  | Vector embeddings and AI prompts       |
| Admin & Monitoring               | System logs and metrics                |

---

# 4. Authentication & User Management

---

## USERS

Main user table storing account information.

### Purpose

* Authentication
* Authorization
* User identity

### Fields

| Field         | Type     | Description           |
| ------------- | -------- | --------------------- |
| id            | UUID     | Primary key           |
| email         | String   | User email            |
| password_hash | String   | Encrypted password    |
| full_name     | String   | Full name             |
| role          | String   | User/Admin role       |
| created_at    | Datetime | Account creation time |

### Relationships

* One user has one preference
* One user has many subscriptions
* One user has many interview sessions

---

## USER_PREFERENCES

Stores user customization settings.

### Purpose

* Language settings
* Interview preferences
* Difficulty level

### Fields

| Field           | Description        |
| --------------- | ------------------ |
| language        | Preferred language |
| interview_level | Difficulty level   |

---

## SUBSCRIPTIONS

Stores premium plan information.

### Purpose

* Billing
* Subscription management
* Feature access control

### Fields

| Field  | Description          |
| ------ | -------------------- |
| plan   | Free / Pro / Premium |
| status | Active / Expired     |

---

# 5. CV & Skill Analysis System

---

## CV_PROFILES

Stores uploaded resumes and extracted content.

### Purpose

* Resume storage
* OCR analysis
* Skill extraction

### Features

* AI resume parsing
* Skill recommendation
* Candidate profiling

### Fields

| Field            | Description         |
| ---------------- | ------------------- |
| resume_url       | File storage path   |
| extracted_skills | AI extracted skills |

---

## SKILLS

Stores standardized skill names.

### Examples

* Python
* React
* Machine Learning
* SQL

---

## USER_SKILLS

Many-to-many mapping between users and skills.

### Purpose

* Skill tracking
* Personalized recommendations

---

# 6. AI Interview System

---

## INTERVIEW_SESSIONS

Stores interview sessions.

### Purpose

* Interview lifecycle tracking
* Multi-mode interviews

### Supported Modes

* Standard Interview
* Zen Interview
* Meet Interview

---

## QUESTIONS

Stores AI-generated interview questions.

### Purpose

* Technical questions
* Behavioral questions
* Dynamic questioning

### Fields

| Field      | Description            |
| ---------- | ---------------------- |
| category   | Technical / Behavioral |
| difficulty | Easy / Medium / Hard   |

---

## ANSWERS

Stores candidate responses.

### Purpose

* AI scoring
* Communication evaluation
* Technical assessment

### Metrics

* Technical score
* Communication score

---

## INTERVIEW_REPORTS

Stores final interview analysis.

### Purpose

* Candidate feedback
* AI recommendations
* Performance analysis

### Generated Content

* Strengths
* Weaknesses
* Improvement roadmap

---

# 7. Learning System

---

## LEARNING_PATHS

Stores personalized learning plans.

### Purpose

* AI-generated learning roadmap
* Career-based recommendations

---

## COURSES

Stores available learning courses.

### Examples

* System Design
* React Basics
* Machine Learning

---

## LESSONS

Stores lesson content inside courses.

### Purpose

* Structured learning modules

---

## QUIZZES

Stores lesson quizzes.

### Purpose

* Knowledge assessment

---

## USER_PROGRESS

Tracks learning progress.

### Metrics

* Completion percentage
* Updated timestamp

---

## GAMIFICATION

Stores gamification information.

### Purpose

* User engagement
* XP system
* Level progression

### Metrics

* XP
* Levels
* Achievement tracking

---

# 8. Community System

---

## FORUM_POSTS

Stores discussion posts.

### Purpose

* Community interaction
* Knowledge sharing

---

## FORUM_COMMENTS

Stores comments under forum posts.

### Purpose

* Discussion threads

---

## NOTES

Stores personal notes.

### Purpose

* Learning annotations
* Interview preparation notes

---

# 9. RAG & AI System

---

## RAG_DOCUMENTS

Stores knowledge base documents.

### Purpose

* Retrieval-Augmented Generation
* Semantic search

### Examples

* PDFs
* Learning materials
* Internal documentation

---

## VECTOR_EMBEDDINGS

Stores vector embedding metadata.

### Purpose

* AI semantic retrieval
* Vector search indexing

### Connected To

* Weaviate vector database

---

## AI_PROMPTS

Stores AI system prompts.

### Purpose

* Prompt engineering
* AI workflow customization

---

# 10. Admin & Monitoring System

---

## ADMIN_LOGS

Stores admin activities.

### Purpose

* Audit logging
* Security tracking

### Examples

* Delete user
* Update course
* Change AI settings

---

## SYSTEM_METRICS

Stores system monitoring metrics.

### Purpose

* Performance monitoring
* Infrastructure analytics

### Metrics

* CPU usage
* Memory usage
* Response time

---

# 11. Database Relationships

## Core Relationships

| Relationship                      | Type        |
| --------------------------------- | ----------- |
| USERS → INTERVIEW_SESSIONS        | One-to-Many |
| INTERVIEW_SESSIONS → QUESTIONS    | One-to-Many |
| QUESTIONS → ANSWERS               | One-to-Many |
| USERS → LEARNING_PATHS            | One-to-Many |
| COURSES → LESSONS                 | One-to-Many |
| USERS → FORUM_POSTS               | One-to-Many |
| RAG_DOCUMENTS → VECTOR_EMBEDDINGS | One-to-Many |

---

# 12. System Workflow

## User Flow

```text
User Login
   ↓
Upload CV
   ↓
AI Resume Analysis
   ↓
Start AI Interview
   ↓
AI Question Generation
   ↓
Answer Evaluation
   ↓
Interview Report
   ↓
Personalized Learning Path
```

---

# 13. AI Architecture Integration

## AI Components

| Component    | Purpose                   |
| ------------ | ------------------------- |
| LangGraph    | Multi-agent orchestration |
| OpenAI / LLM | AI generation             |
| RAG Pipeline | Knowledge retrieval       |
| Weaviate     | Vector similarity search  |
| Redis        | Session cache             |
| Langfuse     | AI tracing                |

---

# 14. Scalability Considerations

The system is designed for scalability:

### Features

* Modular architecture
* Domain separation
* Vector database integration
* AI observability
* Async processing
* Real-time analytics

---

# 15. Security Considerations

### Security Features

| Feature            | Purpose                   |
| ------------------ | ------------------------- |
| Password hashing   | Credential protection     |
| Role-based access  | Admin/User separation     |
| Audit logs         | Activity tracking         |
| JWT Authentication | Secure session management |

---

# 16. Future Improvements

Potential future enhancements:

* Multi-tenant architecture
* AI agent memory
* Real-time collaboration
* Video interview analysis
* Speech emotion detection
* Recommendation engine
* Kubernetes deployment

---

# 17. Conclusion

This database architecture provides a scalable foundation for:

* AI Interview Simulation
* Personalized Learning
* Community Collaboration
* AI Knowledge Retrieval
* Enterprise Monitoring

The system combines:

* relational data (MySQL),
* vector search (Weaviate),
* caching (Redis),
* and AI orchestration (LangGraph)

to build a modern AI-powered interview and learning platform.
