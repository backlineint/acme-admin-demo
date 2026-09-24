# Standardized layout header alignment across application routes: Sidebar Composition Isolated Within Appsidebar Rather

Status: proposed
Date: 2026-09-24
Deciders: AI (signal conversion)

## Context

- Feature views and authenticated route boundaries previously implemented custom header margins, layout wrappers, and alignment overrides directly within individual view components. This resulted in disparate layout wrappers across feature entry points and inconsistent visual alignment between different application areas.
- Recent refactoring across authenticated layout structures, TopNav, AppSidebar, and page headers establishes a unified layout hierarchy. Sidebar composition has been consolidated into AppSidebar, and shared layout primitives now dictate alignment across feature modules and error routes.

## Problem Statement

Should feature modules define their own top navigation margins or delegate all layout alignment to shared layout components?

## Decision

1. MUST: Sidebar composition MUST be isolated within AppSidebar rather than composed or modified directly inside feature module views.

## Policy Block

- MUST Sidebar composition MUST be isolated within AppSidebar rather than composed or modified directly inside feature module views.

In scope:
- Feature root views under src/features/*
- Authenticated routes and error views under src/routes/*
- Shared layout components including TopNav, AppSidebar, AppTitle, and AuthenticatedLayout

Out of scope:
- Internal layout alignment within child components inside a feature view
- Unauthenticated or public landing pages that do not use the authenticated layout

Exceptions:
- ex-1: A specialized full-screen or embedded route explicitly requires overriding standard navigation alignment.

## Rationale

- Enforces a consistent layout hierarchy and header alignment across disparate feature modules and error routes.
- Eliminates disparate layout wrappers and positioning logic in feature entry points, reducing layout maintenance overhead.
- Centralizing sidebar and navigation composition ensures structural changes propagate uniformly across all authenticated routes.

## Consequences

Positive:
- Consistent layout structure and header positioning across all feature and error routes.
- Clear separation of concerns between shared layout framing and feature-specific content.
- Simplified feature views that no longer manage top-level navigation margins.

Negative:
- Feature views lose the ability to apply arbitrary custom margins to their header boundaries without updating shared primitives.

## Alternatives

- Feature-specific header margin and alignment overrides inside individual view components (rejected)
  Rejected because: Creates visual inconsistency, fragmented layout hierarchies, and duplicate wrapper logic across route entry points.
- Parameterized layout wrapper accepting custom margin configurations from route entry points (deferred)
  When valid: When distinct feature workflows demonstrate validated requirements for specialized margin adjustments that shared layout primitives cannot accommodate.

## Risks

- A feature module might require distinct layout spacing that the shared layout primitives do not natively support.
  Mitigation: Evolve shared layout components (TopNav, AuthenticatedLayout) to provide canonical spacing variants rather than allowing inline view overrides.
  Owner: Architecture Review

## Implementation Notes

- Verify TopNav, AppSidebar, and AppTitle imports across feature components and error views.
- Ensure AuthenticatedLayout consistently handles layout frame offsets without delegating positioning to child views.

## Continuation Context


Verify commands:
- Discover and execute layout component test suites verifying header and sidebar composition.
- Discover and execute static analysis checks on feature entry points to detect disallowed margin wrappers.

Accept when:
- All feature root views render using shared layout primitives without custom margin wrappers.
- AppSidebar encapsulates all sidebar composition logic across authenticated routes.
- All layout and route tests pass without violations.

## Enforcement

- Verified by: Code review verification of feature root components to ensure no custom layout margin wrappers are introduced.
- Verified by: Automated linting or static checks targeting style and margin overrides on layout boundaries.
- Violation handling: Pull requests introducing custom top navigation margin wrappers or direct sidebar composition will be blocked during review.
- Violation handling: Feature code must be refactored to use standard layout primitives before merging.
- Exception process: Submit an architectural exception request detailing why the shared layout primitives cannot fulfill the specific layout requirement.

## References

- file:src/components/layout/top-nav.tsx
- file:src/components/layout/app-sidebar.tsx
- file:src/components/layout/app-title.tsx
- file:src/components/layout/authenticated-layout.tsx
- file:src/features/apps/index.tsx
- file:src/features/chats/index.tsx
- file:src/features/dashboard/index.tsx
- file:src/features/settings/index.tsx
- file:src/features/tasks/index.tsx
- file:src/features/users/index.tsx
- file:src/routes/_authenticated/errors/$error.tsx
- file:src/routes/clerk/_authenticated/user-management.tsx
- commit:cc4fcdaef4441f1970f6246078d6953ef81dc43f
- commit:cc3a247a840fdb75e2eb1021039b5ab9c92d2095
- commit:69f5a713cb4f8511ae80d3e16e101d2b33246f73
- pr:#293
- pr:#216