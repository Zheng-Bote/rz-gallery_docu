<!--
  @file docs/build-and-run.md
  @brief Setup and execution instructions for the rz-gallery ecosystem.
  @author ZHENG Robert
  @date 2026-03-22
  @version 1.2.0
-->

# Build and run

## Prerequisites

- CMake **3.28+**
- A C++23-capable toolchain (GCC 13+ or Clang 17+ recommended)
- **OpenSSL**, **PostgreSQL client** (`libpq`) development packages
- **jsoncpp**, **libpqxx**, **jwt-cpp** (fetched via CMake)

## PostgreSQL with Docker

```bash
docker compose up -d
```

## Backend Services

Every service is built independently. Navigate to the service directory:

### Gateway

```bash
cd backend/services/gateway
cmake -S . -B build
cmake --build build
```

### Auth Service

```bash
cd backend/services/auth
cmake -S . -B build
cmake --build build
```

## Start Order

1. **PostgreSQL**
2. **Microservices** (`log_svc` first recommended)
3. **Gateway**

Example run:

```bash
./build/log_svc --env dev
```
