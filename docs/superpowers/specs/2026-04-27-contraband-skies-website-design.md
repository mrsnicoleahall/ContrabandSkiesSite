# Contraband Skies — Marketing Website Design Spec

## Overview

Full marketing website for Contraband Skies, a persistent-match noir extraction shooter. The site serves as the primary public-facing marketing surface for PC/Mac launch on July 1, 2026 at $50 via Steam and Epic Games Store. iOS is on the roadmap but not featured at launch.

No game assets (screenshots, trailers, concept art) exist yet. The site must establish the game's visual identity entirely through CSS, SVG, and typographic art direction — creating a complete noir comic-book atmosphere without dependency on captured game footage.

## Tech Stack

- **Framework:** Astro (static site generation)
- **Styling:** Tailwind CSS
- **Interactivity:** Vanilla JS (scroll animations, mobile nav, accordions)
- **Output:** Static HTML/CSS/JS to `dist/`
- **Deployment:** GitHub Actions → FTP push on merge to `main`

## Visual System — "Panel Cut Noir"

### Design Language

The site uses a hybrid comic-book/editorial aesthetic: diagonal panel cuts and ink texture provide grit and graphic novel energy, while clean typography and generous spacing keep content readable across all page types.

### Color Palette

The palette is 95% grayscale. Color appears sparingly and always with purpose.

| Token | Hex | Usage |
|---|---|---|
| `base` | `#050505` | Page background |
| `panel` | `#0a0a0a` | Content panel backgrounds |
| `panel-alt` | `#111111` | Alternating panel backgrounds |
| `surface` | `#1a1a1a` | Code blocks, input fields, subtle containers |
| `border` | `#222222` | Panel dividers, separator lines |
| `border-subtle` | `#333333` | Card borders, secondary dividers |
| `text` | `#ffffff` | Headlines, primary text |
| `text-secondary` | `#cccccc` | Body copy |
| `text-muted` | `#999999` | Subheadings, descriptive text |
| `text-dim` | `#666666` | Labels, metadata, timestamps |
| `text-faint` | `#444444` | Disabled states, hints |
| `crimson` | `#dc2626` | Primary accent — CTAs, emphasis words, key borders, glows |
| `gold` | `#d4a017` | Secondary accent — used only in map/crafting contexts |
| `toxic` | `#22c55e` | Tertiary accent — used only for bioform/hazard contexts |

**Crimson usage rules:** One crimson element per visual section maximum. It appears as: CTA button fills, single accent words in headlines, thin glowing border lines, small icon highlights. Never crimson backgrounds for large areas. The scarcity is the point.

### Typography

| Role | Font | Weight | Size Range | Tracking | Transform |
|---|---|---|---|---|---|
| Display headline | Georgia, serif | 900 | 36–64px | -1px to -0.5px | uppercase |
| Section heading | Georgia, serif | 700 | 24–32px | normal | uppercase |
| Subheading | Helvetica Neue, Arial, sans-serif | 400 | 16–18px | normal | none |
| Body | Helvetica Neue, Arial, sans-serif | 400 | 14–16px | normal | none |
| Label / metadata | monospace (system) | 400 | 9–11px | 2–4px | uppercase |
| Accent word | Georgia, serif | 400 | varies | 4px | uppercase |

All fonts are system fonts — no external font loading required.

### Panel Cuts

Content sections are divided by diagonal clip-path cuts rather than horizontal lines. This creates the graphic novel page-turn rhythm.

- Typical cut angle: 2–4 degrees (subtle but noticeable)
- Cut implementation: CSS `clip-path: polygon()` on section containers
- Panels alternate between `panel` and `panel-alt` backgrounds
- A thin `border` colored line sits along each cut edge
- Occasional crimson hairline overlaps a diagonal cut for emphasis

### Texture

Every panel has a subtle SVG noise filter overlay at 5–8% opacity. This creates an ink/film grain feel without impacting readability or performance. The filter is defined once in a shared SVG `<defs>` block and referenced via CSS.

### Silhouette Art

Hero sections and feature panels use SVG silhouette illustrations — figures on rooftops, cityscapes, flight poses, weapon outlines. These are pure black shapes against the dark background with optional crimson accent details (muzzle flash, distant glow, single neon sign). This establishes visual identity without game screenshots.

### Interaction & Animation

- **Scroll-triggered reveals:** Sections fade and slide in as they enter viewport (IntersectionObserver, vanilla JS)
- **Panel cut parallax:** Subtle depth shift between overlapping panels on scroll
- **Crimson glow pulse:** CTA buttons have a slow, subtle crimson box-shadow pulse
- **Hover states:** Nav links get crimson underline slide-in; cards lift slightly with border brightening
- **Mobile:** All animations respect `prefers-reduced-motion`

## Navigation

Fixed top bar across all pages.

- **Left:** Wordmark — "CONTRABAND" in white, "SKIES" in crimson. Georgia serif, bold, uppercase.
- **Right:** Page links (GAME, WORLD, MEDIA, FAQ) in monospace uppercase + crimson "BUY NOW" button
- **Mobile:** Hamburger menu, full-screen overlay with stacked links on dark panel background
- **Scroll behavior:** Nav gains a slightly more opaque background and bottom border after scroll threshold
- **Active page:** Current page link is crimson colored

## Pages

### Home

The site's tone-setter. Maximum atmosphere, minimum text density.

**Sections in scroll order:**

1. **Hero (full viewport)**
   - SVG silhouette cityscape spanning full width — rooftops, towers, distant neon glow
   - Figure silhouette on rooftop edge (center or rule-of-thirds)
   - SVG rain effect (thin diagonal lines, very low opacity)
   - Title treatment: "CONTRABAND" large serif + "SKIES" in crimson below
   - Tagline: "Fly fast. Fight grounded. Extract everything." in italic serif
   - Subtle downward scroll indicator

2. **Three Pillars (panel-cut section)**
   - Three feature cards in a row, each with SVG icon art:
     - **Flight** — wing/boost silhouette, "Airborne superhero mobility"
     - **Combat** — weapon silhouette, "Grounded gunfight identity"
     - **Extraction** — uplink/beacon silhouette, "High-stakes loss with recovery"
   - Each card: icon + headline + 1–2 sentence description
   - Diagonal cut transition into next section

3. **The Contraband Loop**
   - Visual cycle diagram (SVG or styled HTML): Deploy → Traverse → Fight → Loot → Extract or Die → Recover → repeat
   - Each step gets a small icon and one-line description
   - Crimson accent on the "Extract" node to emphasize the payoff moment
   - Brief paragraph framing the loop's emotional cadence

4. **Store CTAs**
   - Side-by-side Steam and Epic Games Store buttons
   - Styled as dark containers with platform logos/wordmarks
   - Platform info bar below: "PC & Mac — July 1, 2026 — $50"

5. **Footer**
   - Placeholder slots for social/community links (Discord, X, YouTube, etc.)
   - Copyright line
   - Subtle film grain texture continues into footer
   - Thin crimson top border

### Game

Deep dive into every core system. Content-heavy but structured through panel cuts.

**Sections:**

1. **Flight System**
   - Section header with flight silhouette SVG
   - Grid of flight abilities: Cruise, Boost, Climb, Dive, Brake, Touchdown
   - Each ability: icon + name + one-line description
   - Energy system callout box
   - Key message: "Flight is traversal and evasion, not a firing platform"

2. **Combat**
   - Panel-cut transition
   - Third-person OTS camera callout
   - Weapon families grid: Pulse Carbines, Rail Rifles, Shotguns, SMGs
   - Each weapon: silhouette icon + name + role description
   - "Coming later" teaser for status weapons, shield breakers, utility launchers
   - Key message: "Grounded gunfights where positioning beats DPS"

3. **Extraction**
   - Visual diagram: extraction zone → timer → contested → success/fail outcomes
   - Rules callout: visible, noisy, high pressure, worth defending or ambushing
   - Stash vs active-run inventory explainer (two-column comparison)

4. **Crafting**
   - Crafting tier progression visual: Fieldmade → Specialized → Signature → Legacy
   - Five-layer crafting pipeline: Material → Research → Fabrication → Bio-Binding → Insurance
   - Each layer: brief description of what the player does
   - Bio-bound weapon explainer callout: "Permanently yours. Cannot be stolen."

5. **Insurance**
   - Three insurance tiers: Partial Attunement → Core Preservation → Legacy Policy
   - Each tier: what it protects, what it costs, how long to earn
   - Key message: "Insurance is earned, not bought"

6. **Death & Recovery**
   - Visual flow: Death → Corpse Beacon → Redeploy → Recover or Lose
   - Uninsured vs insured outcomes side by side
   - Emotional framing: "Desperation when racing back to a corpse"

### World

Atmospheric and lore-forward. Less technical, more immersive.

**Sections:**

1. **Map Overview**
   - Brief intro: "Three distinct maps. Same noir grammar. Different combat rhythm."

2. **Urban**
   - Styled card with SVG cityscape art (towers, alleys, rooftops, neon lines)
   - Terrain description, tactical feel, key landmarks
   - Callout: "Strongest vertical escape play and rooftop ambush potential"
   - Gold accent for this map's identity color (single thin border)

3. **Rural**
   - Styled card with SVG landscape art (ridgelines, timber, open fields)
   - Terrain description, longer sightlines, marksman pressure
   - Callout: "Punishes exposed movement"

4. **Suburbs**
   - Styled card with SVG suburban art (houses, strip malls, drainage channels)
   - Terrain description, flanking lanes, close-to-mid pressure
   - Callout: "Ambushes, lane control, house-to-house gunfights"

5. **PvE Factions**
   - Five faction cards in a grid:
     - Scav Raiders — baseline armed roamers
     - Blacksite Security — tech guardians
     - Bioforms — organic threats (toxic green accent)
     - Stormbound Drones — airborne machine patrols
     - Rogue Harvesters — extraction-focused AI competitors
   - Each: SVG icon silhouette + name + one-line role description

6. **World Events Teaser**
   - Brief atmospheric text about rotating contracts, pulsing event zones, storm anomalies
   - "The world doesn't wait for you" framing

### Media

Gallery page designed to look good with placeholder content. The "classified" redaction aesthetic turns missing assets into a feature.

**Sections:**

1. **Trailer**
   - Centered video embed container (16:9 aspect ratio)
   - Placeholder state: dark panel with "CLASSIFIED // FOOTAGE PENDING" in monospace, redaction bar overlays, crimson "COMING SOON" stamp
   - Ready state: YouTube or direct video embed

2. **Screenshots**
   - 3-column responsive grid (2 on tablet, 1 on mobile)
   - 6–9 placeholder slots
   - Each placeholder: dark panel with SVG scan lines, "INTEL REDACTED" label, redaction bars
   - Ready state: image with lightbox on click

3. **Concept Art**
   - Similar grid but 2-column for larger presentation
   - Same redacted placeholder treatment

4. **Press Kit**
   - Single dark card with download icon
   - Placeholder text: "Press kit available on request"

### FAQ / About

Clean, scannable, text-focused. Panel cuts soften to near-horizontal for readability.

**Sections:**

1. **FAQ Accordion**
   - Collapsible Q&A items with smooth height animation
   - Questions in serif bold, answers in body sans-serif
   - Crimson chevron indicator for open/closed state
   - Initial questions:
     - "What platforms is Contraband Skies on?" → PC and Mac at launch. iOS on the roadmap.
     - "How much does it cost?" → $50.
     - "When does it launch?" → July 1, 2026.
     - "Is this a battle royale?" → No. Persistent extraction warzone — always-on shared worlds across three maps, not short isolated matches.
     - "Can I play solo?" → Yes. Full PvE progression path exists. PvP accelerates gains but is never required.
     - "What about controller support?" → TBD placeholder.
     - "What are the system requirements?" → TBD placeholder.
     - "Will there be microtransactions?" → TBD placeholder.

2. **About / Studio**
   - Placeholder section for team/studio info
   - Dark panel with "More info coming soon"

3. **Community Links**
   - Grid of placeholder cards for Discord, X, YouTube, Reddit, etc.
   - Each card: platform icon slot + "Coming soon" label
   - Styled consistently with the noir system

4. **Contact**
   - Simple email/contact placeholder

## Responsive Behavior

- **Desktop (1024px+):** Full panel cuts, multi-column grids, side-by-side layouts
- **Tablet (768–1023px):** Reduced grid columns (3→2), panel cut angles soften slightly
- **Mobile (< 768px):** Single column, panel cuts reduce to 1–2 degree angles or go horizontal, hamburger nav, stacked cards, touch-friendly accordion targets (44px min)

## Build & Deployment

### Astro Configuration

- Static output mode (`output: 'static'`)
- Tailwind CSS integration via `@astrojs/tailwind`
- No SSR, no adapters — pure static HTML generation
- Asset optimization: Astro's built-in image optimization for any future assets

### Project Structure

```
/
├── src/
│   ├── layouts/
│   │   └── BaseLayout.astro          # HTML shell, meta, nav, footer, noise SVG defs
│   ├── components/
│   │   ├── Nav.astro                  # Fixed navigation bar
│   │   ├── Footer.astro               # Site footer with placeholder social links
│   │   ├── Hero.astro                 # Home page hero section
│   │   ├── PanelSection.astro         # Reusable diagonal-cut content panel
│   │   ├── FeatureCard.astro          # Icon + title + description card
│   │   ├── MapCard.astro              # World page map showcase card
│   │   ├── FactionCard.astro          # PvE faction card
│   │   ├── WeaponCard.astro           # Weapon family card
│   │   ├── MediaPlaceholder.astro     # "Classified" redacted media slot
│   │   ├── FaqItem.astro              # Accordion Q&A item
│   │   ├── StoreCta.astro             # Steam/Epic buy buttons
│   │   ├── CraftingTier.astro         # Crafting progression tier display
│   │   └── GameplayLoop.astro         # Circular gameplay loop diagram
│   ├── pages/
│   │   ├── index.astro                # Home
│   │   ├── game.astro                 # Game systems deep dive
│   │   ├── world.astro                # Maps, factions, lore
│   │   ├── media.astro                # Gallery and trailer
│   │   └── faq.astro                  # FAQ and about
│   ├── styles/
│   │   └── global.css                 # Tailwind directives, custom properties, noise filter, panel-cut utilities
│   └── assets/
│       └── svg/                       # SVG silhouette art files
├── public/
│   └── favicon.svg
├── .github/
│   └── workflows/
│       └── deploy.yml                 # Build + FTP deploy workflow
├── astro.config.mjs
├── tailwind.config.mjs
├── package.json
└── tsconfig.json
```

### GitHub Actions Workflow

**Trigger:** Push to `main` branch

**Steps:**
1. Checkout repository
2. Setup Node.js (v20)
3. Install dependencies (`npm ci`)
4. Build site (`npm run build`)
5. Deploy `dist/` via FTP using `SamKirkland/FTP-Deploy-Action@v4.3.5`

**Secrets used:**
- `FTP_SERVER` — FTP host address
- `FTP_USERNAME` — FTP login username
- `FTP_PASSWORD` — FTP login password

**FTP behavior:** Syncs only changed files. Cleans removed files from server. Deploys contents of `dist/` to server root.

## Content Source

All game content (feature descriptions, FAQ answers, map descriptions, faction details) is derived from the Ludos Feature Spec document. Page copy should be written in a voice consistent with the noir tone — terse, atmospheric, punchy — while remaining clear and informative.

## Placeholder Strategy

Since no game assets exist yet, all visual content slots use themed placeholders:

- **Media gallery:** "Classified" redacted panels with scan lines and monospace labels
- **Trailer:** Dark container with "FOOTAGE PENDING" redaction treatment
- **Screenshots:** Dark panels with "INTEL REDACTED" overlay
- **Social links:** Styled cards with "Coming soon" labels
- **Studio info:** "More info coming soon" dark panel

These placeholders are designed to feel intentional and on-brand — they reinforce the noir/contraband theme rather than looking like broken content.
