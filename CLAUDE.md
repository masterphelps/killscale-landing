# KillScale Landing Page

## Overview

Static landing page for KillScale - a Meta Ads performance monitoring SaaS.

**Live URL:** https://killscale.com

## Structure

```
killscale-landing/
├── index.html              # Main landing page
├── dashboard-screenshot.jpg # Hero image showing app dashboard
└── public/                 # Static assets (if any)
```

## Development

No build step - just static HTML/CSS.

```bash
# Local preview - open directly or use live server
open index.html
# or
npx live-server
```

## Deployment

Hosted on Vercel. Auto-deploys on push to main.

## Related

- **App repo:** `../killscale-app/` - Main Next.js application
- **App URL:** https://app.killscale.com

## Key Sections

The landing page includes:
- Hero with dashboard screenshot
- Feature highlights (verdicts, rules, alerts)
- Pricing tiers
- CTA buttons linking to app.killscale.com/signup
