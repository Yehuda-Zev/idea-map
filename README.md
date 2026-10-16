# Idea Map

A single-file flowchart / blueprint editor. Boxes, grid-routed arrows, nodes and groups on an infinite pan-and-zoom canvas. Vanilla JS, no dependencies, installable as a PWA, works with mouse, touch and pen.

**Version 1.4.0** (major.minor.patch)

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole app |
| `sw.js` | Service worker (offline use) |
| `manifest.webmanifest`, `icons/` | PWA install metadata |

## Basics

- **New box:** drag on empty space (or double-tap on touch).
- **Arrow:** drag from a box border to another box, node or arrow.
- **Branch:** drag from an arrow to make a node and start a new arrow.
- **Join:** drop an arrow end on another arrow to make a node there.
- **Rename:** double-click (double-tap) a box title, arrow or group label.
- **Delete:** `Del`. **Undo / redo:** `Ctrl+Z` / `Ctrl+Y`.
- **Pan:** drag empty space on touch, right/middle-drag or `Space`+drag on mouse. **Zoom:** wheel or pinch.

## Boxes

- Title plus any number of text lines; boxes can be collapsed to just the title.
- Resize from the corner handles of a selected box.
- Own color or Auto (follows the theme).
- Hold the collapse button 2 s to collapse/expand all.
- **Copy / Paste / Duplicate** (Box menu, or `Ctrl+C` / `Ctrl+V` / `Ctrl+D`, `Ctrl+X` cuts): copies the selection together with the arrows, nodes and groups that sit entirely inside it. Uses the system clipboard, so it also pastes into another tab or map.

## Arrows

- Routed along a grid with the fewest turns, avoiding boxes (or "Ignore boxes").
- The end anchor is chosen automatically; drag an end dot to set one by hand. Moving the box resets a custom end.
- Per-arrow thickness, line style (solid / dashed / dotted), color, label, and Reverse.
- Arrows that would run on top of each other are spread into parallel lanes so each stays visible (crossings are left alone).
- Boxes that are very close get a straight line; touching boxes show a small direction triangle on the shared border (tap it to select the arrow).

## Nodes

Small junction dots created by branching or joining arrows. They can be dragged, grouped and individually colored (Color menu). A node with no color of its own follows its arrow.

## Groups

- Dotted frames around boxes, nodes and other groups; groups nest, an item has one parent group.
- Make one from the selection (**Group > Group selected**) or by dragging a frame (**Group > Draw frame…**).
- Drag items in or out; the frame grows and shrinks live, and dropped items are placed in the nearest free spot.
- Arrows with both ends in a group belong to it. Select a frame to move it or resize it from its corners (clamped to its contents).
- **Select contents** and **Ungroup** are in the Group menu.
- **Tint:** select a frame and pick a swatch in the Color menu; Auto clears it.

## Selecting

Select menu:

- **All**, **All boxes**, **All arrows** (includes nodes), **Inverse selection**
- **By color**
- **Custom:** tap boxes, arrows and nodes to toggle them, then Done/Cancel
- **Lasso:** drag a freehand shape; everything whose center is inside is selected (replaces the current selection). A box's center is its middle, an arrow's is the midpoint of its path.

Shift-click multi-selects; `Ctrl+A` selects everything.

## Color

Swatch grid applies to the selected boxes, arrows and nodes. Add your own colors with the picker (**Add to colors**) and remove them again (**Remove custom colors**). The palette is saved inside each map file.

## Files and menus

- **File:** New (asks to save unsaved changes), Save… (file dialog), Load…, Undo, Redo, Install app.
- **View:** Reset view, Theme (light by default, dark, or follow system).
- **Box / Arrow / Group / Color:** actions on the current selection.
- The map autosaves in the browser (a note at the bottom says so). Saved files are plain JSON.

## Touch

One-finger drag on empty space pans, two fingers pinch and pan, double-tap empty space makes a box, double-tap a title/arrow/frame label renames it. Hit areas are larger on touch.

## Running

Open `index.html` in a browser, or serve the folder from any static web server for install/offline support.
