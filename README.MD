# AI Interview Platform — Database Architecture Documentation

## Overview

This document describes the database architecture design for the AI Interview Platform system.

The platform is designed to support:

- AI-powered interview simulation
- CV analysis & OCR processing
- Personalized learning roadmap
- Community interaction
- Gamification
- RAG (Retrieval-Augmented Generation)
- Admin management & monitoring

The database architecture is organized into multiple modules to ensure:

- Scalability
- Maintainability
- High cohesion
- Separation of concerns
- AI system extensibility

---

# Database Technology

| Component | Technology |
|---|---|
| Main Database | MySQL |
| Vector Database | Weaviate |
| Cache Layer | Redis |
| Object Storage | MinIO |
| Monitoring | Prometheus + Grafana |
| AI Observability | Langfuse |

---

# Database Modules

The database is divided into the following logical modules:

1. User & Authentication System
2. CV & Interview System
3. Learning System
4. Community System
5. RAG & AI System
6. Admin & Monitoring System

---

# 1. USER & AUTH SYSTEM

This module manages:

- User accounts
- Authentication
- User preferences
- Subscription plans

---

## USERS

Main user table storing platform account information.

### Purpose
- Store user identities
- Manage login accounts
- Role management

### Fields

| Field | Type | Description |
|---|---|---|
| id | uuid | Primary key |
| email | string | User email |
| password_hash | string | Encrypted password |
| full_name | string | Full name |
| role | string | User role |
| created_at | datetime | Account creation time |

---

## USER_PREFERENCES

Stores user customization settings.

### Purpose
- Save learning preferences
- Save interview level preferences

### Fields

| Field | Type |
|---|---|
| id | uuid |
| user_id | uuid |
| language | string |
| interview_level | string |

---

## SUBSCRIPTIONS

Stores premium subscription information.

### Purpose
- Manage paid plans
- Track subscription status

### Fields

| Field | Type |
|---|---|
| id | uuid |
| user_id | uuid |
| plan | string |
| status | string |

---

# 2. CV & INTERVIEW SYSTEM

Core AI interview module.

Handles:
- Resume upload
- Interview sessions
- AI-generated questions
- AI evaluation

---

## CV_PROFILES

Stores uploaded CV data.

### Purpose
- Resume parsing
- OCR extraction
- Skill analysis

### Fields

| Field | Type |
|---|---|
| id | uuid |
| user_id | uuid |
| resume_url | string |
| extracted_skills | text |

---

## INTERVIEW_SESSIONS

Stores interview sessions.

### Purpose
- Track interview process
- Save interview metadata

### Fields

| Field | Type |
|---|---|
| id | uuid |
| user_id | uuid |
| mode | string |
| status | string |
| started_at | datetime |

---

## QUESTIONS

Stores AI-generated interview questions.

### Purpose
- Save interview questions
- Categorize question types

### Fields

| Field | Type |
|---|---|
| id | uuid |
| session_id | uuid |
| question_text | text |
| category | string |

---

## ANSWERS

Stores user answers.

### Purpose
- Evaluate interview performance
- Analyze communication ability

### Fields

| Field | Type |
|---|---|
| id | uuid |
| question_id | uuid |
| answer_text | text |
| technical_score | float |
| communication_score | float |

---

## INTERVIEW_REPORTS

Stores AI-generated interview feedback reports.

### Purpose
- Final scoring
- AI recommendations
- Improvement suggestions

### Fields

| Field | Type |
|---|---|
| id | uuid |
| session_id | uuid |
| overall_score | float |
| recommendations | text |

---

# 3. LEARNING SYSTEM

This module manages personalized education and learning.

---

## LEARNING_PATHS

Stores personalized learning roadmaps.

### Purpose
- AI-generated learning plans
- Career preparation roadmap

### Fields

| Field | Type |
|---|---|
| id | uuid |
| user_id | uuid |
| title | string |
| target_role | string |

---

## COURSES

Stores course information.

### Fields

| Field | Type |
|---|---|
| id | uuid |
| title | string |
| difficulty | string |

---

## LESSONS

Stores lessons inside courses.

### Fields

| Field | Type |
|---|---|
| id | uuid |
| course_id | uuid |
| title | string |

---

## QUIZZES

Stores quizzes for lessons.

### Purpose
- Knowledge testing
- Skill evaluation

### Fields

| Field | Type |
|---|---|
| id | uuid |
| lesson_id | uuid |
| title | string |

---

## USER_PROGRESS

Tracks user learning progress.

### Fields

| Field | Type |
|---|---|
| id | uuid |
| user_id | uuid |
| lesson_id | uuid |
| progress_percent | float |

---

## GAMIFICATION

Stores gamification data.

### Purpose
- Motivation system
- XP & leveling

### Fields

| Field | Type |
|---|---|
| id | uuid |
| user_id | uuid |
| xp | integer |
| level | integer |

---

# 4. COMMUNITY SYSTEM

Handles social interaction inside the platform.

---

## FORUM_POSTS

Stores forum discussions.

### Fields

| Field | Type |
|---|---|
| id | uuid |
| user_id | uuid |
| title | string |

---

## FORUM_COMMENTS

Stores post comments.

### Fields

| Field | Type |
|---|---|
| id | uuid |
| post_id | uuid |
| user_id | uuid |

---

## NOTES

Stores personal notes.

### Fields

| Field | Type |
|---|---|
| id | uuid |
| user_id | uuid |
| title | string |

---

# 5. RAG & AI SYSTEM

AI knowledge retrieval system.

---

## RAG_DOCUMENTS

Stores AI knowledge documents.

### Purpose
- AI context retrieval
- Knowledge indexing

### Fields

| Field | Type |
|---|---|
| id | uuid |
| title | string |
| content | text |

---

## VECTOR_EMBEDDINGS

Stores vector embedding references.

### Purpose
- Semantic search
- Vector retrieval

### Fields

| Field | Type |
|---|---|
| id | uuid |
| document_id | uuid |
| embedding_model | string |

---

## AI_PROMPTS

Stores prompt templates.

### Purpose
- Prompt engineering
- AI configuration

### Fields

| Field | Type |
|---|---|
| id | uuid |
| prompt_name | string |
| prompt_content | text |

---

# 6. ADMIN & MONITORING SYSTEM

Administrative & observability system.

---

## ADMIN_LOGS

Stores admin activity logs.

### Purpose
- Security audit
- System tracking

### Fields

| Field | Type |
|---|---|
| id | uuid |
| admin_id | uuid |
| action | string |

---

## SYSTEM_METRICS

Stores monitoring metrics.

### Purpose
- Performance monitoring
- System health tracking

### Fields

| Field | Type |
|---|---|
| id | uuid |
| cpu_usage | float |
| memory_usage | float |
| response_time | float |

---

# Relationships Summary

| Relationship | Description |
|---|---|
| USERS → INTERVIEW_SESSIONS | User starts interviews |
| INTERVIEW_SESSIONS → QUESTIONS | Session contains questions |
| QUESTIONS → ANSWERS | Questions receive answers |
| INTERVIEW_SESSIONS → INTERVIEW_REPORTS | Session generates report |
| USERS → LEARNING_PATHS | User follows roadmap |
| COURSES → LESSONS | Course contains lessons |
| LESSONS → QUIZZES | Lesson has quizzes |
| USERS → FORUM_POSTS | User creates posts |
| RAG_DOCUMENTS → VECTOR_EMBEDDINGS | Document generates embeddings |

---

# Architecture Design Principles

The database architecture follows:

- Modular design
- AI-oriented architecture
- Scalable schema
- Separation of domains
- Cloud-native system design

---

# Future Improvements

Possible future extensions:

- Real-time analytics
- Multi-agent memory system
- AI behavioral tracking
- Speech emotion analysis
- AI recruiter benchmarking
- Distributed vector search
- Event-driven architecture

---

# Final Notes

This database architecture is designed for:

- AI Interview SaaS platforms
- AI learning assistants
- AI-powered recruitment systems
- Large-scale educational platforms

The schema is optimized for:

- Maintainability
- AI integration
- Future scalability
- Production deployment

---