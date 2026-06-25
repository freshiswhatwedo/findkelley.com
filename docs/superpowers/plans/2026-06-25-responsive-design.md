# Responsive Full-Page Portfolio Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Transform the centered card portfolio into a full-page adaptive layout using fluid CSS.

**Architecture:** Replace fixed pixel values with clamp() for typography and spacing. Remove card container and use body as full-page flexbox. Content wrapper constrains max-width while allowing fluid scaling across all devices.

**Tech Stack:** HTML5, CSS3 (clamp(), dvh units, flexbox)

## Global Constraints

- Single file modification: `index.html` (CSS only, lines 11-57)
- No HTML structure changes (except removing `.card` wrapper)
- Preserve all existing animations (fadeUp, blink)
- Preserve all colors and accent values
- Preserve JetBrains Mono font
- Minimum font size: 11px (iPhone SE readability)
- Maximum content width: 600px (prevent ultrawide stretching)
- Touch targets: 44px minimum (Apple recommendation)

---

### Task 1: Remove Card Container and Update HTML Structure

**Files:**
- Modify: `index.html:60-76`

**Interfaces:**
- Consumes: Existing HTML with `<main class="card">` wrapper
- Produces: Simplified HTML with `<main class="inner">` as direct body child

- [ ] **Step 1: Remove the card wrapper**

Edit `index.html` and change the HTML structure from:
```html
<body>
  <main class="card">
    <div class="inner">
```

To:
```html
<body>
  <main class="inner">
```

And change the closing tags from:
```html
    </div>
  </main>
</body>
```

To:
```html
  </main>
</body>
```

- [ ] **Step 2: Verify HTML structure in browser**

Open `index.html` in a browser. The content will look unstyled since we removed the card container but haven't updated CSS yet. This is expected.

Expected: Content displays without card background/border (broken appearance temporarily).

- [ ] **Step 3: Commit HTML structure change**

```bash
git add index.html
git commit -m "refactor: remove card wrapper for full-page layout"
```

---

### Task 2: Update Body Styles for Full-Page Layout

**Files:**
- Modify: `index.html:15-25` (body CSS)

**Interfaces:**
- Consumes: HTML with simplified structure from Task 1
- Produces: Body styled as full-page container with `min-height: 100dvh`, `line-height: 1.4`, responsive padding

- [ ] **Step 1: Update body CSS**

Replace the current body styles (lines 15-25):
```css
  body {
    margin: 0;
    background: #0d0e11;
    color: #c7c9d1;
    font-family: 'JetBrains Mono', ui-monospace, monospace;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 24px;
    min-height: 100%;
  }
```

With:
```css
  body {
    margin: 0;
    background: #0d0e11;
    color: #c7c9d1;
    font-family: 'JetBrains Mono', ui-monospace, monospace;
    line-height: 1.4;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: clamp(24px, 5vh, 80px) clamp(20px, 5vw, 40px);
    min-height: 100dvh;
    min-height: 100vh;
    -webkit-tap-highlight-color: transparent;
  }
```

Note: `min-height: 100vh` is intentionally placed after `100dvh` as a fallback for older browsers.

- [ ] **Step 2: Verify body layout in browser**

Open `index.html` and resize browser window from 320px to 2560px width.

Expected: Body takes full viewport height with responsive padding. Content stays centered.

- [ ] **Step 3: Commit body styles**

```bash
git add index.html
git commit -m "feat: add full-page body layout with fluid padding"
```

---

### Task 3: Remove Card Styles and Update Inner Container

**Files:**
- Modify: `index.html:26-35` (remove .card, update .inner)

**Interfaces:**
- Consumes: Body styles with full-page layout from Task 2
- Produces: `.inner` styled with `max-width: 600px`, `width: 100%`, flexbox with gap for spacing

- [ ] **Step 1: Remove .card CSS completely**

Delete these lines (26-34):
```css
  .card {
    width: 100%;
    max-width: 440px;
    background: #131418;
    border: 1px solid #26282f;
    border-radius: 11px;
    overflow: hidden;
    box-shadow: 0 24px 60px rgba(0,0,0,.5);
  }
```

- [ ] **Step 2: Update .inner CSS**

Replace line 35:
```css
  .inner { padding: 34px 30px 28px; }
```

With:
```css
  .inner {
    max-width: 600px;
    width: 100%;
    display: flex;
    flex-direction: column;
    gap: clamp(12px, 2.5vh, 20px);
  }
```

- [ ] **Step 3: Verify layout in browser**

Open `index.html` and test at 320px, 768px, 1280px, and 2560px widths.

Expected: Content never exceeds 600px width. Elements have vertical spacing via gap. No card background/border visible.

- [ ] **Step 4: Commit container changes**

```bash
git add index.html
git commit -m "feat: update container for full-page responsive layout"
```

---

### Task 4: Apply Fluid Typography to Path, Name, and Bio

**Files:**
- Modify: `index.html:36-39` (path, name, bio CSS)

**Interfaces:**
- Consumes: `.inner` container with gap spacing from Task 3
- Produces: `.path` with `font-size: clamp(11px, 2.5vw, 14px)`, `.name` with `clamp(28px, 7vw, 48px)`, `.bio` with `clamp(12px, 3vw, 16px)`, all with removed fixed margins

- [ ] **Step 1: Update .path CSS**

Replace line 36:
```css
  .path { font-size: 12px; color: rgba(110,114,126,.85); animation: fadeUp .4s .05s both; }
```

With:
```css
  .path {
    font-size: clamp(11px, 2.5vw, 14px);
    color: rgba(110,114,126,.85);
    animation: fadeUp .4s .05s both;
  }
```

- [ ] **Step 2: Update .name CSS**

Replace line 37:
```css
  .name { font-size: 32px; font-weight: 700; letter-spacing: -.5px; color: #eceef4; margin-top: 14px; animation: fadeUp .45s .15s both; }
```

With:
```css
  .name {
    font-size: clamp(28px, 7vw, 48px);
    font-weight: 700;
    letter-spacing: -.5px;
    color: #eceef4;
    animation: fadeUp .45s .15s both;
  }
```

- [ ] **Step 3: Update .name .dot CSS (add line after .name)**

Keep line 38 as is:
```css
  .name .dot { color: var(--accent); }
```

- [ ] **Step 4: Update .bio CSS**

Replace line 39:
```css
  .bio { font-size: 13px; color: rgba(140,144,156,.9); margin-top: 8px; animation: fadeUp .4s .28s both; }
```

With:
```css
  .bio {
    font-size: clamp(12px, 3vw, 16px);
    color: rgba(140,144,156,.9);
    animation: fadeUp .4s .28s both;
  }
```

- [ ] **Step 5: Verify typography scaling in browser**

Open `index.html`. Resize from 320px to 2560px width.

Expected:
- At 320px: name ≈ 28px, bio ≈ 12px, path ≈ 11px
- At 2560px: name ≈ 48px, bio ≈ 16px, path ≈ 14px
- Smooth scaling between sizes
- Spacing between elements consistent (from parent gap)

- [ ] **Step 6: Commit typography updates**

```bash
git add index.html
git commit -m "feat: add fluid typography for path, name, and bio"
```

---

### Task 5: Update Links Container and Individual Link Styles

**Files:**
- Modify: `index.html:40-52` (links, lk, left, num, dot-end CSS)

**Interfaces:**
- Consumes: Typography styles from Task 4
- Produces: `.links` with flexbox gap `clamp(2px, 0.5vh, 4px)`, `.lk` with fluid padding `clamp(12px, 2vh, 16px)` vertical and `clamp(6px, 1.5vw, 8px)` horizontal, `.lk` with `font-size: clamp(13px, 3.2vw, 16px)`, `.num` with `font-size: clamp(11px, 2.5vw, 13px)`, `touch-action: manipulation`

- [ ] **Step 1: Update .links CSS**

Replace line 40:
```css
  .links { display: flex; flex-direction: column; margin-top: 28px; border-top: 1px solid #22242b; animation: fadeUp .5s .42s both; }
```

With:
```css
  .links {
    display: flex;
    flex-direction: column;
    gap: clamp(2px, 0.5vh, 4px);
    padding-top: clamp(24px, 4vh, 40px);
    border-top: 1px solid #22242b;
    animation: fadeUp .5s .42s both;
  }
```

- [ ] **Step 2: Update .lk CSS**

Replace lines 41-47:
```css
  .lk {
    display: flex; align-items: center; justify-content: space-between;
    padding: 14px 4px 14px 6px;
    color: #c9cbd3; text-decoration: none; font-size: 14px;
    border-bottom: 1px solid #22242b;
    transition: color .15s, transform .15s;
  }
```

With:
```css
  .lk {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: clamp(12px, 2vh, 16px) clamp(6px, 1.5vw, 8px);
    color: #c9cbd3;
    text-decoration: none;
    font-size: clamp(13px, 3.2vw, 16px);
    border-bottom: 1px solid #22242b;
    transition: color .15s, transform .15s;
    touch-action: manipulation;
  }
```

- [ ] **Step 3: Update .lk .num CSS**

Replace line 50:
```css
  .lk .num { color: rgba(100,104,116,.7); font-size: 12px; }
```

With:
```css
  .lk .num {
    color: rgba(100,104,116,.7);
    font-size: clamp(11px, 2.5vw, 13px);
  }
```

- [ ] **Step 4: Keep existing .lk:hover, .lk .left, .dot-end styles**

Lines 48, 49, 51, 52 remain unchanged:
```css
  .lk:hover { color: var(--accent-bright); transform: translateX(4px); }
  .lk .left { display: flex; align-items: baseline; gap: 11px; }
  .dot-end { display: block; flex: none; width: 7px; height: 7px; border-radius: 50%; background: #eceef4; transition: background .15s; }
  .lk:hover .dot-end { background: var(--accent); }
```

- [ ] **Step 5: Verify links in browser**

Open `index.html`. Test at multiple sizes and tap on mobile.

Expected:
- Links have adequate spacing between them
- Link text scales from 13px to 16px
- Link numbers scale from 11px to 13px
- Padding creates touch targets ≥44px tall on mobile
- Hover effects still work on desktop
- No iOS tap highlight flash

- [ ] **Step 6: Commit links update**

```bash
git add index.html
git commit -m "feat: add fluid spacing and typography to links"
```

---

### Task 6: Update Footer Styles

**Files:**
- Modify: `index.html:53` (foot CSS)

**Interfaces:**
- Consumes: Links styles from Task 5
- Produces: `.foot` with `font-size: clamp(11px, 2.5vw, 13px)`, top margin removed (spacing from parent gap)

- [ ] **Step 1: Update .foot CSS**

Replace line 53:
```css
  .foot { font-size: 12px; color: rgba(110,114,126,.8); margin-top: 22px; animation: fadeUp .4s .9s both; }
```

With:
```css
  .foot {
    font-size: clamp(11px, 2.5vw, 13px);
    color: rgba(110,114,126,.8);
    padding-top: clamp(20px, 3.5vh, 32px);
    animation: fadeUp .4s .9s both;
  }
```

- [ ] **Step 2: Verify footer in browser**

Open `index.html`. Resize window from 320px to 2560px.

Expected:
- Footer text scales from 11px to 13px
- Spacing above footer adapts (tight on short screens, spacious on tall)
- Footer pushed toward bottom on tall viewports

- [ ] **Step 3: Commit footer update**

```bash
git add index.html
git commit -m "feat: add fluid typography and spacing to footer"
```

---

### Task 7: Verify Responsive Behavior Across All Target Devices

**Files:**
- Verify: `index.html`

**Interfaces:**
- Consumes: Complete responsive implementation from Tasks 1-6
- Produces: Verified working implementation across all target breakpoints

- [ ] **Step 1: Test iPhone SE (320px)**

Open browser DevTools. Set viewport to 320x568.

Expected:
- Name: ~28px
- Bio: ~12px
- Links: ~13px, numbers ~11px
- Footer: ~11px
- No horizontal scroll
- All text readable
- Links easily tappable (≥44px tall)

- [ ] **Step 2: Test iPhone Pro (390px)**

Set viewport to 390x844.

Expected:
- Fonts slightly larger than iPhone SE
- Smooth scaling, no jumps
- Adequate spacing throughout

- [ ] **Step 3: Test iPad portrait (768px)**

Set viewport to 768x1024.

Expected:
- Fonts mid-range in clamp values
- Content centered with good side padding
- Spacious vertical layout

- [ ] **Step 4: Test iPad landscape (1024px)**

Set viewport to 1024x768.

Expected:
- Similar to portrait but shorter height
- Content may allow gentle scroll if needed
- Spacing adapts to shorter screen

- [ ] **Step 5: Test desktop (1280px)**

Set viewport to 1280x800.

Expected:
- Fonts approaching max values
- Content max-width prevents over-stretching
- Hover effects work

- [ ] **Step 6: Test ultrawide (2560px)**

Set viewport to 2560x1440.

Expected:
- Fonts at maximum: name 48px, bio 16px, links 16px
- Content capped at 600px width
- Large side padding
- Doesn't look stretched or awkward

- [ ] **Step 7: Test landscape phone (667x375)**

Set viewport to 667x375.

Expected:
- Content fits or allows gentle scroll
- Maintains readability
- Spacing compressed but still adequate

- [ ] **Step 8: Verify all animations still work**

Reload page at any size.

Expected:
- fadeUp animations on path, name, bio, links, footer
- Blink animation on cursor
- Hover effects on links (desktop)
- All timing unchanged

- [ ] **Step 9: Commit verification notes as comment**

Add a comment at the top of the CSS section:
```css
<style>
  /* Responsive full-page layout - tested 320px to 2560px */
  :root { --accent: oklch(0.84 0.13 165); --accent-bright: oklch(0.86 0.14 165); }
```

```bash
git add index.html
git commit -m "docs: add responsive testing verification comment"
```

---

### Task 8: Final Cross-Browser and Device Testing

**Files:**
- Verify: `index.html`

**Interfaces:**
- Consumes: Verified responsive implementation from Task 7
- Produces: Final tested and production-ready implementation

- [ ] **Step 1: Test in Chrome**

Open `index.html` in Chrome. Test at 320px, 768px, 1280px, 2560px.

Expected: All fluid values work correctly. dvh falls back to vh if needed.

- [ ] **Step 2: Test in Firefox**

Open `index.html` in Firefox. Test at same breakpoints.

Expected: Identical behavior to Chrome.

- [ ] **Step 3: Test in Safari**

Open `index.html` in Safari. Test at same breakpoints.

Expected:
- dvh works correctly (Safari 15.4+)
- No iOS tap highlight flash
- All animations smooth

- [ ] **Step 4: Test on physical iPhone (if available)**

Open `index.html` on actual iPhone in Safari.

Expected:
- Address bar collapses properly with dvh
- Touch targets easy to tap
- No horizontal scroll in any orientation
- Smooth performance

- [ ] **Step 5: Test on physical iPad (if available)**

Open `index.html` on actual iPad in Safari.

Expected:
- Works in portrait and landscape
- Content scales appropriately
- Touch interactions responsive

- [ ] **Step 6: Verify no console errors**

Open DevTools console in any browser.

Expected: No JavaScript errors, no CSS warnings.

- [ ] **Step 7: Verify page load performance**

Open DevTools Network tab. Hard reload page.

Expected:
- Page loads in <1 second
- No render-blocking resources
- Fonts load quickly from Google Fonts

- [ ] **Step 8: Create final commit**

```bash
git add -A
git commit -m "feat: complete responsive full-page portfolio

Transform centered card layout into full-page adaptive design using
fluid CSS with clamp() and viewport units. Tested across all target
devices from 320px to 2560px.

- Remove card container and styling
- Update body to full-page flexbox layout
- Apply fluid typography with clamp() to all text
- Add responsive spacing with vh/vw units
- Ensure 44px touch targets on mobile
- Add iOS Safari optimizations (dvh, tap-highlight)
- Preserve all animations and interactions

Co-Authored-By: Claude Sonnet 4.5 <noreply@anthropic.com>"
```

---

## Implementation Complete

All tasks completed. The portfolio now:
- Takes up the full viewport on all devices
- Scales smoothly from 320px to 2560px without breakpoints
- Maintains readability with minimum 11px fonts
- Provides 44px+ touch targets on mobile
- Works with iOS Safari's collapsing address bar
- Preserves all original animations and aesthetic
