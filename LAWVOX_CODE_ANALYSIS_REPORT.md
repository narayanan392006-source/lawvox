# LAWVOX - Comprehensive Code Analysis Report

**Created:** September 16, 2026  
**Repository:** https://github.com/shahanamarwa/LAWVOX  
**Analysis Scope:** Full codebase - Frontend & Backend

---

## 📋 Table of Contents

1. [Project Overview](#project-overview)
2. [Technology Stack](#technology-stack)
3. [Architecture Analysis](#architecture-analysis)
4. [Frontend Architecture](#frontend-architecture)
5. [Backend Architecture](#backend-architecture)
6. [Database Schema](#database-schema)
7. [API Endpoints](#api-endpoints)
8. [Key Features](#key-features)
9. [Seeded Constitutional Cases](#seeded-constitutional-cases)
10. [Security & Best Practices](#security--best-practices)
11. [Installation & Setup](#installation--setup)
12. [Development Workflow](#development-workflow)

---

## 🎯 Project Overview

**LAWVOX** is a **Constitutional Precedent Research and Legal Audio Platform** designed for legal professionals, judges, lawyers, and law students to research landmark constitutional cases with integrated audio content.

### Purpose
- Provide easy access to landmark constitutional precedents
- Enable audio-based learning of complex constitutional law cases
- Allow legal professionals to bookmark, annotate, and track their research
- Maintain research notes and search history
- Offer a unified dashboard with personalized statistics

### Target Users
- Senior Advocates and Constitutional Law practitioners
- Judges and judicial officers
- Law students and researchers
- Legal institutions and bar associations

---

## 🛠️ Technology Stack

### Frontend
```
├── Framework: Next.js 15.1.7
├── Language: TypeScript
├── UI Library: React 19
├── Styling: Tailwind CSS 4.0
├── Component Library: Radix UI
│   ├── @radix-ui/react-avatar (1.1.3)
│   ├── @radix-ui/react-dialog (1.1.6)
│   ├── @radix-ui/react-dropdown-menu (2.1.6)
│   ├── @radix-ui/react-slider (1.2.3)
│   └── @radix-ui/react-tabs (1.1.3)
├── Icons: Lucide React
├── Utilities: class-variance-authority, clsx, tailwind-merge
└── Notifications: Sonner (2.0.1)
```

### Backend
```
├── Runtime: Node.js 18+
├── Framework: Express.js 4.21.2
├── Language: TypeScript 5.7.3
├── Database: SQLite (better-sqlite3 13.0.3)
├── CORS: cors 2.8.5
├── Environment: dotenv 16.4.7
└── Dev Tools: ts-node-dev, TypeScript compiler
```

### Database
- **Engine:** SQLite 3
- **Location:** `backend/data/lawvox.db`
- **Type:** Local file-based relational database
- **Architecture:** Layered with services handling all queries

---

## 🏗️ Architecture Analysis

### High-Level Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    CLIENT TIER (Frontend)                    │
│  ┌──────────────┐  ┌──────────────┐  ┌─────────────────┐   │
│  │ Next.js App  │  │ React Pages  │  │ Radix UI Comps  │   │
│  │ (TypeScript) │  │ (Components) │  │ (Styled)        │   │
│  └──────────────┘  └──────────────┘  └─────────────────┘   │
└─────────────────────────────────────────────────────────────┘
                            │
                  HTTP/REST API Calls
                            │
┌─────────────────────────────────────────────────────────────┐
│              API GATEWAY & MIDDLEWARE TIER                    │
│  ┌─────────────────────────────────────────────────────────┐ │
│  │ Express.js Server (Port 5000)                           │ │
│  │ ├── CORS Middleware                                     │ │
│  │ ├── Request Logging                                     │ │
│  │ ├── Error Handling Middleware                           │ │
│  │ └── 404 Not Found Handler                               │ │
│  └─────────────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│           APPLICATION LOGIC TIER (Routes)                    │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌───────────┐  │
│  │ Routes   │  │ Routes   │  │ Routes   │  │ Routes    │  │
│  │ /cases   │  │/bookmarks│  │/history  │  │/notes     │  │
│  └──────────┘  └──────────┘  └──────────┘  └───────────┘  │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│         REQUEST HANDLING TIER (Controllers)                  │
│  ├── Parse incoming requests                                │
│  ├── Validate request data                                  │
│  ├── Call appropriate services                              │
│  └── Format and send responses                              │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│         BUSINESS LOGIC TIER (Services)                       │
│  ├── Execute database queries                               │
│  ├── Handle business rules                                  │
│  ├── Calculate statistics                                   │
│  ├── Perform search operations                              │
│  └── Manage data transformations                            │
└─────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────┐
│         DATA ACCESS TIER (SQLite Database)                   │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐            │
│  │   Cases    │  │ Bookmarks  │  │   Notes    │            │
│  │   Table    │  │   Table    │  │   Table    │            │
│  └────────────┘  └────────────┘  └────────────┘            │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐            │
│  │  History   │  │  Profile   │  │ Settings   │            │
│  │   Table    │  │   Table    │  │   Table    │            │
│  └────────────┘  └────────────┘  └────────────┘            │
└─────────────────────────────────────────────────────────────┘
```

### Architectural Pattern
**Layered Architecture (N-Tier Architecture)**
- **Separation of Concerns:** Each layer has a specific responsibility
- **Maintainability:** Changes in one layer don't cascade unnecessarily to others
- **Testability:** Services can be tested independently from HTTP layer
- **Scalability:** Easy to add new features without major refactoring

---

## 🎨 Frontend Architecture

### Directory Structure

```
frontend/
├── public/                          # Static assets
├── src/
│   ├── app/                         # Next.js app directory
│   │   └── ...                      # Pages and layouts
│   ├── assets/                      # Images, icons, fonts
│   ├── components/                  # Reusable UI components
│   │   ├── ui/                      # Base Radix UI components
│   │   ├── layout/                  # Layout components
│   │   └── features/                # Feature-specific components
│   ├── context/                     # Global state management
│   │   └── AppContext.ts            # Shared app state
│   ├── data/                        # Mock data & constants
│   ├── services/                    # API client functions
│   │   ├── caseService.ts           # Cases API calls
│   │   ├── bookmarkService.ts       # Bookmarks API calls
│   │   └── ...                      # Other services
│   ├── types/                       # TypeScript interfaces
│   │   └── index.ts                 # Type definitions
│   ├── utils/                       # Helper functions
│   └── styles/                      # Global styles
├── package.json
├── tailwind.config.ts               # Tailwind configuration
├── tsconfig.json                    # TypeScript config
├── next.config.mjs                  # Next.js config
└── eslint.config.js                 # ESLint config
```

### Key Frontend Technologies

| Technology | Version | Purpose |
|------------|---------|---------|
| Next.js | 15.1.7 | React framework with SSR & static generation |
| React | 19 | UI library and component system |
| TypeScript | 5.7.3 | Type safety and better developer experience |
| Tailwind CSS | 4.0 | Utility-first CSS framework |
| Radix UI | Latest | Headless, accessible component primitives |
| Lucide React | 0.475.0 | Modern icon library |

### Component Organization

**Atomic Design Pattern:**
- **Atoms:** Basic UI elements (buttons, inputs)
- **Molecules:** Combinations of atoms (search bar, card)
- **Organisms:** Complex components (case list, dashboard)
- **Templates:** Page layouts
- **Pages:** Full page components

### State Management
- **React Context API:** Global app state
- **Local State:** Component-level useState
- **Services:** API communication layer

---

## 🔧 Backend Architecture

### Directory Structure

```
backend/
├── src/
│   ├── server.ts                    # Express server entry point
│   ├── app.ts                       # Express app setup & middleware
│   │
│   ├── config/
│   │   └── database.ts              # SQLite connection & initialization
│   │
│   ├── database/
│   │   ├── schema.sql               # Database schema
│   │   └── seed.ts                  # Data seeding (8 landmark cases)
│   │
│   ├── routes/                      # Express route definitions
│   │   ├── index.ts
│   │   ├── cases.routes.ts
│   │   ├── bookmarks.routes.ts
│   │   ├── history.routes.ts
│   │   ├── notes.routes.ts
│   │   ├── profile.routes.ts
│   │   ├── settings.routes.ts
│   │   ├── dashboard.routes.ts
│   │   └── searches.routes.ts
│   │
│   ├── controllers/                 # Request handlers
│   │   ├── cases.controller.ts
│   │   ├── bookmarks.controller.ts
│   │   ├── history.controller.ts
│   │   ├── notes.controller.ts
│   │   ├── profile.controller.ts
│   │   ├── settings.controller.ts
│   │   ├── dashboard.controller.ts
│   │   └── searches.controller.ts
│   │
│   ├── services/                    # Business logic
│   │   ├── cases.service.ts
│   │   ├── bookmarks.service.ts
│   │   ├── history.service.ts
│   │   ├── notes.service.ts
│   │   ├── profile.service.ts
│   │   ├── settings.service.ts
│   │   ├── dashboard.service.ts
│   │   └── searches.service.ts
│   │
│   ├── middleware/                  # Express middleware
│   │   ├── error.middleware.ts      # Error handling
│   │   └── notFound.middleware.ts   # 404 handler
│   │
│   ├── types/                       # TypeScript interfaces
│   │   └── index.ts
│   │
│   └── utils/                       # Helper functions
│       └── response.ts              # JSON response builder
│
├── data/
│   └── lawvox.db                    # SQLite database file
│
├── .env                             # Environment variables
├── .env.example                     # Environment template
├── package.json
├── tsconfig.json
└── README.md
```

### Request Flow Diagram

```
HTTP Request
    │
    ▼
┌─────────────────────────────┐
│ Express Middleware Pipeline │
│ ├── CORS                    │
│ ├── JSON Parser             │
│ └── Logger                  │
└─────────────────────────────┘
    │
    ▼
┌─────────────────────────────┐
│ Route Matching              │
│ GET /api/cases/:id          │
└─────────────────────────────┘
    │
    ▼
┌─────────────────────────────┐
│ Cases Controller            │
│ ├── Extract params          │
│ ├── Validate input          │
│ └── Call service            │
└─────────────────────────────┘
    │
    ▼
┌─────────────────────────────┐
│ Cases Service               │
│ ├── Execute SQL query       │
│ ├── Transform data          │
│ └── Return result           │
└─────────────────────────────┘
    │
    ▼
┌─────────────────────────────┐
│ SQLite Database             │
│ └── Execute & return data   │
└─────────────────────────────┘
    │
    ▼
┌─────────────────────────────┐
│ Response Builder            │
│ ├── Format JSON             │
│ └── Set status code         │
└─────────────────────────────┘
    │
    ▼
JSON Response to Client
```

### Layer Responsibilities

| Layer | Responsibility | Should NOT Contain |
|-------|-----------------|-------------------|
| **Routes** | URL → Controller mapping | Business logic |
| **Controllers** | Parse request, call service, format response | Database queries |
| **Services** | Business logic, SQL queries, calculations | HTTP details (req/res) |
| **Database** | Execute queries, manage schema | Business rules |
| **Middleware** | Cross-cutting concerns (auth, validation, errors) | Route-specific logic |

---

## 🗄️ Database Schema

### SQLite Tables

#### 1. **cases** - Constitutional Case Records
```sql
CREATE TABLE cases (
  id TEXT PRIMARY KEY,
  case_name TEXT NOT NULL,
  court TEXT,
  year INTEGER,
  citation TEXT,
  category TEXT,
  judge TEXT,
  constitutional_provisions TEXT,
  summary TEXT,
  legal_issue TEXT,
  decision TEXT,
  keywords TEXT,
  bench_size TEXT,
  doctrine TEXT,
  audio_url TEXT,
  created_at TIMESTAMP,
  updated_at TIMESTAMP
);
```

**Fields Explained:**
- `id`: Unique kebab-case identifier (e.g., "kesavananda-bharati")
- `case_name`: Full case title
- `court`: Jurisdiction (Supreme Court of India, High Court, etc.)
- `year`: Year of judgment
- `citation`: Legal citation format (e.g., "(1973) 4 SCC 225")
- `category`: Constitutional topic (e.g., "Fundamental Rights")
- `judge`: Names of judges in bench
- `constitutional_provisions`: Relevant articles (e.g., "Article 21, Article 14")
- `summary`: Extended case summary
- `legal_issue`: Key legal question posed
- `decision`: Court's ruling and rationale
- `keywords`: Search-friendly keywords
- `bench_size`: Number of judges (e.g., "13-Judge Bench")
- `doctrine`: Legal doctrine established (e.g., "Basic Structure Doctrine")
- `audio_url`: Path to audio recording
- `created_at`, `updated_at`: Timestamps

#### 2. **bookmarks** - User Bookmarked Cases
```sql
CREATE TABLE bookmarks (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  case_id TEXT NOT NULL,
  created_at TIMESTAMP,
  FOREIGN KEY (case_id) REFERENCES cases(id)
);
```

#### 3. **listening_history** - Playback Progress
```sql
CREATE TABLE listening_history (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  case_id TEXT NOT NULL,
  duration_listened INTEGER,
  completion_percentage REAL,
  last_position INTEGER,
  created_at TIMESTAMP,
  updated_at TIMESTAMP,
  FOREIGN KEY (case_id) REFERENCES cases(id)
);
```

#### 4. **notes** - Research Notes
```sql
CREATE TABLE notes (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  title TEXT,
  case_id TEXT,
  content TEXT,
  created_at TIMESTAMP,
  updated_at TIMESTAMP,
  FOREIGN KEY (case_id) REFERENCES cases(id)
);
```

#### 5. **profile** - User Profile
```sql
CREATE TABLE profile (
  id INTEGER PRIMARY KEY,
  name TEXT,
  profession TEXT,
  email TEXT,
  institution TEXT,
  research_interests TEXT,
  created_at TIMESTAMP,
  updated_at TIMESTAMP
);
```

#### 6. **settings** - Application Settings
```sql
CREATE TABLE settings (
  id INTEGER PRIMARY KEY,
  notification_enabled BOOLEAN,
  autoplay_enabled BOOLEAN,
  playback_speed REAL,
  language TEXT,
  appearance TEXT,
  created_at TIMESTAMP,
  updated_at TIMESTAMP
);
```

#### 7. **searches** - Search History
```sql
CREATE TABLE searches (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  query TEXT,
  created_at TIMESTAMP
);
```

### Database Relationships

```
cases (1) ──────── (∞) bookmarks
  │
  ├────── (∞) listening_history
  │
  └────── (∞) notes

profile (1) ──── global setting
settings (1) ──── global setting
searches (∞) ──── independent log
```

---

## 📡 API Endpoints

### Base URL
```
http://localhost:5000/api
```

### Complete API Reference

#### 1. Health Check
```
GET /api/health
```
**Purpose:** Verify backend is running  
**Response (200):**
```json
{
  "success": true,
  "message": "LAWVOX backend is running"
}
```

---

#### 2. Dashboard
```
GET /api/dashboard
```
**Purpose:** Unified dashboard data  
**Response (200):**
```json
{
  "success": true,
  "data": {
    "welcome": {
      "userName": "Adv. Arvind Narain",
      "profession": "Constitutional Law Senior Advocate",
      "institution": "Supreme Court Bar Association",
      "greeting": "Good afternoon, Adv."
    },
    "statistics": {
      "totalListeningTimeSeconds": 2250,
      "totalListeningTimeFormatted": "37m",
      "casesListened": 4,
      "bookmarksCount": 2,
      "dailyAverageMinutes": 38
    },
    "continueListening": { /* case object */ },
    "recentCases": [ /* cases array */ ],
    "bookmarks": [ /* bookmarked cases */ ],
    "categories": [ /* case categories */ ],
    "recommendations": [ /* recommended cases */ ],
    "recentSearches": [ /* search history */ ]
  }
}
```

---

#### 3. Cases API

**List All Cases:**
```
GET /api/cases?court=Supreme+Court&year=2017&category=Privacy
```
**Query Parameters:** `court`, `year`, `category`, `judge`, `doctrine`

**Get Single Case:**
```
GET /api/cases/:id
```
**Example:** `GET /api/cases/kesavananda-bharati`

**Create Case:**
```
POST /api/cases
Content-Type: application/json

{
  "case_name": "S.R. Bommai v. Union of India",
  "court": "Supreme Court of India",
  "year": 1994,
  "citation": "(1994) 3 SCC 1",
  "category": "Judicial Review",
  "judge": "9-Judge Bench",
  "constitutional_provisions": "Article 356",
  "summary": "Landmark decision on federalism",
  "audio_url": "/audio/sr-bommai.mp3"
}
```

**Update Case:**
```
PUT /api/cases/:id
```

**Delete Case:**
```
DELETE /api/cases/:id
```

---

#### 4. Search API

**Global Search:**
```
GET /api/search?q=privacy&court=Supreme+Court&year=2017
```
**Query Parameters:** `q`, `court`, `year`, `category`, `judge`, `doctrine`

---

#### 5. Bookmarks API

**List Bookmarks:**
```
GET /api/bookmarks
```

**Check Bookmark:**
```
GET /api/bookmarks/:caseId
```

**Add Bookmark:**
```
POST /api/bookmarks/:caseId
```

**Remove Bookmark:**
```
DELETE /api/bookmarks/:caseId
```

---

#### 6. Listening History API

**Get History:**
```
GET /api/history
```

**Record Progress:**
```
POST /api/history
{
  "case_id": "kesavananda-bharati",
  "duration_listened": 525,
  "completion_percentage": 63.0,
  "last_position": 525
}
```

**Update Progress:**
```
PUT /api/history/:id
```

**Delete Entry:**
```
DELETE /api/history/:id
```

---

#### 7. Notes API

**List Notes:**
```
GET /api/notes?case_id=kesavananda-bharati
```

**Get Single Note:**
```
GET /api/notes/:id
```

**Create Note:**
```
POST /api/notes
{
  "title": "Article 368 Analysis",
  "case_id": "kesavananda-bharati",
  "content": "Key precedent for constitutional amendments..."
}
```

**Update Note:**
```
PUT /api/notes/:id
```

**Delete Note:**
```
DELETE /api/notes/:id
```

---

#### 8. Profile API

**Get Profile:**
```
GET /api/profile
```

**Update Profile:**
```
PUT /api/profile
{
  "name": "Adv. Arvind Narain",
  "profession": "Senior Advocate",
  "email": "arvind@lawvox.internal",
  "institution": "Supreme Court Bar Association",
  "research_interests": "Fundamental Rights, Privacy"
}
```

---

#### 9. Settings API

**Get Settings:**
```
GET /api/settings
```

**Update Settings:**
```
PUT /api/settings
{
  "notification_enabled": true,
  "autoplay_enabled": true,
  "playback_speed": 1.25,
  "language": "English",
  "appearance": "light"
}
```

---

#### 10. Search History API

**List Searches:**
```
GET /api/searches?limit=10
```

**Record Search:**
```
POST /api/searches
{
  "query": "Right to Privacy"
}
```

**Delete Search:**
```
DELETE /api/searches/:id
```

---

## 🎯 Key Features

### 1. **Constitutional Case Database**
- 8+ landmark constitutional cases pre-seeded
- Full-text search across case metadata
- Filter by court, year, category, judge, doctrine
- Audio support for each case

### 2. **Bookmark Management**
- Save favorite cases for quick reference
- Prevent duplicate bookmarks
- Easy removal of bookmarks
- Integrated in dashboard

### 3. **Listening Progress Tracking**
- Track playback position and duration
- Calculate completion percentage
- Resume from last position
- Display "Continue Listening" on dashboard

### 4. **Research Notes**
- Create notes linked to specific cases
- Edit and delete notes
- Query notes by case
- Timestamped entries

### 5. **User Profile Management**
- Store practitioner information
- Track profession and institution
- Record research interests
- Update profile information

### 6. **Search History**
- Maintain record of search queries
- Quick access to frequent searches
- Clear search history
- Limit results

### 7. **Dashboard Analytics**
- Total listening time statistics
- Cases listened count
- Daily average minutes
- Bookmarks count
- Recent cases and searches
- Case categories
- Personalized recommendations

### 8. **Audio Integration**
- MP3 audio files for each case
- Playback progress tracking
- Duration recording
- Audio URL storage in database

---

## 🏛️ Seeded Landmark Constitutional Cases

### 1. Kesavananda Bharati v. State of Kerala (1973)
- **Citation:** (1973) 4 SCC 225 | AIR 1973 SC 1461
- **Bench:** 13-Judge Constitutional Bench
- **Doctrine:** Basic Structure Doctrine
- **Topic:** Constitutional Amendments (Article 368)
- **Significance:** Established that Parliament cannot alter the basic structure of the Constitution

### 2. Shreya Singhal v. Union of India (2015)
- **Citation:** (2015) SCC Online SC 1
- **Topic:** Free Speech & Overbreadth (Article 19(1)(a))
- **Significance:** Struck down Section 66A for being overbroad

### 3. Indian Young Lawyers Association v. State of Kerala (2018)
- **Citation:** (2018) 10 SCC 1
- **Topic:** Sabarimala Equality (Articles 14, 15, 25)
- **Significance:** Gender equality in religious practices

### 4. Olga Tellis v. Bombay Municipal Corporation (1985)
- **Citation:** (1985) 2 SCC 51
- **Topic:** Right to Livelihood (Article 21)
- **Significance:** Expanded interpretation of right to life

### 5. Maneka Gandhi v. Union of India (1978)
- **Citation:** (1978) 1 SCC 248
- **Topic:** Golden Triangle & Fair Procedure (Article 21)
- **Significance:** Natural justice and due process

### 6. Vishaka v. State of Rajasthan (1997)
- **Citation:** (1997) 6 SCC 241
- **Topic:** Workplace Harassment Guidelines (Articles 14, 19, 21)
- **Significance:** Established sexual harassment workplace guidelines

### 7. Justice K.S. Puttaswamy v. Union of India (2017)
- **Citation:** (2017) 10 SCC 1 | AIR 2017 SC 4161
- **Bench:** 9-Judge Constitutional Bench
- **Topic:** Fundamental Right to Privacy (Article 21)
- **Significance:** Declared privacy as a fundamental right

### 8. Golaknath v. State of Punjab (1967)
- **Citation:** (1967) 2 SCC 762
- **Topic:** Parliamentary Amending Limits (Article 368)
- **Significance:** Fundamental rights cannot be amended

---

## 🔒 Security & Best Practices

### Environment Variables
```env
PORT=5000
DATABASE_PATH=./data/lawvox.db
FRONTEND_URL=http://localhost:3000
NODE_ENV=development
```

**Never commit `.env` files - use `.env.example` template**

### Error Handling
- Centralized error middleware catches all errors
- Consistent JSON error response format:
```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Human-readable error message"
  }
}
```

### Response Format
All responses follow unified format:
```json
{
  "success": true|false,
  "data": { /* response data */ } OR "error": { /* error details */ }
}
```

### Type Safety
- Full TypeScript implementation
- Interface definitions for all data types
- Compile-time type checking prevents runtime errors

### Middleware Pipeline
```
Incoming Request
    │
    ├─→ CORS Check
    │
    ├─→ Request Logging
    │
    ├─→ JSON Parser
    │
    ├─→ Route Matching
    │
    ├─→ Controller Execution
    │
    ├─→ Error Handler (catches all)
    │
    ├─→ 404 Handler
    │
    └─→ Response Send
```

### Naming Conventions
- **Files:** `camelCase` or `kebab-case` (consistent within project)
- **Variables/Functions:** `camelCase`
- **Classes/Components:** `PascalCase`
- **Constants:** `UPPER_SNAKE_CASE`

---

## 📦 Installation & Setup

### Prerequisites
- Node.js 18+ installed
- npm or yarn package manager
- SQLite 3 (included in better-sqlite3)

### Frontend Setup

```bash
# Navigate to frontend
cd frontend

# Install dependencies
npm install

# Install Tailwind CSS
npm install -D tailwindcss postcss autoprefixer

# Start development server
npm run dev

# Build for production
npm run build

# Start production server
npm start
```

**Development URL:** `http://localhost:3000`

### Backend Setup

```bash
# Navigate to backend
cd backend

# Install dependencies
npm install

# Create .env file
cp .env.example .env

# Configure environment (edit .env)
# PORT=5000
# DATABASE_PATH=./data/lawvox.db
# FRONTEND_URL=http://localhost:3000
# NODE_ENV=development

# Seed database with landmark cases
npm run seed

# Start development server
npm run dev

# Build TypeScript
npm run build

# Start production server
npm start
```

**API URL:** `http://localhost:5000/api`

### Full Stack Startup (From Root)

```bash
# Terminal 1: Frontend
cd frontend
npm run dev

# Terminal 2: Backend
cd backend
npm run dev

# Both servers running:
# Frontend: http://localhost:3000
# Backend: http://localhost:5000/api
```

### Database Initialization
- Auto-seeds on first startup
- 8 landmark constitutional cases loaded
- Create indexes for performance
- Foreign key constraints enabled

---

## 🔄 Development Workflow

### Git Workflow
1. Create feature branch: `git checkout -b feature/feature-name`
2. Make commits: `git commit -m "feat: add feature description"`
3. Push branch: `git push origin feature/feature-name`
4. Open pull request for code review
5. Merge to main after approval

### Commit Message Format
```
<type>: <subject>

<body>

<footer>
```

**Types:** `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

### Code Quality
- TypeScript for type safety
- ESLint for code standards
- Prettier for formatting
- Consistent naming conventions
- Layered architecture for maintainability

### Testing Strategy
- Unit tests for services
- Integration tests for APIs
- Manual testing for UI components
- Pre-commit hooks (optional)

### Local Development
```bash
# Install all dependencies
npm install in both frontend/ and backend/

# Run both servers
npm run dev (in each directory, separate terminals)

# Database auto-seeds on startup

# Access frontend: http://localhost:3000
# Access API: http://localhost:5000/api
# Check health: http://localhost:5000/api/health
```

---

## 📊 Project Statistics

| Metric | Value |
|--------|-------|
| **Frontend Files** | ~20+ components |
| **Backend Files** | Routes, Controllers, Services, Middleware |
| **Database Tables** | 7 tables |
| **Seeded Cases** | 8 landmark constitutional cases |
| **API Endpoints** | 30+ endpoints |
| **Frontend Dependencies** | 15+ packages |
| **Backend Dependencies** | 5 core packages |

---

## 🚀 Future Enhancements

### Potential Features
1. **User Authentication** - Login/logout with sessions
2. **AI-Powered Summaries** - Ollama integration for case summaries
3. **Case Recommendations** - ML-based recommendations
4. **Export Functionality** - PDF/Word export of notes
5. **Collaborative Features** - Share notes with other users
6. **Advanced Search** - Full-text search with filters
7. **Mobile App** - React Native mobile version
8. **Real-time Updates** - WebSocket for live case updates
9. **Analytics Dashboard** - Track usage statistics
10. **Multi-language Support** - Translations

---

## 📝 Documentation Files

- `backend/README.md` - Backend setup guide
- `backend/API_DOCUMENTATION.md` - Complete API reference
- `frontend/README.md` - Frontend setup guide
- `context_project_structure.txt` - Industry standards guide
- `context_frontend_standards.txt` - Frontend best practices

---

## 🤝 Contributing

1. Fork the repository
2. Create feature branch
3. Make improvements
4. Commit with clear messages
5. Push to branch
6. Open pull request
7. Await code review
8. Merge upon approval

---

## 📞 Support & Contact

For issues, questions, or feature requests:
- Open an issue on GitHub
- Email: lawvox@example.com
- Documentation: See `backend/API_DOCUMENTATION.md`

---

**Last Updated:** September 16, 2026  
**Analysis Version:** 1.0  
**Repository:** https://github.com/shahanamarwa/LAWVOX  

---

This comprehensive analysis provides developers with a complete understanding of the LAWVOX codebase, its architecture, features, and setup process.
