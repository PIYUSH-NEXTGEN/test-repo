# Public Issue Reporting & Tracking Platform

A civic platform where residents report local problems — damaged roads, broken
streetlights, garbage buildup — with photos and locations, then track whether
anything is actually being done. Reports stop disappearing into inboxes: every
submission gets a public tracking reference and a visible status.

## Who it's for

- **Residents** — file reports, attach evidence, follow progress.
- **Staff** — triage assigned issues and update their status.
- **Administrators** — assign reports to teams, manage users, oversee resolution.

## Features

- Report submission with title, description, photo, and location (map pin or address)
- Status lifecycle per report: `received` → `in-progress` → `resolved` / `rejected`
- Public tracking reference so anyone can check progress without logging in
- Admin dashboard for assignment, filtering, and bulk status updates
- Role-based access control separating reporters, staff, and admins
- Secure photo uploads linked to reports


## Project structure

- `/api` — endpoints for reports, auth, uploads, and status updates
- `/components` — UI for the reporting form, tracker, and admin dashboard
- `/lib` — auth helpers, RBAC guards, storage client
- `/prisma` (or equivalent) — schema and migrations

