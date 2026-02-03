---
id: 4a1d9b2c
title: Implement Core Drawer Overlay
created_at: 2026-02-03T20:10:00+10:30
updated_at: 2026-02-03T20:10:00+10:30
status: todo
epic_id: 7f2c1a9b
phase_id: 3b9d7e4c
story_id: 5f8a1c2d
assigned_to: 2026-02-03T18:38:22+10:30
---

# Implement Core Drawer Overlay

## Objective
Implement the drawer overlay component and controller API (open/close/toggle/setContent/setHidden/isOpen).

## Related Story
- story-5f8a1c2d-core-drawer-behavior.md

## Steps
1. Locate extension entry points and decide file placement for the drawer package in `src/`.
2. Implement `DrawerController` with internal overlay handle management and open state tracking.
3. Map `DrawerOptions` to overlay options (anchor, width/minWidth, maxHeight, margin, visible).
4. Build a default drawer container component that enforces width limits using `truncateToWidth`/`wrapTextWithAnsi`.
5. Implement close-on-escape handling via `matchesKey(Key.escape)`.
6. Ensure overlay lifecycle is respected (new component instance per open).

## Expected Outcome
Core drawer overlay API implemented and usable by other integration hooks.

## Actual Outcome
TBD

## Lessons Learned
TBD
