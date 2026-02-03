---
id: 5f8a1c2d
title: Core Drawer Behavior
created_at: 2026-02-03T20:06:00+10:30
updated_at: 2026-02-03T20:06:00+10:30
status: planning
epic_id: 7f2c1a9b
phase_id: 3b9d7e4c
priority: high
story_points: 5
---

# Core Drawer Behavior

## User Story
As a pi extension developer, I want a side drawer overlay that I can open, close, and toggle so that I can present contextual information without reimplementing overlay logic.

## Acceptance Criteria
- [ ] Drawer opens as an overlay with placement left/right and configurable width/minWidth.
- [ ] Drawer supports responsive visibility via a `visible` callback.
- [ ] Drawer content renders within width limits and avoids overflow errors.
- [ ] Drawer respects overlay lifecycle by recreating components on open.
- [ ] Drawer can be hidden or shown without fully closing (visibility toggle).

## Context
Overlays in `ctx.ui.custom()` provide the core mechanism for side panels.

## Out of Scope
- Complex animations beyond basic state updates.
- Multiple simultaneous drawers.

## Tasks
- task-4a1d9b2c-implement-core-drawer.md
- task-6d3c9b4e-add-drawer-tests.md

## Notes
- Must use `truncateToWidth`/`wrapTextWithAnsi` to avoid width violations.
