<!--
  @file docs/architecture.md
  @brief Global system architecture and component overview for rz-gallery.
  @author ZHENG Robert
  @date 2026-03-22
  @version 1.2.0
-->

# Global Architecture - rz-gallery

---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**

- [System Overview](#system-overview)
  - [Ecosystem Components](#ecosystem-components)
    - [1. Backend (`backend/services/`)](#1-backend-backendservices)
    - [2. Frontend (`frontend/`)](#2-frontend-frontend)
- [Repository Structure & Submodules](#repository-structure--submodules)
- [Security & Access Control (RBAC)](#security--access-control-rbac)
- [Data Schema](#data-schema)
- [Diagramming Standards](#diagramming-standards)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

---

## System Overview

The **rz-gallery** project is a distributed photo gallery ecosystem, strictly divided into Backend services and Frontend clients. Each component exists in its own repository and is built independently.

### Ecosystem Components

#### 1. Backend (`backend/services/`)

| Repository | Framework | Role       | Description                                 |
| ---------- | --------- | ---------- | ------------------------------------------- |
| `gateway`  | Drogon    | Dispatcher | RBAC Middleware, Public API Entry.          |
| `auth`     | Crow      | Identity   | User Authentication (JWT issuance), Roles.  |
| `notify`   | Crow      | Notify     | Communication Channels (Email, Push, etc.). |
| `logging`  | Crow      | Logs       | Centralized Event Logging and Analytics.    |
| `common`   | (Library) | Shared     | Shared logic (Submodule).                   |

#### 2. Frontend (`frontend/`)

| Repository           | Platform | Role         | Description                                        |
| -------------------- | -------- | ------------ | -------------------------------------------------- |
| `web_user`           | Web      | Gallery User | Public photo gallery, search, and profile.         |
| `web_admin`          | Web      | System Admin | System monitoring and metadata management.         |
| `desktop_user`       | Desktop  | Native User  | High-performance desktop photo viewing.            |
| `desktop_admin`      | Desktop  | Native Admin | Bulk photo management and export.                  |
| `cli_deliver_photos` | CLI      | Delivery     | Command-line tool for professional photo delivery. |

## Repository Structure & Submodules

The **rz-gallery** ecosystem follows a Git Submodule pattern for shared backend logic. The `common` library is included in each backend service repository under `libs/common`.

```text
/ (RZ-Gallery Root)
├── docs/ (Repo: rz-gallery-docs)
├── backend/
│   ├── services/
│   │   ├── gateway/ (Git Repo) -> Submodule: common
│   │   ├── auth/    (Git Repo) -> Submodule: common
│   │   └── ...
│   ├── libs/
│   │   └── common/  (Git Repo)
│   └── database/ (Common SQL scripts)
├── frontend/
    |── web_user/    (Git Repo)
    |── web_admin/   (Git Repo)
    |── desktop_user/ (Git Repo)
    |── desktop_admin/ (Git Repo)
    └── cli_deliver_photos/ (Git Repo)
```

## Security & Access Control (RBAC)

1. **Authentication**: The `auth` service validates credentials and issues a JWT (JSON Web Token).
2. **Authorization**: The `gateway` acts as a secure dispatcher.
   - It intercepts every request via a **RBAC Middleware**.
   - It validates the JWT against the shared `jwt_secret`.
   - It queries the PostgreSQL permissions database to verify access rights.

## Data Schema

The system uses a shared PostgreSQL database. Refer to `backend/database/init.sql` for the full layout including photos, metadata, roles, and permissions.

## Diagramming Standards

All architectural diagrams in **rz-gallery** must use the **Mermaid** format for version-controllable, text-based visual documentation.
