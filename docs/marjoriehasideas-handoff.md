# marjoriehasideas.com — Build Handoff

## Concept
Umbrella site consolidating personal app projects under one domain, replacing a
per-app domain-buying habit. Personal, quietly clever tone — understated, not
loud/absurdist. Projects are visually distinct "characters," not uniform cards.

**Name:** Marjorie Has Ideas
**Tagline (chosen):** "Some are useful. Some are ridiculous. A surprising number
become websites."

## Layout — "Desk & Log" (winning concept)
Two-zone desktop layout:
- **Left ("desk")**: H1 + tagline + short bio, then a field of scattered,
  organically-placed round "stamps" — one per project.
- **Right ("log panel")**: persistent, always-visible blog index — NOT buried
  below the fold. Each entry is a clickable block: date, bold clicky title,
  italic one-line teaser. Links to a full post page.

Mobile: right panel stacks below; stamps go to a centered flex column
(fixed aspect-ratio 1:1, no stretching); hover tooltips become static
visible blocks under each stamp (no hover-reliant content on touch).

## Stamp design
- Round, "embossed/stamped coin" look: inset box-shadow for depth, dashed
  inner ring, subtle border. Background paper tone (`--paper-2`), NOT white.
- Each project has its **own accent color** (icon + border + hover states) —
  stamps are intentionally non-uniform once real colors/icons are in.
- **Icon evolves with maturity**: idea-stage projects show a simple
  word/emoji/icon placeholder; production-stage projects show the app's
  actual logo (SVG).
- **Hover reveals backstory**: name, one-line description, and a short
  personal note on why/how it started (may be lengthened further — user
  wants ~2x current length eventually).
- **Stage indicator = neutral outer ring** around the stamp (not a corner
  badge, not per-app colored — keeps it quiet):
  - `idea` → dotted ring, faint opacity
  - `in-progress` → dashed ring
  - `production` → solid ring, slightly heavier
  - Stage is also spelled out as plain text inside the hover tooltip.

## Color palette (paper theme)
```
--paper:    #f5f0e6   (page background)
--paper-2:  #ece3d0   (stamp fill, log panel bg)
--ink:      #2b2a28   (primary text)
--ink-soft: #6b6862   (secondary text, ring color)
--rule:     #d8cfb8   (dividers)
```
Typography: Georgia/Iowan Old Style serif for body/bio, Courier New
monospace for labels/headers/dates (quiet typographic contrast, no display
fonts).

## Reference prototype
Static HTML/CSS prototype reflecting all of the above is done and approved:
`derivative-c-desk-and-log.html` (already in your outputs from this chat —
pull it into the new repo as the template starting point).

## Tech stack decision
**Static site generator: Eleventy (11ty)** — not Vue. Rationale: updates are
content changes (new post, new stamp), not app behavior — no client
reactivity/state needed. Output is plain HTML/CSS, so nothing from the
prototype gets thrown away, just templated.

**Hosting:** S3 + CloudFront (same pattern as [[renovation-reference-site]]).

### Suggested project structure
```
marjoriehasideas/
├── .eleventy.js
├── package.json
├── src/
│   ├── _data/
│   │   └── stamps.json        # one entry per project
│   ├── _includes/
│   │   ├── base.njk           # shared shell (head, fonts, footer)
│   │   └── stamp.njk          # single stamp partial (loops over stamps.json)
│   ├── posts/                 # one markdown file per blog post
│   │   └── 2026-08-24-vacuum-pipeline.md
│   ├── css/
│   │   └── style.css          # lifted from the prototype
│   └── index.njk              # homepage: bio + stamp field + post index
├── .github/
│   └── workflows/
│       └── deploy.yml         # CI/CD, see below
└── _site/                     # build output (gitignored)
```

### stamps.json shape
```json
[
  {
    "name": "The Goal Line",
    "stage": "production",
    "icon": "/img/logos/goal-line.svg",
    "color": "#8a6d00",
    "blurb": "weekly NFL predictions vs. the games",
    "backstory": "started because I got tired of guessing at spreads.",
    "url": "https://goalline.marjoriehasideas.com"
  }
]
```
`icon` can be a logo path (production) or a plain emoji/word string
(idea/in-progress) — template just renders whichever is present.

### Blog post frontmatter shape
```md
---
title: Teaching a recommendation engine to shop for a vacuum so I don't have to
date: 2026-08-24
teaser: Wiring the enrichment pipeline into DynamoDB, and what "good enough" data actually means.
---
Full post content here...
```
11ty's built-in collections auto-sort these by date for the log panel index.

## CI/CD (GitHub Actions → S3 → CloudFront)
`.github/workflows/deploy.yml`:
```yaml
name: Deploy site
on:
  push:
    branches: [main]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: 20

      - run: npm ci
      - run: npx @11ty/eleventy   # builds to _site/

      - uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-1

      - name: Sync to S3
        run: aws s3 sync _site/ s3://YOUR_BUCKET_NAME --delete

      - name: Invalidate CloudFront cache
        run: aws cloudfront create-invalidation --distribution-id YOUR_DISTRIBUTION_ID --paths "/*"
```
Store AWS creds as repo secrets. Workflow: write a post or edit stamps.json
locally → commit + push to `main` → site rebuilds and deploys automatically.
No manual `aws s3 sync` needed once this is wired (though you can still run
it by hand for quick fixes).

## Open items / next steps
- [ ] Scaffold the 11ty project from the structure above
- [ ] Port `derivative-c-desk-and-log.html` CSS/markup into `base.njk` + `stamp.njk` + `index.njk`
- [ ] Populate real `stamps.json` (colors/icons/logos per app)
- [ ] Write first 2–3 real blog posts as markdown
- [ ] Provision S3 bucket + CloudFront distribution, wire domain/DNS
- [ ] Add `deploy.yml`, set repo secrets, test end-to-end push→deploy
- [ ] Lengthen hover backstories per app (deferred polish, not blocking)
