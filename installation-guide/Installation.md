# Installation

## Overview

This document describes the process for installing and configuring the TaskFlow application in a development and deployment environment.

The installation process prepares TaskFlow by installing required dependencies, configuring application settings, initializing the database, building application components, and starting required services.

Before beginning the installation, verify that the system meets the requirements described in **System-Requirements.md**.

---

# Installation Prerequisites

Before installing TaskFlow, verify that the following components are available on the system.

| Component        | Minimum Requirement                     |
| ---------------- | --------------------------------------- |
| Operating System | Windows 11, macOS 13+, or Ubuntu 22.04+ |
| Node.js          | Version 20.x or later                   |
| npm              | Version 10.x or later                   |
| Git              | Version 2.x or later                    |
| PostgreSQL       | Version 15.x or later                   |

---

## Verify Installed Dependencies

Confirm that required tools are installed:

```bash
node --version
```

Example output:

```text
v20.11.1
```

Verify npm:

```bash
npm --version
```

Example output:

```text
10.2.4
```

Verify Git:

```bash
git --version
```

Example output:

```text
git version 2.43.0
```

Verify PostgreSQL:

```bash
psql --version
```

Example output:

```text
psql (PostgreSQL) 15.5
```

If any required dependency is missing, install it before continuing.

---

# Download the TaskFlow Repository

Clone the TaskFlow source repository:

```bash
git clone https://github.com/taskflow/taskflow.git
```

Navigate to the project directory:

```bash
cd taskflow
```

Verify the project structure:

```bash
ls
```

Expected output:

```text
backend
frontend
database
docs
package.json
README.md
```

The repository contains the following primary components:

| Directory | Description                     |
| --------- | ------------------------------- |
| backend   | TaskFlow API services           |
| frontend  | TaskFlow web application        |
| database  | Database scripts and migrations |
| docs      | Product documentation           |

---

# Install Application Dependencies

TaskFlow consists of frontend and backend components. Install dependencies separately for each component.

---

## Install Backend Dependencies

Navigate to the backend directory:

```bash
cd backend
```

Install required packages:

```bash
npm install
```

Verify installed dependencies:

```bash
npm list --depth=0
```

---

## Install Frontend Dependencies

Navigate to the frontend directory:

```bash
cd ../frontend
```

Install required packages:

```bash
npm install
```

Verify installed dependencies:

```bash
npm list --depth=0
```

---

# Configure Environment Variables

TaskFlow uses environment variables to configure application behavior and external service connections.

Create a local configuration file from the provided template:

```bash
cp .env.example .env
```

Update the `.env` file with environment-specific values.

Example configuration:

```env
APP_NAME=TaskFlow

APP_PORT=3000

DATABASE_HOST=localhost
DATABASE_PORT=5432
DATABASE_NAME=taskflow
DATABASE_USER=taskflow_admin
DATABASE_PASSWORD=<password>

JWT_SECRET=<secret-key>

LOG_LEVEL=info
```

---

## Environment Variables Reference

| Variable          | Description                               |
| ----------------- | ----------------------------------------- |
| APP_NAME          | Application name displayed by the service |
| APP_PORT          | Port used by the backend service          |
| DATABASE_HOST     | Database server address                   |
| DATABASE_PORT     | Database service port                     |
| DATABASE_NAME     | TaskFlow database name                    |
| DATABASE_USER     | Database authentication username          |
| DATABASE_PASSWORD | Database authentication password          |
| JWT_SECRET        | Secret key used for authentication tokens |
| LOG_LEVEL         | Application logging level                 |

---

# Configure Database

TaskFlow requires PostgreSQL for storing application data.

## Create Database

Connect to PostgreSQL:

```bash
psql -U postgres
```

Create the TaskFlow database:

```sql
CREATE DATABASE taskflow;
```

Create the application database user:

```sql
CREATE USER taskflow_admin WITH PASSWORD '<password>';
```

Assign database permissions:

```sql
GRANT ALL PRIVILEGES ON DATABASE taskflow TO taskflow_admin;
```

Exit PostgreSQL:

```sql
\q
```

---

# Run Database Migration

Navigate to the backend directory:

```bash
cd backend
```

Run database migrations:

```bash
npm run migrate
```

Expected output:

```text
Database migration completed successfully.
```

Verify migration status:

```bash
npm run migration:status
```

---

# Build the Application

Before starting TaskFlow, create application builds.

---

## Build Backend Service

Navigate to the backend directory:

```bash
cd backend
```

Run the build command:

```bash
npm run build
```

Expected output:

```text
Backend build completed successfully.
```

---

## Build Frontend Application

Navigate to the frontend directory:

```bash
cd ../frontend
```

Run the frontend build:

```bash
npm run build
```

The production files are generated in:

```text
frontend/dist/
```

---

# Start TaskFlow Services

TaskFlow requires both backend and frontend services to run.

---

## Start Backend Service

Navigate to the backend directory:

```bash
cd backend
```

Start the API service:

```bash
npm start
```

Expected output:

```text
TaskFlow API running on port 3000
Database connection established
```

The backend service is available at:

```text
http://localhost:3000
```

---

## Start Frontend Application

Navigate to the frontend directory:

```bash
cd frontend
```

Start the frontend server:

```bash
npm run dev
```

Expected output:

```text
Local:
http://localhost:5173
```

Open the application in a browser:

```text
http://localhost:5173
```

---

# Verify Installation

After starting all services, verify that TaskFlow is functioning correctly.

---

## Verify API Health

Run the health check:

```bash
curl http://localhost:3000/api/health
```

Expected response:

```json
{
  "status": "healthy",
  "service": "taskflow-api"
}
```

---

## Verify Database Connection

Run:

```bash
npm run db:check
```

Expected output:

```text
Database connection successful.
```

---

## Verify Application Access

Confirm the following:

| Verification         | Expected Result         |
| -------------------- | ----------------------- |
| Backend service      | Running successfully    |
| Frontend application | Loads without errors    |
| Database             | Connection established  |
| Authentication       | User login available    |
| Dashboard            | Displays workspace data |

Successful completion confirms that TaskFlow is installed correctly.

---

# Application Configuration Files

TaskFlow uses the following configuration files:

| File              | Purpose                             |
| ----------------- | ----------------------------------- |
| `.env`            | Environment-specific settings       |
| `package.json`    | Dependency and script configuration |
| `database/config` | Database configuration              |
| `logs/config`     | Logging configuration               |

Do not commit environment-specific configuration files containing sensitive values.

---

# Application Logs

TaskFlow generates logs to assist with monitoring and troubleshooting.

Default log structure:

```text
logs/
├── application.log
├── error.log
└── access.log
```

View application logs:

```bash
tail -f logs/application.log
```

View error logs:

```bash
tail -f logs/error.log
```

---

# Updating an Existing Installation

Before updating TaskFlow:

* Stop running application services.
* Backup required configuration files.
* Verify database availability.

Pull the latest source changes:

```bash
git pull origin main
```

Install updated dependencies:

```bash
npm install
```

Run database migrations:

```bash
npm run migrate
```

Rebuild application components:

```bash
npm run build
```

Restart TaskFlow services:

```bash
npm restart
```

Verify that the updated installation is working correctly.

---

# Troubleshooting Installation Issues

## Dependency Installation Failure

Check the Node.js version:

```bash
node --version
```

Clear npm cache:

```bash
npm cache clean --force
```

Reinstall dependencies:

```bash
npm install
```

---

## Database Connection Failure

Verify PostgreSQL status:

```bash
systemctl status postgresql
```

Test database connectivity:

```bash
psql -h localhost -U taskflow_admin -d taskflow
```

Confirm that database credentials in `.env` are correct.

---

## Port Conflict

Check the process using the application port:

```bash
lsof -i :3000
```

Stop the conflicting process:

```bash
kill <process_id>
```

Restart the TaskFlow service.

---

## Build Failure

Remove existing build files:

```bash
rm -rf dist
```

Clear dependencies:

```bash
rm -rf node_modules
```

Reinstall packages:

```bash
npm install
```

Run the build again:

```bash
npm run build
```

---

# Uninstall TaskFlow

To remove TaskFlow from the system:

1. Stop all running TaskFlow services.
2. Remove application dependencies.
3. Remove local configuration files.
4. Remove the TaskFlow database if required.

Remove project dependencies:

```bash
rm -rf node_modules
```

Remove local configuration:

```bash
rm .env
```

Remove database:

```sql
DROP DATABASE taskflow;
```

---

# Next Steps

After completing the installation, continue with:

**Configuration.md**

to configure authentication, application settings, and environment-specific options.
