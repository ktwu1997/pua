# Landing Responsive Adaptation Design

**Date:** 2026-03-11

**Status:** Approved

## Context

The `landing/` app is a standalone Vite + React landing page for the `pua` project. It already has a strong black-and-white editorial style on desktop, but the implementation is still desktop-first:

- many sections use fixed multi-column grids such as `repeat(5, 1fr)` and `repeat(3, 1fr)`
- comparison-heavy sections rely on wide tables
- hero controls and tabs compress poorly on smaller widths
- only a shallow `768px` breakpoint exists in CSS

The goal is not just to stop overflow. The goal is to make the landing page read well on mobile and tablet while preserving the existing visual language.

## Goals

- Keep the current monochrome editorial tone and information architecture
- Deliver full responsive support for desktop, tablet, and mobile
- Improve reading rhythm on small screens instead of only shrinking layout
- Preserve bilingual content and current feature set
- Avoid introducing a new UI framework or a large app-level rewrite

## Non-Goals

- No brand redesign or new visual direction
- No content rewrite beyond structure labels needed for responsive presentation
- No migration away from React/Vite/Tailwind setup
- No large-scale reorganization of the entire `landing/src/` tree

## Constraints

- Work directly in the existing repository without using git worktrees, per user request
- Keep desktop presentation recognizable
- Existing `landing` lint baseline already fails on unrelated `react-refresh/only-export-components` errors in UI utility files

## Chosen Approach

Use a responsive restructuring approach:

1. Keep desktop layouts and table views where they are effective.
2. Introduce reusable responsive section primitives so layout behavior is not embedded in dozens of inline style objects.
3. For mobile, switch wide comparison sections from table-only presentation to card-style comparison blocks.
4. Add a fuller breakpoint system in CSS for desktop, tablet, and mobile spacing, density, and control behavior.

This gives the page a much better mobile reading experience without turning the task into a full component-system rewrite.

## Section-Level Design

### Hero

- Keep the two-column desktop layout
- On tablet/mobile, stack headline content before code preview
- Reduce heading scale progressively
- Make CTA buttons stack cleanly on narrow widths
- Reposition and resize the language switch so it does not dominate the top edge

### Card Grids

Apply shared responsive grid behavior to:

- Problems
- Iron Rules
- Levels
- Failure Mode Framework
- Corporate Styles
- Usage
- Pairs

Desktop keeps the current denser multi-column rhythm. Tablet collapses to two columns. Mobile becomes single-column, with adjusted spacing and typography.

### Checklist

- Keep the concise numbered list feel
- Use two columns on larger screens
- Collapse to one column on smaller screens
- Move the “Ask Gate” badge into a clearer secondary position so rows stay readable

### Anti-Rationalization and Scenario Comparison

- Keep semantic tables on desktop
- Add mobile card variants for narrow screens
- Each mobile card shows:
  - title row
  - supporting labels
  - left/right comparison blocks

This preserves comparison logic without forcing horizontal table reading on phones.

### Benchmark

This is the heaviest section and gets the most structural work:

- keep the desktop stats grid + tabbed detail panel
- make the tab strip horizontally scrollable on narrow screens
- tighten the detail panel spacing on smaller widths
- collapse the 3-up summary stats below the active benchmark
- convert the 5-metric overview wall to a compact responsive stat grid

### Install Tabs

- Keep tab interaction
- Increase tap target quality on mobile
- Allow tab row wrap or horizontal scrolling without clipping
- Ensure copy button placement does not cover command text

## Implementation Shape

Primary files:

- `landing/src/App.tsx`
- `landing/src/index.css`

Likely additions:

- a small set of internal rendering helpers/components inside `App.tsx`
- responsive utility class names in `index.css`

## Testing and Verification

Verification will include:

- `npm run build` in `landing/`
- targeted tests for the new responsive rendering helpers/components
- visual sanity checks through local preview-oriented inspection

Known baseline:

- `npm run lint` currently fails before feature work because of existing `react-refresh/only-export-components` issues in unrelated UI utility files

## Risks

- The page currently relies heavily on inline styles, so careless edits could create inconsistent spacing across sections
- Switching comparison sections to dual table/card views can drift if both render paths are not data-driven from the same source
- Tabs with 10 benchmark options can become noisy on mobile if the active state and overflow behavior are not carefully styled

## Acceptance Criteria

- No obvious horizontal overflow on common mobile widths caused by landing page structure
- Hero, grids, benchmark, comparisons, and install flows remain readable on phone-sized screens
- Desktop layout remains recognizably consistent with the current visual identity
- Build passes
- New tests pass
