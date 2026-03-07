# Landing Page Redesign - Design Document

## Problem

The current killscale.com landing page feels dull and muted. Pale lavender backgrounds, massive whitespace, feature-focused hero copy, and a 5-card screenshot collage that doesn't showcase what the product actually creates. Compared to competitors like Creatify, it lacks energy, social proof, and visual density.

## Goals

- Lead with output, not features — show what KillScale creates
- Bold, saturated color palette using actual logo gradient colors
- Eliminate dead whitespace
- Make the page feel alive, dense, and confident

## Design Decisions

### 1. Hero Section

**Background:** Full-width logo gradient (purple #7C56BA -> violet #955FEF -> blue #5E7CD8) at ~85% saturation with subtle noise texture overlay.

**Remove:**
- Eyebrow text ("Welcome To Your New Home for Digital Marketing")
- "Hook -- Hold -- Click -- Convert" hook line
- Old 5-card 3D collage (screenshot-showcase section)
- hero-scroll-spacer (80vh of empty space)

**Replace with:**
- Output-focused headline (white text). Options:
  - "Your product. Stunning ads. Seconds."
  - "From product photo to ad in seconds"
  - "Turn any product into scroll-stopping ads"
- Subtitle: "AI-generated video ads and image ads from just a photo or URL."
- CTA: "Start Free" (white button, purple text) + "See how it works" (white outline ghost button)
- Note: "7-day free trial. No credit card required."

**Video Carousel:**
- 6 phone-shaped vertical frames (9:16 aspect, ~180px wide, rounded corners, white border/shadow)
- Autoplay muted, looping
- Infinite horizontal conveyor belt animation, pauses on hover
- Mobile: horizontal swipe, 2 visible at a time
- Source: pick 6 best from `/Marketing/Video/Generated Videos/` (13 clips available, 2-9MB each)

**Before/After Image Ad Sliders:**
- 2 side-by-side slider cards (~500px wide each)
- Draggable vertical handle overlays before/after images
- Examples: Date Syrup (before: phone photo, after: polished ad) and Rind (same)
- Source: `/Marketing/Image Ads/Website Ads Examples/Image to Ad Examples/`
- Label above: "Image ads from a photo"
- White card frames, rounded corners, soft shadow floating on gradient

### 2. Social Proof Strip

**Background:** Clean white. Tight padding.

**Content:** Single centered bold line: "Built on the strategies behind millions in profitable ad spend"

No logos or stats (add later when available).

### 3. Color Overhaul (Global)

**Kill:**
- Pale lavender background `#FAF8FF`
- `--gradient-soft` (10-14% opacity purple washes)
- Dark section backgrounds `#0c0918`
- Light gray text on light backgrounds

**Replace with alternating rhythm:**
- **Gradient sections:** Full saturation logo gradient background, white text. Replaces all current "dark-section" instances.
- **White sections:** Pure `#FFFFFF`, dark text, tight padding. Breathing room.
- Alternating: gradient -> white -> gradient -> white down the page.

**Button rules:**
- On gradient backgrounds: white fill, purple text
- On white backgrounds: gradient fill, white text (current btn-primary style)

**CSS var changes:**
- `--bg` stays `#FFFFFF` for white sections
- `--dark-bg` replaced with gradient
- `--gradient-soft` removed or replaced with full-strength gradient
- Dark section text vars point to white

### 4. Whitespace Compression

- Kill `hero-scroll-spacer` (80vh)
- Section padding: reduce from ~120-160px to ~80px top/bottom
- Tighten hero internal gaps (headline -> subtitle -> CTA)
- Tighten Ad Studio walkthrough step spacing
- No section exceeds ~80px vertical breathing room

### 5. Page Title

Replace: "KillScale - Meta Ads Management for Growing Businesses"
With: "KillScale - AI Ad Creation & Performance Platform" (or similar)

## What's NOT Changing

- Ad Studio section structure and content (just color treatment)
- Walkthrough sections structure
- Pricing section structure
- Features cards section
- Footer
- Navigation structure

## Assets Required

**Videos (pick 6):**
- AI Video - Image-to-video.mp4
- AI Video - Open-prompt (1-9).mp4
- AI Video - Ugc.mp4
- Open Prompt - open-prompt.mp4
- Open Prompt - open-prompt (1).mp4

**Images:**
- Date Before.jpeg / Date After.PNG
- Rind Before.jpeg / Rind After.PNG

Videos need to be copied into the repo (likely under `/videos/` or hosted externally for size reasons).

## Technical Notes

- Video carousel: CSS animation for conveyor belt, IntersectionObserver for autoplay/pause
- Before/after slider: vanilla JS drag handler, CSS clip-path or overflow approach
- Videos should be compressed/optimized for web (current sizes 2-9MB each, target <2MB)
- Consider lazy loading videos below the fold
- Mobile: carousel becomes swipeable, sliders stack vertically
