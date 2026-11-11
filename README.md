# HRMS – Human Resource Management System

A web-based Human Resource Management System (HRMS) developed as an academic major project. It brings together administrative dashboards and employee attendance workflows, with a Node.js API and MongoDB persistence.

> **Project status:** In development. Some screens currently use local demonstration data, while attendance and leave workflows have backend API integration. See [Implementation Notes](#implementation-notes) before treating every screen as database-backed.

## Features

- Admin dashboard and Super Admin interface
- Employee information interface
- Attendance punch-in / punch-out and attendance history
- Leave application and approval workflow
- Payroll interface
- Reports and system settings interfaces
- Separate attendance-facing frontend

## Technology Stack

| Layer | Technologies |
|---|---|
| Main admin frontend | React 18, Vite 5, React Router |
| UI and styling | CSS, Tailwind CSS, Radix UI components, Lucide icons, Recharts |
| Attendance frontend | Next.js 16, React 19, TypeScript, Tailwind CSS |
| Backend API | Node.js, Express 5 |
| Database access | MongoDB, Mongoose 9 |
| Configuration | dotenv, environment variables |
| Deployment workflow | GitHub Actions workflow (review before enabling) |

## Repository Structure

```text
HRMS-Human-Resource-Management-System/
├── backend/                 # Express API and Mongoose models
├── hrms-admin-panel/        # Main React + Vite admin application
├── att/                     # Next.js attendance-facing application
├── .github/workflows/       # CI/deployment workflows
├── .gitignore
└── README.md
```

## Database Overview

The backend connects to MongoDB using the `MONGODB_URI` environment variable. Mongoose models are currently defined in `backend/server.js`.

| Collection / Model | Purpose | Example fields |
|---|---|---|
| User | Registered user accounts | name, email, password |
| Attendance | Daily attendance records | userId, userName, email, workMode, punchInTime, punchOutTime, date, totalDuration |
| Leave | Employee leave requests | userId, userName, email, leaveType, startDate, endDate, reason, status, appliedDate |

The application creates and updates records through API requests. **A MongoDB database export, real employee dataset, or production database is not included in this repository.** Do not commit database dumps, real employee information, credentials, or private HR documents.

## Getting Started

### Prerequisites

- Node.js (use a version compatible with the installed project dependencies)
- npm
- MongoDB locally, or a MongoDB Atlas connection string

### 1. Backend API

```bash
cd backend
npm install
```

Create a local `.env` file using the example:

```bash
cp .env.example .env
```

Set `MONGODB_URI` to your own local or Atlas database connection string. Then start the API:

```bash
npm run dev
```

The backend defaults to port `9000`.

### 2. Main Admin Panel

Open a second terminal:

```bash
cd hrms-admin-panel
npm install
```

Create the local environment file:

```bash
cp .env.example .env
```

Set `VITE_BASE_URL=http://localhost:9000`, then run:

```bash
npm run dev
```

Vite will print the local development URL in the terminal.

### 3. Attendance Frontend

If you are using the separate Next.js attendance application:

```bash
cd att
npm install
```

Create its local environment file:

```bash
cp .env.example .env
```

Set `NEXT_PUBLIC_BASE_URL=http://localhost:9000`, then run:

```bash
npm run dev
```

The attendance application is configured to use port `3001`.

## Environment Variables

Never commit actual `.env` files. Use the committed `.env.example` templates and supply your own local values.

| File | Variable | Purpose |
|---|---|---|
| `backend/.env` | `PORT` | Backend server port |
| `backend/.env` | `MONGODB_URI` | MongoDB connection string |
| `hrms-admin-panel/.env` | `VITE_BASE_URL` | Backend API base URL |
| `att/.env` | `NEXT_PUBLIC_BASE_URL` | Backend API base URL used by the Next.js frontend |

Only put values safe to expose in browser-side variables such as `VITE_*` and `NEXT_PUBLIC_*`. Never put database credentials or private API secrets in frontend environment variables.

## Implementation Notes

The codebase contains both API-backed functionality and demonstration UI data. For example, the employee list, payroll records, and Super Admin company list currently include hardcoded sample entries in frontend components. These should be understood as UI demonstration data, not as a live database export.

The backend currently defines User, Attendance, and Leave models and exposes registration/login, attendance, leave, and dashboard API routes. The admin frontend calls backend endpoints for some attendance and leave operations. Verify each module's integration before describing it as fully functional.

### Security and Production Readiness

This repository is an academic development project and should not be deployed with real employee data until security work is completed. In particular:

- Implement password hashing (Argon2 or bcrypt) and secure authentication/session handling.
- Add authorization and role-based access checks to protected API routes.
- Restrict CORS to trusted frontend origins.
- Validate and sanitize request data; avoid returning internal error details to clients.
- Keep secrets in environment variables and rotate any credential that was accidentally exposed.
- Use synthetic records for screenshots and demonstrations.

<!--
## Screenshots

Add screenshots of the running application in a `docs/screenshots/` directory and link them here.
-->

## License

No license has been applied yet. Unless a license is added, normal copyright restrictions apply. Add an open-source license only if you have the rights and intend to permit reuse.
