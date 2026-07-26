# Verification

## Overview

This document describes the procedures for verifying that the TaskFlow application has been installed, configured, and deployed successfully.

Verification ensures that all major application components are functioning correctly, including:

* Application services.
* Configuration settings.
* Database connectivity.
* API availability.
* Authentication workflows.
* Workspace access.
* User interface functionality.
* Application logging.

Complete verification after:

* Installing TaskFlow.
* Updating application components.
* Changing configuration settings.
* Deploying a new application release.

Before starting verification, ensure that the installation and configuration steps described in **Installation.md** and **Configuration.md** are complete.

---

# Verification Checklist

Use the following checklist to confirm that TaskFlow is ready for use.

| Verification Area   | Expected Result                        |
| ------------------- | -------------------------------------- |
| System requirements | Required dependencies are installed    |
| Configuration       | Required settings are loaded correctly |
| Backend service     | API service starts successfully        |
| Database            | Database connection is available       |
| API                 | Endpoints return expected responses    |
| Authentication      | Users can authenticate successfully    |
| Workspace access    | Users can access assigned resources    |
| Frontend            | Application interface loads correctly  |
| Logging             | Application events are recorded        |

---

# Verify System Requirements

Before validating TaskFlow services, confirm that required dependencies are installed.

## Verify Node.js

Run:

```bash
node --version
```

Expected output:

```text
v20.x.x
```

---

## Verify npm

Run:

```bash
npm --version
```

Expected output:

```text
10.x.x
```

---

## Verify Git

Run:

```bash
git --version
```

Expected output:

```text
git version 2.x.x
```

---

## Verify PostgreSQL

Run:

```bash
psql --version
```

Expected output:

```text
psql (PostgreSQL) 15.x
```

All dependencies must meet the versions specified in **System-Requirements.md**.

---

# Verify Configuration

TaskFlow requires valid configuration values before starting application services.

Run configuration validation:

```bash
npm run config:check
```

Expected output:

```text
Configuration validation successful.
```

---

## Verify Environment Variables

Check required environment variables:

```bash
grep -E 'APP_PORT|DATABASE_HOST|DATABASE_NAME|JWT_SECRET' .env
```

Expected output:

```text
APP_PORT=3000
DATABASE_HOST=localhost
DATABASE_NAME=taskflow
JWT_SECRET=<configured>
```

Verify that:

* Required values are present.
* Environment settings match the deployment environment.
* Sensitive values are configured correctly.

---

# Verify Application Services

## Verify Backend Service

Confirm that the TaskFlow API service is running.

Run:

```bash
ps aux | grep taskflow
```

Expected output:

```text
taskflow-api running
```

The backend service should be available at:

```text
http://localhost:3000
```

---

## Verify Frontend Application

Open the TaskFlow application:

```text
http://localhost:5173
```

Verify that:

* Login page loads successfully.
* Static resources load correctly.
* No browser console errors appear.
* Application navigation is available.

---

# Verify Database Connectivity

TaskFlow requires an active database connection.

Run:

```bash
npm run db:check
```

Expected output:

```text
Database connection successful.
```

---

## Verify Database Migration Status

Check the current migration state:

```bash
npm run migration:status
```

Expected output:

```text
Migration status: Up to date.
```

If migrations are pending, run:

```bash
npm run migrate
```

---

# Verify API Health

TaskFlow provides a health endpoint to verify service availability.

Run:

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

A successful response confirms that:

* The API service is running.
* Required dependencies are available.
* Service communication is working.

---

# Verify API Authentication

Verify that protected API endpoints require authentication.

Send a request without an access token:

```bash
curl http://localhost:3000/api/projects
```

Expected response:

```http
HTTP/1.1 401 Unauthorized
```

Example response:

```json
{
  "error": "AuthenticationError",
  "message": "Access token is required.",
  "status": 401
}
```

This confirms that API authentication is enabled.

---

# Verify User Authentication

Test user login functionality.

Send an authentication request:

```bash
curl -X POST http://localhost:3000/api/auth/login \
-H "Content-Type: application/json" \
-d '{
  "email": "user@example.com",
  "password": "password"
}'
```

Expected response:

```json
{
  "accessToken": "<token>",
  "expiresIn": "24h"
}
```

Verify that:

* Valid credentials are accepted.
* Access tokens are generated.
* Token expiry is configured correctly.

---

# Verify Workspace Access

After authentication, verify workspace availability.

Confirm that:

* The assigned workspace is displayed.
* Projects are visible.
* Tasks load successfully.
* User permissions are applied.

Expected result:

```text
Workspace: Engineering

Access: Granted

Projects: Available
```

If workspace access fails, verify user permissions and workspace configuration.

---

# Verify Frontend Functionality

Perform the following user interface checks:

| Test            | Expected Result                  |
| --------------- | -------------------------------- |
| Login           | User signs in successfully       |
| Dashboard       | Workspace information displays   |
| Project view    | Project details load correctly   |
| Task management | Tasks can be viewed and updated  |
| Navigation      | Application pages open correctly |

---

# Verify Application Logs

TaskFlow records application activity in log files.

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

Expected successful startup logs:

```text
INFO Application starting...
INFO Configuration loaded successfully.
INFO Database connection established.
INFO API server running on port 3000.
INFO TaskFlow startup completed successfully.
```

---

## Verify Error Logs

Review error logs:

```bash
tail -f logs/error.log
```

Example error:

```text
ERROR Database connection failed.
ERROR Unable to connect to localhost:5432.
ERROR Application startup aborted.
```

Critical errors should be resolved before deployment.

---

# Automated Verification

TaskFlow provides a verification script to automate common checks.

Create the verification script:

```bash
touch verify.sh
```

Add the following:

```bash
#!/bin/bash

echo "Starting TaskFlow verification..."

echo "Checking configuration..."
npm run config:check

echo "Checking database..."
npm run db:check

echo "Checking API health..."

curl -f http://localhost:3000/api/health

if [ $? -eq 0 ]; then
    echo "API health check passed."
else
    echo "API health check failed."
    exit 1
fi

echo "TaskFlow verification completed successfully."
```

Make the script executable:

```bash
chmod +x verify.sh
```

Run verification:

```bash
./verify.sh
```

Expected output:

```text
Starting TaskFlow verification...

Configuration validation successful.

Database connection successful.

API health check passed.

TaskFlow verification completed successfully.
```

---

# Verify Docker Deployment

If TaskFlow is deployed using Docker, verify container status.

List running containers:

```bash
docker ps
```

Expected output:

```text
taskflow-api

taskflow-frontend

taskflow-database
```

View container logs:

```bash
docker logs <container_name>
```

Restart containers:

```bash
docker compose restart
```

---

# Verify Production Deployment

Before production release, confirm:

## Security

Verify:

* Debug mode is disabled.
* Production secrets are configured.
* Database credentials are protected.
* HTTPS is enabled.

Example:

```env
APP_ENV=production

LOG_LEVEL=warn
```

---

## Performance

Verify:

* Application starts within expected time.
* API responses complete successfully.
* Database queries execute without failures.
* Server resources remain available.

---

# Post-Deployment Verification Checklist

Complete the following checklist after deployment.

| Verification Item             | Status |
| ----------------------------- | ------ |
| Dependencies verified         | ☐      |
| Configuration validated       | ☐      |
| Database connection confirmed | ☐      |
| Database migrations completed | ☐      |
| API health check passed       | ☐      |
| Authentication verified       | ☐      |
| Workspace access confirmed    | ☐      |
| Frontend loaded successfully  | ☐      |
| Logs reviewed                 | ☐      |
| No critical errors detected   | ☐      |

---

# Verification Failures

If verification fails:

1. Record the error message.
2. Review application logs.
3. Validate configuration values.
4. Check service availability.
5. Refer to **Troubleshooting.md**.

---

# Next Steps

After successful verification:

* Continue using TaskFlow.
* Review **Developer-Guide.md** for development workflows.
* Review **API-Guidelines.md** for API usage and integration details.

