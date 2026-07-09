# Project Structure

## Overview

The TaskFlow repository is organized into modules with well-defined responsibilities. Each module owns a specific part of the application and exposes only the interfaces required by other modules.

This structure reduces coupling, simplifies maintenance, and allows features to evolve independently.

---

## Directory Layout

```text
src/
├── api/
├── assets/
├── components/
├── config/
├── features/
├── hooks/
├── layouts/
├── models/
├── pages/
├── services/
├── shared/
├── store/
├── styles/
├── types/
└── utils/
```

The top-level directories separate application concerns rather than implementation details.

---

## Directory Responsibilities

### api/

Contains HTTP clients, request definitions, response models, and API-specific utilities.

The `api` module is responsible for communication with external services. Business logic should not be implemented in this module.

---

### assets/

Stores static application resources such as images, icons, fonts, and localization files.

Assets should not contain executable code.

---

### components/

Contains reusable UI components shared across multiple features.

Components should remain presentation-focused and should not contain feature-specific business logic.

---

### config/

Contains application configuration, environment settings, feature flags, and runtime constants.

Configuration values should be accessed through this module rather than referenced directly throughout the application.

---

### features/

Contains feature modules that implement business functionality.

Each feature owns its UI, services, state, and supporting logic.

Features should remain independent wherever possible and communicate through shared interfaces.

---

### hooks/

Contains reusable application hooks.

Hooks encapsulate reusable behavior and should avoid implementing business rules specific to a single feature.

---

### layouts/

Contains reusable page layouts used throughout the application.

Layouts define page structure but should not contain business logic.

---

### models/

Defines domain models used throughout the application.

Models represent business entities and should remain independent of presentation and infrastructure concerns.

---

### pages/

Contains application entry points and route-level components.

Pages compose layouts, features, and shared components to construct complete application views.

---

### services/

Contains application services responsible for business operations.

Services coordinate workflows, communicate with APIs, and encapsulate business behavior that is shared across multiple features.

---

### shared/

Contains modules used across the application, including reusable utilities, constants, and common abstractions.

Shared modules must remain generic and independent of feature-specific implementations.

---

### store/

Contains application state management.

Global state, reducers, actions, selectors, and middleware are maintained within this module.

Feature-specific state should remain inside the owning feature unless shared across the application.

---

### styles/

Contains global styles, themes, design tokens, and styling utilities.

Application styling should be centralized to ensure consistency across the user interface.

---

### types/

Contains shared type definitions and interfaces.

Common types should be defined once and reused throughout the application.

---

### utils/

Contains general-purpose helper functions.

Utilities should be stateless, reusable, and independent of application features.

---

## Dependency Rules

The project structure enforces clear dependency boundaries.

* Features may depend on shared modules.
* Features may use services and models.
* Components must not depend directly on pages.
* Pages compose features but should not implement business logic.
* Shared modules must not depend on feature modules.
* Utilities must remain framework-independent wherever practical.

Maintaining these boundaries reduces coupling and simplifies long-term maintenance.

---

## Design Guidelines

Follow these principles when introducing new modules.

* Place code in the module that owns the responsibility.
* Reuse existing modules before creating new ones.
* Avoid circular dependencies.
* Keep modules focused on a single responsibility.
* Expose only the public interfaces required by other modules.

Changes to the project structure should preserve clear ownership and predictable dependencies across the application.

