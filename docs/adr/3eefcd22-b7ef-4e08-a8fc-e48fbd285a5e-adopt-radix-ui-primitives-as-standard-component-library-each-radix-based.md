# Adopt Radix UI Primitives as Standard Component Library: Each Radix Based

Status: proposed
Date: 2024-01-09
Deciders: Detection Pipeline (automated)

## Context

- The codebase contains 59 files with consistent usage of Radix UI primitives for building accessible UI components, indicating a standardized approach to component architecture
- UI components such as dropdown-menu, avatar, scroll-area, form, select, skeleton, tabs, alert-dialog, and badge all follow the Radix UI primitive pattern with high confidence (88.77%)
- The project requires accessible, unstyled, and composable UI primitives that can be customized with application-specific styling while maintaining WCAG compliance
- Authentication flows, error pages, and feature modules all leverage these Radix-based components, demonstrating cross-cutting architectural significance
- The pattern appears in both route components and reusable UI components, suggesting a deliberate architectural decision rather than ad-hoc usage

## Problem Statement

Teams need a consistent, accessible, and maintainable approach to building UI components that avoids reinventing common interaction patterns, ensures WCAG compliance, and provides flexibility for custom styling without sacrificing accessibility or requiring extensive accessibility expertise from every developer.

## Decision

1. MUST: Each Radix-based component MUST re-export the primitive's sub-components with appropriate TypeScript types and ref forwarding

## Policy Block

- MUST Each Radix-based component MUST re-export the primitive's sub-components with appropriate TypeScript types and ref forwarding

In scope:
- All interactive UI components requiring keyboard navigation, focus management, or ARIA attributes
- Form controls (select, checkbox, radio, switch)
- Overlay components (dialog, dropdown, popover, tooltip, context menu)
- Navigation components (tabs, accordion, navigation menu)
- Feedback components (alert dialog, toast)
- Components used in authentication flows, error pages, and feature modules

Out of scope:
- Static presentational components without interaction (typography, spacing utilities)
- Layout components (grid, flex containers) unless they require specific accessibility features
- Third-party widget integrations that provide their own accessibility layer
- Canvas or WebGL-based interactive elements where DOM-based accessibility is not applicable

Exceptions:
- EXC-001: A required interaction pattern is not available in Radix UI and cannot be reasonably composed from existing primitives
- EXC-002: Performance profiling demonstrates that Radix primitive overhead is unacceptable for a specific high-frequency interaction

## Rationale

- The detection of 59 files with 88.77% confidence indicates this is an established, successful pattern rather than an experiment, demonstrating proven value in production
- Radix UI primitives provide battle-tested accessibility implementations that comply with WAI-ARIA standards, reducing the risk of accessibility defects and legal compliance issues
- Using unstyled primitives allows the team to maintain consistent visual design while ensuring interaction patterns remain accessible and predictable
- Standardizing on a single component library reduces cognitive load, improves code review efficiency, and enables better code reuse across features

## Consequences

Positive:
- Consistent accessibility across all UI components without requiring deep accessibility expertise from every developer
- Reduced development time for new features by leveraging pre-built, tested interaction patterns
- Improved maintainability through standardized component APIs and predictable behavior
- Better keyboard navigation and screen reader support out of the box, improving user experience for assistive technology users
- Easier onboarding for new developers who can learn one component system rather than multiple custom implementations

Negative:
- Dependency on external library means breaking changes in Radix UI require coordinated updates across the codebase
- Bundle size increases compared to minimal custom implementations, though this is mitigated by tree-shaking
- Learning curve for developers unfamiliar with Radix UI's composition patterns and prop APIs
- Potential limitations when highly custom interaction patterns are needed that don't align with Radix's design philosophy
- Additional abstraction layer may complicate debugging of interaction issues

## Alternatives

- Build custom accessible components from scratch using native HTML and ARIA attributes (rejected)
  Rejected because: Requires significant accessibility expertise, increases maintenance burden, and has higher risk of accessibility defects. The 59-file adoption of Radix demonstrates the team chose a more reliable path.
  When valid: Only for highly specialized interactions where no library solution exists and accessibility expertise is available
- Adopt a complete UI framework like Material-UI or Ant Design with opinionated styling (rejected)
  Rejected because: Opinionated styling makes it difficult to achieve custom design requirements and increases bundle size. Radix's unstyled approach provides better flexibility.
  When valid: For projects where the framework's design system aligns perfectly with product requirements
- Use Headless UI (by Tailwind Labs) as the primitive layer (rejected)
  Rejected because: Radix UI has broader primitive coverage and more active development. The existing 59-file investment makes migration costly without clear benefits.
  When valid: For new projects heavily invested in Tailwind CSS ecosystem with simpler component needs

## Risks

- Radix UI library becomes unmaintained or introduces breaking changes that require extensive refactoring
  Mitigation: Monitor Radix UI release notes and community health metrics. Maintain abstraction layer in src/components/ui/ that isolates direct Radix dependencies. Budget time for major version upgrades.
  Owner: Engineering team and architecture review board
- Developers may incorrectly override accessibility features while customizing component styling
  Mitigation: Implement automated accessibility testing in CI pipeline. Provide component development guidelines with examples. Conduct accessibility-focused code reviews for new components.
  Owner: Frontend team leads and accessibility champions
- Bundle size growth as more Radix primitives are adopted across the application
  Mitigation: Monitor bundle size in CI. Ensure tree-shaking is properly configured. Use dynamic imports for less-frequently-used components. Set bundle size budgets and alerts.
  Owner: Performance engineering team

## Implementation Notes

- Create wrapper components in src/components/ui/ that re-export Radix primitives with project-specific styling and TypeScript types
- Use React.forwardRef for all component wrappers to ensure ref forwarding works correctly with Radix primitives
- Document component usage with Storybook or similar tool, showing both basic usage and accessibility features (keyboard navigation, screen reader announcements)
- Establish naming convention: file names should match Radix primitive names in kebab-case (e.g., dropdown-menu.tsx for @radix-ui/react-dropdown-menu)
- Include unit tests for custom component logic and integration tests for accessibility features using @testing-library/react and jest-axe

## Continuation Context


Verify commands:
- grep -r "@radix-ui/react-" src/components/ui/ | wc -l
- find src/components/ui -name '*.tsx' -exec grep -L 'React.forwardRef' {} \; | wc -l
- npm list @radix-ui/react-dropdown-menu @radix-ui/react-dialog @radix-ui/react-select

Accept when:
- All interactive components in src/components/ui/ import from @radix-ui packages
- Component wrappers properly forward refs using React.forwardRef
- Radix UI dependencies are present in package.json and properly versioned
- Automated accessibility tests pass for all Radix-based components

## Enforcement

- Verified by: Automated CI checks scanning for Radix UI imports in src/components/ui/
- Verified by: Code review checklist requiring verification of Radix primitive usage for new interactive components
- Verified by: Accessibility testing in CI using jest-axe or similar automated a11y testing tools
- Verified by: Bundle size monitoring to track impact of Radix UI adoption
- Violation handling: CI pipeline fails if new interactive components in src/components/ui/ don't use Radix primitives without documented exception
- Violation handling: Code review blocks merge if accessibility attributes are overridden or removed without justification
- Violation handling: Architecture review required for custom component implementations that bypass Radix primitives
- Violation handling: Quarterly audits of component library to identify and refactor non-compliant components
- Exception process: Submit exception request to architecture review board with technical justification and accessibility impact analysis
- Exception process: For approved exceptions, document rationale in component file comments and maintain in exceptions registry
- Exception process: Require accessibility audit and WCAG compliance testing for all exception cases
- Exception process: Review exceptions quarterly to determine if Radix UI has added support for the required pattern