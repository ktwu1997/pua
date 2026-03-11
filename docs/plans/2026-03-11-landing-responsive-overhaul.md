# Landing Responsive Overhaul Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Make the `landing` module fully responsive across desktop, tablet, and mobile while preserving the current editorial visual style.

**Architecture:** Refactor the landing page toward reusable responsive rendering primitives inside `App.tsx`, then apply a breakpoint-driven CSS layer in `index.css`. Keep desktop table layouts where they work and add mobile card views for comparison-heavy sections so the same data powers both presentations.

**Tech Stack:** React 19, TypeScript, Vite 7, CSS, Vitest, React Testing Library

---

### Task 1: Add test infrastructure for landing page rendering

**Files:**
- Modify: `landing/package.json`
- Modify: `landing/package-lock.json`
- Create: `landing/vitest.config.ts`
- Create: `landing/src/test/setup.ts`

**Step 1: Write the failing test**

Create a component test file import target before the test infrastructure exists.

```tsx
import { describe, expect, it } from "vitest"

describe("landing responsive views", () => {
  it("renders mobile comparison cards", () => {
    expect(true).toBe(false)
  })
})
```

**Step 2: Run test to verify it fails**

Run: `npm run test -- landing/src/App.test.tsx`

Expected: FAIL because test script / Vitest config does not exist yet.

**Step 3: Write minimal implementation**

- add `test` script using Vitest
- add `vitest.config.ts` with `jsdom` environment
- add shared test setup file for RTL and jest-dom matchers
- install `vitest`, `jsdom`, `@testing-library/react`, `@testing-library/jest-dom`

**Step 4: Run test to verify it passes infrastructure boot**

Run: `npm run test -- --run`

Expected: Vitest starts successfully, even if the placeholder assertion still fails for the next task.

**Step 5: Commit**

```bash
git add landing/package.json landing/package-lock.json landing/vitest.config.ts landing/src/test/setup.ts
git commit -m "test(landing): add responsive page test setup"
```

### Task 2: Add failing tests for responsive landing structures

**Files:**
- Create: `landing/src/App.test.tsx`
- Modify: `landing/src/App.tsx`

**Step 1: Write the failing test**

Add tests that expect:

- mobile comparison cards for excuses and scenarios are rendered
- benchmark tab labels are rendered from data
- install section exposes all three install modes

```tsx
it("renders mobile comparison cards for scenario data", () => {
  render(<App />)
  expect(screen.getByTestId("scenarios-mobile")).toBeInTheDocument()
})
```

**Step 2: Run test to verify it fails**

Run: `npm run test -- --run`

Expected: FAIL because the page does not yet expose the tested responsive structures.

**Step 3: Write minimal implementation**

- expose stable test ids / semantic wrappers for responsive comparison views
- keep rendering data-driven from existing constants

**Step 4: Run test to verify it passes**

Run: `npm run test -- --run`

Expected: PASS for the new structural assertions.

**Step 5: Commit**

```bash
git add landing/src/App.test.tsx landing/src/App.tsx
git commit -m "test(landing): cover responsive landing structures"
```

### Task 3: Refactor landing layout into reusable responsive primitives

**Files:**
- Modify: `landing/src/App.tsx`

**Step 1: Write the failing test**

Add a test that asserts responsive grid helpers and comparison sections are rendered through reusable wrappers rather than duplicated one-off blocks.

```tsx
it("renders both desktop and mobile comparison containers for scenarios", () => {
  render(<App />)
  expect(screen.getByTestId("scenarios-desktop")).toBeInTheDocument()
  expect(screen.getByTestId("scenarios-mobile")).toBeInTheDocument()
})
```

**Step 2: Run test to verify it fails**

Run: `npm run test -- --run`

Expected: FAIL until the wrappers and containers are implemented.

**Step 3: Write minimal implementation**

- add small internal helpers such as:
  - `SectionGrid`
  - `MetricGrid`
  - `ResponsiveTableCards`
  - `ComparisonCard`
- replace repeated inline grid declarations with these helpers where practical
- keep content data and copy unchanged

**Step 4: Run test to verify it passes**

Run: `npm run test -- --run`

Expected: PASS.

**Step 5: Commit**

```bash
git add landing/src/App.tsx
git commit -m "refactor(landing): add responsive layout primitives"
```

### Task 4: Implement responsive CSS overhaul

**Files:**
- Modify: `landing/src/index.css`
- Modify: `landing/src/App.tsx`

**Step 1: Write the failing test**

Add test assertions for class-backed structural hooks used by the responsive layout:

```tsx
it("marks responsive sections with mobile card view hooks", () => {
  render(<App />)
  expect(screen.getByTestId("excuses-mobile")).toHaveClass("mobile-only")
})
```

**Step 2: Run test to verify it fails**

Run: `npm run test -- --run`

Expected: FAIL because the responsive class hooks and CSS-backed structure are not in place.

**Step 3: Write minimal implementation**

- add responsive utility classes and section classes
- add tablet/mobile breakpoints
- adjust hero spacing, typography, button stacking, and language-switch positioning
- make tabs scroll or wrap on smaller screens
- create mobile card styling for table-derived sections
- normalize spacing and max-width behavior across sections

**Step 4: Run test to verify it passes**

Run: `npm run test -- --run`

Expected: PASS.

**Step 5: Commit**

```bash
git add landing/src/index.css landing/src/App.tsx
git commit -m "feat(landing): implement responsive editorial layout"
```

### Task 5: Verify final behavior and prepare contribution output

**Files:**
- Modify: `docs/plans/2026-03-11-landing-responsive-design.md`
- Modify: `docs/plans/2026-03-11-landing-responsive-overhaul.md`

**Step 1: Write the failing test**

Use final verification as the gate:

- build must pass
- tests must pass
- lint status must match or improve relative to baseline

**Step 2: Run verification to confirm current gaps**

Run:

- `npm run test -- --run`
- `npm run build`
- `npm run lint`

Expected: identify any remaining regressions or baseline-only failures.

**Step 3: Write minimal implementation**

- fix any regressions introduced by the responsive refactor
- document baseline lint issue if unchanged
- prepare PR summary and test plan text

**Step 4: Run verification to verify final state**

Run:

- `npm run test -- --run`
- `npm run build`
- `npm run lint`

Expected:

- tests pass
- build passes
- lint either passes or only reports the known pre-existing UI file errors

**Step 5: Commit**

```bash
git add docs/plans/2026-03-11-landing-responsive-design.md docs/plans/2026-03-11-landing-responsive-overhaul.md landing
git commit -m "docs: record landing responsive implementation plan"
```
