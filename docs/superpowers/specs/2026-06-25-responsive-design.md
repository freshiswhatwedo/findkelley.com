# Responsive Full-Page Portfolio Design

**Date:** 2026-06-25
**Project:** findkelley.com
**Status:** Approved

## Overview

Transform the existing centered card portfolio into a full-page adaptive layout that works seamlessly across all devices and screen sizes using modern fluid CSS techniques.

## Goals

- Make the portfolio take up the entire viewport instead of being a centered card
- Equal priority across all breakpoints (mobile, tablet, desktop)
- Smooth scaling without breakpoint jumps
- Maintain readability and usability on all devices
- Preserve existing aesthetic and animations

## Current State

- Single HTML file (`index.html`) with inline styles
- Centered card layout with fixed `max-width: 440px`
- Fixed pixel values for all typography and spacing
- Basic viewport meta tag present
- JetBrains Mono font, dark theme, animated entrance effects

## Design Approach

### 1. Layout Architecture

**Full-page transformation:**
- Remove `.card` wrapper element from HTML (main element)
- Remove `.card` CSS rules (background, border, border-radius, box-shadow)
- Body becomes the full-page container using `min-height: 100dvh` for proper mobile browser handling
- Body uses flexbox with `flex-direction: column`, `align-items: center`, `justify-content: center`
- Body gets responsive padding: `clamp(24px, 5vh, 80px) clamp(20px, 5vw, 40px)`
- `.inner` container gets `max-width: 600px` and `width: 100%` to prevent excessive stretching
- `.inner` uses flexbox `display: flex`, `flex-direction: column`, `gap` for spacing between sections

**Scrolling strategy:**
- Allow natural scrolling on short screens (landscape phones, small devices)
- Minimum font sizes in clamp() ensure readability
- No force-fitting that compromises usability

### 2. Typography System

**Fluid font sizing using clamp():**

| Element | clamp() Value | Purpose |
|---------|---------------|---------|
| Path (`~ / soitskelley`) | `clamp(11px, 2.5vw, 14px)` | Subtle header, stays readable |
| Name (`soitskelley.`) | `clamp(28px, 7vw, 48px)` | Hero element, significant scaling |
| Bio | `clamp(12px, 3vw, 16px)` | Balances with name |
| Link text | `clamp(13px, 3.2vw, 16px)` | Touch-friendly sizing |
| Link numbers (01, 02...) | `clamp(11px, 2.5vw, 13px)` | Proportional to links |
| Footer | `clamp(11px, 2.5vw, 13px)` | Subtle like path |

**Typography principles:**
- Minimum values ensure readability on iPhone SE (320px width)
- Maximum values prevent oversized text on ultrawide monitors
- Viewport width (vw) creates smooth scaling
- Consistent ratios between elements maintained across all sizes
- Add `line-height: 1.4` to body for better mobile readability

### 3. Spacing System

**Vertical spacing with viewport units:**

| Spacing | clamp() Value | Behavior |
|---------|---------------|----------|
| Path to name | `clamp(12px, 2.5vh, 20px)` | Tightens on short screens |
| Name to bio | `clamp(6px, 1.5vh, 12px)` | Keeps grouped |
| Bio to links | `clamp(24px, 4vh, 40px)` | Main separator |
| Between links | `gap: clamp(2px, 0.5vh, 4px)` | Tight group |
| Links to footer | `clamp(20px, 3.5vh, 32px)` | Pushes down on tall screens |

**Horizontal spacing:**
- Content padding: `clamp(20px, 5vw, 40px)` left/right
- Link padding: `clamp(12px, 2vh, 16px)` vertical, `clamp(6px, 1.5vw, 8px)` horizontal

**Implementation:**
- `.inner` uses flexbox with individual gaps for each section (path, name, bio, links, footer)
- Use CSS custom properties for spacing values to keep them maintainable
- `.links` uses flexbox with `gap: clamp(2px, 0.5vh, 4px)` for vertical spacing between link items
- Individual `.lk` elements get updated padding using clamp()
- Remove all fixed margins from child elements - spacing controlled by parent's gap property

### 4. Component Adaptations

**Touch targets:**
- Links expand to minimum 44px tall on mobile (Apple's recommendation)
- Padding values ensure adequate touch areas via clamp()
- Existing hover effects (color change + translateX) preserved for desktop

**Animations:**
- All existing animations stay intact (fadeUp, blink cursor)
- Animation timing and delays unchanged
- Viewport-independent, work at any size

**Visual elements:**
- Remove card-specific styles: `background: #131418`, `border`, `border-radius`, `box-shadow`
- Keep body `background: #0d0e11` as main background
- Preserve all accent colors and dot styling
- Border-top on links section remains for visual separation

### 5. Mobile & iOS Optimization

**iOS Safari handling:**
- Use `100dvh` (dynamic viewport height) for collapsing address bar
- Fallback to `vh` automatic in older browsers
- Add `-webkit-tap-highlight-color: transparent` to remove tap flash
- Add `touch-action: manipulation` on links for faster tap response

**Cross-device testing targets:**
- iPhone SE (320px) - smallest modern phone
- iPhone Pro (390px) - standard phone
- iPad (768px) - tablet portrait
- iPad landscape (1024px) - tablet landscape
- Desktop (1280px+) - standard desktop
- Ultrawide (1920px+) - large monitors

### 6. Browser Support & Fallbacks

**Modern CSS features:**
- `clamp()` - supported in all modern browsers (Safari 13.1+, Chrome 79+, Firefox 75+)
- `dvh` units - Safari 15.4+, Chrome 108+, Firefox 101+
- Automatic fallback to `vh` in older browsers

**Fallback strategy:**
```css
@supports not (height: clamp(1px, 1vh, 1px)) {
  /* Pixel fallbacks for ancient browsers */
}
```

## Success Criteria

- Portfolio displays correctly on screens from 320px to 2560px width
- Text remains readable at all sizes (minimum 11px, never exceeds maximums)
- Touch targets meet 44px minimum on mobile devices
- No horizontal scrolling on any device
- Smooth scaling without jarring breakpoint jumps
- All animations and interactions work across devices
- Page loads and renders in under 1 second

## Implementation Notes

**File changes:**
- Single file modification: `index.html` (CSS section only)
- No new files, no external CSS
- No JavaScript required

**Testing approach:**
- Browser DevTools responsive mode for quick testing
- Physical device testing on iPhone and iPad recommended
- Test landscape and portrait orientations
- Verify at extreme sizes (320px, 2560px)

**Preservation requirements:**
- Keep all existing functionality
- Maintain dark theme aesthetic
- Preserve animations and timing
- Keep JetBrains Mono font
- No changes to HTML structure or content
- All external links remain unchanged

## Out of Scope

- Adding new features or content
- Changing color scheme or branding
- Adding JavaScript interactivity
- Creating separate mobile/desktop versions
- Performance optimization beyond CSS changes
- Analytics or tracking implementation
