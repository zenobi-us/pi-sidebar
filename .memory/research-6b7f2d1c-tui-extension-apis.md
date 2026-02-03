---
id: 6b7f2d1c
title: TUI Extension APIs and Side Drawer Feasibility
created_at: 2026-02-03T19:45:00+10:30
updated_at: 2026-02-03T19:45:00+10:30
status: completed
epic_id: 7f2c1a9b
phase_id: 4e1c2f9a
related_task_id: a1c4e2f7
---

# TUI Extension APIs and Side Drawer Feasibility

## Research Questions
- What pi TUI extension APIs and registration points exist in `@mariozechner/pi-tui`?
- What layout/overlay constraints and patterns enable a side drawer?
- How is focus/keyboard input routed (including IME support)?
- What animation/transition support exists?
- Are there drawer-like examples or patterns?
- What known constraints/limitations exist?
- How can other extensions integrate with a side drawer?

## Summary
pi extensions can render custom TUI components via `ctx.ui.custom()` and overlays (`overlay: true`) with rich `overlayOptions` (anchor, size, margins, responsive visibility). A side drawer can be built as an overlay panel (e.g., right-center anchor with width percentage/minWidth) and controlled via overlay handles for visibility. Input routing relies on `handleInput`/`matchesKey` and IME support via the `Focusable` interface. There is no built-in transition system, but overlays can be animated by repeatedly requesting renders; examples show 30 FPS overlay animation. Key constraints: rendered lines must not exceed component width, overlays are disposed on close, and focus must be propagated in container components.

## Findings
### Extension entry points and UI hooks
- Extensions subscribe to lifecycle events with `pi.on(...)` and can register tools, commands, and shortcuts (`pi.registerTool`, `pi.registerCommand`, `pi.registerShortcut`). This provides integration points for other extensions to open/close the drawer or push content (commands/tools), and for the drawer extension to respond to app events. [extensions.md]
- Custom TUI components are rendered via `ctx.ui.custom()`, and there is explicit overlay mode with `overlay: true` and `overlayOptions`. Overlay handles can control visibility (`handle.setHidden`, `handle.hide`) and enable programmatic control. [extensions.md, tui.md]
- Extensions can also add widgets (`ctx.ui.setWidget`) and custom footers (`ctx.ui.setFooter`), which could be used by other extensions to display drawer status indicators. [extensions.md]

### Layout, overlay, and side drawer patterns
- Overlays support size (`width`, `minWidth`, `maxHeight`), anchor positioning (e.g., `right-center`), offsets, margins, absolute/percentage positioning, and responsive `visible` callbacks. This fits side drawer requirements. [tui.md, pi-tui README]
- The overlay QA examples include a responsive side panel using `anchor: "right-center"`, `width: "25%"`, `minWidth: 30`, `margin: { right: 1 }`, and `visible` when terminal width >= 100. This is a concrete pattern for a right-side drawer. [overlay-qa-tests.ts]

### Input, focus, and keyboard routing
- Components implement `handleInput(data)` to receive key input; `matchesKey()` provides key detection helpers (e.g., `Key.escape`, `Key.enter`, `Key.ctrl("c")`). [tui.md]
- For IME support, focusable components implement `Focusable` and emit `CURSOR_MARKER`. Container components must propagate focus to embedded inputs to correctly position the hardware cursor. This is relevant if the drawer contains inputs or editors. [tui.md, pi-tui README]

### Animation and transitions
- There is no built-in transition system, but the `ctx.ui.custom()` handle exposes `requestRender()` to force rerenders. The overlay QA tests include an animation demo (~30 FPS) to prove real-time updates, suggesting drawer open/close animation could be done by updating state and calling `requestRender()` on a timer. [tui.md, overlay-qa-tests.ts]

### Constraints and limitations
- Each rendered line must not exceed the provided width, or the TUI will error. Use `truncateToWidth`/`wrapTextWithAnsi` for safety. This is critical for narrow drawers. [tui.md, pi-tui README]
- Overlay components are disposed when closed; do not reuse component instances across open/close cycles—create fresh instances. This impacts how the drawer is reopened. [tui.md]

### Integration approaches for other extensions
- Provide drawer control via commands (`pi.registerCommand`) or tools (`pi.registerTool`) so other extensions can toggle or populate the drawer.
- Use overlay handle controls to expose visibility toggles (e.g., a shared API surface in the drawer extension).
- Use shared status widgets (`ctx.ui.setWidget`, `setFooter`) for lightweight integration if full drawer usage is unnecessary.

## References
1. `/home/zenobius/.local/share/mise/installs/npm-mariozechner-pi-coding-agent/0.51.2/lib/node_modules/@mariozechner/pi-coding-agent/docs/extensions.md` (Custom UI, extension API, widgets/footers). Credibility 9/10 — official documentation.
2. `/home/zenobius/.local/share/mise/installs/npm-mariozechner-pi-coding-agent/0.51.2/lib/node_modules/@mariozechner/pi-coding-agent/docs/tui.md` (component interface, overlays, focus/IME, keyboard input, constraints). Credibility 9/10 — official documentation.
3. `/home/zenobius/.local/share/mise/installs/npm-mariozechner-pi-coding-agent/0.51.2/lib/node_modules/@mariozechner/pi-coding-agent/examples/extensions/overlay-qa-tests.ts` (overlay side panel, animation demo, overlay options). Credibility 8/10 — official examples.
4. `/mnt/Store/Projects/Mine/Github/pi-sidebar/node_modules/@mariozechner/pi-tui/README.md` (overlay API, handle methods, constraints). Credibility 8/10 — package README.
