# Landing Page Redesign Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Redesign the KillScale landing page hero with output-focused content (video carousel + before/after sliders), bold saturated gradient colors, and compressed whitespace.

**Architecture:** Single-file static HTML/CSS/JS page (`index.html`). All changes are in this one file plus new assets (videos, images) copied into the repo. No build step.

**Tech Stack:** Vanilla HTML, CSS (custom properties, animations, clip-path), vanilla JS (drag handlers, IntersectionObserver)

**Design doc:** `docs/plans/2026-03-07-landing-page-redesign-design.md`

---

### Task 1: Copy and optimize assets

**Files:**
- Create: `videos/` directory in repo root
- Create: `images/before-after/` directory
- Copy 6 videos + 4 before/after images

**Step 1: Create asset directories**

```bash
mkdir -p /Users/masterphelps/killscale/killscale-landing/videos
mkdir -p /Users/masterphelps/killscale/killscale-landing/images/before-after
```

**Step 2: Copy before/after images into repo**

```bash
cp "/Users/masterphelps/killscale/Marketing/Image Ads/Website Ads Examples/Image to Ad Examples/Date Before.jpeg" /Users/masterphelps/killscale/killscale-landing/images/before-after/date-before.jpg
cp "/Users/masterphelps/killscale/Marketing/Image Ads/Website Ads Examples/Image to Ad Examples/Date After.PNG" /Users/masterphelps/killscale/killscale-landing/images/before-after/date-after.png
cp "/Users/masterphelps/killscale/Marketing/Image Ads/Website Ads Examples/Image to Ad Examples/Rind Before.jpeg" /Users/masterphelps/killscale/killscale-landing/images/before-after/rind-before.jpg
cp "/Users/masterphelps/killscale/Marketing/Image Ads/Website Ads Examples/Image to Ad Examples/Rind After.PNG" /Users/masterphelps/killscale/killscale-landing/images/before-after/rind-after.png
```

**Step 3: Pick 6 best videos and copy them**

Review all 13 videos. Pick 6 with variety (different products, styles). Copy to repo:

```bash
# These are the recommended 6 — adjust based on visual review:
cp "/Users/masterphelps/killscale/Marketing/Video/Generated Videos/AI Video - Image-to-video.mp4" /Users/masterphelps/killscale/killscale-landing/videos/ad-1.mp4
cp "/Users/masterphelps/killscale/Marketing/Video/Generated Videos/AI Video - Ugc.mp4" /Users/masterphelps/killscale/killscale-landing/videos/ad-2.mp4
cp "/Users/masterphelps/killscale/Marketing/Video/Generated Videos/AI Video - Open-prompt (1).mp4" /Users/masterphelps/killscale/killscale-landing/videos/ad-3.mp4
cp "/Users/masterphelps/killscale/Marketing/Video/Generated Videos/AI Video - Open-prompt (2).mp4" /Users/masterphelps/killscale/killscale-landing/videos/ad-4.mp4
cp "/Users/masterphelps/killscale/Marketing/Video/Generated Videos/AI Video - Open-prompt (3).mp4" /Users/masterphelps/killscale/killscale-landing/videos/ad-5.mp4
cp "/Users/masterphelps/killscale/Marketing/Video/Generated Videos/Open Prompt - open-prompt.mp4" /Users/masterphelps/killscale/killscale-landing/videos/ad-6.mp4
```

**Step 4: Compress videos for web (target <2MB each)**

```bash
# Requires ffmpeg. For each video:
for i in 1 2 3 4 5 6; do
  ffmpeg -i "/Users/masterphelps/killscale/killscale-landing/videos/ad-$i.mp4" \
    -vcodec libx264 -crf 28 -preset slow \
    -vf "scale=360:-2" \
    -an \
    "/Users/masterphelps/killscale/killscale-landing/videos/ad-$i-web.mp4" -y
  mv "/Users/masterphelps/killscale/killscale-landing/videos/ad-$i-web.mp4" "/Users/masterphelps/killscale/killscale-landing/videos/ad-$i.mp4"
done
```

Note: `-an` strips audio (autoplay muted anyway), `scale=360` keeps them small for phone frames, `crf 28` balances quality/size.

**Step 5: Verify assets**

```bash
ls -la /Users/masterphelps/killscale/killscale-landing/videos/
ls -la /Users/masterphelps/killscale/killscale-landing/images/before-after/
```

Expected: 6 videos under 2MB each, 4 images.

**Step 6: Commit**

```bash
git add videos/ images/before-after/
git commit -m "Add video ad clips and before/after image ad assets for landing page redesign"
```

---

### Task 2: Update page title and meta tags

**Files:**
- Modify: `index.html` lines 6, 38, 49-50, 59-60

**Step 1: Update title and meta descriptions**

Change line 6:
```html
<title>KillScale - AI Ad Creation & Performance Platform</title>
```

Change line 38 meta description:
```html
<meta name="description" content="KillScale turns your product photos into stunning video and image ads in seconds. AI-powered ad creation, performance tracking, and scaling — all in one platform.">
```

Update og:title (line 49) and twitter:title (line 60) to match:
```
KillScale - AI Ad Creation & Performance Platform
```

Update og:description (line 50) and twitter:description (line 61):
```
Turn any product photo into scroll-stopping video and image ads in seconds. AI-powered creation, tracking, and scaling.
```

**Step 2: Verify in browser**

Open `index.html`, check the browser tab title.

**Step 3: Commit**

```bash
git add index.html
git commit -m "Update page title and meta tags to output-focused messaging"
```

---

### Task 3: Color overhaul — CSS custom properties and dark-section replacement

**Files:**
- Modify: `index.html` CSS `:root` vars (lines 138-162)
- Modify: `.dark-section` styles (lines 1277-1355)
- Modify: `.feature-section` padding (line 795)
- Modify: Various section paddings

This is the biggest single change. It touches CSS only, no HTML structure changes.

**Step 1: Update `:root` variables (lines 138-162)**

Replace the entire `:root` block with:

```css
:root {
  --bg:             #FFFFFF;
  --bg-alt:         #FFFFFF;
  --bg-white:       #FFFFFF;
  --text:           #1A1A1A;
  --text-secondary: #4B5563;
  --text-tertiary:  #6B7280;
  --accent:         #7C56BA;
  --accent-hover:   #6A48A0;
  --border:         #D8CEF0;
  --green:          #10b981;
  --red:            #ef4444;
  --purple:         #955FEF;
  --amber:          #f59e0b;
  --gradient:       linear-gradient(115deg, #7C56BA 0%, #8F55D4 25%, #955FEF 45%, #796EE3 70%, #5E7CD8 100%);
  --gradient-bg:    linear-gradient(135deg, #7C56BA 0%, #8F55D4 20%, #955FEF 40%, #796EE3 65%, #5E7CD8 85%, #7C56BA 100%);

  /* Gradient section palette (replaces dark mode) */
  --grad-text:      #ffffff;
  --grad-text-sec:  rgba(255,255,255,0.75);
  --grad-text-tert: rgba(255,255,255,0.50);
  --grad-surface:   rgba(255,255,255,0.10);
  --grad-border:    rgba(255,255,255,0.15);
}
```

Key changes: `--bg` is now pure white. `--text-secondary` bumped darker (was #6B7280, now #4B5563). `--gradient-soft` removed. Dark vars replaced with `--grad-*` vars.

**Step 2: Replace `.dark-section` with `.gradient-section` (lines 1277-1355)**

Replace the entire `.dark-section` block with:

```css
/* ========== GRADIENT SECTIONS (replaces dark-section) ========== */
.gradient-section {
  background: var(--gradient-bg) !important;
  position: relative;
  overflow: hidden;
}
.gradient-section::before {
  content: '';
  position: absolute;
  inset: 0;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
  opacity: 0.04;
  pointer-events: none;
  z-index: 0;
}
.gradient-section > * { position: relative; z-index: 1; }

/* Text color overrides for gradient sections */
.gradient-section,
.gradient-section h2,
.gradient-section h3,
.gradient-section .section-heading,
.gradient-section .feature-text h2,
.gradient-section .feature-text h3,
.gradient-section .walkthrough-step-title,
.gradient-section .walkthrough-header h2,
.gradient-section .ad-studio-step h3,
.gradient-section .ad-studio-section h2,
.gradient-section .features-cards-header h2,
.gradient-section .verdict-title,
.gradient-section .final-cta h2 {
  color: var(--grad-text) !important;
}
.gradient-section p,
.gradient-section .feature-text p,
.gradient-section .walkthrough-step-desc,
.gradient-section .feature-list li,
.gradient-section .ad-studio-step p,
.gradient-section .final-cta p {
  color: var(--grad-text-sec) !important;
}
.gradient-section .eyebrow,
.gradient-section .feature-text .eyebrow,
.gradient-section .walkthrough-header .eyebrow,
.gradient-section .ad-studio-section .eyebrow,
.gradient-section .features-cards-header .eyebrow {
  background: linear-gradient(90deg, #e0d0ff, #b8c8f0) !important;
  -webkit-background-clip: text !important;
  -webkit-text-fill-color: transparent !important;
  background-clip: text !important;
}

/* Gradient section cards and images */
.gradient-section .walkthrough-step-image img,
.gradient-section .feature-image img,
.gradient-section .ad-studio-step img {
  border-color: var(--grad-border) !important;
  box-shadow: 0 25px 50px rgba(0, 0, 0, 0.3) !important;
}

/* Buttons on gradient backgrounds */
.gradient-section .btn-primary {
  background: #ffffff !important;
  color: var(--accent) !important;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
}
.gradient-section .btn-primary:hover {
  background: #f0f0f0 !important;
}
.gradient-section .btn-secondary {
  border-color: rgba(255,255,255,0.3) !important;
  color: #ffffff !important;
}
.gradient-section .btn-secondary:hover {
  background: rgba(255,255,255,0.1) !important;
  border-color: rgba(255,255,255,0.5) !important;
}
```

**Step 3: Find-and-replace `dark-section` class in HTML**

In the HTML (around lines 2371-3100), replace every `class="... dark-section"` with `class="... gradient-section"`. There are approximately 8 instances:

- `ad-studio-section dark-section` -> `ad-studio-section gradient-section`
- `walkthrough walkthrough-create dark-section` -> `walkthrough walkthrough-create gradient-section`
- `feature-section dark-section` -> `feature-section gradient-section`
- `walkthrough walkthrough-insights dark-section` -> `walkthrough walkthrough-insights gradient-section`
- `feature-section dark-section` (second instance) -> `feature-section gradient-section`
- `walkthrough walkthrough-scale dark-section` -> `walkthrough walkthrough-scale gradient-section`
- `features-cards-section dark-section` -> `features-cards-section gradient-section`
- `final-cta dark-section` -> `final-cta gradient-section`

Also remove any `<div class="dark-glow dark-glow-1">` and `dark-glow-2` divs inside these sections — the gradient background replaces them.

**Step 4: Remove the `.orb-section` lavender backgrounds**

The `.orb-section` class applies the pale lavender look. Find its CSS and either remove it or change it to plain white background. These sections should now be clean white.

**Step 5: Update `.feature-section` background**

The `.feature-section` without `.gradient-section` should have `background: #ffffff`. Ensure no lingering `var(--bg)` references pull in lavender.

**Step 6: Verify in browser**

Open `index.html`. Scroll through. Sections should alternate between vibrant purple-blue gradient and clean white. No more pale lavender. No more dark (#0c0918) backgrounds.

**Step 7: Commit**

```bash
git add index.html
git commit -m "Replace dark/lavender sections with saturated gradient sections and clean white"
```

---

### Task 4: Whitespace compression

**Files:**
- Modify: `index.html` CSS — multiple padding values and the hero-scroll-spacer

**Step 1: Kill the hero-scroll-spacer**

Find CSS (line ~523):
```css
.hero-scroll-spacer {
  height: 80vh;
}
```
Change to:
```css
.hero-scroll-spacer {
  display: none;
}
```

Also remove the HTML element `<div class="hero-scroll-spacer"></div>` (around line 2345).

**Step 2: Compress section padding**

Find and replace these padding values:

- `.feature-section` (line 795): `padding: 120px 0` -> `padding: 80px 0`
- `.ad-studio-section` (line 903): `padding: 120px 0` -> `padding: 80px 0`
- `.ad-studio-section .section-header` (line 908): `margin-bottom: 72px` -> `margin-bottom: 48px`
- `.final-cta` (line 1256): `padding: 120px 0` -> `padding: 80px 0`
- `.pricing` padding: find and reduce to `80px 0`
- Walkthrough sections: find their padding and reduce to `80px 0`

**Step 3: Tighten hero internals**

- `.hero` (line 364): `padding: 160px 0 80px` -> `padding: 140px 0 60px`
- `.hero h1` (line 394): `margin-bottom: 24px` -> `margin-bottom: 16px`
- `.hero-subtitle` (line 419): `margin: 0 auto 36px` -> `margin: 0 auto 28px`

**Step 4: Verify in browser**

Scroll through — no section should have more than ~80px of dead space. The page should feel tight and punchy.

**Step 5: Commit**

```bash
git add index.html
git commit -m "Compress whitespace across all sections"
```

---

### Task 5: New hero section — HTML and CSS

**Files:**
- Modify: `index.html` — hero HTML (lines 2302-2320) and hero CSS (lines 363-435)
- Modify: screenshot-showcase section (lines 2322-2346) — replace with new content

**Step 1: Update hero CSS**

Replace the hero CSS (lines 363-435) with:

```css
/* ========== HERO ========== */
.hero {
  padding: 140px 0 60px;
  position: relative;
  overflow: visible;
  background: var(--gradient-bg);
}
.hero::before {
  content: '';
  position: absolute;
  inset: 0;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
  opacity: 0.04;
  pointer-events: none;
  z-index: 0;
}
.hero > * { position: relative; z-index: 1; }

.hero-content {
  max-width: 800px;
  margin: 0 auto;
  text-align: center;
  position: relative;
}

.hero h1 {
  font-family: 'DM Serif Display', Georgia, serif;
  font-size: clamp(42px, 6vw, 72px);
  font-weight: 400;
  line-height: 1.1;
  margin-bottom: 16px;
  letter-spacing: -1px;
  color: #ffffff;
}

.hero h1 em {
  font-style: italic;
}

.hero-subtitle {
  font-size: 18px;
  color: rgba(255,255,255,0.80);
  max-width: 600px;
  margin: 0 auto 28px;
  line-height: 1.7;
}

.hero-cta {
  display: flex;
  gap: 12px;
  justify-content: center;
  flex-wrap: wrap;
  margin-bottom: 14px;
}

.hero-cta .btn-primary {
  background: #ffffff;
  color: var(--accent);
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
}
.hero-cta .btn-primary:hover {
  background: #f0f0f0;
  transform: translateY(-1px);
}
.hero-cta .btn-secondary {
  border-color: rgba(255,255,255,0.3);
  color: #ffffff;
}
.hero-cta .btn-secondary:hover {
  background: rgba(255,255,255,0.1);
  border-color: rgba(255,255,255,0.5);
}

.hero-note {
  font-size: 13px;
  color: rgba(255,255,255,0.55);
}
```

**Step 2: Add video carousel CSS**

Add after the hero CSS:

```css
/* ========== VIDEO CAROUSEL ========== */
.video-carousel {
  margin-top: 48px;
  overflow: hidden;
  padding: 16px 0;
}

.video-carousel-label {
  text-align: center;
  font-size: 14px;
  font-weight: 600;
  color: rgba(255,255,255,0.6);
  text-transform: uppercase;
  letter-spacing: 1.5px;
  margin-bottom: 20px;
}

.video-carousel-track {
  display: flex;
  gap: 16px;
  animation: carousel-scroll 30s linear infinite;
  width: max-content;
}

.video-carousel-track:hover {
  animation-play-state: paused;
}

.video-card {
  flex-shrink: 0;
  width: 180px;
  aspect-ratio: 9/16;
  border-radius: 16px;
  overflow: hidden;
  border: 2px solid rgba(255,255,255,0.2);
  box-shadow: 0 8px 32px rgba(0,0,0,0.3);
  background: #000;
}

.video-card video {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
}

@keyframes carousel-scroll {
  0% { transform: translateX(0); }
  100% { transform: translateX(-50%); }
}
```

**Step 3: Add before/after slider CSS**

```css
/* ========== BEFORE/AFTER SLIDER ========== */
.before-after-section {
  margin-top: 48px;
}

.before-after-label {
  text-align: center;
  font-size: 14px;
  font-weight: 600;
  color: rgba(255,255,255,0.6);
  text-transform: uppercase;
  letter-spacing: 1.5px;
  margin-bottom: 20px;
}

.before-after-row {
  display: flex;
  gap: 24px;
  justify-content: center;
  flex-wrap: wrap;
}

.before-after-card {
  width: 280px;
  aspect-ratio: 9/16;
  border-radius: 16px;
  overflow: hidden;
  position: relative;
  cursor: ew-resize;
  border: 2px solid rgba(255,255,255,0.2);
  box-shadow: 0 8px 32px rgba(0,0,0,0.3);
  background: #000;
}

.before-after-card img {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  pointer-events: none;
}

.before-after-card .after-img {
  clip-path: inset(0 0 0 50%);
}

.before-after-handle {
  position: absolute;
  top: 0;
  bottom: 0;
  left: 50%;
  width: 3px;
  background: #ffffff;
  transform: translateX(-50%);
  z-index: 10;
  pointer-events: none;
}

.before-after-handle::after {
  content: '';
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 36px;
  height: 36px;
  background: #ffffff;
  border-radius: 50%;
  box-shadow: 0 2px 8px rgba(0,0,0,0.3);
}

.before-after-handle::before {
  content: '\2194';
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  font-size: 18px;
  color: var(--accent);
  z-index: 11;
  pointer-events: none;
}

.before-after-card .label {
  position: absolute;
  bottom: 12px;
  font-size: 11px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 1px;
  color: #fff;
  background: rgba(0,0,0,0.5);
  padding: 4px 10px;
  border-radius: 4px;
  z-index: 5;
}
.before-after-card .label-before { left: 12px; }
.before-after-card .label-after { right: 12px; }
```

**Step 4: Update hero HTML (lines 2302-2346)**

Replace the hero section AND the screenshot-showcase section with:

```html
<section class="hero">
  <div class="container">
    <div class="hero-content">
      <h1>Turn Any Product Into<br>Scroll-Stopping Ads</h1>
      <p class="hero-subtitle">
        AI-generated video ads and image ads from just a photo or URL.
      </p>
      <div class="hero-cta">
        <a href="https://app.killscale.com/signup" class="btn btn-primary btn-large">Start Free</a>
        <a href="/blog/create-facebook-ad-images-videos-from-product-url.html" class="btn btn-secondary btn-large">See how it works</a>
      </div>
      <p class="hero-note">7-day free trial. No credit card required.</p>
    </div>

    <!-- Video Carousel -->
    <div class="video-carousel">
      <p class="video-carousel-label">AI-generated video ads</p>
      <div class="video-carousel-track">
        <div class="video-card"><video src="videos/ad-1.mp4" autoplay muted loop playsinline></video></div>
        <div class="video-card"><video src="videos/ad-2.mp4" autoplay muted loop playsinline></video></div>
        <div class="video-card"><video src="videos/ad-3.mp4" autoplay muted loop playsinline></video></div>
        <div class="video-card"><video src="videos/ad-4.mp4" autoplay muted loop playsinline></video></div>
        <div class="video-card"><video src="videos/ad-5.mp4" autoplay muted loop playsinline></video></div>
        <div class="video-card"><video src="videos/ad-6.mp4" autoplay muted loop playsinline></video></div>
        <!-- Duplicate set for infinite scroll illusion -->
        <div class="video-card"><video src="videos/ad-1.mp4" autoplay muted loop playsinline></video></div>
        <div class="video-card"><video src="videos/ad-2.mp4" autoplay muted loop playsinline></video></div>
        <div class="video-card"><video src="videos/ad-3.mp4" autoplay muted loop playsinline></video></div>
        <div class="video-card"><video src="videos/ad-4.mp4" autoplay muted loop playsinline></video></div>
        <div class="video-card"><video src="videos/ad-5.mp4" autoplay muted loop playsinline></video></div>
        <div class="video-card"><video src="videos/ad-6.mp4" autoplay muted loop playsinline></video></div>
      </div>
    </div>

    <!-- Before/After Image Ads -->
    <div class="before-after-section">
      <p class="before-after-label">Image ads from a photo</p>
      <div class="before-after-row">
        <div class="before-after-card" data-slider>
          <img src="images/before-after/date-before.jpg" alt="Product photo before" class="before-img">
          <img src="images/before-after/date-after.png" alt="AI-generated ad after" class="after-img">
          <div class="before-after-handle"></div>
          <span class="label label-before">Before</span>
          <span class="label label-after">After</span>
        </div>
        <div class="before-after-card" data-slider>
          <img src="images/before-after/rind-before.jpg" alt="Product photo before" class="before-img">
          <img src="images/before-after/rind-after.png" alt="AI-generated ad after" class="after-img">
          <div class="before-after-handle"></div>
          <span class="label label-before">Before</span>
          <span class="label label-after">After</span>
        </div>
      </div>
    </div>
  </div>
</section>
```

**Step 5: Remove old hero animation classes**

Search for and remove references to `gsap-hero-eyebrow`, `gsap-hero-h1`, `gsap-hero-subtitle`, `gsap-hero-cta`, `gsap-hero-note`, `gsap-hero-visual` in the CSS (around lines 1572-1578) and any GSAP JS that targets them (around line 3180+).

Also remove the old `.hero-eyebrow`, `.hero-hook`, and old `.hero-collage*` CSS blocks since they're no longer used.

**Step 6: Verify in browser**

Open `index.html`. The hero should show:
- Gradient background (purple->blue)
- White headline: "Turn Any Product Into Scroll-Stopping Ads"
- White subtitle and CTAs
- 6 videos in phone frames scrolling horizontally
- 2 before/after sliders below

**Step 7: Commit**

```bash
git add index.html
git commit -m "New hero section with video carousel and before/after sliders on gradient background"
```

---

### Task 6: Before/after slider JavaScript

**Files:**
- Modify: `index.html` — add JS at bottom of `<script>` section

**Step 1: Add the slider interaction JS**

Find the main `<script>` block at the bottom of `index.html`. Add this code:

```javascript
// ========== BEFORE/AFTER SLIDERS ==========
document.querySelectorAll('[data-slider]').forEach(card => {
  const afterImg = card.querySelector('.after-img');
  const handle = card.querySelector('.before-after-handle');
  let isDragging = false;

  function updateSlider(x) {
    const rect = card.getBoundingClientRect();
    let pct = ((x - rect.left) / rect.width) * 100;
    pct = Math.max(5, Math.min(95, pct));
    afterImg.style.clipPath = `inset(0 0 0 ${pct}%)`;
    handle.style.left = pct + '%';
  }

  card.addEventListener('mousedown', e => { isDragging = true; updateSlider(e.clientX); });
  card.addEventListener('touchstart', e => { isDragging = true; updateSlider(e.touches[0].clientX); }, { passive: true });

  window.addEventListener('mousemove', e => { if (isDragging) updateSlider(e.clientX); });
  window.addEventListener('touchmove', e => { if (isDragging) updateSlider(e.touches[0].clientX); }, { passive: true });

  window.addEventListener('mouseup', () => { isDragging = false; });
  window.addEventListener('touchend', () => { isDragging = false; });
});
```

**Step 2: Verify in browser**

Open page, scroll to before/after cards. Drag the handle left/right — the after image should reveal/hide based on handle position. Test on both sliders. Test touch drag if possible.

**Step 3: Commit**

```bash
git add index.html
git commit -m "Add before/after slider drag interaction"
```

---

### Task 7: Social proof strip

**Files:**
- Modify: `index.html` — add HTML after hero, add CSS

**Step 1: Add social proof CSS**

```css
/* ========== SOCIAL PROOF STRIP ========== */
.social-proof-strip {
  padding: 40px 0;
  background: #ffffff;
  text-align: center;
}

.social-proof-strip p {
  font-size: 17px;
  font-weight: 600;
  color: var(--text-secondary);
  letter-spacing: 0.3px;
}
```

**Step 2: Add HTML after the hero closing tag**

Right after `</section>` (hero), before the metrics-bar section:

```html
<!-- Social Proof -->
<section class="social-proof-strip">
  <div class="container">
    <p>Built on the strategies behind millions in profitable ad spend</p>
  </div>
</section>
```

**Step 3: Remove old metrics-bar section**

The current metrics-bar section ("AI Features That Actually Work" with "Clone competitor ads / AI creative scoring / Generate ads from URL") — keep it for now per user's instruction ("leave it for now"). But remove the old screenshot-showcase section HTML if any remains.

**Step 4: Verify in browser**

Social proof line should appear as a clean white strip between the gradient hero and the next section.

**Step 5: Commit**

```bash
git add index.html
git commit -m "Add social proof strip below hero"
```

---

### Task 8: Mobile responsive adjustments

**Files:**
- Modify: `index.html` — CSS media queries

**Step 1: Add mobile styles for video carousel**

Find the existing mobile media queries (around line 1624+) and add:

```css
@media (max-width: 768px) {
  .video-carousel-track {
    animation-duration: 20s;
  }
  .video-card {
    width: 140px;
  }
  .before-after-row {
    flex-direction: column;
    align-items: center;
  }
  .before-after-card {
    width: 240px;
  }
  .hero h1 {
    font-size: clamp(32px, 8vw, 48px);
  }
}
```

**Step 2: Verify at mobile widths**

Resize browser to ~375px wide. Check:
- Hero text is readable, not overflowing
- Video carousel scrolls smoothly, cards are properly sized
- Before/after sliders stack vertically
- Slider drag works on touch

**Step 3: Commit**

```bash
git add index.html
git commit -m "Add mobile responsive styles for new hero components"
```

---

### Task 9: Clean up removed code

**Files:**
- Modify: `index.html` — remove unused CSS and JS

**Step 1: Remove old hero collage CSS**

Delete the entire `/* ========== HERO COLLAGE ========== */` block (lines ~436-525) including all `.collage-card` variants.

**Step 2: Remove old hero-specific classes**

Delete `.hero-eyebrow` (lines ~377-387) and `.hero-hook` (lines ~403-413) CSS blocks.

**Step 3: Remove old GSAP hero animation JS**

Find the `// ========== HERO ENTRANCE ==========` block in the `<script>` section (around line 3180) and remove any GSAP animations targeting the old hero elements (`.gsap-hero-eyebrow`, `.gsap-hero-h1`, etc.).

**Step 4: Remove old `.dark-section` CSS if still present**

If any `.dark-section` references remain in CSS, remove them. All should now be `.gradient-section`.

**Step 5: Remove `.dark-glow` CSS and HTML**

Delete the `.dark-glow`, `.dark-glow-1`, `.dark-glow-2` CSS rules. Remove all `<div class="dark-glow dark-glow-1">` and `dark-glow-2` HTML elements from gradient sections.

**Step 6: Remove `.orb-section` and `.light-orb` CSS/HTML**

These created the old lavender glow effects. Remove the CSS rules and the HTML `<div class="light-orb ...">` elements.

**Step 7: Verify in browser**

Full scroll-through. No visual regressions. No console errors about missing elements.

**Step 8: Commit**

```bash
git add index.html
git commit -m "Remove unused collage, dark-section, orb, and old hero CSS/JS"
```

---

### Task 10: Final visual review and polish

**Files:**
- Modify: `index.html` — any tweaks found during review

**Step 1: Full browser review**

Open `index.html` and check each section top to bottom:
- [ ] Hero: gradient bg, white text, videos playing, sliders working
- [ ] Social proof: clean white strip
- [ ] Metrics bar: readable, no lavender
- [ ] Ad Studio: gradient bg, white text, screenshots visible
- [ ] Walkthroughs: alternating gradient/white
- [ ] Feature sections: alternating gradient/white
- [ ] Pricing: white bg, cards readable
- [ ] Final CTA: gradient bg
- [ ] Footer: correct colors
- [ ] No console JS errors
- [ ] Mobile (375px): everything stacks and reads well

**Step 2: Fix any issues found**

Address color contrast issues, misaligned elements, broken images, etc.

**Step 3: Final commit**

```bash
git add index.html
git commit -m "Polish and finalize landing page redesign"
```
