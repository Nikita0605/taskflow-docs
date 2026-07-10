# Coding Standards

## Overview

This document defines the engineering standards used to develop and maintain TaskFlow.

The goal is to produce code that is predictable, maintainable, and consistent across the application. These standards apply to all production code regardless of feature, module, or contributor.

Code reviews should validate compliance with these standards before implementation is approved.

---

# Design Principles

Every implementation should follow these principles.

| Principle              | Standard                                                                   |
| ---------------------- | -------------------------------------------------------------------------- |
| Single Responsibility  | A component should solve one problem and have one reason to change.        |
| Explicit Dependencies  | Declare dependencies through public interfaces. Avoid hidden dependencies. |
| Separation of Concerns | Keep presentation, business logic, and infrastructure independent.         |
| Composition            | Prefer composition over inheritance when extending functionality.          |
| Simplicity             | Choose the simplest implementation that satisfies the requirement.         |

These principles take precedence over individual implementation preferences.

---

# Code Organization

Organize code around business capabilities instead of technical convenience.

Related business logic, models, validation, and workflows should remain within the same module. Avoid distributing a single business process across multiple unrelated modules.

Before creating a new component, determine whether the functionality belongs to an existing ownership boundary.

**Preferred**

* Keep related logic together.
* Group code by capability.
* Minimize public surface area.

**Avoid**

* Creating modules for a single utility or helper.
* Splitting related business logic across multiple modules.
* Sharing implementation details between modules.

---

# Business Logic

Business rules belong to the capability that owns the business process.

Do not place business logic in presentation components, shared utilities, configuration classes, or infrastructure services.

Business rules should execute consistently regardless of how the capability is invoked.

When business logic is duplicated across multiple modules, move it into the owning capability instead of maintaining parallel implementations.

---

# Dependencies

Every dependency should have a clear purpose.

Before introducing a dependency, verify that:

* The dependency supports the current module responsibility.
* A public interface is available.
* The dependency does not introduce unnecessary coupling.
* The dependency does not duplicate existing functionality.

Avoid implementation dependencies when a stable contract is available.

---

# Public Interfaces

Expose only functionality that other modules are expected to consume.

Public interfaces define the supported contract between modules. Internal implementation details should remain private and may change without affecting consumers.

Expanding a public interface increases the long-term maintenance responsibility of the owning module. New public members should be introduced only when they represent a stable capability.

---

# Validation

Validate data as close as possible to the point where it enters the application.

Business validation belongs to the owning capability.

Do not duplicate validation across presentation, service, and persistence layers unless each layer enforces a different requirement.

Validation failures should produce predictable and actionable results.

---

# Error Handling

Handle errors where meaningful recovery is possible.

Do not suppress exceptions, ignore failures, or return ambiguous results that hide the underlying problem.

Errors should provide enough context for diagnosis without exposing implementation details to consumers.

Unexpected failures should be logged before they leave the current execution boundary.

---

# Code Reviews

Every change should be reviewed for engineering quality, not only functional correctness.

During review, verify the following:

* Does the implementation follow the established ownership boundaries?
* Is business logic implemented in the correct module?
* Does the change introduce unnecessary dependencies?
* Can the implementation be simplified without reducing clarity?
* Are new public interfaces justified?
* Does the implementation follow existing patterns?

A change that functions correctly but weakens the architecture should not be approved.

---

# Exceptions

Standards may be relaxed only when a technical constraint prevents the preferred implementation.

Document the reason for the exception within the code review or design discussion. Future contributors should understand why the standard was not followed and whether the exception remains valid.

