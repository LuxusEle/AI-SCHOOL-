# The Atlas â€” Full Virtual School Platform
*Complete Technical & Product Design â€” Enhanced Edition v2.0*
**Built on the Assessly AI Grading Foundation**

---

## Table of Contents

1. [The Core Philosophy](#1-the-core-philosophy)
2. [High-Level System Architecture](#2-high-level-system-architecture)
3. [Assessly Foundation â€” What We Inherit & Extend](#3-assessly-foundation--what-we-inherit--extend)
4. [Database Schema â€” Production-Ready with RLS](#4-database-schema--production-ready-with-rls)
5. [API Contract â€” Full Specification](#5-api-contract--full-specification)
6. [Frontend Architecture â€” Deep Dive](#6-frontend-architecture--deep-dive)
7. [AI Grading Pipeline â€” Extended from Assessly](#7-ai-grading-pipeline--extended-from-assessly)
8. [AI Tutoring & Visual Explanation Engine](#8-ai-tutoring--visual-explanation-engine)
9. [UI/UX â€” Full Design Specification](#9-ui-ux--full-design-specification)
10. [Security â€” Multi-Layered](#10-security--multi-layered)
11. [Observability & DevOps](#11-observability--devops)
12. [Testing Strategy](#12-testing-strategy)
13. [Implementation Roadmap from Assessly](#13-implementation-roadmap-from-assessly)
14. [Appendices](#14-appendices)

---

## 1. The Core Philosophy

The "source of truth" is not grades â€” it is the real-time **grasp matrix**: a per-student, per-topic understanding score that drives every decision. The human teacher remains the final authority on grades and learning paths, but AI handles 80% of repetitive work (grading, question generation, feedback). This frees teachers to give their undivided attention to the **weakest students** â€” the ones who need it most â€” while advanced learners progress as quickly as they are able.

### Core Principles

| Principle | Description |
|-----------|-------------|
| **Grasp over grades** | Grades are a lagging indicator; grasp scores are a leading indicator of learning |
| **AI does the repetitive, human does the critical** | Grading, question generation, and feedback drafting are automated. Final verdict, pedagogical decisions, and 1:1 intervention stay with the teacher |
| **Voice + visuals + text â€” like a real teacher** | The AI tutor does not just chat. It speaks, draws diagrams, highlights errors visually, and explains step by step |
| **Weakest first** | The teacher's dashboard surfaces struggling students before they ask for help |
| **Continuous, low-stakes assessment** | Daily quizzes feed the grasp matrix. High-stakes exams happen monthly on top of continuous data |

### Success Metric

Success is measured not by the average grade but by the **reduction in the number of students below proficiency** in any topic. The system automatically flags these students to the tutor before they even ask for help.

---

## 2. High-Level System Architecture

The platform follows a **modular, event-driven microservices architecture** with a modern full-stack foundation. The core builds on the **Assessly** grading engine, extending it with teaching, tutoring, and question-generation layers.

### Architecture Diagram

```
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚                              CLIENT LAYER                                           â”‚
â”œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¤
â”‚   Student Web   â”‚   Student App   â”‚   Teacher Web   â”‚   Parent View (read-only)    â”‚
â”‚   (Next.js 15)  â”‚   React Native  â”‚   (Next.js 15)  â”‚   (Next.js, restricted)      â”‚
â”‚   + shadcn/ui   â”‚   + Expo        â”‚   + shadcn/ui   â”‚                              â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”´â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
         â”‚                 â”‚                  â”‚                           â”‚
         â–¼                 â–¼                  â–¼                           â–¼
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚                          API GATEWAY (Kong / Nginx)                                 â”‚
â”‚                     Auth, Rate Limiting, Request Routing, CORS                       â”‚
â”‚               Supabase Auth (JWT) -> RLS -> PostgreSQL for data queries              â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
         â”‚                 â”‚                  â”‚                           â”‚
         â–¼                 â–¼                  â–¼                           â–¼
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚                         SUPABASE (PostgreSQL 15 + Realtime + Auth)                   â”‚
â”‚      Users â”‚ Schools â”‚ Classes â”‚ Enrollments â”‚ Submissions â”‚ GraspScores             â”‚
â”‚      Questions â”‚ Assessments â”‚ Challenges â”‚ Notifications â”‚ TopicEmbeddings         â”‚
â”‚      Row Level Security on EVERY table                                                â”‚
â”‚      Realtime channels on: grasp_scores, submissions, notifications                  â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
         â”‚                 â”‚                  â”‚                           â”‚
         â–¼                 â–¼                  â–¼                           â–¼
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚                         MESSAGE BUS (Redis 7 / RabbitMQ)                             â”‚
â”‚                     Celery Broker + Result Backend + Cache                            â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
         â”‚                 â”‚                  â”‚                           â”‚
         â–¼                 â–¼                  â–¼                           â–¼
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”  â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”  â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”  â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚   Grading    â”‚  â”‚   Question   â”‚  â”‚   Tutoring   â”‚  â”‚  Analytics   â”‚
â”‚   Engine     â”‚  â”‚   Engine     â”‚  â”‚   Engine     â”‚  â”‚  Engine      â”‚
â”‚  (FastAPI)   â”‚  â”‚  (FastAPI)   â”‚  â”‚  (FastAPI)   â”‚  â”‚  (FastAPI)   â”‚
â”‚  Celery      â”‚  â”‚  Celery      â”‚  â”‚  Sync only   â”‚  â”‚  Celery      â”‚
â””â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”˜  â””â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”˜  â””â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”˜  â””â”€â”€â”€â”€â”€â”€â”¬â”€â”€â”€â”€â”€â”€â”€â”˜
       â”‚                 â”‚                  â”‚                 â”‚
       â–¼                 â–¼                  â–¼                 â–¼
â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”  â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”  â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”  â”Œâ”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”
â”‚  OCR Layer   â”‚  â”‚   RAG        â”‚  â”‚  TTS/STT     â”‚  â”‚   Object     â”‚
â”‚  PaddleOCR-  â”‚  â”‚  Knowledge   â”‚  â”‚  Whisper +   â”‚  â”‚   Storage    â”‚
â”‚  VL -> GPT-4 â”‚  â”‚  Graph       â”‚  â”‚  ElevenLabs  â”‚  â”‚   (S3/Supabase)â”‚
â”‚  Vision      â”‚  â”‚  (pgvector)  â”‚  â”‚              â”‚  â”‚              â”‚
â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜  â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜  â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜  â””â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”€â”˜
```

### Technology Stack Summary

| Layer | Technology | Rationale |
|-------|-----------|-----------|
| **Web Frontend** | Next.js 15 (App Router), TypeScript 5, Tailwind CSS 4, shadcn/ui | Assessly already uses this stack; full SSR/SSG/ISR support |
| **Mobile Frontend** | React Native with Expo (iOS + Android) | Code sharing with web via shared types |
| **Backend API** | FastAPI 0.104+ (Python 3.11) | Assessly uses this; async-native, auto OpenAPI docs |
| **Background Tasks** | Celery 5.3 + Redis 7 | Assessly defines it but doesn't use it â€” we'll activate it properly |
| **Database** | Supabase (PostgreSQL 15) with pgvector, RLS, Realtime | All-in-one: DB, auth, real-time, storage, file storage |
| **AI Grading (Math)** | GPT-4o (primary), Gemini 2.0 Flash (fallback) | Same as Assessly; highest accuracy for equations |
| **AI Grading (Conceptual)** | Gemini 2.0 Flash | Cost-effective, excellent for open-ended answers |
| **OCR Engine** | Novita PaddleOCR-VL â†’ GPT-4 Vision fallback | Same as Assessly; proven pipeline |
| **Question Generation** | GPT-4o-mini with RAG + Bloom's Taxonomy prompts | Lower cost than GPT-4o; fine-tuned prompts suffice |
| **Tutoring Core (Text)** | Gemini 2.0 Flash (via OpenRouter) or Qwen2.5 via Ollama | Balance of quality and cost |
| **Speech-to-Text** | OpenAI Whisper (large-v3) | Best accuracy for educational vocabulary; multi-speaker |
| **Text-to-Speech** | ElevenLabs (pedagogical voices) or Google TTS | Natural prosody for teaching |
| **Diagram Generation** | Mermaid.js (rendered client-side via react-mermaid) | Instant SVG, no image hosting needed |
| **Object Storage** | Supabase Storage (S3-compatible) | Integrated with RLS; no separate service needed |
| **Real-time** | Supabase Realtime (WebSocket-based) | Direct PostgreSQL change subscription |
| **Monitoring** | Sentry (errors) + OpenTelemetry + Grafana (metrics) | Stack-agnostic, self-hosted or cloud |

---
## 3. Assessly Foundation â€” What We Inherit & Extend

### 3.1 Assessly File Map â†’ Atlas Migration

| Assessly File | Atlas Fate | Notes |
|---------------|-----------|-------|
| `backend/app/main.py` | **Extend** | Add routers for auth, assessments, tutoring |
| `backend/app/api/upload.py` | **Extend** | Add multipart upload for student photos |
| `backend/app/api/grading.py` | **Refactor** | Add per-question scoring + grasp matrix update |
| `backend/app/api/websocket.py` | **Extend** | Add tutoring chat, challenge progress channels |
| `backend/app/services/ocr_service.py` | **Keep** | PaddleOCR â†’ GPT-4 Vision chain is solid |
| `backend/app/services/grading_engine.py` | **Refactor** | Add rubric-optional grading, IRT updates |
| `backend/app/services/multi_question_grader.py` | **Keep** | Multi-question logic is correct |
| `backend/app/services/math_validator.py` | **Integrate** | Currently unused; wire into grading for symbolic validation |
| `backend/app/services/rubric_parser.py` | **Keep** | PDF rubric â†’ JSON parsing works |
| `backend/app/services/student_extractor.py` | **Keep** | Name/reg# extraction is functional |
| `backend/app/models/*.py` | **Replace** | Supabase-managed schema replaces SQLAlchemy auto-create |
| `backend/app/config.py` | **Extend** | Add Atlas-specific env vars |
| `backend/celery_worker.py` | **Fix & Use** | Currently defined but NOT used â€” wire into grading |
| `backend/docker-compose.yml` | **Extend** | Add Supabase services |
| `frontend/components/grading-canvas-integrated.tsx` | **Keep** | Annotation canvas is excellent |
| `frontend/components/grading-editor-content-live.tsx` | **Keep** | Live grading editor |
| `frontend/components/grade-canvas-content-integrated.tsx` | **Keep** | Upload flow |
| `frontend/app/grade-canvas/*` | **Keep** | Routes are correct |
| `frontend/app/classes/*` | **Extend** | Add student-specific views, grasp matrix |
| `frontend/lib/api-client.ts` | **Extend** | Add tutoring, assessments, challenges endpoints |
| *Everything else* | **New** | Student dashboard, tutor panel, teacher alerts, auth |

### 3.2 What Assessly Has (Working, Keep As-Is)

- **OCR pipeline** â€” Novita PaddleOCR-VL â†’ GPT-4 Vision fallback
- **AI grading** â€” Dual-model (GPT-4o for math, Gemini for conceptual)
- **Rubric parsing** â€” PDF â†’ structured JSON with partial credit
- **Async jobs** â€” UUID-based grading jobs with Redis status tracking
- **Annotation canvas** â€” Drag, resize, minimize, delete, save annotations on PDF overlay
- **Multi-question grading** â€” Distributes rubric questions across workbook pages
- **Student extraction** â€” Name, registration number, class, subject from OCR text
- **PDF viewer** â€” iframe-based with annotation overlays
- **Docker deployment** â€” Multi-stage build, Cloud Run + Render configs

### 3.3 What Assessly Has But Needs Fixing

| Feature | Problem | Fix |
|---------|---------|-----|
| **Celery** | Defined in `celery_worker.py` but FastAPI uses `BackgroundTasks` instead | Route grading through Celery; add task priority queues |
| **math_validator.py** | SymPy module exists but is never called | Integrate as pre-grading validation step |
| **Database init** | Uses `Base.metadata.create_all()` (no migrations) | Replace with Alembic migrations against Supabase |
| **Job dedup** | By `job_id` and student identity â€” works but fragile | Add explicit dedup via Redis lock |
| **Auth** | No authentication at all | Add Supabase Auth + JWT middleware |
| **Tests** | Zero test files in the entire codebase | Add pytest + Vitest + Playwright (see Section 12) |
| **Analytics page** | Sidebar link exists but route returns 404 | Build analytics dashboard from grasp_scores |

### 3.4 What's Entirely New for Atlas

- **Multi-tenant** â€” schools table, user roles (student/teacher/admin/parent)
- **Curriculum knowledge graph** â€” topics, prerequisites, embeddings
- **Question generation** â€” RAG + Bloom's taxonomy prompts
- **Grasp matrix** â€” Per-student, per-topic scoring engine
- **Tutoring** â€” Whisper STT â†’ RAG â†’ LLM â†’ TTS + Mermaid diagrams
- **Challenge workflow** â€” Student challenges â†’ AI explains â†’ Teacher verdict
- **Student dashboard** â€” Heatmap, pending assessments, progress
- **Teacher alert wall** â€” Real-time weak student flags
- **Notifications** â€” In-app + push + email
- **Parent view** â€” Read-only progress dashboard
- **RLS policies** â€” Every table has per-role row-level security
- **Mobile apps** â€” React Native + Expo for iOS and Android

---
## 4. Database Schema â€” Production-Ready with RLS

### 4.1 Complete Schema (Supabase + pgvector)

```sql
-- =====================================================
--  EXTENSIONS
-- =====================================================
CREATE EXTENSION IF NOT EXISTS "pgcrypto";         -- gen_random_uuid()
CREATE EXTENSION IF NOT EXISTS "vector";            -- pgvector for embeddings
CREATE EXTENSION IF NOT EXISTS "pg_stat_statements";-- query performance

-- =====================================================
--  CORE TENANT STRUCTURE (Multi-tenant by school)
-- =====================================================
CREATE TABLE schools (
    id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name        TEXT NOT NULL,
    subdomain   TEXT UNIQUE NOT NULL,
    settings    JSONB DEFAULT '{}'::jsonb,
    is_active   BOOLEAN DEFAULT true,
    created_at  TIMESTAMPTZ DEFAULT now()
);

-- =====================================================
--  USERS & ROLES (managed by Supabase Auth + public table)
-- =====================================================
CREATE TABLE user_roles (
    id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    role        TEXT NOT NULL CHECK (role IN ('student', 'teacher', 'admin', 'parent')),
    description TEXT
);
-- Seed default roles
INSERT INTO user_roles (id, role, description) VALUES
    ('00000000-0000-0000-0000-000000000001', 'student', 'Learner'),
    ('00000000-0000-0000-0000-000000000002', 'teacher', 'Instructor'),
    ('00000000-0000-0000-0000-000000000003', 'admin', 'School administrator'),
    ('00000000-0000-0000-0000-000000000004', 'parent', 'Parent/guardian');

CREATE TABLE users (
    id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email           TEXT UNIQUE NOT NULL,
    full_name       TEXT NOT NULL,
    avatar_url      TEXT,
    role_id         UUID NOT NULL REFERENCES user_roles(id),
    school_id       UUID NOT NULL REFERENCES schools(id) ON DELETE CASCADE,
    is_active       BOOLEAN DEFAULT true,
    last_active     TIMESTAMPTZ,
    metadata        JSONB DEFAULT '{}'::jsonb,
    created_at      TIMESTAMPTZ DEFAULT now()
);
CREATE INDEX idx_users_school_role ON users(school_id, role_id);
CREATE INDEX idx_users_last_active ON users(last_active) WHERE last_active IS NOT NULL;

-- Many-to-many: students can have multiple parents
CREATE TABLE student_parents (
    student_id      UUID REFERENCES users(id) ON DELETE CASCADE,
    parent_id       UUID REFERENCES users(id) ON DELETE CASCADE,
    relationship    TEXT,
    PRIMARY KEY (student_id, parent_id)
);

-- =====================================================
--  ACADEMIC STRUCTURE
-- =====================================================
CREATE TABLE subjects (
    id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name            TEXT NOT NULL,
    display_name    TEXT,
    grade_level     INTEGER NOT NULL CHECK (grade_level BETWEEN 1 AND 13),
    school_id       UUID REFERENCES schools(id) ON DELETE CASCADE,
    is_active       BOOLEAN DEFAULT true,
    UNIQUE(name, grade_level, school_id)
);

CREATE TABLE classes (
    id                UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name              TEXT NOT NULL,
    display_name      TEXT,
    subject_id        UUID NOT NULL REFERENCES subjects(id) ON DELETE CASCADE,
    teacher_id        UUID REFERENCES users(id) ON DELETE SET NULL,
    academic_year     TEXT NOT NULL,
    term              TEXT,
    join_code         TEXT UNIQUE,
    schedule          JSONB DEFAULT '{}'::jsonb,
    is_active         BOOLEAN DEFAULT true,
    created_at        TIMESTAMPTZ DEFAULT now()
);
CREATE INDEX idx_classes_teacher ON classes(teacher_id) WHERE teacher_id IS NOT NULL;
CREATE INDEX idx_classes_subject ON classes(subject_id);

CREATE TABLE enrollments (
    student_id        UUID REFERENCES users(id) ON DELETE CASCADE,
    class_id          UUID REFERENCES classes(id) ON DELETE CASCADE,
    enrolled_at       TIMESTAMPTZ DEFAULT now(),
    dropped_at        TIMESTAMPTZ,
    is_active         BOOLEAN DEFAULT true,
    PRIMARY KEY (student_id, class_id)
);
CREATE INDEX idx_enrollments_active ON enrollments(is_active) WHERE is_active = true;

-- =====================================================
--  CURRICULUM & KNOWLEDGE GRAPH (RAG Foundation)
-- =====================================================
CREATE TABLE curriculum_topics (
    id                UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    subject_id        UUID NOT NULL REFERENCES subjects(id) ON DELETE CASCADE,
    name              TEXT NOT NULL,
    description       TEXT,
    parent_topic_id   UUID REFERENCES curriculum_topics(id),
    sequence_order    INTEGER DEFAULT 0,
    bloom_tier        TEXT CHECK (bloom_tier IN ('remember','understand','apply','analyze','evaluate','create')),
    estimated_hours   DECIMAL(4,1),
    is_active         BOOLEAN DEFAULT true,
    created_at        TIMESTAMPTZ DEFAULT now()
);
CREATE INDEX idx_topics_subject ON curriculum_topics(subject_id);
CREATE INDEX idx_topics_parent ON curriculum_topics(parent_topic_id) WHERE parent_topic_id IS NOT NULL;

-- Prerequisites graph (e.g., "Fractions" before "Algebra")
CREATE TABLE topic_prerequisites (
    topic_id          UUID REFERENCES curriculum_topics(id) ON DELETE CASCADE,
    prerequisite_id   UUID REFERENCES curriculum_topics(id) ON DELETE CASCADE,
    PRIMARY KEY (topic_id, prerequisite_id)
);

-- Embeddings for RAG retrieval
CREATE TABLE topic_embeddings (
    topic_id          UUID PRIMARY KEY REFERENCES curriculum_topics(id) ON DELETE CASCADE,
    embedding         vector(1536),           -- text-embedding-3-small output
    chunk_text        TEXT NOT NULL,
    chunk_source      TEXT,                   -- 'textbook', 'teacher_notes', 'rubric'
    content_hash      TEXT,
    updated_at        TIMESTAMPTZ DEFAULT now()
);
```

### 4.2 Questions, Assessments, Submissions

```sql
-- =====================================================
--  QUESTIONS & BANK (with IRT parameters)
-- =====================================================
CREATE TABLE questions (
    id                UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    topic_id          UUID REFERENCES curriculum_topics(id) ON DELETE CASCADE,
    difficulty        INTEGER CHECK (difficulty BETWEEN 1 AND 5),
    bloom_tier        TEXT CHECK (bloom_tier IN ('remember','understand','apply','analyze','evaluate','create')),
    question_text     TEXT NOT NULL,
    question_type     TEXT CHECK (question_type IN ('mcq','open_ended','calculation','diagram','fill_blank')),
    correct_answer    TEXT,
    rubric_json       JSONB,
    ai_generated      BOOLEAN DEFAULT false,
    created_by_user   UUID REFERENCES users(id) ON DELETE SET NULL,
    is_active         BOOLEAN DEFAULT true,
    usage_count       INTEGER DEFAULT 0,
    avg_score         DECIMAL(5,2),
    discrimination    DECIMAL(5,2),          -- IRT discrimination parameter
    difficulty_param  DECIMAL(5,2),          -- IRT difficulty parameter
    created_at        TIMESTAMPTZ DEFAULT now()
);
CREATE INDEX idx_questions_topic ON questions(topic_id);
CREATE INDEX idx_questions_difficulty ON questions(difficulty);

CREATE TABLE question_options (
    id                UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    question_id       UUID REFERENCES questions(id) ON DELETE CASCADE,
    option_text       TEXT NOT NULL,
    is_correct        BOOLEAN DEFAULT false,
    sequence          INTEGER
);
CREATE INDEX idx_options_question ON question_options(question_id);

-- =====================================================
--  ASSESSMENTS
-- =====================================================
CREATE TABLE assessments (
    id                UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    class_id          UUID NOT NULL REFERENCES classes(id) ON DELETE CASCADE,
    title             TEXT NOT NULL,
    description       TEXT,
    assessment_type   TEXT CHECK (assessment_type IN ('daily_quiz','monthly_test','past_paper','ai_generated','mock_exam')),
    scheduled_at      TIMESTAMPTZ NOT NULL,
    due_at            TIMESTAMPTZ,
    time_limit_min    INTEGER,
    total_marks       DECIMAL(6,2),
    is_active         BOOLEAN DEFAULT true,
    ai_config         JSONB,                -- { difficulty_range, topic_focus, question_count }
    created_by        UUID REFERENCES users(id) ON DELETE SET NULL,
    created_at        TIMESTAMPTZ DEFAULT now()
);
CREATE INDEX idx_assessments_class ON assessments(class_id);
CREATE INDEX idx_assessments_scheduled ON assessments(scheduled_at);

CREATE TABLE assessment_questions (
    assessment_id     UUID REFERENCES assessments(id) ON DELETE CASCADE,
    question_id       UUID REFERENCES questions(id) ON DELETE CASCADE,
    marks             DECIMAL(5,2),
    sequence          INTEGER,
    PRIMARY KEY (assessment_id, question_id)
);

-- =====================================================
--  SUBMISSIONS & GRADING DETAIL
-- =====================================================
CREATE TABLE submissions (
    id                  UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    assessment_id       UUID NOT NULL REFERENCES assessments(id) ON DELETE CASCADE,
    student_id          UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    answer_text         TEXT,
    answer_image_urls   TEXT[],
    status              TEXT DEFAULT 'submitted' CHECK (status IN ('submitted','queued','ocr_processing','grading','graded','challenged','challenge_closed')),
    ai_grade            DECIMAL(5,2),
    ai_feedback_json    JSONB,
    teacher_grade       DECIMAL(5,2),
    teacher_feedback    TEXT,
    final_grade         DECIMAL(5,2),
    graded_by_llm       TEXT,
    grading_job_id      TEXT,
    ocr_confidence      DECIMAL(5,2),
    ocr_method          TEXT,
    started_grading_at  TIMESTAMPTZ,
    graded_at           TIMESTAMPTZ,
    created_at          TIMESTAMPTZ DEFAULT now()
);
CREATE INDEX idx_submissions_student ON submissions(student_id);
CREATE INDEX idx_submissions_assessment ON submissions(assessment_id);
CREATE INDEX idx_submissions_status ON submissions(status);
CREATE UNIQUE INDEX idx_submissions_unique ON submissions(assessment_id, student_id);

CREATE TABLE submission_details (
    id                  UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    submission_id       UUID NOT NULL REFERENCES submissions(id) ON DELETE CASCADE,
    question_id         UUID NOT NULL REFERENCES questions(id),
    student_answer      TEXT,
    ai_score            DECIMAL(5,2),
    ai_explanation      TEXT,
    teacher_score       DECIMAL(5,2),
    teacher_note        TEXT,
    final_score         DECIMAL(5,2),
    is_correct          BOOLEAN,
    time_spent_seconds  INTEGER,
    annotation_json     JSONB
);
CREATE INDEX idx_sub_details_submission ON submission_details(submission_id);
```

### 4.3 Grasp Matrix, Challenges, Tutoring, Notifications

```sql
-- =====================================================
--  GRASP MATRIX (The Source of Truth)
-- =====================================================
CREATE TABLE grasp_scores (
    student_id          UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    topic_id            UUID NOT NULL REFERENCES curriculum_topics(id) ON DELETE CASCADE,
    score               DECIMAL(5,2) CHECK (score BETWEEN 0 AND 100),
    confidence          INTEGER CHECK (confidence BETWEEN 1 AND 10),
    num_attempts        INTEGER DEFAULT 0,
    last_question_id    UUID REFERENCES questions(id) ON DELETE SET NULL,
    trend               TEXT CHECK (trend IN ('improving','stable','declining')) DEFAULT 'stable',
    last_updated        TIMESTAMPTZ DEFAULT now(),
    PRIMARY KEY (student_id, topic_id)
);
CREATE INDEX idx_grasp_student ON grasp_scores(student_id);
CREATE INDEX idx_grasp_score ON grasp_scores(score);
CREATE INDEX idx_grasp_alert ON grasp_scores(score) WHERE score < 50;

-- Auto-update trend trigger
CREATE OR REPLACE FUNCTION update_grasp_trend()
RETURNS TRIGGER AS $$
DECLARE
    prev_score DECIMAL(5,2);
BEGIN
    SELECT score INTO prev_score
    FROM grasp_scores
    WHERE student_id = NEW.student_id AND topic_id = NEW.topic_id
    FOR UPDATE;
    NEW.trend := CASE
        WHEN NEW.score > prev_score + 5 THEN 'improving'::text
        WHEN NEW.score < prev_score - 5 THEN 'declining'::text
        ELSE 'stable'::text
    END;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER grasp_trend_update
    BEFORE UPDATE ON grasp_scores
    FOR EACH ROW
    EXECUTE FUNCTION update_grasp_trend();

-- =====================================================
--  CHALLENGES & DISCUSSION
-- =====================================================
CREATE TABLE challenges (
    id                  UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    submission_id       UUID NOT NULL REFERENCES submissions(id) ON DELETE CASCADE,
    submission_detail_id UUID REFERENCES submission_details(id) ON DELETE CASCADE,
    student_id          UUID NOT NULL REFERENCES users(id),
    teacher_id          UUID REFERENCES users(id),
    reason              TEXT NOT NULL,
    description         TEXT,
    ai_explanation      TEXT,
    teacher_verdict     TEXT CHECK (teacher_verdict IN ('grade_updated','grade_upheld','pending')),
    final_grade         DECIMAL(5,2),
    resolved_at         TIMESTAMPTZ,
    created_at          TIMESTAMPTZ DEFAULT now()
);
CREATE INDEX idx_challenges_submission ON challenges(submission_id);
CREATE INDEX idx_challenges_teacher ON challenges(teacher_id) WHERE teacher_id IS NULL;

CREATE TABLE challenge_messages (
    id                  UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    challenge_id        UUID NOT NULL REFERENCES challenges(id) ON DELETE CASCADE,
    sender_id           UUID NOT NULL REFERENCES users(id),
    message             TEXT,
    message_type        TEXT CHECK (message_type IN ('text','diagram','voice_note','system')) DEFAULT 'text',
    diagram_svg         TEXT,
    audio_url           TEXT,
    metadata            JSONB,
    sent_at             TIMESTAMPTZ DEFAULT now()
);
CREATE INDEX idx_chat_challenge ON challenge_messages(challenge_id);

-- =====================================================
--  TUTORING SESSIONS
-- =====================================================
CREATE TABLE tutoring_sessions (
    id                  UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    student_id          UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    topic_id            UUID REFERENCES curriculum_topics(id) ON DELETE SET NULL,
    session_type        TEXT CHECK (session_type IN ('chat','voice','challenge_discussion','explain_topic')),
    status              TEXT CHECK (status IN ('active','paused','closed')) DEFAULT 'active',
    started_at          TIMESTAMPTZ DEFAULT now(),
    ended_at            TIMESTAMPTZ,
    summary             TEXT,
    grasp_before        DECIMAL(5,2),
    grasp_after         DECIMAL(5,2)
);

CREATE TABLE tutoring_messages (
    id                  UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    session_id          UUID NOT NULL REFERENCES tutoring_sessions(id) ON DELETE CASCADE,
    sender_type         TEXT CHECK (sender_type IN ('student','ai')),
    message_text        TEXT,
    diagram_svg         TEXT,
    audio_url           TEXT,
    metadata            JSONB,
    created_at          TIMESTAMPTZ DEFAULT now()
);
CREATE INDEX idx_tutor_session ON tutoring_messages(session_id);

-- =====================================================
--  NOTIFICATIONS
-- =====================================================
CREATE TABLE notifications (
    id                  UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id             UUID NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    type                TEXT CHECK (type IN ('grasp_alert','grade_new','challenge_raised','challenge_resolved','assignment_due','system','tutor_message')),
    title               TEXT NOT NULL,
    body                TEXT NOT NULL,
    data_json           JSONB,
    is_read             BOOLEAN DEFAULT false,
    created_at          TIMESTAMPTZ DEFAULT now()
);
CREATE INDEX idx_notif_user ON notifications(user_id);
CREATE INDEX idx_notif_unread ON notifications(user_id, is_read) WHERE is_read = false;
```

### 4.4 Row Level Security (RLS) Policies

```sql
-- =====================================================
--  ENABLE RLS ON ALL TABLES
-- =====================================================
ALTER TABLE users ENABLE ROW LEVEL SECURITY;
ALTER TABLE subjects ENABLE ROW LEVEL SECURITY;
ALTER TABLE classes ENABLE ROW LEVEL SECURITY;
ALTER TABLE enrollments ENABLE ROW LEVEL SECURITY;
ALTER TABLE curriculum_topics ENABLE ROW LEVEL SECURITY;
ALTER TABLE questions ENABLE ROW LEVEL SECURITY;
ALTER TABLE assessments ENABLE ROW LEVEL SECURITY;
ALTER TABLE submissions ENABLE ROW LEVEL SECURITY;
ALTER TABLE submission_details ENABLE ROW LEVEL SECURITY;
ALTER TABLE grasp_scores ENABLE ROW LEVEL SECURITY;
ALTER TABLE challenges ENABLE ROW LEVEL SECURITY;
ALTER TABLE challenge_messages ENABLE ROW LEVEL SECURITY;
ALTER TABLE notifications ENABLE ROW LEVEL SECURITY;
ALTER TABLE tutoring_sessions ENABLE ROW LEVEL SECURITY;
ALTER TABLE tutoring_messages ENABLE ROW LEVEL SECURITY;

-- =====================================================
--  USERS TABLE: Students read own; Teachers read school; Admins full; Parents read children
-- =====================================================
CREATE POLICY "students_read_own" ON users
    FOR SELECT USING (auth.uid() = id);

CREATE POLICY "teachers_read_school" ON users
    FOR SELECT USING (
        EXISTS (SELECT 1 FROM users u WHERE u.id = auth.uid()
            AND u.role_id = (SELECT id FROM user_roles WHERE role = 'teacher')
            AND u.school_id = users.school_id));

CREATE POLICY "admins_full_access" ON users
    FOR ALL USING (
        EXISTS (SELECT 1 FROM users u WHERE u.id = auth.uid()
            AND u.role_id = (SELECT id FROM user_roles WHERE role = 'admin')
            AND u.school_id = users.school_id));

CREATE POLICY "parents_read_children" ON users
    FOR SELECT USING (
        EXISTS (SELECT 1 FROM student_parents sp
            WHERE sp.parent_id = auth.uid() AND sp.student_id = users.id));

-- =====================================================
--  ENROLLMENTS: Students see own; Teachers see their classes
-- =====================================================
CREATE POLICY "students_own_enrollments" ON enrollments
    FOR SELECT USING (student_id = auth.uid());

CREATE POLICY "teachers_class_enrollments" ON enrollments
    FOR SELECT USING (
        EXISTS (SELECT 1 FROM classes c
            WHERE c.id = enrollments.class_id AND c.teacher_id = auth.uid()));

-- =====================================================
--  GRASP SCORES: Students see own; Teachers see their students; Parents see children
-- =====================================================
CREATE POLICY "students_own_grasp" ON grasp_scores
    FOR SELECT USING (student_id = auth.uid());

CREATE POLICY "teachers_class_grasp" ON grasp_scores
    FOR SELECT USING (
        EXISTS (SELECT 1 FROM enrollments e
            JOIN classes c ON c.id = e.class_id
            WHERE e.student_id = grasp_scores.student_id AND c.teacher_id = auth.uid()));

CREATE POLICY "parents_children_grasp" ON grasp_scores
    FOR SELECT USING (
        EXISTS (SELECT 1 FROM student_parents sp
            WHERE sp.parent_id = auth.uid() AND sp.student_id = grasp_scores.student_id));

-- =====================================================
--  SUBMISSIONS: Students insert/read own; Teachers full CRUD on own classes
-- =====================================================
CREATE POLICY "students_insert_own" ON submissions
    FOR INSERT WITH CHECK (student_id = auth.uid());

CREATE POLICY "students_read_own_submissions" ON submissions
    FOR SELECT USING (student_id = auth.uid());

CREATE POLICY "teachers_class_submissions" ON submissions
    FOR ALL USING (
        EXISTS (SELECT 1 FROM assessments a
            JOIN classes c ON c.id = a.class_id
            WHERE a.id = submissions.assessment_id AND c.teacher_id = auth.uid()));

-- =====================================================
--  NOTIFICATIONS: Users read/write only their own
-- =====================================================
CREATE POLICY "users_own_notifications" ON notifications
    FOR ALL USING (user_id = auth.uid());
```

### 4.5 Materialized Views for Performance

```sql
-- Teacher alert wall: weak students in last 7 days
CREATE MATERIALIZED VIEW weekly_grasp_alerts AS
SELECT
    g.student_id, u.full_name AS student_name,
    t.name AS topic_name, g.score, g.trend, g.last_updated,
    c.id AS class_id, c.name AS class_name,
    c.teacher_id, u2.full_name AS teacher_name
FROM grasp_scores g
JOIN users u ON u.id = g.student_id
JOIN curriculum_topics t ON t.id = g.topic_id
JOIN enrollments e ON e.student_id = g.student_id AND e.is_active = true
JOIN classes c ON c.id = e.class_id
JOIN users u2 ON u2.id = c.teacher_id
WHERE g.score < 50 AND g.last_updated > now() - INTERVAL '7 days'
ORDER BY g.score ASC;

CREATE UNIQUE INDEX idx_mv_alert ON weekly_grasp_alerts(student_id, topic_id);

-- Refresh function (run every 15 min via pg_cron)
CREATE OR REPLACE FUNCTION refresh_grasp_alerts()
RETURNS void AS $$
BEGIN
    REFRESH MATERIALIZED VIEW CONCURRENTLY weekly_grasp_alerts;
END;
$$ LANGUAGE plpgsql;

-- Class performance summary
CREATE MATERIALIZED VIEW class_performance_summary AS
SELECT
    c.id AS class_id, c.name AS class_name,
    t.id AS topic_id, t.name AS topic_name,
    COUNT(DISTINCT g.student_id) AS total_students,
    AVG(g.score) AS avg_score,
    COUNT(*) FILTER (WHERE g.score < 50) AS struggling_count,
    COUNT(*) FILTER (WHERE g.score >= 80) AS proficient_count
FROM classes c
JOIN enrollments e ON e.class_id = c.id AND e.is_active = true
JOIN grasp_scores g ON g.student_id = e.student_id
JOIN curriculum_topics t ON t.id = g.topic_id
GROUP BY c.id, c.name, t.id, t.name;
```

### 4.6 pgvector Similarity Search Function

```sql
CREATE OR REPLACE FUNCTION match_topic_embeddings(
    query_embedding vector(1536),
    topic_id UUID,
    match_threshold float DEFAULT 0.7,
    match_count int DEFAULT 5
)
RETURNS TABLE(chunk_text TEXT, chunk_source TEXT, similarity float)
LANGUAGE plpgsql AS $$
BEGIN
    RETURN QUERY
    SELECT te.chunk_text, te.chunk_source,
           1 - (te.embedding <=> query_embedding) AS similarity
    FROM topic_embeddings te
    WHERE te.topic_id = match_topic_embeddings.topic_id
      AND 1 - (te.embedding <=> query_embedding) > match_threshold
    ORDER BY te.embedding <=> query_embedding
    LIMIT match_count;
END;
$$;
```

### 4.7 Supabase Realtime Configuration

```sql
ALTER PUBLICATION supabase_realtime ADD TABLE grasp_scores;
ALTER PUBLICATION supabase_realtime ADD TABLE submissions;
ALTER PUBLICATION supabase_realtime ADD TABLE notifications;
ALTER PUBLICATION supabase_realtime ADD TABLE challenges;
ALTER PUBLICATION supabase_realtime ADD TABLE tutoring_messages;
```

**Frontend subscription example (React):**
```typescript
// Subscribe to grasp changes for a specific student
supabase.channel('grasp-updates')
  .on('postgres_changes',
    { event: '*', schema: 'public', table: 'grasp_scores',
      filter: `student_id=eq.${studentId}` },
    (payload) => {
      queryClient.invalidateQueries({ queryKey: ['grasp', studentId] });
      if (payload.new.score < 50) {
        toast.warning(`Your score in this topic dropped to ${payload.new.score}%`);
      }
    })
  .subscribe();
```

---
## 5. API Contract â€” Full Specification

### 5.1 Authentication Endpoints

All endpoints require a valid Supabase JWT in the `Authorization: Bearer <token>` header except `/auth/*` and `/health`.

| Method | Path | Purpose | Auth Required |
|--------|------|---------|:---:|
| POST | `/auth/register` | Register (Supabase Auth) | No |
| POST | `/auth/login` | Login (returns JWT) | No |
| POST | `/auth/refresh` | Refresh JWT | Yes |
| POST | `/auth/logout` | Invalidate session | Yes |

### 5.2 Response Envelope

```json
{"success": true, "data": {...}, "error": null, "meta": {"page": 1, "per_page": 50, "total": 120}}
```
Error: `{"success": false, "data": null, "error": {"code": "NOT_FOUND", "message": "..."}}`

### 5.3 API Endpoint Reference

#### Users & Profile
| Method | Path | Roles |
|--------|------|-------|
| GET | `/api/v1/users/me` | All |
| PATCH | `/api/v1/users/me` | All |
| GET | `/api/v1/users/students` | Teacher, Admin |
| GET | `/api/v1/users/students/{id}` | Teacher, Admin, Parent (own children) |
| GET | `/api/v1/users/teachers` | Admin |

#### Classes & Enrollments
| Method | Path | Roles |
|--------|------|-------|
| GET | `/api/v1/classes` | All (filtered by role) |
| POST | `/api/v1/classes` | Teacher, Admin |
| GET | `/api/v1/classes/{id}` | All (enrolled/teaching) |
| PATCH | `/api/v1/classes/{id}` | Teacher, Admin |
| POST | `/api/v1/classes/{id}/join` | Student |
| GET | `/api/v1/classes/{id}/students` | Teacher, Admin |

#### Curriculum & Topics
| Method | Path | Roles |
|--------|------|-------|
| GET | `/api/v1/subjects` | All |
| POST | `/api/v1/subjects` | Admin |
| GET | `/api/v1/topics` | All (filtered by subject_id) |
| GET | `/api/v1/topics/{id}` | All |
| POST | `/api/v1/topics` | Teacher, Admin |
| GET | `/api/v1/topics/{id}/graph` | All (prerequisite tree) |

#### Questions
| Method | Path | Roles |
|--------|------|-------|
| GET | `/api/v1/questions` | Teacher, Admin |
| GET | `/api/v1/questions/{id}` | Teacher, Admin |
| POST | `/api/v1/questions` | Teacher, Admin |
| PATCH | `/api/v1/questions/{id}` | Teacher, Admin (own) |
| POST | `/api/v1/questions/generate` | **AI-generate questions** | Teacher, Admin |
| POST | `/api/v1/questions/{id}/duplicate` | Teacher, Admin |

**POST `/api/v1/questions/generate` Request:**
```json
{
  "topic_ids": ["uuid1", "uuid2"],
  "difficulty_range": [2, 4],
  "bloom_tiers": ["apply", "analyze"],
  "question_types": ["mcq", "calculation"],
  "count": 5,
  "language": "en"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "generated_questions": [{
      "id": "uuid", "question_text": "...", "question_type": "calculation",
      "difficulty": 3, "bloom_tier": "apply", "topic_id": "uuid1",
      "correct_answer": "...", "rubric_json": {...}, "status": "draft"
    }],
    "generation_job_id": "celery-job-id",
    "total_generated": 5
  }
}
```

#### Assessments
| Method | Path | Roles |
|--------|------|-------|
| GET | `/api/v1/assessments` | All (filtered) |
| POST | `/api/v1/assessments` | Teacher |
| GET | `/api/v1/assessments/{id}` | All (assigned) |
| PATCH | `/api/v1/assessments/{id}` | Teacher (own) |
| POST | `/api/v1/assessments/{id}/publish` | Teacher |
| POST | `/api/v1/assessments/{id}/questions` | Teacher |

**POST `/api/v1/assessments` Request:**
```json
{
  "class_id": "uuid", "title": "Quadratic Equations Quiz",
  "assessment_type": "daily_quiz",
  "scheduled_at": "2026-05-01T09:00:00Z", "due_at": "2026-05-01T09:45:00Z",
  "time_limit_min": 30,
  "ai_config": {
    "difficulty_range": [2, 4], "topic_ids": ["uuid1", "uuid2"],
    "question_count": 5, "generate_if_empty": true
  }
}
```

#### Submissions & Grading
| Method | Path | Roles |
|--------|------|-------|
| GET | `/api/v1/submissions` | All (filtered) |
| POST | `/api/v1/submissions` | Student |
| GET | `/api/v1/submissions/{id}` | Student (own), Teacher (class) |
| POST | `/api/v1/submissions/{id}/images` | Student (own) |
| GET | `/api/v1/submissions/{id}/status` | Student (own) |
| POST | `/api/v1/submissions/{id}/challenge` | Student (own graded) |
| GET | `/api/v1/grading/jobs/{job_id}` | Student, Teacher |
| POST | `/api/v1/grading/complete` | Teacher |
| GET | `/api/v1/grading/classes/{id}/export` | Teacher, Admin |

**POST `/api/v1/submissions` (multipart/form-data):**
```
assessment_id: uuid
answer_text: optional JSON
images: file[] (max 5, 10MB each)
```

#### Grasp Matrix
| Method | Path | Roles |
|--------|------|-------|
| GET | `/api/v1/grasp/student/{student_id}` | Student (own), Teacher (class), Parent |
| GET | `/api/v1/grasp/class/{class_id}` | Teacher |
| GET | `/api/v1/grasp/alerts` | Teacher (own classes) |
| GET | `/api/v1/grasp/heatmap` | Student (own) |

#### Challenges
| Method | Path | Roles |
|--------|------|-------|
| GET | `/api/v1/challenges` | All (filtered) |
| GET | `/api/v1/challenges/{id}` | All (participant) |
| POST | `/api/v1/challenges/{id}/messages` | All (participant) |
| POST | `/api/v1/challenges/{id}/resolve` | Teacher |

**POST `/api/v1/challenges/{id}/resolve` Request:**
```json
{"verdict": "grade_updated", "final_grade": 8.5, "teacher_feedback": "..."}
```

#### Tutoring
| Method | Path | Roles |
|--------|------|-------|
| POST | `/api/v1/tutoring/sessions` | Student |
| GET | `/api/v1/tutoring/sessions/{id}` | Student (own), Teacher |
| POST | `/api/v1/tutoring/sessions/{id}/messages` | Student |
| POST | `/api/v1/tutoring/sessions/{id}/explain` | Student |
| POST | `/api/v1/tutoring/sessions/{id}/close` | Student |

#### Notifications
| Method | Path | Roles |
|--------|------|-------|
| GET | `/api/v1/notifications` | All |
| GET | `/api/v1/notifications/unread/count` | All |
| PATCH | `/api/v1/notifications/{id}/read` | All (own) |
| POST | `/api/v1/notifications/read-all` | All |

### 5.4 WebSocket Endpoints

**Grading Progress:**
```
WS /ws/grading/{job_id}
Server messages:
{"type": "status", "status": "ocr_processing", "progress": 25}
{"type": "status", "status": "grading", "progress": 60}
{"type": "status", "status": "complete", "progress": 100, "submission_id": "uuid"}
{"type": "error", "code": "OCR_FAILED", "message": "Could not read handwriting"}
```

**Tutoring Session:**
```
WS /ws/tutoring/{session_id}?token={jwt}
Client: {"type": "message", "content": "Explain quadratics", "include_diagram": true}
Client: {"type": "audio", "data": "<base64>", "format": "webm"}
Server: {"type": "response", "text": "...", "diagram_svg": "<svg>..."}
Server: {"type": "audio_url", "url": "https://...mp3"}
```

**Challenge Discussion:**
```
WS /ws/challenge/{challenge_id}?token={jwt}
Server: {"type": "status_change", "verdict": "grade_updated", "final_grade": 8.5}
```

### 5.5 Rate Limits

| Role | Endpoint Group | Limit |
|------|---------------|-------|
| Student | Submissions | 10/min |
| Student | Tutoring | 60 msg/hr |
| Student | Challenge | 5/day |
| Teacher | Grading | 100/min |
| Teacher | Question Gen | 20/hr |
| Any | GET | 200/min |
| Any | POST/PATCH | 60/min |

---
## 6. Frontend Architecture â€” Deep Dive

### 6.1 Project Structure (Next.js 15 App Router)

```
atlas-frontend/
â”œâ”€â”€ app/
â”‚   â”œâ”€â”€ layout.tsx                          # Root layout (Supabase provider, theme)
â”‚   â”œâ”€â”€ page.tsx                            # Role-aware redirect
â”‚   â”œâ”€â”€ globals.css                         # Tailwind v4
â”‚   â”œâ”€â”€ (auth)/                             # No sidebar
â”‚   â”‚   â”œâ”€â”€ login/page.tsx
â”‚   â”‚   â”œâ”€â”€ register/page.tsx
â”‚   â”‚   â””â”€â”€ forgot-password/page.tsx
â”‚   â”œâ”€â”€ (dashboard)/                        # Has sidebar + header
â”‚   â”‚   â”œâ”€â”€ student/
â”‚   â”‚   â”‚   â”œâ”€â”€ page.tsx                    # Dashboard (heatmap, pending)
â”‚   â”‚   â”‚   â”œâ”€â”€ assessments/
â”‚   â”‚   â”‚   â”‚   â”œâ”€â”€ page.tsx                # Pending list
â”‚   â”‚   â”‚   â”‚   â””â”€â”€ [id]/
â”‚   â”‚   â”‚   â”‚       â”œâ”€â”€ page.tsx            # Take assessment
â”‚   â”‚   â”‚   â”‚       â””â”€â”€ results/page.tsx    # View graded results
â”‚   â”‚   â”‚   â”œâ”€â”€ grasp/page.tsx              # Full grasp heatmap
â”‚   â”‚   â”‚   â”œâ”€â”€ tutor/page.tsx              # AI tutor panel
â”‚   â”‚   â”‚   â””â”€â”€ challenges/
â”‚   â”‚   â”‚       â”œâ”€â”€ page.tsx                # My challenges
â”‚   â”‚   â”‚       â””â”€â”€ [id]/page.tsx           # Discussion thread
â”‚   â”‚   â”œâ”€â”€ teacher/
â”‚   â”‚   â”‚   â”œâ”€â”€ page.tsx                    # Alert wall + pending
â”‚   â”‚   â”‚   â”œâ”€â”€ classes/
â”‚   â”‚   â”‚   â”‚   â”œâ”€â”€ page.tsx                # My classes
â”‚   â”‚   â”‚   â”‚   â””â”€â”€ [id]/
â”‚   â”‚   â”‚   â”‚       â”œâ”€â”€ page.tsx            # Class detail + grasp matrix
â”‚   â”‚   â”‚   â”‚       â””â”€â”€ student/[sid]/page.tsx
â”‚   â”‚   â”‚   â”œâ”€â”€ assessments/
â”‚   â”‚   â”‚   â”‚   â”œâ”€â”€ page.tsx, new/page.tsx
â”‚   â”‚   â”‚   â”‚   â””â”€â”€ [id]/
â”‚   â”‚   â”‚   â”‚       â”œâ”€â”€ page.tsx            # Assessment detail
â”‚   â”‚   â”‚   â”‚       â””â”€â”€ grade/page.tsx      # GradeCanvas (Assessly)
â”‚   â”‚   â”‚   â”œâ”€â”€ challenges/
â”‚   â”‚   â”‚   â”‚   â”œâ”€â”€ page.tsx                # Queue
â”‚   â”‚   â”‚   â”‚   â””â”€â”€ [id]/page.tsx           # Resolution panel
â”‚   â”‚   â”‚   â””â”€â”€ questions/
â”‚   â”‚   â”‚       â”œâ”€â”€ page.tsx                # Bank
â”‚   â”‚   â”‚       â””â”€â”€ generate/page.tsx       # AI generation
â”‚   â”‚   â”œâ”€â”€ parent/
â”‚   â”‚   â”‚   â”œâ”€â”€ page.tsx                    # Children overview
â”‚   â”‚   â”‚   â””â”€â”€ child/[id]/page.tsx
â”‚   â”‚   â””â”€â”€ admin/
â”‚   â”‚       â”œâ”€â”€ page.tsx, teachers/page.tsx
â”‚   â”‚       â”œâ”€â”€ classes/page.tsx, subjects/page.tsx
â”‚   â”‚       â””â”€â”€ settings/page.tsx
â”‚   â””â”€â”€ grade-canvas/                       # Assessly routes (preserved)
â”‚       â”œâ”€â”€ page.tsx, integrated/page.tsx
â”‚       â”œâ”€â”€ editor/page.tsx, annotate/page.tsx
â”œâ”€â”€ components/
â”‚   â”œâ”€â”€ ui/                                 # shadcn/ui (63 components)
â”‚   â”œâ”€â”€ layout/
â”‚   â”‚   â”œâ”€â”€ sidebar.tsx, header.tsx
â”‚   â”‚   â”œâ”€â”€ mobile-nav.tsx, role-gate.tsx
â”‚   â”œâ”€â”€ student/
â”‚   â”‚   â”œâ”€â”€ grasp-heatmap.tsx               # Color-coded topic grid
â”‚   â”‚   â”œâ”€â”€ assessment-card.tsx             # Card with countdown
â”‚   â”‚   â”œâ”€â”€ assessment-timer.tsx            # Countdown timer
â”‚   â”‚   â”œâ”€â”€ submission-uploader.tsx         # Photo + text upload
â”‚   â”‚   â”œâ”€â”€ grade-review.tsx                # Interactive answer review
â”‚   â”‚   â””â”€â”€ challenge-button.tsx
â”‚   â”œâ”€â”€ teacher/
â”‚   â”‚   â”œâ”€â”€ alert-wall.tsx                  # Real-time weak student list
â”‚   â”‚   â”œâ”€â”€ grasp-matrix-table.tsx          # Students x Topics
â”‚   â”‚   â”œâ”€â”€ challenge-queue.tsx
â”‚   â”‚   â”œâ”€â”€ question-generator.tsx
â”‚   â”‚   â””â”€â”€ assessment-creator.tsx
â”‚   â”œâ”€â”€ tutor/
â”‚   â”‚   â”œâ”€â”€ chat-panel.tsx                  # Messages + diagrams
â”‚   â”‚   â”œâ”€â”€ voice-recorder.tsx              # Mic button
â”‚   â”‚   â”œâ”€â”€ diagram-renderer.tsx            # Mermaid SVG
â”‚   â”‚   â””â”€â”€ audio-player.tsx                # TTS playback
â”‚   â””â”€â”€ shared/
â”‚       â”œâ”€â”€ notification-bell.tsx, user-avatar.tsx
â”‚       â””â”€â”€ loading-states.tsx
â”œâ”€â”€ lib/
â”‚   â”œâ”€â”€ supabase/
â”‚   â”‚   â”œâ”€â”€ client.ts                       # Browser client
â”‚   â”‚   â”œâ”€â”€ server.ts                       # Server Components
â”‚   â”‚   â”œâ”€â”€ middleware.ts                   # Auth check
â”‚   â”‚   â””â”€â”€ realtime.ts                     # Channel subscriptions
â”‚   â”œâ”€â”€ api/
â”‚   â”‚   â”œâ”€â”€ client.ts                       # Axios + JWT interceptor
â”‚   â”‚   â”œâ”€â”€ assessments.ts, submissions.ts
â”‚   â”‚   â”œâ”€â”€ tutoring.ts, grasp.ts, challenges.ts
â”‚   â”œâ”€â”€ hooks/
â”‚   â”‚   â”œâ”€â”€ use-grasp.ts                    # React Query
â”‚   â”‚   â”œâ”€â”€ use-assessments.ts, use-realtime.ts
â”‚   â”‚   â”œâ”€â”€ use-voice.ts, use-tutoring-session.ts
â”‚   â”œâ”€â”€ stores/
â”‚   â”‚   â”œâ”€â”€ auth-store.ts                   # Zustand
â”‚   â”‚   â”œâ”€â”€ ui-store.ts, notification-store.ts
â”‚   â””â”€â”€ utils/
â”‚       â”œâ”€â”€ cn.ts, format.ts, mermaid.ts
â”œâ”€â”€ types/
â”‚   â”œâ”€â”€ database.ts, api.ts, grading.ts
â””â”€â”€ public/
```

### 6.2 State Management Strategy

| State Type | Solution | Rationale |
|-----------|----------|-----------|
| **Server state** (API data) | TanStack React Query v5 | Caching, dedup, background refetch |
| **Real-time data** | Supabase Realtime hooks | Direct PostgreSQL subscription |
| **Client state** (UI toggles) | Zustand | Minimal boilerplate |
| **Form state** | React Hook Form + Zod | Validation, file uploads |
| **Auth state** | Zustand + Supabase Auth listener | JWT session + role redirects |
| **WebSocket state** | Custom React hooks | Tutoring, grading progress |

### 6.3 API Client (Axios + JWT Interceptor)

```typescript
// lib/api/client.ts
import axios from 'axios';
import { supabase } from '@/lib/supabase/client';

const apiClient = axios.create({
  baseURL: process.env.NEXT_PUBLIC_API_URL || 'http://localhost:8000',
  timeout: 30_000,
  headers: { 'Content-Type': 'application/json' },
});

// Attach JWT on every request
apiClient.interceptors.request.use(async (config) => {
  const { data: { session } } = await supabase.auth.getSession();
  if (session?.access_token) {
    config.headers.Authorization = `Bearer ${session.access_token}`;
  }
  return config;
});

// Auto-refresh on 401
apiClient.interceptors.response.use(
  (res) => res,
  async (error) => {
    if (error.response?.status === 401) {
      const { error: refreshError } = await supabase.auth.refreshSession();
      if (refreshError) window.location.href = '/login';
    }
    return Promise.reject(error);
  }
);

// Upload helper
export async function uploadSubmission(
  assessmentId: string, images: File[], answerText?: string
) {
  const form = new FormData();
  form.append('assessment_id', assessmentId);
  if (answerText) form.append('answer_text', answerText);
  images.forEach((img) => form.append('images', img));
  return apiClient.post('/api/v1/submissions', form, {
    headers: { 'Content-Type': 'multipart/form-data' },
    timeout: 120_000,
  });
}
```

### 6.4 Real-Time Subscription Hook

```typescript
// lib/hooks/use-realtime.ts
import { useEffect } from 'react';
import { useQueryClient } from '@tanstack/react-query';
import { supabase } from '@/lib/supabase/client';

export function useGraspRealtime(studentId: string) {
  const queryClient = useQueryClient();

  useEffect(() => {
    const channel = supabase
      .channel('grasp-updates')
      .on('postgres_changes', {
        event: '*', schema: 'public', table: 'grasp_scores',
        filter: `student_id=eq.${studentId}`,
      }, (payload) => {
        queryClient.invalidateQueries({ queryKey: ['grasp', studentId] });
      })
      .subscribe();
    return () => { supabase.removeChannel(channel); };
  }, [studentId, queryClient]);
}
```

### 6.5 Key State Machines

**Assessment Taking:**
```
IDLE -> LOADING -> IN_PROGRESS -> UPLOADING -> SUBMITTED
                                       |
                                       +-> ERROR -> RETRY
```

**Tutoring Session:**
```
CLOSED -> CONNECTING -> ACTIVE -> AI_THINKING -> ACTIVE (with response)
                                      |
                                      +-> CHALLENGE_OPEN -> ACTIVE
```

**Challenge Resolution:**
```
SUBMISSION_GRADED -> CHALLENGE_CREATED -> [TEACHER_REVIEWS | STUDENT_REPLIES]
    -> RESOLVED -> GRASP_UPDATED
```

### 6.6 Mobile Architecture (React Native + Expo)

```
atlas-mobile/
â”œâ”€â”€ App.tsx                         # Supabase provider
â”œâ”€â”€ app/
â”‚   â”œâ”€â”€ (auth)/                     # Login/register
â”‚   â”œâ”€â”€ (tabs)/                     # Bottom tabs
â”‚   â”‚   â”œâ”€â”€ dashboard/, assessments/
â”‚   â”‚   â”œâ”€â”€ tutor/, profile/
â”‚   â””â”€â”€ challenge/[id]/
â”œâ”€â”€ components/                     # Shared RN components
â”œâ”€â”€ lib/
â”‚   â”œâ”€â”€ supabase.ts                 # RN client
â”‚   â””â”€â”€ api.ts                      # Same patterns as web
â””â”€â”€ app.json
```

**Key mobile considerations:**
- Offline support via AsyncStorage for last-known grasp data
- Camera integration: `expo-camera` for snapping handwritten answers
- Voice input: `expo-av` for recording -> Whisper API
- Push notifications via Expo push service
- Camera photos compressed to max 2MB before upload

---
## 7. AI Grading Pipeline â€” Extended from Assessly

### 7.1 Complete Grading Flow (Celery Task)

```
Student submits photos
    |
    v
1. QUEUED -> Celery task created, student sees WebSocket progress bar
    |
    v
2. OCR_PHASE
   â”œâ”€â”€ Try PyPDF2 text extraction (if PDF)
   â”œâ”€â”€ Try PaddleOCR-VL (handwriting + layout, primary)
   â””â”€â”€ Fallback: GPT-4 Vision (expensive, for complex math)
    |
    v
3. STUDENT_EXTRACTION -> Name, reg#, class, subject from OCR
    |
    v
4. QUESTION_MAPPING -> Parse rubric, map to pages/regions
    |
    v
5. GRADING (per question, in parallel)
   â”œâ”€â”€ Math/Calculation: GPT-4o (primary)
   â”‚   â””â”€â”€ SymPy validation: AI score vs symbolic check
   â”œâ”€â”€ Conceptual/Open-ended: Gemini 2.0 Flash (primary)
   â””â”€â”€ MCQ: Exact string match (no AI needed)
    |
    v
6. SCORE_AGGREGATION -> Per-question + total scores + feedback
    |
    v
7. GRASP_UPDATE -> Update grasp_scores, recalculate trends, fire alerts if <50
    |
    v
8. COMPLETE -> WebSocket push, notification to student
```

### 7.2 Celery Configuration (Activated)

```python
# backend/celery_worker.py â€” Atlas edition
from celery import Celery
from app.config import settings

celery_app = Celery('atlas_grading',
    broker=settings.REDIS_URL,
    backend=settings.REDIS_URL,
    include=['app.tasks.grading', 'app.tasks.question_gen', 'app.tasks.notifications']
)

celery_app.conf.update(
    task_serializer='json',
    accept_content=['json'],
    result_serializer='json',
    timezone='UTC',
    enable_utc=True,
    task_track_started=True,
    task_acks_late=True,              # Re-deliver on worker crash
    worker_prefetch_multiplier=1,     # Fair dispatch
    task_reject_on_worker_lost=True,
    task_routes={
        'grade_submission': {'queue': 'grading', 'routing_key': 'grading.high'},
        'generate_questions': {'queue': 'question_gen', 'routing_key': 'gen.medium'},
        'send_notification': {'queue': 'notifications', 'routing_key': 'notif.low'},
    },
    task_queue_max_priority=10,
    task_default_priority=5,
)
```

```python
# app/tasks/grading.py â€” New for Atlas
from celery import shared_task
from app.services.ocr_service import OCRService
from app.services.grading_engine import GradingEngine
from app.services.grasp_service import GraspService

@shared_task(bind=True, max_retries=3, default_retry_delay=30)
def grade_submission(self, submission_id: str):
    """Full grading pipeline as Celery task."""
    try:
        update_submission_status(submission_id, 'ocr_processing')
        ocr_service = OCRService()
        ocr_result = ocr_service.process(submission_id)
        if ocr_result['confidence'] < 50:
            ocr_result = ocr_service.process_with_vision_fallback(submission_id)

        update_submission_status(submission_id, 'grading')
        grading_engine = GradingEngine()
        grading_result = grading_engine.grade_submission(
            submission_id=submission_id,
            ocr_text=ocr_result['text'],
            ocr_confidence=ocr_result['confidence'])

        grasp_service = GraspService()
        grasp_service.update_from_grading(
            submission_id=submission_id,
            scores=grading_result['per_question'])

        update_submission_status(submission_id, 'graded', {
            'ai_grade': grading_result['total_score'],
            'ai_feedback': grading_result['feedback'],
            'ocr_confidence': ocr_result['confidence'],
            'ocr_method': ocr_result['method']})

        alerts = grasp_service.check_alerts(submission_id)
        fire_alerts_if_needed(alerts)
        return grading_result
    except Exception as exc:
        update_submission_status(submission_id, 'failed',
            {'error': str(exc), 'attempt': self.request.retries})
        raise self.retry(exc=exc)
```

### 7.3 Grading Prompt Template

```
You are an expert examiner for {subject}, Grade {grade_level}.
Curriculum scope: {topic_names}
Rubric: {rubric_json}

Student answer: {student_answer_text}
Question: {question_text}
Maximum points: {max_points}

Evaluate carefully:
1. Correctness â€” is the final answer correct?
2. Process â€” is the working correct even if the final answer has a minor error?
3. Partial credit â€” partial marks for partially correct work
4. Common errors â€” note specific misconceptions

Return ONLY valid JSON:
{
  "score": <float>,
  "max_score": <float>,
  "strengths": ["..."],
  "weaknesses": ["..."],
  "stepwise_breakdown": [
    {"step": "<step_name>", "correct": <bool>, "notes": "<string>"}
  ],
  "common_error_identified": "<string or null>",
  "conceptual_misunderstanding": "<string or null>"
}
```

### 7.4 Error Handling & Retry

| Error | Action | Retry? | Alert Human? |
|-------|--------|:----:|:-----------:|
| OCR returns empty text | Retry with GPT-4 Vision fallback | 1x | If both fail |
| LLM returns malformed JSON | Re-prompt with stricter instructions | 2x | If still fails |
| LLM API timeout | Exponential backoff (30s, 60s, 120s) | 3x | No |
| LLM rate limit | Queue delay + retry | Yes (5min) | No |
| DB write failure | Log error, retry | 3x | If all fail |
| Image corrupted | Mark submission as failed | No | Yes |

### 7.5 Model Routing

| Question Type | Primary Model | Fallback | Rationale |
|--------------|---------------|----------|-----------|
| Mathematics / Calculation | GPT-4o | Gemini 2.0 Flash | Highest equation accuracy |
| Science / Multi-step | GPT-4o | Gemini 2.0 Flash | Complex reasoning |
| Conceptual / Open-ended | Gemini 2.0 Flash | GPT-4o | Cost-effective, good quality |
| MCQ / Fill-blank | Exact match | LLM | No AI needed |
| Diagram-based | GPT-4 Vision | Gemini Vision | Image understanding |

---

## 8. AI Tutoring & Visual Explanation Engine

### 8.1 Architecture

```
Student (Web/Mobile)
    |
    â”œâ”€â”€ Text: Chat message
    â”œâ”€â”€ Voice: Microphone -> Whisper STT
    â””â”€â”€ Context: Current topic/challenge
        |
        v
+----------------------------+
|    TUTORING ORCHESTRATOR   |  (FastAPI, sync - low latency)
+----------------------------+
    |             |             |
    v             v             v
+---------+  +---------+  +-----------+
|   RAG   |  |   LLM   |  |  NOVELTY  |
| Retrieve|  |  Reason |  |  CHECK    |
+---------+  +---------+  +-----------+
    |             |             |
    v             v             v
+----------------------------+
|     OUTPUT GENERATORS      |
+-----------+----------------+
| Text +    | TTS (ElevenLabs)|
| Mermaid   | (async)         |
+-----------+----------------+
```

### 8.2 RAG Implementation

```python
# app/services/rag_service.py
from openai import OpenAI

class RAGIngestionService:
    def __init__(self):
        self.client = OpenAI()
        self.splitter = RecursiveCharacterTextSplitter(
            chunk_size=512, chunk_overlap=50,
            separators=["\n\n", "\n", ".", " "]
        )

    async def ingest_textbook(self, topic_id: str, text: str, source: str):
        """Chunk + embed textbook content into pgvector."""
        chunks = self.splitter.split_text(text)
        for chunk in chunks:
            content_hash = hashlib.sha256(chunk.encode()).hexdigest()
            # Skip if already exists
            existing = await supabase.table('topic_embeddings')
                .select('id').eq('content_hash', content_hash)
                .single().execute()
            if existing.data: continue

            # Generate embedding
            emb = self.client.embeddings.create(
                input=chunk, model="text-embedding-3-small")
            await supabase.table('topic_embeddings').insert({
                'topic_id': topic_id,
                'embedding': emb.data[0].embedding,
                'chunk_text': chunk,
                'chunk_source': source,
                'content_hash': content_hash,
            }).execute()

    async def retrieve(self, query: str, topic_id: str, top_k: int = 5):
        """Cosine similarity search via pgvector."""
        emb = self.client.embeddings.create(
            input=query, model="text-embedding-3-small")
        result = await supabase.rpc('match_topic_embeddings', {
            'query_embedding': emb.data[0].embedding,
            'topic_id': topic_id,
            'match_threshold': 0.7,
            'match_count': top_k
        }).execute()
        return [row['chunk_text'] for row in result.data]
```

### 8.3 Tutoring Prompt Template

```
You are an encouraging, patient AI tutor for {subject} at Grade {grade_level}.
Use the curriculum context below to ensure your answers are factually aligned.

CURRICULUM CONTEXT:
{retrieved_chunks}

STUDENT'S QUESTION: {student_message}
PREVIOUS CONTEXT: {conversation_history}

RULES:
1. NEVER give the full answer first â€” guide with Socratic questions.
2. If the concept involves a sequence, relationship, or structure,
   include a Mermaid diagram inside ```mermaid blocks.
3. Use simple language appropriate for Grade {grade_level}.
4. Praise effort, not just correct answers.
5. If the student seems stuck, offer a concrete hint.
6. If the student made an error, explain WHY and show the correct approach.
7. Reference specific topics from the curriculum context.
```

### 8.4 Mermaid Diagram Types for Teaching

| Scenario | Diagram Type | Example |
|----------|-------------|---------|
| Problem-solving steps | `flowchart TD` | "Steps to solve a quadratic equation" |
| Concept relationships | `mindmap` | "How forces are related" |
| Process flow | `flowchart LR` | "How photosynthesis works" |
| Compare & contrast | `flowchart` (branches) | "Mitosis vs Meiosis" |
| Math derivation | `flowchart TD` | "Completing the square" |

**Example LLM output:**
```
Great question! Let me show you how the quadratic formula is derived:

```mermaid
flowchart TD
    A[Start: ax\u00b2+bx+c=0] --> B[Divide by a: x\u00b2+b/a x+c/a=0]
    B --> C[Move constant: x\u00b2+b/a x=-c/a]
    C --> D[Add (b/2a)\u00b2 to both sides]
    D --> E[Left side: (x + b/2a)\u00b2]
    E --> F[Take square root]
    F --> G[x = -b \u00b1 \u221a(b\u00b2-4ac) / 2a]
```

The key insight is in step D â€” by adding (b/2a)\u00b2, we create a perfect
square trinomial. Let me know if you want to walk through each step.
```

### 8.5 Voice Pipeline

```python
# app/services/voice_service.py
class VoiceService:
    def __init__(self):
        self.stt_client = OpenAI()
        self.tts_provider = "elevenlabs"

    async def speech_to_text(self, audio_bytes: bytes) -> str:
        response = await self.stt_client.audio.transcriptions.create(
            model="whisper-1",
            file=("input.webm", audio_bytes, "audio/webm"),
            language="en", response_format="text")
        return response

    async def text_to_speech(self, text: str, voice_id: str = "...") -> str:
        audio = generate(text=text, voice=voice_id,
            model="eleven_multilingual_v2",
            output_format="mp3_44100_128")
        file_path = f"tutoring/audio/{uuid.uuid4()}.mp3"
        await supabase.storage.from_('tutoring').upload(file_path, audio)
        return supabase.storage.from_('tutoring').get_public_url(file_path)
```

### 8.6 Challenge Workflow (State Machine)

```
SUBMISSION_GRADED
    | student clicks "Challenge"
    v
CHALLENGE_CREATED -> AI auto-explains its grading
    | -> Teacher notified
    v
+-- STUDENT_REPLIES ----+
|  - Messages, questions | ----+
+-----------------------+     |
                              v
+-- TEACHER_REVIEWS ----+    |
|  - Sees AI logic      |<---+
|  - Sees all evidence  |
+-----------------------+     |
    | teacher issues verdict  |
    v                         |
RESOLVED                      |
  grade_updated / grade_upheld|
    |
    v
GRASP_UPDATED (recalculated with final grade)
```

### 8.7 Hallucination Guard

```python
class NoveltyCheckService:
    async def verify_response(self, response: str, context: list[str]) -> dict:
        """Verify AI tutor claims against retrieved curriculum context."""
        prompt = f"""
        Given curriculum context and an AI tutor response, identify claims
        NOT supported by the context.

        CONTEXT: {' '.join(context)}
        RESPONSE: {response}

        Return JSON:
        {{"has_unsupported_claims": bool, "unsupported_claims": [...],
          "confidence": "high|medium|low"}}
        """
        result = await self.checker.chat(prompt)
        return result
```

---
## 9. UI/UX â€” Full Design Specification

### 9.1 Student Dashboard

```
+----------------------------------------------------------+
| [Logo] The Atlas     ðŸ”” 3   ðŸ‘¤ John Doe    [Logout]       |
+----------------------------------------------------------+
|  ðŸ“Š Dashboard                                             |
|  ðŸ“ Assessments   +-----------------------------------+   |
|  ðŸ“ˆ My Progress   |  GRASP HEATMAP                   |   |
|  ðŸ¤– AI Tutor      |  +-----+-----+-----+-----+      |   |
|  âš¡ Challenges     |  | ðŸ”´  | ðŸŸ¡  | ðŸŸ¢  | ðŸŸ¢  |      |   |
|                   |  | QF  | Trig| Calc| Stat|      |   |
|                   |  +-----+-----+-----+-----+      |   |
|                   |  | ðŸŸ¢  | ðŸ”´  | ðŸŸ¡  | ðŸŸ¢  |      |   |
|                   |  | Alg | Geo | Pro | Num |      |   |
|                   |  +-----+-----+-----+-----+      |   |
|                   |  ðŸ”´ = needs help   ðŸŸ¢ = mastered|   |
|                   +-----------------------------------+   |
|                   +-----------------------------------+   |
|                   |  PENDING ASSESSMENTS              |   |
|                   |  ðŸ“ Algebra Quiz â€” due in 2h  â±   |   |
|                   |  â–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–‘â–‘ 80% time remaining   |   |
|                   |  [Start Quiz â†’]                   |   |
|                   |  ---                              |   |
|                   |  ðŸ“ Physics Test â€” due in 3 days   |   |
|                   |  [Start Quiz â†’]                   |   |
|                   +-----------------------------------+   |
|                   +-----------------------------------+   |
|                   |  RECENT FEEDBACK                  |   |
|                   |  â€¢ Algebra Q3: "Check your units" |   |
|                   |  â€¢ Physics Q1: âœ… Perfect         |   |
|                   |  â€¢ Trig Q2: âš ï¸ Minor error        |   |
|                   +-----------------------------------+   |
|                   +-----------------------------------+   |
|                   |  ðŸ¤– [Chat with Tutor AI]           |   |
|                   +-----------------------------------+   |
+----------------------------------------------------------+
```

### 9.2 Assessment Taking Interface

```
+----------------------------------------------------------+
|  â† Back                           â± 22:34 remaining      |
|  ðŸ“ Algebra â€” Weekly Quiz (5 questions)                   |
+----------------------------------------------------------+
|  Question 3 of 5  (5 marks)                              |
|  +----------------------------------------------------+  |
|  | Solve for x: 2xÂ² + 5x - 3 = 0                      |  |
|  | Show all working.                                   |  |
|  +----------------------------------------------------+  |
|                                                          |
|  Your answer:                                            |
|  +----------------------------------------------------+  |
|  | [Text input area or handwriting canvas]             |  |
|  +----------------------------------------------------+  |
|                                                          |
|  ðŸ“· [Snap photo of handwritten work]                     |
|                                                          |
|  +----------------------------------------------------+  |
|  | < Prev             [Save Draft]          Next >    |  |
|  +----------------------------------------------------+  |
|                                                          |
|  Progress: â–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–ˆâ–‘â–‘â–‘â–‘â–‘â–‘â–‘â–‘â–‘â–‘ 3/5 answered              |
|                        [Submit All Answers â†’]             |
+----------------------------------------------------------+
```

### 9.3 Graded Assessment Review (Student)

```
+----------------------------------------------------------+
|  â† Back                        ðŸ“ Algebra Quiz â€” Results |
+----------------------------------------------------------+
|  Score: 16 / 20  (80%)  ðŸŸ¢ Good work!                    |
|                                                          |
|  Q1: Solve 2x + 3 = 7           âœ… 3/3                   |
|  Q2: Factor xÂ² - 4               âœ… 2/2                   |
|  Q3: Solve 2xÂ² + 5x - 3 = 0     âš ï¸ 4/5                  |
|  |  Your answer: x = 0.5, -3                            |
|  |  AI: Correct but missing factoring steps.             |
|  |  [View breakdown â†’] [ðŸ¤– Explain this â†’] [âš¡ Challenge] |
|                                                          |
|  Q4: Simplify (x+2)(x-3)         âœ… 3/3                   |
|  Q5: Word problem                 âš ï¸ 4/5                  |
+----------------------------------------------------------+
```

### 9.4 AI Tutor Panel

```
+----------------------------------------------------------+
|  ðŸ¤– AI Tutor â€” Quadratic Equations                       |
+----------------------------------------------------------+
|  You: Why did I lose marks on Q3?                        |
|                                                          |
|  Tutor: Let me show you what happened. Your answer       |
|  was correct (x=0.5, x=-3), but the question asked to    |
|  show FACTORING steps. Here's what was missing:          |
|                                                          |
|  [Mermaid diagram: factoring steps visualization]        |
|                                                          |
|  The key step is finding factors of -6 that add to +5.   |
|  Can you tell me what those factors are?                 |
|                                                          |
|  ðŸ”Š [Play voice explanation]                             |
|                                                          |
|  +----------------------------------------------------+  |
|  | [Type your message...]           ðŸŽ¤ [Mic]   ðŸ“Ž     |  |
|  +----------------------------------------------------+  |
|  [End Session]                    [Share Summary]        |
+----------------------------------------------------------+
```

### 9.5 Teacher Alert Wall

```
+----------------------------------------------------------+
|  [Logo] The Atlas   ðŸ”” 5 Alerts   ðŸ‘¤ Ms. Smith            |
+----------------------------------------------------------+
|  ðŸ“Š Dashboard                                             |
|  ðŸ“‹ Classes           âš ï¸ PRIORITY ALERTS (Last 7 days)    |
|  ðŸ“ Assessments       +------------------------------+    |
|  â“ Question Bank     | Student     Topic     Score   |    |
|  âš¡ Challenges (3)    | ðŸ”´ Amy Tan  Quadratics  32%  |    |
|  ðŸ“ˆ Analytics         |    [View â†’] [Assign â†’]       |    |
|                       | ðŸŸ¡ Ben Lee  Trig       45%  |    |
|                       |    [View â†’] [Assign â†’]       |    |
|                       | ðŸŸ¡ Chen Wei  Vectors    48%  |    |
|                       |    [View â†’] [Assign â†’]       |    |
|                       +------------------------------+    |
|                       +------------------------------+    |
|                       | PENDING CHALLENGES (3)       |    |
|                       | âš¡ All Quizzes â€” Amy Q2       |    |
|                       | âš¡ Physics â€” Ben Q5           |    |
|                       | âš¡ Trig â€” Chen Q1             |    |
|                       +------------------------------+    |
+----------------------------------------------------------+
```

### 9.6 Teacher Grasp Matrix View

```
+----------------------------------------------------------+
|  Grade 10A â€” Mathematics     [Export CSV â–¼]  [Filter â–¼]  |
+----------------------------------------------------------+
|  Student \\ Topic | Quadratics | Trig | Vectors | Calc  |
|  ----------------+------------+------+---------+------  |
|  Amy Tan         |  â–ˆâ–ˆ 32%ðŸ”´  | 75%  | 62%     | 88%  |
|  Ben Lee         |  â–ˆâ–ˆ 45%ðŸŸ¡  | 52%  | 78%     | 91%  |
|  Chen Wei        |  â–ˆâ–ˆ 48%ðŸŸ¡  | 81%  | 84%     | 73%  |
|  Diana Park      |  82%ðŸŸ¢     | 91%  | 87%     | 95%  |
|  Ethan Fox       |  71%ðŸŸ¢     | 65%  | â–ˆâ–ˆ 43%ðŸ”´| 81%  |
|  ----------------+------------+------+---------+------  |
|  Class Average   |  56%       | 73%  | 71%     | 86%  |
|  Struggling      |  3         | 1    | 1       | 0    |
+----------------------------------------------------------+
  ðŸ”´ Below 50%   ðŸŸ¡ 50-69%   ðŸŸ¢ 70%+
  [Click any cell to see assessment history for that topic]
```

### 9.7 Challenge Resolution Panel (Teacher)

```
+----------------------------------------------------------+
|  âš¡ Challenge â€” Amy Tan â€” Algebra Quiz Q2                |
+----------------------------------------------------------+
|  +----------------------+  +---------------------------+  |
|  | STUDENT SUBMISSION   |  | AI GRADING EXPLANATION    |  |
|  | [Image of student     |  | Rubric: Q2 (5 marks)      |  |
|  |   handwritten work]  |  |  - Setup: âœ… 2/2          |  |
|  |                      |  |  - Substitution: âŒ 0/2    |  |
|  |                      |  |  - Final answer: âœ… 1/1    |  |
|  |                      |  |  AI verdict: 3/5           |  |
|  |                      |  |  "Used wrong formula       |  |
|  |                      |  |   in substitution step."   |  |
|  +----------------------+  +---------------------------+  |
|                                                          |
|  STUDENT CHALLENGE:                                      |
|  "I used the quadratic formula correctly. The marker     |
|   misread my '2a' as 'a'. Please check again."           |
|                                                          |
|  AI SUGGESTED ADJUSTMENT:                                |
|  "Upon review, student shows 2a in denominator.          |
|   Recommended: score 4/5 (partial for handwriting)."     |
|                                                          |
|  YOUR VERDICT:  â—‹ Grade upheld   â— Grade updated: [4] /5 |
|  Teacher note: [I see 2a in denominator. Adjusted.]      |
|                                                          |
|  [Cancel]                              [Submit Verdict]  |
+----------------------------------------------------------+
```

### 9.8 Accessibility Checklist (WCAG 2.1 AA)

| Requirement | Implementation |
|------------|---------------|
| Color contrast â‰¥ 4.5:1 | Tailwind CSS with contrast-verified palette |
| Keyboard navigation | All interactive elements focusable, tab-order logical |
| Screen reader support | ARIA labels on canvas, diagrams, and charts |
| Focus indicators | Visible focus ring on all interactive elements |
| Text resizing up to 200% | Responsive layout with rem units |
| Alt text on all images | Required for uploaded photos and diagrams |
| Captions on voice content | STT text displayed alongside TTS audio |
| Error identification | Inline validation with specific error messages |
| Time limits adjustable | Assessment timer extendable by teacher |
| Motion sensitivity | Reduced motion media query for animations |

---
## 10. Security â€” Multi-Layered

### 10.1 Authentication & Authorization Stack

| Layer | Mechanism | Notes |
|-------|-----------|-------|
| **Identity** | Supabase Auth (GoTrue) | Email/password, Google OAuth, Magic Link |
| **Session** | JWT (Supabase-generated) | 1-hour expiry, refresh token rotation |
| **Authorization** | Supabase RLS | Row-level policies on every table |
| **API Gateway** | Kong rate limiting | Per-role limits defined in section 5.5 |
| **Role enforcement** | `user_roles` table + middleware | Backend middleware checks role before processing |

### 10.2 RLS Access Matrix

| Table | Student | Teacher | Admin | Parent |
|-------|:-------:|:-------:|:-----:|:-----:|
| `users` | Own record | Own school | Own school | Children only |
| `classes` | Enrolled only | Teaching only | All school | Children's |
| `submissions` | Own only | Own class | Via teacher | Children's |
| `grasp_scores` | Own only | Own class | Via teacher | Children's |
| `assessments` | Assigned | Own created | All school | Children's |
| `challenges` | Own | Own class | Via teacher | Children's |
| `questions` | No read | Read/Write | Full | No read |
| `notifications` | Own | Own | Own | Own |

### 10.3 Input Validation

```python
# backend/app/middleware/validation.py
from pydantic import BaseModel, Field, validator
import magic

class SubmissionCreate(BaseModel):
    assessment_id: str = Field(pattern=r'^[0-9a-f-]{36}$')  # UUID
    answer_text: Optional[str] = Field(max_length=50000)

    @validator('answer_text')
    def sanitize_text(cls, v):
        if v:
            import re
            v = re.sub(r'<[^>]*>', '', v)  # Strip HTML
            return v
        return v

class FileUploadValidator:
    ALLOWED_MIME = {'image/jpeg', 'image/png', 'image/webp', 'application/pdf'}
    MAX_FILE_SIZE = 10 * 1024 * 1024  # 10MB
    MAX_FILES = 5

    @staticmethod
    async def validate(file: UploadFile) -> bool:
        content = await file.read()
        if len(content) > FileUploadValidator.MAX_FILE_SIZE:
            raise HTTPException(413, "File too large")
        mime = magic.from_buffer(content[:2048], mime=True)
        if mime not in FileUploadValidator.ALLOWED_MIME:
            raise HTTPException(415, f"Unsupported type: {mime}")
        if b'<?php' in content or b'<script' in content:
            raise HTTPException(400, "Disallowed content")
        return True
```

### 10.4 API Key Management

```python
# backend/app/config.py â€” Enhanced
from pydantic_settings import BaseSettings

class Settings(BaseSettings):
    # NEVER hardcode â€” always via environment variables
    openai_api_key: str
    novita_api_key: str
    gemini_api_key: Optional[str] = None
    database_url: str
    supabase_url: str
    supabase_service_key: str      # Server-side only
    supabase_anon_key: str         # Safe for frontend (RLS-protected)
    redis_url: str = "redis://localhost:6379/0"
    allowed_origins: list[str] = ["http://localhost:3000"]
    rate_limit_enabled: bool = True
    default_grading_model: str = "gpt-4o"
    tutoring_model: str = "gemini-2.0-flash"

    class Config:
        env_file = ".env"
        secrets_dir = "/run/secrets"  # Docker secrets
```

### 10.5 Data Privacy (GDPR / FERPA)

| Requirement | Implementation |
|------------|---------------|
| Right to access | `GET /api/v1/users/me/export` â€” all data as JSON |
| Right to deletion | `DELETE /api/v1/users/me` â€” cascades to anonymize |
| Data portability | Export as JSON or CSV |
| Breach notification | Sentry alerts + admin email within 72h |
| Encryption at rest | Supabase AES-256 encryption |
| Encryption in transit | TLS 1.3 at gateway level |
| PII minimization | Only email + name + avatar; UUIDs everywhere else |
| Retention | Submissions kept for current year + 1 year, then anonymized |
| No PII to LLMs | Only anonymized answer text sent to OpenAI/Gemini |

### 10.6 Content Moderation

```python
class ContentModerator:
    async def moderate(self, text: str) -> dict:
        response = await self.openai.moderations.create(input=text)
        result = response.results[0]
        if result.flagged:
            return {"allowed": False,
                "categories": [k for k, v in result.categories.items() if v]}
        return {"allowed": True}
```

---
## 11. Observability & DevOps

### 11.1 OpenTelemetry Tracing

```python
# backend/app/telemetry.py
from opentelemetry import trace
from opentelemetry.exporter.otlp.proto.grpc.trace_exporter import OTLPSpanExporter
from opentelemetry.instrumentation.fastapi import FastAPIInstrumentor
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchSpanProcessor

def setup_telemetry(app):
    provider = TracerProvider()
    provider.add_span_processor(BatchSpanProcessor(
        OTLPSpanExporter(endpoint=os.getenv("OTEL_EXPORTER_OTLP_ENDPOINT"))))
    trace.set_tracer_provider(provider)
    FastAPIInstrumentor.instrument_app(app)
```

**Key trace spans and alert thresholds:**

| Span Name | What It Measures | Alert at |
|-----------|-----------------|----------|
| `grade_submission` | Total grading time | > 60s |
| `ocr_process` | OCR pipeline latency | > 15s |
| `llm_inference.gpt4o` | GPT-4o API round-trip | > 10s |
| `llm_inference.gemini` | Gemini API round-trip | > 8s |
| `rag_retrieval` | pgvector similarity search | > 500ms |
| `tts_generation` | ElevenLabs TTS latency | > 3s |
| `stt_transcription` | Whisper time per minute | > 10s |

### 11.2 Prometheus Metrics

```prometheus
# Grading
atlas_grading_jobs_total{status="success|failed|retried"}
atlas_grading_duration_seconds{model="gpt-4o|gemini-2.0-flash"}

# Tutoring
atlas_tutoring_sessions_active
atlas_tutoring_response_time_seconds

# Business
atlas_grasp_alerts_total{class="10A"}
atlas_challenges_resolved{verdict="upheld|updated"}
atlas_students_below_proficiency{class="10A"}
```

### 11.3 Alert Rules (Prometheus)

```yaml
groups:
  - name: atlas_alerts
    rules:
      - alert: HighGradingLatency
        expr: histogram_quantile(0.95, atlas_grading_duration_seconds) > 60
        for: 5m
        labels: { severity: warning }
        annotations:
          summary: "Grading P95 latency > 60s for 5 minutes"

      - alert: HighOCRFailureRate
        expr: rate(atlas_grading_jobs_total{status="failed"}[15m])
              / rate(atlas_grading_jobs_total[15m]) > 0.1
        for: 5m
        labels: { severity: critical }
        annotations:
          summary: "OCR failure rate exceeds 10%"

      - alert: TutoringLatencyHigh
        expr: histogram_quantile(0.95, atlas_tutoring_response_time_seconds) > 10
        for: 5m
        labels: { severity: warning }

      - alert: LowOpenAIQuota
        expr: atlas_llm_api_remaining_quota{provider="openai"} < 1000
        for: 1m
        labels: { severity: critical }
```

### 11.4 Logging Strategy

```python
# backend/app/utils/logger.py
from loguru import logger
import json, sys

logger.remove()
logger.add(sys.stderr,
    format="{time} | {level} | {name}:{function}:{line} | {message}")
logger.add("logs/atlas.json",
    format=lambda r: json.dumps({
        "timestamp": r["time"].isoformat(), "level": r["level"].name,
        "module": r["name"], "function": r["function"],
        "line": r["line"], "message": r["message"], "extra": r["extra"]}),
    rotation="100 MB", retention="30 days", serialize=True)
logger.add("logs/errors.json",
    filter=lambda r: r["level"].name in ("ERROR", "CRITICAL"),
    rotation="50 MB", retention="90 days", serialize=True)
```

### 11.5 Docker Compose (Extended from Assessly)

```yaml
version: '3.8'
services:
  postgres:
    image: supabase/postgres:15.1.0.110
    ports: ["5432:5432"]
    environment:
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
      POSTGRES_DB: atlas
    volumes: [postgres_data:/var/lib/postgresql/data]

  redis:
    image: redis:7-alpine
    ports: ["6379:6379"]
    command: redis-server --appendonly yes
    volumes: [redis_data:/data]

  backend:
    build: ./backend
    ports: ["8000:8000"]
    depends_on: [postgres, redis]
    environment:
      DATABASE_URL: postgresql://postgres:${POSTGRES_PASSWORD}@postgres:5432/atlas
      REDIS_URL: redis://redis:6379/0
    volumes: [uploads:/app/uploads, ./backend:/app]

  celery_worker:
    build: ./backend
    command: celery -A celery_worker worker -l info -Q grading,question_gen,notifications -c 4
    depends_on: [postgres, redis]
    environment:
      DATABASE_URL: postgresql://postgres:${POSTGRES_PASSWORD}@postgres:5432/atlas
      REDIS_URL: redis://redis:6379/0

  flower:
    build: ./backend
    command: celery -A celery_worker flower --port=5555
    ports: ["5555:5555"]
    depends_on: [redis]

  frontend:
    build: . # Next.js Dockerfile
    ports: ["3000:3000"]
    depends_on: [backend]
    environment:
      NEXT_PUBLIC_API_URL: http://backend:8000
      NEXT_PUBLIC_SUPABASE_URL: ${SUPABASE_URL}
      NEXT_PUBLIC_SUPABASE_ANON_KEY: ${SUPABASE_ANON_KEY}

volumes:
  postgres_data:
  redis_data:
  uploads:
```

---
## 12. Testing Strategy

### 12.1 Backend Testing (pytest)

```python
# backend/tests/conftest.py
import pytest
from fastapi.testclient import TestClient
from app.main import app

@pytest.fixture
def client(test_db):
    with TestClient(app) as c:
        yield c

@pytest.fixture
def mock_llm(mocker):
    mocker.patch('app.services.grading_engine.GradingEngine._call_llm',
        return_value={
            "score": 4.0, "max_score": 5.0,
            "strengths": ["Correct approach"],
            "weaknesses": ["Minor arithmetic error"],
            "stepwise_breakdown": []})
    return mocker
```

**Test files and what they cover:**

```python
# tests/test_grading.py
class TestGradingPipeline:
    async def test_full_pipeline(self, client, mock_llm):
        """End-to-end: upload -> OCR -> grade -> grasp update"""
    async def test_ocr_fallback(self, client):
        """PaddleOCR failure triggers GPT-4 Vision fallback"""
    async def test_multi_question_grading(self, client):
        """Each question graded independently"""
    async def test_math_symbolic_validation(self, client):
        """SymPy cross-validates LLM math grading"""

# tests/test_grasp.py
class TestGraspMatrix:
    async def test_score_update_after_grading(self, test_db):
        """Grasp score recalculated after submission graded"""
    async def test_alert_generation_below_50(self, test_db):
        """Alert triggered when grasp drops below 50"""
    async def test_trend_calculation(self, test_db):
        """Trend: improving/stable/declining"""

# tests/test_challenge.py
class TestChallengeWorkflow:
    async def test_challenge_creates_discussion(self, client):
        """Challenge submission creates a thread"""
    async def test_teacher_resolution_updates_grade(self, client):
        """Verdict updates final_grade"""
```

### 12.2 Frontend Testing (Vitest + Testing Library)

```typescript
// __tests__/components/grasp-heatmap.test.tsx
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { GraspHeatmap } from '@/components/student/grasp-heatmap';

describe('GraspHeatmap', () => {
  it('renders color-coded topic cells', () => {
    render(<GraspHeatmap data={[
      { topic: 'Algebra', score: 85 },  // green
      { topic: 'Trig', score: 32 },     // red
    ]} />);
    expect(screen.getByText('Algebra')).toBeInTheDocument();
    expect(screen.getByText('Trig')).toBeInTheDocument();
  });

  it('shows tooltip on hover', async () => {
    render(<GraspHeatmap data={[{ topic: 'Algebra', score: 85 }]} />);
    await userEvent.hover(screen.getByText('Algebra'));
    expect(screen.getByText('Score: 85%')).toBeInTheDocument();
  });
});

// __tests__/hooks/use-realtime.test.ts
describe('useGraspRealtime', () => {
  it('subscribes to supabase channel on mount', () => {
    const { result } = renderHook(() => useGraspRealtime('student-123'));
    // Verify supabase.channel() called with correct params
  });

  it('unsubscribes on unmount', () => {
    const { unmount } = renderHook(() => useGraspRealtime('student-123'));
    unmount();
    // Verify removeChannel() called
  });
});
```

### 12.3 E2E Testing (Playwright)

```typescript
// e2e/student-assessment-flow.spec.ts
import { test, expect } from '@playwright/test';

test('student takes assessment and sees grade', async ({ page }) => {
  await page.goto('/login');
  await page.fill('[name="email"]', 'student@test.com');
  await page.fill('[name="password"]', 'password123');
  await page.click('button[type="submit"]');
  await expect(page).toHaveURL('/student/dashboard');

  await page.click('text=Algebra Quiz');
  await page.fill('[data-testid="answer-input"]', 'x = 5');
  await page.click('text=Next');
  await page.fill('[data-testid="answer-input"]', 'y = 3');
  await page.click('text=Submit All Answers');

  await page.waitForSelector('text=Graded', { timeout: 60000 });
  await expect(page.locator('[data-testid="total-score"]')).toBeVisible();
});

test('student challenges a grade', async ({ page }) => {
  await page.goto('/student/assessments/some-id/results');
  await page.click('text=Challenge');
  await page.fill('[data-testid="challenge-reason"]', 'Answer was correct');
  await page.click('text=Submit Challenge');
  await expect(page.locator('text=Challenge submitted')).toBeVisible();
});

test('teacher resolves challenge', async ({ page }) => {
  await page.goto('/login');
  await page.fill('[name="email"]', 'teacher@test.com');
  await page.fill('[name="password"]', 'password123');
  await page.click('button[type="submit"]');

  await page.click('text=Challenges');
  await page.click('text=Amy Tan');
  await page.selectOption('[name="verdict"]', 'grade_updated');
  await page.fill('[name="final_grade"]', '4');
  await page.fill('[name="teacher_feedback"]', 'Adjusted after review');
  await page.click('text=Submit Verdict');
  await expect(page.locator('text=Challenge resolved')).toBeVisible();
});
```

### 12.4 LLM Output Evaluation

```python
# tests/test_llm_outputs.py
"""Evaluate LLM grading accuracy against human-graded samples."""

class TestLLMGradingAccuracy:
    """Gold-standard: compare AI grades against human expert grades."""

    @pytest.mark.parametrize("sample", [
        # Loaded from test fixtures directory
        {"question": "2+2", "student_answer": "4",
         "human_grade": 5.0, "max": 5.0},
        {"question": "Solve 2x+3=7", "student_answer": "x=2",
         "human_grade": 5.0, "max": 5.0},
        {"question": "Explain photosynthesis",
         "student_answer": "Plants use sunlight...",
         "human_grade": 3.0, "max": 5.0},
    ])
    async def test_grading_accuracy(self, sample):
        """AI grade within 1 point of human grade on 5-point scale."""
        engine = GradingEngine()
        result = await engine.grade_single_question(
            question_text=sample["question"],
            student_answer=sample["student_answer"],
            max_points=sample["max"])
        assert abs(result["score"] - sample["human_grade"]) <= 1.0

    async def test_no_hallucinated_points(self):
        """AI must not invent rubric criteria that don't exist."""

    async def test_consistent_grading(self):
        """Same answer gets same grade (deterministic under same settings)."""
```

### 12.5 Coverage Targets

| Layer | Tool | Target |
|-------|------|--------|
| Backend services | pytest + coverage.py | 90% line coverage |
| Backend API routes | pytest + TestClient | 100% route coverage |
| Frontend components | Vitest + Testing Library | 80% line coverage |
| Frontend hooks | Vitest | 90% |
| E2E critical paths | Playwright | 10 core flows |
| LLM output quality | Custom eval suite | â‰¥90% agreement with humans |

---
## 13. Implementation Roadmap from Assessly

### Phase 0: Fork Assessly + Auth (2 weeks)

| # | Task | Acceptance Criteria |
|---|------|-------------------|
| 1 | Fork Assessly, set up monorepo | Builds locally |
| 2 | Integrate Supabase Auth | Student/teacher login works |
| 3 | Add user_roles + schools tables | RLS policies testable |
| 4 | Replace SQLAlchemy auto-create with Alembic migrations | `alembic upgrade head` succeeds |
| 5 | Fix Celery integration | Grading runs via Celery, not BackgroundTasks |
| 6 | Add login page + role-based redirect | Student sees student dash, teacher sees teacher dash |

**Dependencies:** Supabase project, OpenAI API key, Novita API key

---

### Phase 1: Curriculum + Grasp Matrix (3 weeks)

| # | Task | Acceptance Criteria |
|---|------|-------------------|
| 1 | Curriculum topics schema + admin CRUD | Admin can add topics with prerequisites |
| 2 | pgvector setup + topic embedding ingestion | Textbook PDF -> embedded topics |
| 3 | Grasp matrix calculation service | Grading pipeline updates grasp_scores |
| 4 | Grasp heatmap frontend component | Student sees color-coded topic grid |
| 5 | Teacher grasp matrix view | Teacher sees class grasp table |
| 6 | Alert wall (materialized view + frontend) | Teacher sees struggling students |

**Dependencies:** Phase 0 complete

---

### Phase 2: Question Generation + Assessments (3 weeks)

| # | Task | Acceptance Criteria |
|---|------|-------------------|
| 1 | AI question generation endpoint | Teacher gets 5 draft questions |
| 2 | Question bank CRUD + search | Teacher can browse, edit, delete |
| 3 | Assessment creation with AI-assist | Teacher creates assessment with AI-suggested questions |
| 4 | Assessment scheduling + student notif | Publishing notifies enrolled students |
| 5 | Student assessment-taking UI | Timed quiz with photo/text upload |
| 6 | IRT difficulty tracking | Question difficulty updates after each attempt |

**Dependencies:** Phase 1 (need topics + grasp)

---

### Phase 3: AI Tutoring (3 weeks)

| # | Task | Acceptance Criteria |
|---|------|-------------------|
| 1 | RAG retrieval service | Tutoring queries return relevant curriculum chunks |
| 2 | Tutoring session endpoint + WebSocket | Student can start session, get response |
| 3 | Mermaid diagram rendering | AI-generated Mermaid renders as SVG |
| 4 | Whisper STT integration | Student speaks, message appears as text |
| 5 | ElevenLabs TTS integration | AI response plays as audio |
| 6 | Challenge workflow | Student challenges -> AI explains -> Teacher resolves |
| 7 | Hallucination guard | Responses verified against RAG context |

**Dependencies:** Phase 1 (need RAG with topic embeddings)

---

### Phase 4: Mobile + Polish (3 weeks)

| # | Task | Acceptance Criteria |
|---|------|-------------------|
| 1 | React Native app shell + login | Student can log in on mobile |
| 2 | Mobile assessment taking | Student can take quiz on phone |
| 3 | Mobile tutor (voice-first) | Student can speak, hear+see answer |
| 4 | Push notifications | Student gets push for grade, challenge |
| 5 | Parent read-only view | Parent sees children's progress |
| 6 | WCAG accessibility audit | Passes automated aXe scan |
| 7 | Performance optimization | Lighthouse > 85 on all pages |

**Dependencies:** Phases 0-3 complete

---

### Phase 5: Pilot + Iteration (ongoing)

| Task | Timeline |
|------|----------|
| Teacher training + documentation | 1 week |
| Pilot with 2-3 schools (100 students) | 4 weeks |
| Gather feedback, iterate | 2 weeks |
| Expand to more schools | Ongoing |
| Fine-tune LLM on school-specific curricula | As needed |

---

### Key Success Metrics (KPIs)

| Metric | Target |
|--------|--------|
| **Weak student reduction** | â‰¥30% decrease in students with grasp <50% in 6 months |
| **Teacher time saved** | â‰¥15 hours/week per teacher (from grading) |
| **AI-teacher grade agreement** | â‰¥90% on math, â‰¥85% on conceptual |
| **Challenge resolution time** | â‰¤24 hours for 80% of challenges |
| **Student engagement** | â‰¥4 active sessions/week per student |
| **Monthly retention** | â‰¥85% of enrolled students active |

---

## 14. Appendices

### Appendix A â€” Assessly to Atlas File Mapping

| Assessly File | Atlas File | Change |
|---------------|-----------|--------|
| `backend/app/main.py` | Extend | Add routers |
| `backend/app/api/upload.py` | Keep | â€” |
| `backend/app/api/grading.py` | Refactor | Add grasp update |
| `backend/app/api/websocket.py` | Extend | Add tutoring WS |
| `backend/app/services/ocr_service.py` | Keep | â€” |
| `backend/app/services/grading_engine.py` | Refactor | Add IRT |
| `backend/app/services/multi_question_grader.py` | Keep | â€” |
| `backend/app/services/math_validator.py` | Integrate | Wire into pipeline |
| `backend/app/models/*.py` | Remove | Supabase-managed |
| `backend/celery_worker.py` | Fix | Actually use |
| `frontend/components/grading-canvas-integrated.tsx` | Keep | Move to teacher/ |
| `frontend/components/grading-editor-content-live.tsx` | Keep | Move to teacher/ |
| `frontend/lib/api-client.ts` | Extend | Add JWT interceptor |
| â€” NEW â€” | `backend/app/api/auth.py` | Auth endpoints |
| â€” NEW â€” | `backend/app/api/curriculum.py` | Topics CRUD |
| â€” NEW â€” | `backend/app/api/questions.py` | Question gen + CRUD |
| â€” NEW â€” | `backend/app/api/assessments.py` | Assessment CRUD |
| â€” NEW â€” | `backend/app/api/tutoring.py` | Tutoring endpoints |
| â€” NEW â€” | `backend/app/api/challenges.py` | Challenge workflow |
| â€” NEW â€” | `backend/app/api/grasp.py` | Grasp matrix API |
| â€” NEW â€” | `backend/app/services/grasp_service.py` | Grasp calc |
| â€” NEW â€” | `backend/app/services/rag_service.py` | RAG retrieval |
| â€” NEW â€” | `backend/app/services/voice_service.py` | STT/TTS |
| â€” NEW â€” | `backend/app/services/novelty_check.py` | Hallucination guard |
| â€” NEW â€” | `backend/app/services/irt_service.py` | IRT params |
| â€” NEW â€” | `backend/app/tasks/grading.py` | Celery grading |
| â€” NEW â€” | `backend/app/tasks/question_gen.py` | Celery Q gen |
| â€” NEW â€” | `backend/app/tasks/notifications.py` | Celery notifs |
| â€” NEW â€” | `frontend/components/student/*` | 8 components |
| â€” NEW â€” | `frontend/components/teacher/*` | 6 components |
| â€” NEW â€” | `frontend/components/tutor/*` | 4 components |
| â€” NEW â€” | `frontend/lib/hooks/*` | 6 hooks |
| â€” NEW â€” | `frontend/lib/stores/*` | 3 stores |

### Appendix B â€” Environment Variables Checklist

```bash
# Supabase
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_ANON_KEY=eyJ...                # Public, safe in frontend
SUPABASE_SERVICE_KEY=eyJ...             # Server-only, KEEP SECRET

# Database
DATABASE_URL=postgresql://postgres:password@host:5432/atlas

# AI Providers
OPENAI_API_KEY=sk-...                   # GPT-4o, Whisper, embeddings
NOVITA_API_KEY=...                      # PaddleOCR-VL
GEMINI_API_KEY=AIza...                  # Gemini 2.0 Flash
ELEVENLABS_API_KEY=...                  # TTS

# Redis / Celery
REDIS_URL=redis://localhost:6379/0

# Storage
STORAGE_BUCKET=atlas-uploads
STORAGE_REGION=us-east-1

# Deployment
ALLOWED_ORIGINS=http://localhost:3000,https://your-app.com
SITE_URL=https://your-app.com
DEBUG=false
PORT=8000

# Monitoring
SENTRY_DSN=https://...
OTEL_EXPORTER_OTLP_ENDPOINT=http://localhost:4317

# Security
RATE_LIMIT_ENABLED=true
MAX_FILE_SIZE=10485760                  # 10MB
JWT_EXPIRY_MINUTES=60
```

### Appendix C â€” LLM Cost Projection (1000 Students)

**Assumptions:** Daily quizzes (5 questions), 2 monthly tests, 5 tutoring sessions/month at 10 msgs each, 50% math (GPT-4o) / 50% conceptual (Gemini Flash)

| Operation | Volume/month | Model | Cost/Unit | Monthly Cost |
|-----------|-------------|-------|-----------|-------------|
| Grading: Math | 30,000 | GPT-4o | $0.01 | $300 |
| Grading: Conceptual | 30,000 | Gemini Flash | $0.0004 | $12 |
| OCR: PaddleOCR-VL | 10,000 | Novita API | $0.003 | $30 |
| OCR: Vision fallback (10%) | 1,000 | GPT-4 Vision | $0.02 | $20 |
| Question Generation | 500 | GPT-4o-mini | $0.001 | $0.50 |
| Tutoring Messages | 50,000 | Gemini Flash | $0.0005 | $25 |
| TTS (ElevenLabs) | 500k chars | ElevenLabs | $0.0001/char | $50 |
| STT (Whisper) | 5,000 min | Whisper | $0.006/min | $30 |
| Embeddings | 10,000 chunks | text-embedding-3-small | ~$0.00002 | $0.10 |
| **TOTAL** | | | | **~$467/mo** |

**Cost reduction strategies:**
1. Cache identical grading prompts (Redis) â€” saves ~20% LLM calls
2. Use OpenRouter for competitive Gemini Flash pricing
3. Self-host Qwen2.5 via Ollama for tutoring at high volume
4. Batch embed 100 chunks at a time (OpenAI supports batch)
5. Use community TTS models at scale instead of ElevenLabs
6. Limit tutoring session duration per student (e.g. 20 min/day)

### Appendix D â€” Risk Matrix

| Risk | Impact | Likelihood | Mitigation |
|------|:------:|:----------:|-----------|
| LLM hallucinates answers | High | Medium | RAG before generation; novelty check; teacher override |
| OCR fails on messy handwriting | Medium | Medium | GPT-4 Vision fallback; manual teacher review |
| Grading latency > 5s | Medium | Low | Celery priority queues; cached rubric patterns |
| Data privacy violation | High | Low | Self-host LLMs (Ollama); never send PII to APIs |
| Teacher resistance to AI | Medium | Medium | Pilot as "AI assistant"; show time savings early |
| Over-reliance on AI teaching | Low | Medium | Human stays final authority; AI never overrides |

---

**This document provides a complete blueprint** â€” from database schema to API contract, from frontend component tree to testing strategy, from Assessly migration path to LLM cost projections. The platform builds incrementally on the proven Assessly grading foundation, extending it into a full virtual school where every weak student gets prioritized support, and the source of truth â€” the grasp matrix â€” drives every decision.

**The north star remains:** *Nothing is more important than the successful learning of every student.*

---

**End of document** â€” v2.0 Enhanced Edition
