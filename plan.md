# Andrew's Homework Tracker: Project Context and Plan

## Purpose

Andrew's Homework Tracker is a lightweight, browser-based dashboard for reviewing assignments, recording Andrew's progress, comparing that progress with Canvas, verifying completed work, and maintaining a manually prioritized todo list. The application is intentionally deployable as static files while a Google Apps Script endpoint supplies and updates assignment data.

## Current Application

### Dashboard (`index.html`)

- Loads assignment data from the Google Apps Script API and caches the latest response in `localStorage` for a fast fallback view.
- Shows class-level summary counts and grades.
- Groups assignments by date in desktop and mobile layouts.
- Supports class, verification, follow-up, Canvas completion, and missing-date filters.
- Allows Andrew status, verification, and notes to be updated through the API.
- Uses row and group colors to communicate workflow status; assignments without dates are neutral white and appear after dated assignments.

### Todo Priority (`todo.html`)

- Shows active In-Progress and Following-Up assignments.
- Stores a separate manual order for each list in `localStorage`.
- Provides compact controls for reprioritizing assignments and changing their status.
- Allows notes to be edited and saved through the API.

### Data and Automation

- `data.csv` and the archived CSV files provide local data snapshots.
- `.github/workflows/update-csv.yml` accepts repository-dispatch updates and commits a refreshed `data.csv`.
- `.github/workflows/pr-preview.yml` publishes a preview for pull requests.

## Product Rules

1. Keep the dashboard usable as a static site without a build step.
2. Preserve desktop and mobile behavior whenever dashboard rendering changes.
3. Treat missing dates as unknown, not overdue: keep those assignments white and sort them after all dated groups.
4. Apply filters only to the assignment list; summary behavior should remain deliberate and documented when changed.
5. Keep status comparisons case-insensitive where API values may vary.
6. Preserve stable assignment UIDs when sending updates to the API.
7. Bump the affected page's visible `VERSION` for each user-facing revision.
8. Avoid committing credentials or private student information beyond the existing project data contract.

## Delivery Plan

### Phase 1: Dashboard Reliability

- [x] Provide responsive desktop and mobile assignment views.
- [x] Add the main workflow and Canvas filters.
- [x] Keep undated assignments neutral, place them at the bottom, and make them optionally hideable.
- [ ] Add lightweight automated tests for date parsing, status priority, filtering, and sorting.
- [ ] Add clear loading, empty, stale-cache, and API error states.

### Phase 2: Views and Workflow

- [x] Add a manually ordered todo-priority view.
- [ ] Add the planned Andrew view.
- [ ] Add the planned Andrew one-day view.
- [ ] Add the planned teacher view.
- [ ] Define which filters and saved preferences should persist between sessions.

### Phase 3: Maintainability

- [ ] Move shared constants, parsing, status, API, and escaping helpers into reusable JavaScript.
- [ ] Move shared visual tokens and responsive styles into reusable CSS.
- [ ] Document the Google Apps Script request/response schema and supported status values.
- [ ] Add formatting and static validation to continuous integration.

### Phase 4: Data Safety and Operations

- [ ] Validate API responses before rendering or caching them.
- [ ] Surface update failures without discarding local edits.
- [ ] Review the CSV update workflow for concurrency and malformed-payload handling.
- [ ] Document deployment, preview, rollback, and recovery procedures.

## Revision Checklist

For every change:

1. Confirm the behavior against both cached and fresh API data paths.
2. Check desktop and mobile layouts when `index.html` is affected.
3. Check dated, undated, verified, and each Andrew/Canvas status combination when filtering or coloring changes.
4. Bump the visible version for each affected page.
5. Run syntax and project-specific checks, then review the final diff.
6. Update this plan when a milestone, product rule, or architecture decision changes.
