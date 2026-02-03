---
id: 8e2f7c1a
title: Implement Drawer Events and Commands
created_at: 2026-02-03T20:10:00+10:30
updated_at: 2026-02-03T20:10:00+10:30
status: todo
epic_id: 7f2c1a9b
phase_id: 3b9d7e4c
story_id: 7b2d9f4e
assigned_to: 2026-02-03T18:38:22+10:30
---

# Implement Drawer Events and Commands

## Objective
Wire `pi.events` integration and optional command registration for drawer control.

## Related Story
- story-7b2d9f4e-extension-integration.md

## Steps
1. Register `pi.events` listeners for `drawer:open`, `drawer:close`, `drawer:toggle`, `drawer:set-content`, `drawer:set-hidden`.
2. Define payload schemas and merge options with defaults safely.
3. Register optional `/drawer-open`, `/drawer-close`, `/drawer-toggle` commands for manual use.
4. Ensure event handlers interact with the core `DrawerController`.

## Expected Outcome
Other extensions can drive the drawer via events or commands.

## Actual Outcome
TBD

## Lessons Learned
TBD
