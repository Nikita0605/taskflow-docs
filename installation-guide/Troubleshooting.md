# Troubleshooting

## Overview

This document provides troubleshooting procedures for resolving common issues encountered while installing, configuring, and running TaskFlow.

The troubleshooting process helps identify application errors, configuration problems, service failures, and connectivity issues.

Before troubleshooting, verify that:

* TaskFlow is installed correctly.
* Configuration values are valid.
* Required services are running.
* Application logs are available.

For installation procedures, refer to **Installation.md**.

For configuration settings, refer to **Configuration.md**.

---

# Troubleshooting Workflow

Follow this general troubleshooting process:

1. Identify the issue and collect error details.
2. Review application logs.
3. Verify configuration settings.
4. Check service availability.
5. Apply the recommended resolution.
6. Restart affected services.
7. Verify that the issue is resolved.

---

# Collect Diagnostic Information

Before troubleshooting an issue, collect the following information:

| Information      | Description                         |
| ---------------- | ----------------------------------- |
| TaskFlow version | Installed application version       |
| Operating system | Host environment details            |
| Error message    | Complete error output               |
| Application logs | Runtime and error logs              |
| Recent changes   | Configuration or deployment changes |

---

# View Application Logs

TaskFlow stores logs for monitoring and troubleshooting.

Default log location:

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

Increase logging detail for troubleshooting:

```env
LOG_LEVEL=debug
```

Restart TaskFlow after changing the logging level.

---

# Installation Issues

## Installation Package Cannot Be Executed

### Symptoms

* Installer does not start.
* Installation stops immediately.
* Permission errors appear.

### Possible Causes

* Insufficient user permissions.
* Corrupted installation package.
* Unsupported operating system.

### Resolution

Verify system requirements:

```bash
node --version

npm --version

git --version
```

Download the installation package again from the approved source.

Verify that the user has required installation permissions.

---

# Dependency Installation Failure

## npm install Fails

### Symptoms

* Dependency installation stops.
* npm errors appear.
* Missing package errors occur.

### Possible Causes

* Unsupported Node.js version.
* Corrupted npm cache.
* Network connectivity issues.

### Resolution

Verify Node.js version:

```bash
node --version
```

Clear npm cache:

```bash
npm cache clean --force
```

Remove existing dependencies:

```bash
rm -rf node_modules
```

Remove package lock file:

```bash
rm package-lock.json
```

Reinstall dependencies:

```bash
npm install
```

---

# Application Startup Issues

## TaskFlow Service Does Not Start

### Symptoms

* Backend service fails during startup.
* Application exits immediately.
* Server does not respond.

### Possible Causes

* Missing environment variables.
* Database connection failure.
* Invalid configuration values.

### Resolution

Validate configuration:

```bash
npm run config:check
```

Check environment variables:

```bash
grep -E 'APP_PORT|DATABASE_HOST|JWT_SECRET' .env
```

Review application logs:

```bash
tail -f logs/error.log
```

Restart the service:

```bash
npm restart
```

---

# Port Already in Use

## Error Message

```text
Error: Port 3000 is already in use
```

### Cause

Another process is currently using the configured application port.

### Resolution

Identify the process:

```bash
lsof -i :3000
```

Example output:

```text
node   12345 user   TCP localhost:3000
```

Stop the process:

```bash
kill 12345
```

Restart TaskFlow:

```bash
npm start
```

---

# Database Issues

## Database Connection Failed

### Symptoms

* Application cannot start.
* Database timeout errors appear.
* API requests fail.

### Possible Causes

* Incorrect database credentials.
* Database service is unavailable.
* Network connection failure.

### Resolution

Verify PostgreSQL status:

```bash
systemctl status postgresql
```

Test database connection:

```bash
psql -h localhost -U taskflow_admin -d taskflow
```

Verify configuration:

```env
DATABASE_HOST=localhost

DATABASE_PORT=5432

DATABASE_NAME=taskflow

DATABASE_USER=taskflow_admin
```

---

# Database Migration Failure

## Symptoms

* Application update fails.
* Database schema is outdated.
* Migration errors appear.

### Resolution

Check migration status:

```bash
npm run migration:status
```

Run pending migrations:

```bash
npm run migrate
```

Review migration logs for failed operations.

---

# Authentication Issues

## User Cannot Sign In

### Symptoms

* Login fails.
* Invalid token errors appear.
* Sessions expire unexpectedly.

### Possible Causes

* Incorrect JWT configuration.
* Expired token.
* Invalid user permissions.

### Resolution

Verify authentication settings:

```env
JWT_SECRET=<configured-secret>

TOKEN_EXPIRY=24h
```

Generate a new secret if required:

```bash
openssl rand -hex 32
```

Restart TaskFlow after updating authentication configuration.

---

# Authorization Failure

## User Cannot Access Workspace

### Symptoms

* Workspace is unavailable.
* Projects are hidden.
* Access denied errors appear.

### Possible Causes

* Missing permissions.
* Incorrect workspace configuration.
* Incorrect user account.

### Resolution

Verify workspace settings:

```env
DEFAULT_WORKSPACE=engineering

WORKSPACE_ACCESS_MODE=restricted
```

Confirm that the user has required workspace permissions.

---

# API Issues

## API Returns 500 Error

### Symptoms

* API requests fail.
* Server returns internal errors.

### Possible Causes

* Application exception.
* Database failure.
* Invalid request data.

### Resolution

Check API logs:

```bash
tail -f logs/error.log
```

Verify API health:

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

# API Timeout Errors

## Symptoms

* Requests take too long.
* Client receives timeout errors.

### Possible Causes

* Slow database queries.
* High server load.
* Incorrect timeout configuration.

### Resolution

Check API timeout configuration:

```env
API_TIMEOUT=30000
```

Review application performance logs.

Verify database performance.

---

# Frontend Issues

## Application Page Does Not Load

### Symptoms

* Blank screen.
* Browser errors.
* UI does not display.

### Possible Causes

* Frontend build failure.
* Backend unavailable.
* Incorrect API endpoint.

### Resolution

Verify frontend service:

```bash
npm run dev
```

Check API configuration:

```env
API_BASE_URL=http://localhost:3000
```

Review browser console errors.

---

# Build Failures

## Application Build Fails

### Symptoms

* Build process stops.
* Compilation errors appear.

### Resolution

Remove existing build files:

```bash
rm -rf dist
```

Remove dependencies:

```bash
rm -rf node_modules
```

Reinstall packages:

```bash
npm install
```

Run build again:

```bash
npm run build
```

---

# Logging and Debugging

## Enable Debug Logging

Update configuration:

```env
LOG_LEVEL=debug
```

Restart TaskFlow:

```bash
npm restart
```

After troubleshooting is complete, restore:

```env
LOG_LEVEL=info
```

---

# Docker Troubleshooting

## Container Does Not Start

Check running containers:

```bash
docker ps
```

View container logs:

```bash
docker logs <container_name>
```

Restart containers:

```bash
docker compose restart
```

Stop and recreate containers:

```bash
docker compose down

docker compose up -d
```

---

# Common Error Reference

| Error                        | Possible Cause           | Resolution                     |
| ---------------------------- | ------------------------ | ------------------------------ |
| Database connection refused  | Database unavailable     | Start PostgreSQL service       |
| Missing environment variable | Invalid configuration    | Update `.env` file             |
| Port already in use          | Existing process running | Stop conflicting process       |
| Invalid authentication token | JWT configuration issue  | Verify authentication settings |
| Build failed                 | Dependency issue         | Reinstall packages             |
| Workspace unavailable        | Permission issue         | Verify user access             |

---

# When to Contact Support

Contact the support team when:

* The issue continues after troubleshooting.
* Application logs indicate an unknown failure.
* Data corruption is suspected.
* Production services are unavailable.

Provide:

* TaskFlow version.
* Error messages.
* Relevant logs.
* Steps to reproduce the issue.
* Recent configuration changes.

---

# Next Steps

After resolving the issue, verify that:

* TaskFlow services are running.
* Users can authenticate successfully.
* Workspaces are accessible.
* Application logs contain no critical errors.

Continue with:

**Developer-Guide.md** for application development workflows and contribution guidelines.

