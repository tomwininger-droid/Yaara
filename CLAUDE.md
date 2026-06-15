# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**SocialPortfolio_v2.html** is a single-file responsive portfolio website for a social media manager (Yaara Zin / Alex Rivera). It's a self-contained HTML file with embedded CSS and JavaScript — no build process, dependencies, or assets needed beyond images and videos referenced in the HTML.

## Key Architecture

### Structure
- **Single HTML file** containing all markup, styling, and interactivity
- **CSS Design Token System**: Color palette, typography, and spacing defined via CSS custom properties (variables) at `:root` — see the `/* ── TOKENS ──` section for the full list
- **JavaScript Initialization**: All interactivity is initialized via `initAll()` which calls modular functions (`initCursor()`, `initNav()`, etc.)

### Main Sections
1. **Preloader** — Animated loading screen that displays for 1.2s
2. **Navigation** — Fixed header with scroll detection (adds `.scrolled` class to change styling)
3. **Hero** — Full-viewport section with portrait image (top-right), centered headline with typing text effect
4. **Portfolio Grid** — Two categories: "Posts" (Instagram-style cards) and "Reels" (video cards with stats)
5. **Differentiators** ("Why Choose Me") — Four-item grid with icons
6. **About** — Bio, skills grid, and tools/platforms
7. **CTA** — Call-to-action section
8. **Footer** — Links and social
9. **Modal** — Detailed case study view triggered by clicking portfolio cards

## Key Content to Update

### Personal Information
- **Lines 6, 1021**: Title and footer name
- **Line 1014**: Email address (currently `mailto:alex@socialbyalex.com`)
- **Lines 1024-1027**: Social media links (Instagram, LinkedIn, TikTok, X)

### Portfolio Projects
- **Lines 1054-1059**: JavaScript `projects` array contains all case study data (platform, title, subtitle, stats, challenge, strategy, tactics, tags)
- Each card in the grid has a `data-project` attribute pointing to the index in the `projects` array
- Video paths: e.g., `src="images/reel1.MP4"` — images and videos go in an `images/` folder (not in repo; reference only)

### Hero Section
- **Lines 121-147**: Portrait image (top-right corner) — controlled by CSS positioning
- **Line 161-200**: Headline typography and animation
- **Lines 1089-1101**: Typing text phrases — modify the `phrases` array to change rotating text

### Design Tokens
Modify colors in the `:root` CSS variable block (lines 12-35):
- `--pink`, `--pink-hi`, `--pink-lt`: Primary brand colors
- `--bg`, `--bg2`, `--bg3`: Background gradients
- `--ink`, `--ink-dim`, `--ink-xs`: Text colors
- `--serif`, `--sans`, `--cond`, `--hand`, `--mono`: Font families

## JavaScript Functions

- **`initCursor()`** — Custom cursor that scales on hover over interactive elements; disabled on touch devices
- **`initNav()`** — Adds `.scrolled` class to nav when user scrolls past 60px; enables smooth-scroll anchors
- **`initTyping()`** — Rotates through phrases with character-by-character typing effect
- **`initReveal()`** — Uses IntersectionObserver to trigger `.revealed` class on elements with `.reveal` class when scrolled into view
- **`initModals()`** — Wires up project card clicks to open modal; handles close button, overlay click, and Escape key
- **`initVideoHover()`** — Auto-plays videos on card hover, resets on leave
- **`openModal(p)` / `closeModal()`** — Modal state management and content population

## Animation System

- **Reveal animations**: Elements with `.reveal` class fade/slide in on scroll (CSS transitions triggered by `.revealed` class)
- **Delay staggering**: `.reveal-delay-1`, `.reveal-delay-2`, `.reveal-delay-3` add sequential delays
- **Preloader**: Animated bar fills over 1.2s, then page fades in
- **Typing effect**: Character-by-character animation in `initTyping()` with 2-second pause between phrases
- **Cursor tracking**: Mouse position interpolated at 12% per frame for smooth following motion

## Image References

The HTML references images that are not in the repo (must be added):
- `images/person.jpg` — About section portrait (or placeholder div with initials if image fails to load)
- `images/reel1.MP4`, etc. — Video files for portfolio cards (can be omitted; videos will fail silently if missing)

If images are missing, the `onerror` attribute on the `<img>` tag shows a fallback placeholder (see line 980-981).

## Customization Patterns

**Changing a color**: Update the CSS variable at `:root`, e.g., `--pink: #FF00FF;`

**Adding a new project**: Add a new object to the `projects` array (lines 1053-1059), then add a card in the HTML with `data-project="[index]"`.

**Adjusting animations**: 
- Scroll reveal threshold: Line 1126 `{threshold:0.1,...}`
- Typing speed: Lines 1098 (`50ms` for delete, `95ms` for type)
- Cursor follow speed: Line 1108 (currently `0.12` — increase for looser follow, decrease for snappier)

**Responsive design**: Uses `clamp()` for fluid typography, CSS Grid with auto-fit, and `@media` queries. No mobile-specific breakpoints hardcoded — scales smoothly from mobile to desktop.

## Quick Start & Development

**No build process** — this is production-ready. Just open the HTML file in a browser:
```bash
# macOS
open SocialPortfolio_v2.html

# Or use a simple HTTP server for local development
python3 -m http.server 8000
# Then visit http://localhost:8000/SocialPortfolio_v2.html
```

**Why use a server?** Ensures video autoplay and smooth scrolling work correctly. Direct file:// access may block video playback on some browsers.

## Common Edits Cheat Sheet

| Task | Location | What to Change |
|------|----------|----------------|
| Update email | Line 1014 | `mailto:alex@socialbyalex.com` |
| Change name | Lines 1021, 6 | "Alex Rivera" → your name |
| Update social links | Lines 1024–1027 | `href="#"` → actual URLs |
| Change primary color | Line 19 | `--pink: #E8006F;` → your color |
| Change brand fonts | Lines 30–34 | Font family names in `:root` |
| Update typing phrases | Lines 1092 | `phrases` array in JavaScript |
| Add a new project | Lines 1053–1059 | Add object to `projects` array, then add card with `data-project="[index]"` |
| Replace portrait image | Line 980 | `src="images/person.jpg"` |
| Add portfolio videos | Lines 907, 925, 943, etc. | `src="images/reel#.MP4"` |

## Testing & Verification

After making changes, test:
- **Desktop**: Full-width display, nav scrolling behavior, modal opens/closes, cursor effect
- **Tablet**: Touch interactions work, layout reflows correctly
- **Mobile**: No horizontal scroll, buttons are tappable, videos scale appropriately
- **Video playback**: Check that videos autoplay on hover (desktop) and play on tap (mobile)
- **Animation**: Scroll through page and verify reveal animations trigger correctly
- **Modal**: Click each portfolio card to verify the correct case study data loads

Quick test commands:
```bash
# Open in default browser
open SocialPortfolio_v2.html

# Test with local server (for video support)
python3 -m http.server 8000 &
# Then visit http://localhost:8000/SocialPortfolio_v2.html in Chrome, Safari, Firefox
```

## Asset Requirements

### Images
- **`images/person.jpg`**: Portrait photo (recommended 280×340px, ~100KB). Falls back to placeholder div with initials if missing.
- Fallback behavior: See line 980 — `onerror` attribute shows initials "AR" if image fails to load.

### Videos
- **Format**: MP4 (H.264 video codec, AAC audio) for broad browser support
- **Duration**: 5–15 seconds (autoplays silently on hover)
- **Dimensions**: Recommend 1080×1920 (vertical, for TikTok-style format)
- **File size**: Keep under 10MB per video for fast loading
- **Path pattern**: `images/reel1.MP4`, `images/reel2.MP4`, etc.
- **Fallback**: If video fails to load, the card still displays correctly with a static thumbnail from CSS background

## Section-by-Section Reference

| Section | Start (approx.) | Key Classes | Purpose |
|---------|-----------------|-------------|---------|
| Hero | Line 300 | `.hero`, `.hero-headline` | Full-viewport intro with portrait & typing effect |
| Posts Grid | Line 650 | `.posts-grid`, `.post-card` | Instagram-style portfolio pieces |
| Reels Grid | Line 850 | `.reels-grid`, `.reel-card` | Video portfolio with stats overlays |
| Why Choose Me | Line 961 | `.diff`, `.diff-item` | Four-column differentiator section |
| About | Line 975 | `.about`, `.skills-grid` | Bio, skills, and tools |
| CTA | Line 1009 | `.cta` | Call-to-action with email & CV links |
| Modal | Line 1032 | `.modal-overlay`, `.modal` | Case study detail view |

## Debugging Checklist

**Videos not playing?**
- Confirm files exist at `images/reel[N].MP4`
- Test with a local HTTP server (see Quick Start)
- Check browser console for 404 errors

**Animations not triggering?**
- Scroll through page — reveal animations fire on scroll in-view
- Check DevTools: verify `.reveal` class is present on elements
- Threshold is `0.1` (elements must be 10% visible) — adjust at line 1126 if needed

**Modal won't close?**
- Verify `#modalClose` button is not hidden (check CSS at line 623)
- Test Escape key and overlay click — both should close modal
- Check DevTools console for JavaScript errors

**Custom cursor not showing?**
- Disabled on touch devices (intentional) — test on desktop only
- Check if touch is detected (line 1105) — temporarily comment out if testing on desktop with touch input
- Verify `#cursor` element exists in HTML

**Responsive layout broken?**
- Check viewport meta tag (line 5) is not removed
- Verify CSS Grid/Flexbox values in media queries
- Test with browser zoom (Ctrl/Cmd + +)

## Deployment

This file is deployment-ready as-is. Simply upload to any web host:
```bash
# Example: Deploy to a static hosting service
# 1. Upload SocialPortfolio_v2.html to your hosting provider
# 2. Create an images/ folder and upload images/person.jpg and reel[N].MP4 files
# 3. Set the file to be served as text/html (most hosts do this by default)
```

**Hosting options**: Netlify, Vercel, GitHub Pages, AWS S3, or any standard web server.

**Domain + SSL**: Works with HTTPS (recommended) and custom domains — no special configuration needed.
