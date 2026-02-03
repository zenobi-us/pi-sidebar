---
id: 2d9c5f8a
title: Side Drawer Extension API
created_at: 2026-02-03T19:48:00+10:30
updated_at: 2026-02-03T20:00:00+10:30
status: completed
epic_id: 7f2c1a9b
phase_id: 8c5a6d1e
priority: high
story_points: 5
---

# Side Drawer Extension API

## User Story
As a pi extension developer, I want a configurable side drawer extension so that I can display contextual content without reimplementing overlay logic.

## Acceptance Criteria
- [x] Provide an API for opening/closing the drawer with placement (left/right), width, and responsive visibility options.
- [x] Support focus/input routing for drawer content, including IME-safe focus propagation guidelines.
- [x] Define integration hooks for other extensions (commands/tools or shared state) to push content.
- [x] Document constraints (overlay lifecycle, line width limits, visibility handling).
- [x] Include example usage snippet demonstrating a right-side drawer and toggle behavior.

## Context
Research indicates overlays and `ctx.ui.custom()` are the primary mechanism for side panels, with overlay options for sizing/positioning and handle-based visibility controls.

## Out of Scope
- Full implementation details or tests (covered in later phase).
- Multi-drawer stacking or complex animation systems beyond basic open/close.

## Tasks
- task-91b6c0d2-draft-drawer-api.md

## Notes
- Drawer must respect overlay lifecycle (fresh component instances on reopen).
- Consider responsive `visible` callbacks for narrow terminals.
