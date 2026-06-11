# DECTIFY Backend Engineering Assessment

## ⏰ Submission Deadline

📅 13 June 2026 (Saturday) — 11:59 PM IST

⚠️ Ensure your repository and deliverables are submitted before the deadline.

---

## 📚 Quick Links

- [Overview](#overview)
- [Business Context](#business-context)
- [General Instructions](#general-instructions)
- [Approved Technologies](#approved-technologies)
- [Assignment Details](#assignment-details)
- [Required Endpoints](#required-endpoints)
- [Authentication Requirements](#authentication-requirements)
- [NASA API Integration](#nasa-api-integration)
- [Caching Requirements](#caching-requirements)
- [Database Requirements](#database-requirements)
- [Validation & Error Handling](#validation--error-handling)
- [Testing Requirements](#testing-requirements)
- [Deliverables](#deliverables)
- [Bonus Tasks](#bonus-tasks)
- [Evaluation Criteria](#evaluation-criteria)
- [Submission Instructions](#submission-instructions)
- [Frontend Engineering Assessment](#dectify-frontend-engineering-assessment)

---

# Backend Engineering Intern Technical Assessment

Build a production-quality backend service that aggregates NASA data, provides authentication, implements caching, and manages user favorites.

---

## Overview

At DECTIFY, backend systems frequently integrate with multiple third-party services while maintaining performance, reliability, and security.

This assessment evaluates your ability to:

- Build scalable REST APIs
- Implement secure JWT authentication
- Integrate external APIs
- Design caching systems
- Handle failures gracefully
- Write maintainable and testable code

You will build a backend service called:

### Space Today API

A REST API that serves NASA data to authenticated users while minimizing upstream API usage through intelligent caching.

---

## Business Context

At DECTIFY, our backend systems often aggregate data from external services such as:

- Maps & Geolocation APIs
- Weather APIs
- Communication Services
- Traffic Intelligence Systems
- Surveillance Data Providers

This assessment simulates a real-world backend challenge involving:

- Third-party API integration
- Authentication & authorization
- Caching strategies
- Rate limiting
- User personalization

The frontend consumes your API. NASA APIs must never be exposed directly to clients.

---

## General Instructions

- Submit all code through a public GitHub repository.
- Include a complete README.md.
- Include setup instructions and environment variables.
- Do not commit secrets or API keys.
- You may use open-source libraries.
- You must understand and explain your implementation.
- Use clean architecture and modular design.

---

## Approved Technologies

### Python

- FastAPI (Preferred)
- Django REST Framework

### Node.js

- Express.js
- Fastify
- NestJS

TypeScript is strongly encouraged for Node.js implementations.

---

## Assignment Details

### Assignment Title

**Space Today Backend API**

Build a backend service that:

- Authenticates users
- Retrieves NASA data
- Caches responses
- Stores user favorites
- Provides OpenAPI documentation
- Handles upstream failures gracefully

### Focus Areas

| Area | Description |
|--------|--------|
| REST APIs | Production-grade API design |
| JWT Authentication | Secure auth flows |
| Third-Party APIs | NASA integration |
| Caching | Efficient rate-limit protection |
| Validation | Robust input validation |
| Database Design | User & favorites management |

---

## Required Endpoints

All endpoints return JSON.

### Authentication

| Method | Endpoint | Auth Required |
|----------|----------|----------|
| POST | /auth/signup | No |
| POST | /auth/login | No |
| POST | /auth/refresh | Refresh Token |
| POST | /auth/logout | Yes |
| GET | /auth/me | Yes |

### NASA Data

| Method | Endpoint | Auth Required |
|----------|----------|----------|
| GET | /space/apod | Yes |
| GET | /space/asteroids | Yes |
| GET | /space/mars-photos | Yes |
| GET | /space/earth-events | Yes |
| GET | /space/dashboard | Yes |

### Favorites

| Method | Endpoint |
|----------|----------|
| POST | /favorites |
| GET | /favorites |
| DELETE | /favorites/:id |

---

## Authentication Requirements

### JWT Authentication

Implement:

- Access Token
- Refresh Token

### Token Expiry

| Token | Expiry |
|---------|---------|
| Access Token | 15 Minutes |
| Refresh Token | 7 Days |

### Security Requirements

- Password hashing using bcrypt or Argon2
- Secret loaded from environment variables
- No plaintext password storage
- Refresh tokens stored server-side
- Logout must revoke refresh tokens

### HTTP Status Codes

| Status | Meaning |
|----------|----------|
| 401 | Missing or Invalid Token |
| 403 | Permission Denied |

---

## NASA API Integration

Use NASA Open APIs.

Base URL:

```text
https://api.nasa.gov
```

### Required Services

#### APOD

Astronomy Picture of the Day

```text
/planetary/apod
```

#### Near Earth Objects

```text
/neo/rest/v1/feed
```

#### Mars Rover Photos

```text
/mars-photos/api/v1/rovers
```

#### EONET Events

```text
https://eonet.gsfc.nasa.gov/api/v3/events
```

### Requirements

- NASA_API_KEY loaded from environment
- Backend proxies all requests
- API key never exposed to frontend

### Dashboard Endpoint

```text
GET /space/dashboard
```

Must:

- Fetch all NASA endpoints in parallel
- Return combined response
- Handle partial failures gracefully

Example:

If Mars API fails but APOD succeeds:

```json
{
  "apod": {},
  "mars_photos": {
    "error": "Service unavailable"
  }
}
```

Do not return HTTP 500 for partial failures.

---

## Caching Requirements

This section carries significant evaluation weight.

### Requirements

- Cache NASA responses for at least 1 hour
- APOD can be cached longer if desired
- Cache layer must be swappable

Examples:

- Memory Cache (Development)
- Redis (Production)

### Design Expectations

Use a dedicated cache abstraction layer.

Avoid:

```js
cache.set(...)
```

being scattered across route handlers.

### README Documentation

Explain:

- Cache Keys
- TTL Strategy
- Cache Invalidation
- Cache Hit Behavior

---

## Database Requirements

### Database

Accepted:

- SQLite
- PostgreSQL (Bonus)

### Required Tables

#### Users

Stores:

- User account information

#### Refresh Tokens

Stores:

- Active refresh tokens
- Revoked tokens

#### Favorites

Stores:

- user_id
- item_type
- item_payload
- saved_at

### Migrations

Must be included.

Examples:

- Alembic
- Prisma
- Knex

---

## Validation & Error Handling

### Validation

Use schema validation.

Examples:

- Pydantic
- Zod
- Joi

### Required Status Codes

- 200
- 201
- 400
- 401
- 403
- 404
- 422
- 429
- 500

### Standard Error Format

```json
{
  "error": {
    "code": "INVALID_TOKEN",
    "message": "Token is invalid"
  }
}
```

### Rate Limiting

Implement:

#### General API

```text
100 requests/minute
```

#### Authentication Routes

```text
5 requests/minute
```

---

## Testing Requirements

### Unit Tests

Cover:

- Password hashing
- JWT generation
- JWT validation
- Refresh flow

### Integration Tests

Test at least:

- Two protected endpoints
- Authentication flow

### NASA API Testing

NASA APIs must be mocked.

Never call real NASA services during CI.

### Test Command

Single command execution:

```bash
pytest
```

or

```bash
npm test
```

Must be documented in README.

---

## Deliverables

Your submission must include:

- Public GitHub Repository
- Source Code
- README.md
- OpenAPI Documentation
- Database Migrations
- Unit Tests
- Integration Tests

---

### README Requirements

Include:

#### Setup Instructions

How to run locally.

#### Environment Variables

Document all variables.

#### Run Commands

Examples:

```bash
npm install
npm run dev
```

or

```bash
pip install -r requirements.txt
uvicorn app.main:app --reload
```

#### Example Requests

Provide sample curl requests for every endpoint.

---

### Design Report

Include a short report (400–600 words) covering:

- Authentication Strategy
- Cache Design
- Upstream Failure Handling
- Database Design
- Future Improvements

---

## Bonus Tasks

Optional but highly valued:

- Dockerfile
- Docker Compose
- Redis Integration
- PostgreSQL
- Scheduled Cache Refresh
- Health Check Endpoint
- Structured Logging
- Email Verification
- GitHub Actions CI/CD
- Live Deployment
- Request Tracing
- Background Jobs

---

## Evaluation Criteria

| Area | What We Evaluate |
|--------|--------|
| Technical Correctness | End-to-end functionality |
| Code Quality | Structure, maintainability, readability |
| Results & Tests | Unit and integration coverage |
| Problem Understanding | Architecture and design decisions |
| Creativity | Additional engineering improvements |

---

## Submission Instructions

1. Complete the assessment.
2. Push your code to a public GitHub repository.
3. Include all required deliverables.
4. Verify tests pass successfully.
5. Submit the repository link before the deadline.

### Submission Package

- GitHub Repository Link
- OpenAPI Documentation
- Design Report

Optional:

- Loom Walkthrough Video

---

## Good Luck

We look forward to seeing how you think, design, and build.

This assessment reflects the type of API engineering, integrations, and backend architecture challenges encountered at DECTIFY.

**— DECTIFY Recruitment Team**

---
---

# DECTIFY Frontend Engineering Assessment

## ⏰ Submission Deadline

📅 48 Hours from Receipt of Assignment

⚠️ Ensure your repository and deliverables are submitted before the deadline.

---

## 📚 Quick Links

- [Overview](#overview-1)
- [Business Context](#business-context-1)
- [General Instructions](#general-instructions-1)
- [Approved Technologies](#approved-technologies-1)
- [Assignment Details](#assignment-details-1)
- [Required Pages & Views](#required-pages--views)
- [Authentication Requirements](#authentication-requirements-1)
- [API Integration](#api-integration)
- [State & Caching](#state--caching)
- [UI/UX Requirements](#uiux-requirements)
- [Validation & Error Handling](#validation--error-handling-1)
- [Testing Requirements](#testing-requirements-1)
- [Deliverables](#deliverables-1)
- [Bonus Tasks](#bonus-tasks-1)
- [Evaluation Criteria](#evaluation-criteria-1)
- [Submission Instructions](#submission-instructions-1)

---

# Frontend Developer Intern Technical Assessment

Build a production-quality web client for the **Space Today API** (described above) — an authenticated dashboard that surfaces NASA data, lets users manage favorites, and gracefully handles a flaky backend.

---

## Overview {#overview-1}

At DECTIFY, frontend systems frequently consume internal REST APIs that aggregate third-party data, enforce authentication, and may partially fail.

This assessment evaluates your ability to:

- Build a responsive, accessible web application
- Implement secure JWT-based auth flows (including silent refresh)
- Consume and render data from a REST API
- Handle loading, empty, and partial-failure states gracefully
- Write maintainable, componentized, testable code

You will build a frontend client called:

### Space Today

A web app that lets authenticated users explore daily NASA data (APOD, near-Earth asteroids, Mars rover photos, EONET events) and save favorites for later.

---

## Business Context {#business-context-1}

At DECTIFY, our frontend applications often sit on top of backend services that:

- Aggregate data from multiple third-party providers
- Enforce JWT authentication with short-lived access tokens
- Cache aggressively, so data may be slightly stale
- Can return partial failures for combined/dashboard endpoints

This assessment simulates a real-world frontend challenge involving:

- Auth flows (login, signup, token refresh, logout)
- Consuming a dashboard endpoint with mixed success/error fields per section
- Personalization via a favorites system
- Designing around rate limits (429 responses)

---

## General Instructions {#general-instructions-1}

- Submit all code through a public GitHub repository.
- Include a complete README.md.
- Include setup instructions and environment variables (e.g. `VITE_API_BASE_URL`).
- Do not commit secrets or API keys.
- You may use open-source libraries.
- You must understand and explain your implementation.
- Use clean architecture, modular components, and a clear folder structure.
- You may either point your app at the **Space Today backend** (provided separately) or build against a mocked API layer using the contract below — clearly document which approach you took.

---

## Approved Technologies {#approved-technologies-1}

### Frameworks

- React (Preferred) — with Vite or Next.js
- Vue 3
- Svelte / SvelteKit

### Styling

- Tailwind CSS (Preferred)
- CSS Modules
- Styled Components

### State Management

- React Query / TanStack Query (Preferred for server state)
- Redux Toolkit / Zustand (optional for client state)
- Context API (acceptable for small scope)

TypeScript is strongly encouraged.

---

## Assignment Details {#assignment-details-1}

### Assignment Title

**Space Today Frontend Client**

Build a web application that:

- Authenticates users (signup, login, refresh, logout)
- Displays a personalized dashboard of NASA data
- Allows browsing APOD, asteroids, Mars photos, and EONET events individually
- Lets users save and manage favorites
- Handles loading, empty, and partial-failure states
- Is responsive across desktop and mobile

### Focus Areas

| Area | Description |
|--------|--------|
| Auth Flows | Login, signup, silent token refresh, logout |
| Data Fetching | Consuming REST endpoints, caching, retries |
| Component Design | Reusable, composable UI components |
| State Management | Server state vs. UI state separation |
| Error & Edge Cases | Partial failures, rate limits, empty states |
| Accessibility & Responsiveness | Usable on mobile and with keyboard/screen readers |

---

## Required Pages & Views

### Auth

| Route | Description | Auth Required |
|--------|--------|--------|
| `/signup` | Create account form | No |
| `/login` | Login form | No |
| `/logout` | Triggers logout, clears session | Yes |

### Core App

| Route | Description | Auth Required |
|--------|--------|--------|
| `/dashboard` | Combined view: APOD, asteroid summary, Mars photo preview, recent EONET events | Yes |
| `/apod` | Astronomy Picture of the Day, with date picker for past entries | Yes |
| `/asteroids` | List/table of near-Earth objects with key stats (size, velocity, hazard flag) | Yes |
| `/mars-photos` | Gallery of Mars rover photos with rover/camera/sol filters | Yes |
| `/earth-events` | List/map of EONET natural events (wildfires, storms, etc.) | Yes |
| `/favorites` | User's saved items, grouped by type, with remove action | Yes |
| `/profile` | Current user info (`/auth/me`), session management | Yes |

---

## Authentication Requirements {#authentication-requirements-1}

### Token Handling

- Store the **access token** in memory (not localStorage) where possible; if persisted, document the tradeoff.
- Store the **refresh token** per the backend's mechanism (httpOnly cookie preferred; document if using storage instead).
- Implement **silent refresh**: when a request returns 401 due to an expired access token, attempt `/auth/refresh` once, then retry the original request.
- On refresh failure, redirect the user to `/login` and clear local session state.

### Required Flows

- **Signup**: form with client-side validation (email format, password strength), surfaces server-side validation errors (422).
- **Login**: form with validation, handles 401 (invalid credentials) and 429 (rate limited) distinctly.
- **Logout**: calls `/auth/logout`, clears tokens and cached data, redirects to `/login`.
- **Protected routes**: unauthenticated users attempting to access protected routes are redirected to `/login`, with a return-to path preserved.
- **Session restore**: on app load, attempt to restore session via `/auth/me` (using refresh token if access token is missing/expired) so refreshing the page doesn't log the user out.

---

## API Integration

Base URL is configurable via environment variable (e.g. `VITE_API_BASE_URL`), pointing at the Space Today backend described in the Backend section above.

### Endpoints Consumed

| Method | Endpoint | Used For |
|----------|----------|----------|
| POST | `/auth/signup` | Signup form |
| POST | `/auth/login` | Login form |
| POST | `/auth/refresh` | Silent token refresh |
| POST | `/auth/logout` | Logout action |
| GET | `/auth/me` | Profile page, session restore |
| GET | `/space/apod` | APOD page |
| GET | `/space/asteroids` | Asteroids page |
| GET | `/space/mars-photos` | Mars photos page |
| GET | `/space/earth-events` | Earth events page |
| GET | `/space/dashboard` | Dashboard page |
| GET | `/favorites` | Favorites page |
| POST | `/favorites` | "Save to favorites" actions across pages |
| DELETE | `/favorites/:id` | Removing a favorite |

### Dashboard Partial Failures

The `/space/dashboard` endpoint may return a mix of successful sections and error objects, e.g.:

```json
{
  "apod": { "...": "..." },
  "mars_photos": {
    "error": "Service unavailable"
  }
}
```

The dashboard UI **must**:

- Render each section independently.
- Show a clear inline error/empty state for any section with an `error` field, without breaking the rest of the page.
- Never show a full-page error for partial failures.

---

## State & Caching

- Use a server-state library (React Query or equivalent) to manage API data, with sensible `staleTime`/`cacheTime` aligned to the backend's caching (NASA data cached ~1 hour server-side — avoid unnecessary refetching).
- Implement optimistic updates for adding/removing favorites, with rollback on failure.
- Avoid prop-drilling for auth state — use context or a dedicated store.
- Document your caching/invalidation strategy in the README.

---

## UI/UX Requirements

- Responsive layout (mobile, tablet, desktop).
- Consistent design system: shared components for buttons, inputs, cards, modals, loaders, and toasts/notifications.
- Loading states (skeletons or spinners) for all async data.
- Empty states for: no favorites, no asteroids on a given date, no Mars photos for a filter combination, no EONET events.
- Toast/notification feedback for auth errors, rate limits, and favorite add/remove actions.
- Basic accessibility: semantic HTML, labeled form fields, keyboard-navigable interactive elements, sufficient color contrast.

---

## Validation & Error Handling {#validation--error-handling-1}

### Client-Side Validation

- Signup/login forms validate required fields, email format, and password rules before submission.
- Filter forms (Mars photos, asteroid date range) validate input ranges/formats before triggering requests.

### Handling Backend Error Format

The backend returns errors in the shape:

```json
{
  "error": {
    "code": "INVALID_TOKEN",
    "message": "Token is invalid"
  }
}
```

The frontend must:

- Parse this shape consistently via a shared API client/error handler.
- Map common codes (e.g. `INVALID_TOKEN`, `VALIDATION_ERROR`, `RATE_LIMITED`) to user-friendly messages.
- Show distinct UI for:
  - 401 (session expired → trigger refresh or redirect to login)
  - 403 (permission denied → friendly "not allowed" message)
  - 422 (inline field-level validation errors)
  - 429 (rate limit → "please slow down" messaging, ideally with retry-after countdown)
  - 500 / network errors (generic retry option)

---

## Testing Requirements {#testing-requirements-1}

### Unit Tests

Cover:

- API client error handling (mapping error codes to UI states)
- Auth context/store (login, logout, token refresh logic)
- At least 2 reusable components (e.g. form inputs, favorite button)

### Integration / Component Tests

Test at least:

- Login flow (success and failure)
- Dashboard rendering with a mocked partial-failure response
- Adding and removing a favorite

### Mocking

- All API calls must be mocked in tests (e.g. MSW — Mock Service Worker).
- Tests must not depend on a live backend or real NASA API.

### Test Command

Single command execution, documented in README:

```bash
npm test
```

---

## Deliverables {#deliverables-1}

Your submission must include:

- Public GitHub Repository
- Source Code
- README.md
- Component/Folder Structure Overview
- Unit Tests
- Integration/Component Tests

### README Requirements

Include:

#### Setup Instructions

How to run locally, including how to point the app at the backend (or mock server).

#### Environment Variables

Document all variables (e.g. `VITE_API_BASE_URL`).

#### Run Commands

```bash
npm install
npm run dev
npm test
```

#### Architecture Overview

Briefly describe folder structure, state management approach, and API client design.

---

### Design Report

Include a short report (400–600 words) covering:

- Authentication & session handling strategy
- State management and caching approach
- How partial/dashboard failures are handled in the UI
- Accessibility and responsiveness considerations
- Future improvements

---

## Bonus Tasks {#bonus-tasks-1}

Optional but highly valued:

- Dark mode
- EONET events shown on an interactive map
- Mars photo gallery with infinite scroll/pagination
- Offline/cached fallback when the API is unreachable
- Storybook for shared components
- E2E tests (Playwright/Cypress)
- CI pipeline (GitHub Actions) running lint/tests on PRs
- Live deployment (Vercel/Netlify) with link in README

---

## Evaluation Criteria {#evaluation-criteria-1}

| Area | What We Evaluate |
|--------|--------|
| Technical Correctness | End-to-end functionality against the API contract |
| Code Quality | Component structure, reusability, readability |
| Results & Tests | Unit and integration coverage |
| Problem Understanding | Handling of auth, caching, and partial failures |
| UI/UX & Accessibility | Polish, responsiveness, and usability |
| Creativity | Additional engineering or design improvements |

---

## Submission Instructions {#submission-instructions-1}

1. Complete the assessment.
2. Push your code to a public GitHub repository.
3. Include all required deliverables.
4. Verify tests pass successfully.
5. Submit the repository link before the deadline.

### Submission Package

- GitHub Repository Link
- Design Report

Optional:

- Loom Walkthrough Video
- Live Deployment Link

---

## Good Luck

We look forward to seeing how you think, design, and build.

This assessment reflects the type of frontend engineering, API integration, and UX challenges encountered at DECTIFY.

**— DECTIFY Recruitment Team**
