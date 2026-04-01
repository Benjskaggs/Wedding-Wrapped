# iOS Safe Area Bleeding Issue - Case Study

## The Problem

On iOS devices with notches (iPhone X and later), the website displays a **solid light blue bar (#F0F8FF)** in the safe area regions (top notch/status bar area and bottom home indicator area) instead of allowing the background and content to "bleed" edge-to-edge.

### Affected Areas:
1. **Main Page** - The gradient background and floating hearts should extend into the notch/home indicator areas
2. **Gallery View** - Photos should be visible when scrolling into the safe area regions
3. **Wrapped Summary Page** - Cards should not have a hard cutoff at the safe area boundary

## Current Implementation Analysis

### What's Currently Set (index.html)

```html
<!-- Line 5: Viewport with cover mode - CORRECT -->
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">

<!-- Line 7: Status bar style - POTENTIALLY PROBLEMATIC -->
<meta name="apple-mobile-web-app-status-bar-style" content="default">
```

```css
/* Lines 19-25: Body background - THE VISIBLE COLOR IN SAFE AREA */
html, body {
    height: 100%;
    margin: 0;
    padding: 0;
    background: #F0F8FF;  /* <-- This is the light blue showing in safe area */
    overflow: hidden;
}

/* Lines 33-41: Main container - CREATES PADDING THAT PREVENTS BLEEDING */
.wrapped-container {
    display: flex;
    flex-direction: column;
    height: 100vh;
    height: -webkit-fill-available;
    padding: env(safe-area-inset-top) env(safe-area-inset-right) env(safe-area-inset-bottom) env(safe-area-inset-left);
    background: transparent;
    overflow: hidden;
}

/* Lines 898-905: Bleed layer - DOES NOT EXTEND INTO SAFE AREA */
#bleed-layer {
    position: absolute;
    inset: 0;
    z-index: 0;
    pointer-events: none;
    overflow: visible;
}

/* Lines 908-916: Floating hearts - TRIES TO EXTEND BUT PARENT IS CONSTRAINED */
.floating-hearts {
    position: absolute;
    top: calc(-100vh - env(safe-area-inset-top));
    left: 0;
    width: 100%;
    height: calc(300vh + env(safe-area-inset-top) + env(safe-area-inset-bottom));
    pointer-events: none;
    overflow: visible;
}
```

## Root Cause Analysis

### The Chain of Problems:

1. **`viewport-fit=cover`** is set correctly - this ALLOWS content to extend into safe areas
2. **BUT** `html` and `body` have `background: #F0F8FF` which fills the ENTIRE viewport including safe areas
3. **AND** `.wrapped-container` has `padding: env(safe-area-inset-*)` which pushes ALL content away from the edges
4. **RESULT**: The body background (#F0F8FF) shows through in the safe area regions because content is padded away

### Visual Diagram:

```
┌─────────────────────────────┐
│     SAFE AREA (notch)       │ ← Shows #F0F8FF (body background)
│─────────────────────────────│
│                             │
│   CONTENT AREA              │ ← Where your actual content lives
│   (padded by env())         │
│                             │
│─────────────────────────────│
│   SAFE AREA (home bar)      │ ← Shows #F0F8FF (body background)
└─────────────────────────────┘
```

### What Should Happen:

```
┌─────────────────────────────┐
│   ❤️   HEARTS/BG    ❤️      │ ← Background + hearts extend here
│─────────────────────────────│
│                             │
│   CONTENT AREA              │ ← Interactive content still padded
│   (text, buttons, etc)      │
│                             │
│─────────────────────────────│
│   ❤️   HEARTS/BG    ❤️      │ ← Background + hearts extend here
└─────────────────────────────┘
```

## Key Questions for Diagnosis

1. **Is `viewport-fit=cover` being respected?**
   - Test: Do `env(safe-area-inset-*)` values return non-zero on notched devices?

2. **Is the status bar style affecting transparency?**
   - `default` = white/light status bar with dark text
   - `black-translucent` = transparent status bar, content shows through

3. **Is iOS creating a separate layer for safe area fill?**
   - Sometimes iOS Safari fills safe areas with a color derived from the page

4. **Is the background actually extending, but being covered?**
   - The `#bleed-layer` might be extending but something is occluding it

5. **Are there iOS-specific compositing/layering issues?**
   - Fixed positioning + transforms can cause layer clipping on iOS

---

## Diagnostic Test Page

See `safe-area-diagnostic.html` for a standalone test page that will help identify exactly what's happening.

---

## Potential Solutions to Investigate

### Solution A: Status Bar Style Change
```html
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
```
This makes the status bar area transparent instead of filled.

### Solution B: Separate Background Layer
Create a truly fixed background that ignores safe area padding:
```css
.background-layer {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    /* No env() padding - extends edge to edge */
}

.content-layer {
    position: relative;
    padding: env(safe-area-inset-top) env(safe-area-inset-right) env(safe-area-inset-bottom) env(safe-area-inset-left);
}
```

### Solution C: Negative Margins on Background Elements
```css
#bleed-layer {
    position: fixed;
    top: calc(-1 * env(safe-area-inset-top));
    left: calc(-1 * env(safe-area-inset-left));
    right: calc(-1 * env(safe-area-inset-right));
    bottom: calc(-1 * env(safe-area-inset-bottom));
}
```

### Solution D: Gradient on HTML/Body
```css
html {
    /* Match the visual background so safe area doesn't look different */
    background: linear-gradient(to bottom, #your-gradient);
}
```

### Solution E: Investigate iOS PWA Mode
If added to home screen, different rules may apply. Test in:
- Safari browser
- Safari with `apple-mobile-web-app-capable` (PWA mode)
- Chrome iOS

---

## Files to Reference

- `index.html` - All CSS is inline, starting at line 12
- Key sections:
  - Lines 5-7: Meta tags
  - Lines 19-25: html/body styles
  - Lines 33-41: .wrapped-container styles
  - Lines 898-916: Bleed layer and floating hearts
  - Lines 780-788: Summary content styles
  - Lines 639-648: Gallery content styles

---

## Test Environment Requirements

To properly diagnose, you need:
1. A physical iPhone with a notch (X, 11, 12, 13, 14, 15, 16 series)
2. Safari browser (or Chrome iOS for comparison)
3. The ability to inspect elements (Safari Web Inspector connected to Mac)
4. The diagnostic test page running locally or on a server
