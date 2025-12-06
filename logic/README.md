# Logic File Patches

This directory contains unified diff patches for RTL (right-to-left) coordinate adjustments to logic files.

## How It Works

During translation import (`npm run import-translations`), these patches are automatically applied to adjust object positions, movements, and animations for Hebrew RTL display.

## Current Patches

- **1.agilogic.patch** - Title sequence animation (RTL mirrored coordinates)
- **140.agilogic.patch** - Opening credits scene (RTL coordinate adjustments only)

## Patch Format

Patches use standard unified diff format (`.patch` files):
- Applied automatically during `npm run import-translations`
- Applied to `final/src/logic/` after copying from `tmp/src/logic/`
- **Only coordinate adjustments** (RTL positioning)
- **NO message translations** - those are handled separately via `messages.json`

## Creating New Patches

When you need to adjust coordinates for a new logic file:

### 1. Make Your Changes

Edit the logic file in `project/final/src/logic/` after running import:

```bash
npm run import-translations
# Edit project/final/src/logic/N.agilogic manually
# Adjust position(), move.obj(), add.to.pic() coordinates for RTL
```

### 2. Generate the Patch

```bash
diff -u project/tmp/src/logic/N.agilogic \
        project/final/src/logic/N.agilogic \
        > translations/sq2/logic/N.agilogic.patch
```

### 3. Test

```bash
npm run import-translations
# Verify patches apply correctly

npm run build
# Test in ScummVM
```

### 4. Commit

```bash
git add translations/sq2/logic/N.agilogic.patch
git commit -m "Add RTL patches for logic N"
```

## What Gets Patched

**Commands affected by RTL coordinate changes:**
- `position(obj, x, y)` - Object positioning
- `move.obj(obj, x, y, ...)` - Object movement
- `add.to.pic(view, loop, cel, x, y, ...)` - Picture additions
- `posn(obj, x1, y1, x2, y2)` - Position checks

**X-axis transformation:**
Typically: `RTL_X = 160 - LTR_X` (screen width varies by room)

**Not patched:**
- Message translations (use `translations/sq2/messages.json`)
- Variable coordinates (e.g., `position.v(o0, v70, v71)`)
- Y-axis coordinates (unchanged)

## Notes

- Patches must match the indexed source files in `project/tmp/src/logic/`
- If upstream changes occur, patches may need regeneration
- Use `-p3` strip level (handled automatically by apply-patches.ts)
