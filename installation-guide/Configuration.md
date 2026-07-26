# Configuration

## Overview

This document describes how to configure the TaskFlow application after installation.

Configuration settings define how TaskFlow connects to external services, manages application behavior, controls authentication, configures logging, and enables optional application features.

The configuration process includes:

* Setting application environment values.
* Configuring database connectivity.
* Configuring authentication settings.
* Managing feature flags.
* Configuring logging behavior.
* Validating deployment configuration.

Before configuring TaskFlow, complete the installation steps described in **Installation.md**.

---

# Configuration Files

TaskFlow uses environment variables and configuration files to manage application settings.

The primary configuration files are:

| File                     | Description                                                  |
| ------------------------ | ------------------------------------------------------------ |
| `.env`                   | Stores environment-specific application configuration values |
| `.env.example`           | Template containing required configuration variables         |
| `config/database.config` | Defines database connection settings                         |
| `config/auth.config`     | Defines authentication configuration                         |
| `config/logging.config`  | Defines application logging behavior                         |

Configuration files should be maintained separately for each environment.

Example environments:

```text
development
testing
production
```

Avoid using production configuration values in development environments.

---

# Environment Configuration

TaskFlow loads runtime configuration values from environment variables.

Create a local configuration file:

```bash
cp .env.example .env
```

Open the `.env` file and update the required values.

Example:

```env
APP_NAME=TaskFlow

APP_ENV=development

APP_PORT=3000


DATABASE_HOST=localhost
DATABASE_PORT=5432
DATABASE_NAME=taskflow
DATABASE_USER=taskflow_admin
DATABASE_PASSWORD=<password>


JWT_SECRET=<secret-key>

TOKEN_EXPIRY=24h


LOG_LEVEL=info
```

After modifying environment variables, restart TaskFlow services for changes to take effect.

---

# Application Configuration

Application-level settings control the runtime behavior of TaskFlow.

## Application Variables

| Variable | Description            | Example     |
| -------- | ---------------------- | ----------- |
| APP_NAME | Application identifier | TaskFlow    |
| APP_ENV  | Runtime environment    | development |
| APP_PORT | Backend service port   | 3000        |

Example:

```env
APP_NAME=TaskFlow

APP_ENV=development

APP_PORT=3000
```

---

# Database Configuration

TaskFlow uses PostgreSQL as the primary database system.

Database configuration defines how the application connects to the database service.

## Database Variables

| Variable          | Description                      | Example        |
| ----------------- | -------------------------------- | -------------- |
| DATABASE_HOST     | Database server address          | localhost      |
| DATABASE_PORT     | Database service port            | 5432           |
| DATABASE_NAME     | TaskFlow database name           | taskflow       |
| DATABASE_USER     | Database authentication user     | taskflow_admin |
| DATABASE_PASSWORD | Database authentication password | ********       |

Example:

```env
DATABASE_HOST=localhost

DATABASE_PORT=5432

DATABASE_NAME=taskflow

DATABASE_USER=taskflow_admin

DATABASE_PASSWORD=password
```

---

# Verify Database Configuration

After configuring database settings, verify connectivity.

Run:

```bash
npm run db:check
```

Expected output:

```text
Database connection successful.
```

If the connection fails:

1. Verify database credentials.
2. Confirm PostgreSQL is running.
3. Check network connectivity.
4. Verify database permissions.

---

# Database Migration Configuration

Database migrations update the database schema when application changes are introduced.

Run pending migrations:

```bash
npm run migrate
```

Check migration status:

```bash
npm run migration:status
```

Expected output:

```text
Migration status: Up to date.
```

Rollback the latest migration:

```bash
npm run migration:rollback
```

Database migrations should be completed before starting a new application version.

---

# Authentication Configuration

TaskFlow uses token-based authentication to secure application access.

Authentication settings control token generation and validation.

## Authentication Variables

| Variable     | Description                                       |
| ------------ | ------------------------------------------------- |
| JWT_SECRET   | Secret key used for signing authentication tokens |
| TOKEN_EXPIRY | Authentication token validity duration            |

Example:

```env
JWT_SECRET=<secret-key>

TOKEN_EXPIRY=24h
```

---

# Generate Authentication Secret

Generate a secure authentication key:

```bash
openssl rand -hex 32
```

Example output:

```text
9d7a3f8b5c2e1a4f7b8d6c5e4a3f2b1c
```

Update the generated value:

```env
JWT_SECRET=9d7a3f8b5c2e1a4f7b8d6c5e4a3f2b1c
```

Do not share authentication secrets or commit them to source control.

---

# API Configuration

API configuration controls communication between TaskFlow components.

## API Variables

| Variable     | Description                      | Example               |
| ------------ | -------------------------------- | --------------------- |
| API_BASE_URL | Backend API endpoint             | http://localhost:3000 |
| API_TIMEOUT  | Maximum request timeout duration | 30000                 |

Example:

```env
API_BASE_URL=http://localhost:3000

API_TIMEOUT=30000
```

---

# Logging Configuration

TaskFlow provides configurable logging levels to control application output.

## Supported Log Levels

| Level | Description                                  |
| ----- | -------------------------------------------- |
| error | Records application failures                 |
| warn  | Records warnings and errors                  |
| info  | Records normal application activity          |
| debug | Records detailed troubleshooting information |

Configure logging:

```env
LOG_LEVEL=info
```

Enable detailed logs for troubleshooting:

```env
LOG_LEVEL=debug
```

Disable debug logging in production environments.

---

# Logging Configuration Example

Application logging reads the configured log level during startup.

Example:

```javascript
const logLevel = process.env.LOG_LEVEL || "info";

logger.configure({
    level: logLevel
});
```

---

# Workspace Configuration

Workspace configuration controls the default project environment available to users.

## Workspace Variables

| Variable              | Description                                     |
| --------------------- | ----------------------------------------------- |
| DEFAULT_WORKSPACE     | Default workspace selected after authentication |
| WORKSPACE_ACCESS_MODE | Controls workspace visibility rules             |

Example:

```env
DEFAULT_WORKSPACE=engineering

WORKSPACE_ACCESS_MODE=restricted
```

Available access modes:

| Mode       | Description                             |
| ---------- | --------------------------------------- |
| open       | Allows all authenticated users          |
| restricted | Requires assigned workspace permissions |

---

# Feature Configuration

TaskFlow uses feature flags to enable or disable optional functionality.

Feature flags allow teams to control feature availability without modifying application code.

Example:

```env
FEATURE_NOTIFICATIONS=true

FEATURE_ANALYTICS=true

FEATURE_EXPORT=false
```

---

# Feature Flag Reference

| Variable              | Description                 |
| --------------------- | --------------------------- |
| FEATURE_NOTIFICATIONS | Enables task notifications  |
| FEATURE_ANALYTICS     | Enables dashboard analytics |
| FEATURE_EXPORT        | Enables project data export |

Example usage:

```javascript
if (process.env.FEATURE_ANALYTICS === "true") {
    enableAnalytics();
}
```

Restart TaskFlow after changing feature flags.

---

# Configuration Validation

TaskFlow validates required configuration values before starting application services.

Run configuration validation:

```bash
npm run config:check
```

Expected output:

```text
Configuration validation successful.
```

---

# Manual Configuration Validation

Required environment variables can also be checked manually.

Run:

```bash
grep -E 'APP_PORT|DATABASE_HOST|DATABASE_NAME|JWT_SECRET' .env
```

Example output:

```text
APP_PORT=3000

DATABASE_HOST=localhost

DATABASE_NAME=taskflow

JWT_SECRET=<configured>
```

---

# Production Configuration

Before deploying TaskFlow to a production environment, update configuration values.

Recommended production settings:

```env
APP_ENV=production

LOG_LEVEL=warn

FEATURE_DEBUG=false
```

Production environments should use:

* Secure database credentials.
* Strong authentication secrets.
* Restricted workspace access.
* Appropriate logging levels.
* External secret management.

---

# Secrets Management

Sensitive configuration values should not be stored directly in source code.

Sensitive values include:

* Database passwords.
* Authentication secrets.
* API keys.
* Service credentials.

Recommended secret management solutions:

* HashiCorp Vault.
* AWS Secrets Manager.
* Azure Key Vault.

---

# Configuration Backup

Create a backup before modifying configuration files.

Backup configuration:

```bash
cp .env .env.backup
```

Restore configuration:

```bash
cp .env.backup .env
```

Store configuration backups securely.

---

# Security Recommendations

Follow these security practices:

* Do not commit `.env` files to source control.
* Rotate authentication secrets regularly.
* Use separate credentials for each environment.
* Restrict database user permissions.
* Disable debug logging in production.
* Protect configuration backup files.

---

# Troubleshooting Configuration Issues

## Application Startup Failure

Check environment variables:

```bash
grep -E 'APP_PORT|DATABASE_HOST|JWT_SECRET' .env
```

Review application logs:

```bash
tail -f logs/error.log
```

---

## Database Connection Failure

Verify PostgreSQL availability:

```bash
systemctl status postgresql
```

Test database connection:

```bash
psql -h localhost -U taskflow_admin -d taskflow
```

Verify database values in the `.env` file.

---

## Authentication Failure

Verify authentication configuration:

```env
JWT_SECRET=<configured-secret>

TOKEN_EXPIRY=24h
```

Restart TaskFlow services after updating authentication settings.

---

## Feature Flag Not Applied

If a feature change is not visible:

1. Verify the feature flag value.
2. Restart TaskFlow services.
3. Clear application cache if applicable.
4. Verify user permissions.

---



