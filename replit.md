# SmartBus – Intelligent Campus Transport System

## Overview

Full-stack web application for managing campus transit with real-time tracking, QR-based boarding, smart alternative suggestions, and analytics.

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **Frontend**: React + Vite + Tailwind CSS (artifacts/smartbus)
- **API framework**: Express 5 (artifacts/api-server)
- **Database**: PostgreSQL + Drizzle ORM
- **Validation**: Zod (`zod/v4`), `drizzle-zod`
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (CJS bundle)
- **Charts**: Recharts
- **QR Code**: qrcode

## Architecture

### Frontend (artifacts/smartbus)
- **Auth**: LocalStorage-based auth with role detection (student/driver/admin)
- **Pages**:
  - `/` – Login/Signup page with role selection
  - `/dashboard` – Student dashboard with assigned bus, stats, alerts
  - `/tracking` – Live bus tracking grid (5s auto-refresh)
  - `/alternatives` – AI-ranked alternative bus finder
  - `/boarding` – QR code generation + simulated scanner
  - `/notifications` – Real-time notification center
  - `/driver` – Driver panel: status control, passenger management
  - `/admin` – Admin panel: bus management, monitoring, analytics charts

### Backend (artifacts/api-server)
- **Routes**:
  - `/api/buses` – CRUD buses, boarding, alternatives, dashboard summary
  - `/api/buses/:id/board` – QR boarding (increments passengers, sends notifications)
  - `/api/buses/:id/alight` – Passenger exits
  - `/api/buses/alternatives` – Smart alternative bus ranking
  - `/api/buses/dashboard-summary` – Fleet statistics
  - `/api/notifications` – CRUD notifications, mark read
  - `/api/users` – CRUD users with role management
  - `/api/analytics/overview` – Hourly boarding data, route usage
  - `/api/boarding-logs` – Boarding history

### Database Schema (lib/db/src/schema/)
- `buses` – Bus info, capacity, location, status
- `users` – User profiles with roles (student/driver/admin)
- `notifications` – Real-time alerts by type
- `boarding_logs` – Audit trail of all boarding events

## Key Commands

- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks from OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- `pnpm --filter @workspace/api-server run dev` — run API server locally
- `pnpm --filter @workspace/smartbus run dev` — run frontend locally

## Features Implemented

- Role-based auth (student, driver, admin) with protected routes
- Live bus tracking with 5-second auto-refresh
- Real-time seat occupancy with color-coded status badges
- Smart AI-ranked alternative bus finder with scoring logic
- QR code generation for boarding pass display
- QR scanner simulation (enter bus ID to board)
- Automatic passenger count tracking + bus full detection
- Real-time notifications pushed on boarding and bus full events
- Driver panel: update status, record alighting
- Admin panel: CRUD buses, live monitoring, Recharts analytics (hourly bar, route pie, occupancy bars)
- Offline detection banner
- Dark/light theme support (defaults to dark)
- Responsive sidebar with role-based navigation
- Unread notification count badge

## Seed Data

6 buses seeded with different statuses (on_time, delayed, full, idle) and passenger counts.
