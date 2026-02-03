---
id: 7b2d9f4e
title: Extension Integration Hooks
created_at: 2026-02-03T20:06:00+10:30
updated_at: 2026-02-03T20:06:00+10:30
status: planning
epic_id: 7f2c1a9b
phase_id: 3b9d7e4c
priority: high
story_points: 3
---

# Extension Integration Hooks

## User Story
As an extension author, I want to integrate with the drawer via commands or events so that my extension can push content or toggle the drawer without direct dependency.

## Acceptance Criteria
- [ ] Drawer listens to `pi.events` (`drawer:open`, `drawer:close`, `drawer:toggle`, `drawer:set-content`, `drawer:set-hidden`).
- [ ] Optional commands (`/drawer-open`, `/drawer-close`, `/drawer-toggle`) are registered for manual use.
- [ ] Payloads accept content + options overrides without breaking defaults.

## Context
`pi.events` is the shared event bus for inter-extension communication.

## Out of Scope
- Permissioning or access control for events.

## Tasks
- task-8e2f7c1a-implement-events-commands.md
- task-6d3c9b4e-add-drawer-tests.md

## Notes
- Keep event payloads stable and documented.
