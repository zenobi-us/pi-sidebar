---
id: 5a7c2e1b
title: Side Drawer Extension API Proposal
created_at: 2026-02-03T19:55:00+10:30
updated_at: 2026-02-03T19:55:00+10:30
status: planning
area: api-ux-design
tags:
  - tui
  - extension-api
  - overlay
learned_from:
  - research-6b7f2d1c-tui-extension-apis.md
  - story-2d9c5f8a-side-drawer-extension-api.md
---

# Side Drawer Extension API Proposal

## Overview
Provide a reusable side drawer extension built on `ctx.ui.custom()` overlays. The drawer exposes a small programmatic API plus an inter-extension event contract (`pi.events`) so other extensions can open, update, or close the drawer without direct coupling.

## Details
### Proposed API Surface (package-level)
```ts
type DrawerPlacement = 'left' | 'right';

interface DrawerOptions {
  placement?: DrawerPlacement;          // default: 'right'
  width?: number | string;              // number or percentage string
  minWidth?: number;                    // default: 30
  maxHeight?: number | string;          // default: '100%'
  margin?: number | { top?: number; right?: number; bottom?: number; left?: number };
  visible?: (termWidth: number, termHeight: number) => boolean;
  closeOnEscape?: boolean;              // default: true
}

interface DrawerContent {
  title?: string;
  render: (tui: TUI, theme: Theme) => Component;
}

interface DrawerController {
  open(content?: DrawerContent): void;
  close(): void;
  toggle(content?: DrawerContent): void;
  setContent(content: DrawerContent): void;
  setHidden(hidden: boolean): void;
  isOpen(): boolean;
}

interface DrawerExtensionConfig {
  options?: DrawerOptions;
  initialContent?: DrawerContent;
  commands?: boolean;   // register /drawer-open /drawer-close /drawer-toggle
  events?: boolean;     // register pi.events listeners
}
```

### Overlay/Rendering Behavior
- Uses `ctx.ui.custom()` with `{ overlay: true }` and `overlayOptions` derived from `DrawerOptions`.
- Placement maps to `anchor`: `right-center` or `left-center` with optional margin/offset.
- Uses `overlayOptions.visible` for responsive hide on narrow terminals.
- Drawer component must enforce line width using `truncateToWidth` or `wrapTextWithAnsi`.
- Overlay handle stored to allow `setHidden(true/false)` and `close()`.
- Recreate overlay component instances on each open (overlay lifecycle constraint).

### Focus/Input Handling
- Drawer content components should implement `handleInput` for keyboard interaction.
- If content includes inputs/editors, wrapper components must implement `Focusable` and propagate `focused` to child inputs for IME safety.
- Provide `closeOnEscape` handling using `matchesKey(data, Key.escape)`.

### Integration Contract (pi.events)
Emit/subscribe to events so other extensions can integrate without direct imports:
- `drawer:open` `{ content?: DrawerContent; options?: DrawerOptions }`
- `drawer:close` `{ reason?: string }`
- `drawer:toggle` `{ content?: DrawerContent }`
- `drawer:set-content` `{ content: DrawerContent }`
- `drawer:set-hidden` `{ hidden: boolean }`

### Optional Commands (for manual testing)
- `/drawer-open`, `/drawer-close`, `/drawer-toggle` to exercise behavior and make manual QA easy.

### Example Usage
```ts
pi.events.emit('drawer:open', {
  content: {
    title: 'Context',
    render: (_tui, theme) => new Text(theme.fg('accent', 'Drawer content'), 1, 1),
  },
  options: { placement: 'right', width: '25%', minWidth: 30 },
});
```

### Constraints/Notes
- Rendered lines must not exceed drawer width.
- Overlay components are disposed on close; reopen creates fresh instances.
- Animations require manual state updates + `requestRender()` loops (no built-in transitions).
