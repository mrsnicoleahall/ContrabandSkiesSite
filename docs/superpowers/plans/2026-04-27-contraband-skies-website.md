# Contraband Skies Website Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a 5-page Astro marketing site for Contraband Skies with a "Panel Cut Noir" visual system and GitHub Actions FTP deployment.

**Architecture:** Static Astro site with Tailwind CSS. Reusable `PanelSection` component creates the diagonal-cut noir rhythm. All visual identity comes from CSS custom properties, SVG noise textures, clip-path panel cuts, and inline SVG silhouette art. No external assets or fonts required.

**Tech Stack:** Astro 5, Tailwind CSS 4, vanilla JS, GitHub Actions + SamKirkland/FTP-Deploy-Action

**Spec:** `docs/superpowers/specs/2026-04-27-contraband-skies-website-design.md`

---

## Task 1: Project Scaffold & Tooling

**Files:**
- Create: `package.json`
- Create: `astro.config.mjs`
- Create: `tailwind.config.mjs`
- Create: `tsconfig.json`
- Create: `src/styles/global.css`
- Create: `.gitignore`
- Create: `.github/workflows/deploy.yml`
- Create: `public/favicon.svg`

- [ ] **Step 1: Initialize Astro project**

```bash
cd /Users/nicole/Documents/ContrabandSkies
npm create astro@latest . -- --template minimal --no-install --no-git --typescript strict
```

- [ ] **Step 2: Install dependencies**

```bash
npm install
npm install @astrojs/tailwind tailwindcss @tailwindcss/vite
```

- [ ] **Step 3: Configure Astro**

Replace `astro.config.mjs` with:

```js
import { defineConfig } from 'astro/config';
import tailwindcss from '@tailwindcss/vite';

export default defineConfig({
  vite: {
    plugins: [tailwindcss()],
  },
});
```

- [ ] **Step 4: Configure Tailwind**

Replace `src/styles/global.css` with:

```css
@import 'tailwindcss';

@theme {
  --color-base: #050505;
  --color-panel: #0a0a0a;
  --color-panel-alt: #111111;
  --color-surface: #1a1a1a;
  --color-border: #222222;
  --color-border-subtle: #333333;
  --color-text: #ffffff;
  --color-text-secondary: #cccccc;
  --color-text-muted: #999999;
  --color-text-dim: #666666;
  --color-text-faint: #444444;
  --color-crimson: #dc2626;
  --color-gold: #d4a017;
  --color-toxic: #22c55e;
  --font-display: Georgia, serif;
  --font-body: 'Helvetica Neue', Arial, sans-serif;
  --font-mono: ui-monospace, SFMono-Regular, 'SF Mono', Menlo, monospace;
}

html {
  background-color: var(--color-base);
  color: var(--color-text);
  font-family: var(--font-body);
  scroll-behavior: smooth;
}

@media (prefers-reduced-motion: reduce) {
  html { scroll-behavior: auto; }
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

- [ ] **Step 5: Create favicon**

Write `public/favicon.svg`:

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32">
  <rect width="32" height="32" rx="4" fill="#050505"/>
  <text x="4" y="23" font-family="Georgia,serif" font-size="22" font-weight="900" fill="#fff">C</text>
  <rect x="2" y="26" width="12" height="2" fill="#dc2626"/>
</svg>
```

- [ ] **Step 6: Create .gitignore**

```
node_modules/
dist/
.astro/
.superpowers/
.DS_Store
```

- [ ] **Step 7: Create GitHub Actions workflow**

Write `.github/workflows/deploy.yml`:

```yaml
name: Build & Deploy

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm

      - name: Install dependencies
        run: npm ci

      - name: Build
        run: npm run build

      - name: Deploy via FTP
        uses: SamKirkland/FTP-Deploy-Action@v4.3.5
        with:
          server: ${{ secrets.FTP_SERVER }}
          username: ${{ secrets.FTP_USERNAME }}
          password: ${{ secrets.FTP_PASSWORD }}
          local-dir: ./dist/
```

- [ ] **Step 8: Verify build works**

```bash
npm run build
```

Expected: Build succeeds, `dist/` directory created with `index.html`.

- [ ] **Step 9: Initialize git and commit**

```bash
git init
git add .
git commit -m "chore: scaffold Astro project with Tailwind and FTP deploy workflow"
```

---

## Task 2: Base Layout, Nav, and Footer

**Files:**
- Create: `src/layouts/BaseLayout.astro`
- Create: `src/components/Nav.astro`
- Create: `src/components/Footer.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create Nav component**

Write `src/components/Nav.astro`:

```astro
---
const currentPath = Astro.url.pathname;
const links = [
  { href: '/game', label: 'GAME' },
  { href: '/world', label: 'WORLD' },
  { href: '/media', label: 'MEDIA' },
  { href: '/faq', label: 'FAQ' },
];
---

<nav id="site-nav" class="fixed top-0 left-0 right-0 z-50 transition-colors duration-300 bg-base/80 backdrop-blur-sm">
  <div class="max-w-7xl mx-auto px-6 h-16 flex items-center justify-between">
    <a href="/" class="font-display text-lg font-bold tracking-tight uppercase">
      CONTRABAND <span class="text-crimson">SKIES</span>
    </a>

    <!-- Desktop links -->
    <div class="hidden md:flex items-center gap-7">
      {links.map(link => (
        <a
          href={link.href}
          class:list={[
            'font-mono text-[10px] tracking-[2px] uppercase transition-colors hover:text-crimson',
            currentPath === link.href || currentPath === link.href + '/' ? 'text-crimson' : 'text-text-dim',
          ]}
        >
          {link.label}
        </a>
      ))}
      <a
        href="#buy"
        class="font-mono text-[10px] tracking-[2px] uppercase bg-crimson text-white px-4 py-2 hover:bg-crimson/80 transition-colors"
      >
        BUY NOW
      </a>
    </div>

    <!-- Mobile hamburger -->
    <button
      id="nav-toggle"
      class="md:hidden w-8 h-8 flex flex-col justify-center items-center gap-1.5"
      aria-label="Toggle menu"
      aria-expanded="false"
    >
      <span class="nav-bar block w-5 h-px bg-white transition-all duration-300"></span>
      <span class="nav-bar block w-5 h-px bg-white transition-all duration-300"></span>
      <span class="nav-bar block w-5 h-px bg-white transition-all duration-300"></span>
    </button>
  </div>

  <!-- Mobile menu overlay -->
  <div
    id="mobile-menu"
    class="md:hidden fixed inset-0 top-16 bg-base/95 backdrop-blur-md flex flex-col items-center justify-center gap-8 opacity-0 pointer-events-none transition-opacity duration-300"
  >
    {links.map(link => (
      <a
        href={link.href}
        class:list={[
          'font-mono text-sm tracking-[3px] uppercase transition-colors hover:text-crimson',
          currentPath === link.href || currentPath === link.href + '/' ? 'text-crimson' : 'text-text-muted',
        ]}
      >
        {link.label}
      </a>
    ))}
    <a
      href="#buy"
      class="font-mono text-sm tracking-[3px] uppercase bg-crimson text-white px-6 py-3 hover:bg-crimson/80 transition-colors"
    >
      BUY NOW
    </a>
  </div>
</nav>

<script>
  const toggle = document.getElementById('nav-toggle');
  const menu = document.getElementById('mobile-menu');
  const nav = document.getElementById('site-nav');

  toggle?.addEventListener('click', () => {
    const open = menu?.classList.toggle('opacity-0');
    menu?.classList.toggle('pointer-events-none');
    toggle.setAttribute('aria-expanded', open ? 'false' : 'true');
  });

  window.addEventListener('scroll', () => {
    if (window.scrollY > 40) {
      nav?.classList.add('bg-base/95', 'border-b', 'border-border');
      nav?.classList.remove('bg-base/80');
    } else {
      nav?.classList.remove('bg-base/95', 'border-b', 'border-border');
      nav?.classList.add('bg-base/80');
    }
  });
</script>
```

- [ ] **Step 2: Create Footer component**

Write `src/components/Footer.astro`:

```astro
---
const socialSlots = ['Discord', 'X', 'YouTube', 'Reddit'];
const year = new Date().getFullYear();
---

<footer class="border-t border-crimson/30 bg-panel mt-0">
  <!-- SVG noise overlay -->
  <div class="absolute inset-0 pointer-events-none opacity-[0.04]">
    <svg width="100%" height="100%"><filter id="footerNoise"><feTurbulence baseFrequency="0.65" numOctaves="3" seed="9"/><feColorMatrix type="saturate" values="0"/></filter><rect width="100%" height="100%" filter="url(#footerNoise)"/></svg>
  </div>

  <div class="relative max-w-7xl mx-auto px-6 py-16">
    <!-- Social link placeholders -->
    <div class="flex flex-wrap justify-center gap-4 mb-10">
      {socialSlots.map(name => (
        <div class="flex items-center gap-2 px-4 py-2 border border-border-subtle text-text-faint font-mono text-[9px] tracking-[2px] uppercase">
          {name}
          <span class="text-text-dim">// SOON</span>
        </div>
      ))}
    </div>

    <!-- Bottom bar -->
    <div class="text-center">
      <div class="font-display text-sm font-bold tracking-tight uppercase mb-2">
        CONTRABAND <span class="text-crimson">SKIES</span>
      </div>
      <p class="font-mono text-[9px] text-text-faint tracking-[2px]">
        &copy; {year} CONTRABAND SKIES. ALL RIGHTS RESERVED.
      </p>
    </div>
  </div>
</footer>
```

- [ ] **Step 3: Create BaseLayout**

Write `src/layouts/BaseLayout.astro`:

```astro
---
import Nav from '../components/Nav.astro';
import Footer from '../components/Footer.astro';
import '../styles/global.css';

interface Props {
  title: string;
  description?: string;
}

const { title, description = 'Contraband Skies — a persistent airborne extraction shooter. Fly fast. Fight grounded. Extract everything.' } = Astro.props;
---

<!doctype html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <meta name="description" content={description} />
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <title>{title}</title>
  </head>
  <body class="bg-base text-text min-h-screen overflow-x-hidden">
    <!-- Shared SVG noise filter definition -->
    <svg class="hidden" aria-hidden="true">
      <defs>
        <filter id="noise">
          <feTurbulence baseFrequency="0.65" numOctaves="3" seed="7" />
          <feColorMatrix type="saturate" values="0" />
        </filter>
      </defs>
    </svg>

    <Nav />
    <main>
      <slot />
    </main>
    <Footer />

    <!-- Scroll-reveal observer -->
    <script>
      const observer = new IntersectionObserver(
        (entries) => {
          entries.forEach((entry) => {
            if (entry.isIntersecting) {
              entry.target.classList.add('revealed');
              observer.unobserve(entry.target);
            }
          });
        },
        { threshold: 0.1 }
      );

      document.querySelectorAll('.reveal').forEach((el) => observer.observe(el));
    </script>
  </body>
</html>
```

- [ ] **Step 4: Add reveal animation to global.css**

Append to `src/styles/global.css`:

```css
/* Scroll-reveal animation */
.reveal {
  opacity: 0;
  transform: translateY(24px);
  transition: opacity 0.6s ease-out, transform 0.6s ease-out;
}
.reveal.revealed {
  opacity: 1;
  transform: translateY(0);
}

/* Noise texture overlay utility */
.noise-overlay {
  position: relative;
}
.noise-overlay::before {
  content: '';
  position: absolute;
  inset: 0;
  background: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='200' height='200'%3E%3Cfilter id='n'%3E%3CfeTurbulence baseFrequency='0.65' numOctaves='3' seed='7'/%3E%3CfeColorMatrix type='saturate' values='0'/%3E%3C/filter%3E%3Crect width='200' height='200' filter='url(%23n)'/%3E%3C/svg%3E");
  opacity: 0.06;
  pointer-events: none;
  z-index: 1;
}
.noise-overlay > * {
  position: relative;
  z-index: 2;
}

/* Crimson glow pulse for CTAs */
@keyframes glow-pulse {
  0%, 100% { box-shadow: 0 0 8px rgba(220, 38, 38, 0.3); }
  50% { box-shadow: 0 0 20px rgba(220, 38, 38, 0.5); }
}
.glow-pulse {
  animation: glow-pulse 3s ease-in-out infinite;
}
```

- [ ] **Step 5: Update index.astro to use layout**

Replace `src/pages/index.astro` with:

```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
---

<BaseLayout title="Contraband Skies — Fly Fast. Fight Grounded. Extract Everything.">
  <section class="h-screen flex items-center justify-center">
    <h1 class="font-display text-6xl font-black uppercase tracking-tight text-center">
      CONTRABAND <span class="text-crimson">SKIES</span>
    </h1>
  </section>
</BaseLayout>
```

- [ ] **Step 6: Verify dev server and build**

```bash
npm run dev
```

Open `http://localhost:4321`. Verify: dark background, nav bar visible with working links, footer at bottom, title renders. Then:

```bash
npm run build
```

Expected: Build succeeds.

- [ ] **Step 7: Commit**

```bash
git add src/layouts/ src/components/Nav.astro src/components/Footer.astro src/pages/index.astro src/styles/global.css
git commit -m "feat: add base layout with nav, footer, noise texture, and scroll-reveal"
```

---

## Task 3: PanelSection and FeatureCard Components

**Files:**
- Create: `src/components/PanelSection.astro`
- Create: `src/components/FeatureCard.astro`

- [ ] **Step 1: Create PanelSection component**

Write `src/components/PanelSection.astro`:

```astro
---
interface Props {
  variant?: 'panel' | 'panel-alt' | 'base';
  cut?: 'top' | 'bottom' | 'both' | 'none';
  class?: string;
}

const { variant = 'panel', cut = 'both', class: className = '' } = Astro.props;

const bgColor = {
  panel: 'bg-panel',
  'panel-alt': 'bg-panel-alt',
  base: 'bg-base',
}[variant];

const clipPath = {
  top: 'polygon(0 4%, 100% 0, 100% 100%, 0 100%)',
  bottom: 'polygon(0 0, 100% 0, 100% 96%, 0 100%)',
  both: 'polygon(0 4%, 100% 0, 100% 96%, 0 100%)',
  none: 'none',
}[cut];
---

<section
  class:list={['relative py-24 px-6 noise-overlay', bgColor, className]}
  style={clipPath !== 'none' ? `clip-path: ${clipPath}; margin-top: -2rem; padding-top: 4rem; margin-bottom: -2rem; padding-bottom: 4rem;` : undefined}
>
  <div class="max-w-7xl mx-auto reveal">
    <slot />
  </div>
</section>
```

- [ ] **Step 2: Create FeatureCard component**

Write `src/components/FeatureCard.astro`:

```astro
---
interface Props {
  title: string;
  description: string;
  icon: string;
  accent?: string;
}

const { title, description, icon, accent = 'crimson' } = Astro.props;

const accentColor = {
  crimson: 'border-crimson/30 hover:border-crimson/60',
  gold: 'border-gold/30 hover:border-gold/60',
  toxic: 'border-toxic/30 hover:border-toxic/60',
}[accent] ?? 'border-crimson/30 hover:border-crimson/60';
---

<div class:list={['group border bg-surface/50 p-6 transition-all duration-300 hover:-translate-y-1', accentColor]}>
  <div class="text-3xl mb-4" set:html={icon} />
  <h3 class="font-display text-lg font-bold uppercase tracking-wide mb-2">{title}</h3>
  <p class="text-text-muted text-sm leading-relaxed">{description}</p>
</div>
```

- [ ] **Step 3: Verify with a quick smoke test on index page**

Temporarily add to `src/pages/index.astro` below the hero section (inside the layout slot):

```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
import PanelSection from '../components/PanelSection.astro';
import FeatureCard from '../components/FeatureCard.astro';
---

<BaseLayout title="Contraband Skies — Fly Fast. Fight Grounded. Extract Everything.">
  <section class="h-screen flex items-center justify-center">
    <h1 class="font-display text-6xl font-black uppercase tracking-tight text-center">
      CONTRABAND <span class="text-crimson">SKIES</span>
    </h1>
  </section>

  <PanelSection variant="panel-alt" cut="top">
    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
      <FeatureCard
        title="Flight"
        description="Test card"
        icon='<svg viewBox="0 0 40 40" class="w-10 h-10"><circle cx="20" cy="20" r="16" fill="none" stroke="#dc2626" stroke-width="1"/></svg>'
      />
    </div>
  </PanelSection>
</BaseLayout>
```

Run dev server and verify the panel section renders with a diagonal top cut, noise texture, and the card displays correctly. Then revert index.astro to the simple placeholder from Step 5 of Task 2 (we'll build the real home page in Task 5).

- [ ] **Step 4: Commit**

```bash
git add src/components/PanelSection.astro src/components/FeatureCard.astro
git commit -m "feat: add PanelSection and FeatureCard reusable components"
```

---

## Task 4: Remaining Shared Components

**Files:**
- Create: `src/components/StoreCta.astro`
- Create: `src/components/MapCard.astro`
- Create: `src/components/FactionCard.astro`
- Create: `src/components/WeaponCard.astro`
- Create: `src/components/MediaPlaceholder.astro`
- Create: `src/components/FaqItem.astro`
- Create: `src/components/CraftingTier.astro`
- Create: `src/components/GameplayLoop.astro`

- [ ] **Step 1: Create StoreCta component**

Write `src/components/StoreCta.astro`:

```astro
<div id="buy" class="flex flex-col sm:flex-row items-center justify-center gap-4">
  <a
    href="#"
    class="group flex items-center gap-3 bg-surface border border-border-subtle px-6 py-4 hover:border-text-dim transition-colors w-64"
  >
    <svg viewBox="0 0 24 24" class="w-6 h-6 fill-white shrink-0">
      <path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm-2 15l-5-5 1.41-1.41L10 14.17l7.59-7.59L19 8l-9 9z"/>
    </svg>
    <div>
      <div class="font-mono text-[9px] text-text-dim tracking-[2px] uppercase">Available on</div>
      <div class="font-display font-bold text-lg">Steam</div>
    </div>
  </a>
  <a
    href="#"
    class="group flex items-center gap-3 bg-surface border border-border-subtle px-6 py-4 hover:border-text-dim transition-colors w-64"
  >
    <svg viewBox="0 0 24 24" class="w-6 h-6 fill-white shrink-0">
      <path d="M3 3h18v18H3V3zm2 2v14h14V5H5z"/>
    </svg>
    <div>
      <div class="font-mono text-[9px] text-text-dim tracking-[2px] uppercase">Available on</div>
      <div class="font-display font-bold text-lg">Epic Games</div>
    </div>
  </a>
</div>
<p class="text-center font-mono text-[10px] text-text-dim tracking-[3px] uppercase mt-6">
  PC &amp; MAC — JULY 1, 2026 — $50
</p>
```

- [ ] **Step 2: Create MapCard component**

Write `src/components/MapCard.astro`:

```astro
---
interface Props {
  name: string;
  description: string;
  callout: string;
  accent?: 'crimson' | 'gold' | 'toxic';
  landmarks: string[];
  mapArt: string;
}

const { name, description, callout, accent = 'crimson', landmarks, mapArt } = Astro.props;

const accentClasses = {
  crimson: 'border-crimson/40 text-crimson',
  gold: 'border-gold/40 text-gold',
  toxic: 'border-toxic/40 text-toxic',
}[accent];
---

<div class:list={['border bg-surface/30 overflow-hidden', accentClasses.split(' ')[0]]}>
  <!-- Map art area -->
  <div class="h-48 bg-panel relative overflow-hidden flex items-center justify-center">
    <div class="absolute inset-0 opacity-[0.04]">
      <svg width="100%" height="100%"><filter id={`mapNoise-${name}`}><feTurbulence baseFrequency="0.5" numOctaves="3" seed="3"/><feColorMatrix type="saturate" values="0"/></filter><rect width="100%" height="100%" filter={`url(#mapNoise-${name})`}/></svg>
    </div>
    <div class="relative z-10" set:html={mapArt} />
  </div>

  <div class="p-6">
    <h3 class="font-display text-2xl font-bold uppercase tracking-wide mb-3">{name}</h3>
    <p class="text-text-muted text-sm leading-relaxed mb-4">{description}</p>

    <!-- Landmarks -->
    <div class="flex flex-wrap gap-2 mb-4">
      {landmarks.map(l => (
        <span class="font-mono text-[9px] text-text-dim tracking-[1px] uppercase border border-border-subtle px-2 py-1">{l}</span>
      ))}
    </div>

    <!-- Callout -->
    <p class:list={['font-mono text-[10px] tracking-[2px] uppercase', accentClasses.split(' ')[1]]}>
      {callout}
    </p>
  </div>
</div>
```

- [ ] **Step 3: Create FactionCard component**

Write `src/components/FactionCard.astro`:

```astro
---
interface Props {
  name: string;
  role: string;
  icon: string;
  accent?: string;
}

const { name, role, icon, accent = 'crimson' } = Astro.props;

const borderColor = {
  crimson: 'border-crimson/20',
  gold: 'border-gold/20',
  toxic: 'border-toxic/20',
}[accent] ?? 'border-crimson/20';
---

<div class:list={['flex items-start gap-4 border bg-surface/30 p-4', borderColor]}>
  <div class="w-10 h-10 shrink-0 flex items-center justify-center" set:html={icon} />
  <div>
    <h4 class="font-display text-sm font-bold uppercase tracking-wide">{name}</h4>
    <p class="text-text-muted text-sm mt-1">{role}</p>
  </div>
</div>
```

- [ ] **Step 4: Create WeaponCard component**

Write `src/components/WeaponCard.astro`:

```astro
---
interface Props {
  name: string;
  role: string;
  icon: string;
}

const { name, role, icon } = Astro.props;
---

<div class="border border-border-subtle bg-surface/30 p-5 hover:border-crimson/30 transition-colors">
  <div class="mb-3" set:html={icon} />
  <h4 class="font-display text-sm font-bold uppercase tracking-wide mb-1">{name}</h4>
  <p class="text-text-muted text-xs leading-relaxed">{role}</p>
</div>
```

- [ ] **Step 5: Create MediaPlaceholder component**

Write `src/components/MediaPlaceholder.astro`:

```astro
---
interface Props {
  label?: string;
  aspect?: 'video' | 'screenshot' | 'art';
}

const { label = 'INTEL REDACTED', aspect = 'screenshot' } = Astro.props;

const aspectClass = {
  video: 'aspect-video',
  screenshot: 'aspect-video',
  art: 'aspect-[4/3]',
}[aspect];
---

<div class:list={['relative bg-panel border border-border-subtle overflow-hidden', aspectClass]}>
  <!-- Scan lines -->
  <div class="absolute inset-0" style="background:repeating-linear-gradient(0deg,transparent,transparent 2px,rgba(255,255,255,0.015) 2px,rgba(255,255,255,0.015) 4px);"></div>

  <!-- Noise -->
  <div class="absolute inset-0 opacity-[0.06]">
    <svg width="100%" height="100%"><filter id="mediaNoise"><feTurbulence baseFrequency="0.8" numOctaves="2" seed="5"/><feColorMatrix type="saturate" values="0"/></filter><rect width="100%" height="100%" filter="url(#mediaNoise)"/></svg>
  </div>

  <!-- Redaction bars -->
  <div class="absolute top-1/3 left-[10%] right-[15%] h-3 bg-text-faint/20"></div>
  <div class="absolute top-1/2 left-[20%] right-[10%] h-2 bg-text-faint/15"></div>

  <!-- Label -->
  <div class="absolute inset-0 flex flex-col items-center justify-center gap-2">
    <span class="font-mono text-[10px] text-text-dim tracking-[3px] uppercase">{label}</span>
    <span class="font-mono text-[8px] text-crimson tracking-[4px] uppercase border border-crimson/40 px-3 py-1">
      CLASSIFIED
    </span>
  </div>
</div>
```

- [ ] **Step 6: Create FaqItem component**

Write `src/components/FaqItem.astro`:

```astro
---
interface Props {
  question: string;
  answer: string;
}

const { question, answer } = Astro.props;
const id = question.toLowerCase().replace(/[^a-z0-9]+/g, '-').replace(/(^-|-$)/g, '');
---

<div class="border-b border-border">
  <button
    class="faq-toggle w-full flex items-center justify-between py-5 text-left group"
    aria-expanded="false"
    aria-controls={`faq-${id}`}
  >
    <span class="font-display text-base font-bold pr-4">{question}</span>
    <svg class="w-4 h-4 text-crimson shrink-0 transition-transform duration-300 group-aria-expanded:rotate-180" viewBox="0 0 16 16" fill="none" stroke="currentColor" stroke-width="2">
      <path d="M4 6l4 4 4-4"/>
    </svg>
  </button>
  <div id={`faq-${id}`} class="faq-content overflow-hidden max-h-0 transition-all duration-300">
    <p class="text-text-muted text-sm leading-relaxed pb-5">{answer}</p>
  </div>
</div>

<script>
  document.querySelectorAll('.faq-toggle').forEach(btn => {
    btn.addEventListener('click', () => {
      const expanded = btn.getAttribute('aria-expanded') === 'true';
      btn.setAttribute('aria-expanded', expanded ? 'false' : 'true');
      const content = btn.nextElementSibling as HTMLElement;
      if (expanded) {
        content.style.maxHeight = '0';
      } else {
        content.style.maxHeight = content.scrollHeight + 'px';
      }
    });
  });
</script>
```

- [ ] **Step 7: Create CraftingTier component**

Write `src/components/CraftingTier.astro`:

```astro
---
interface Props {
  name: string;
  description: string;
  tier: number;
  totalTiers: number;
}

const { name, description, tier, totalTiers } = Astro.props;
const fillPercent = (tier / totalTiers) * 100;
---

<div class="flex items-start gap-4">
  <!-- Tier indicator bar -->
  <div class="w-1 h-16 bg-border-subtle rounded-full overflow-hidden shrink-0">
    <div class="w-full bg-crimson rounded-full" style={`height: ${fillPercent}%`}></div>
  </div>
  <div>
    <h4 class="font-display text-sm font-bold uppercase tracking-wide">{name}</h4>
    <p class="text-text-muted text-xs leading-relaxed mt-1">{description}</p>
  </div>
</div>
```

- [ ] **Step 8: Create GameplayLoop component**

Write `src/components/GameplayLoop.astro`:

```astro
---
const steps = [
  { label: 'Deploy', icon: '&#9654;', desc: 'Drop into the persistent world' },
  { label: 'Traverse', icon: '&#9992;', desc: 'Fly fast across the island' },
  { label: 'Fight', icon: '&#9876;', desc: 'Touch down and engage' },
  { label: 'Loot', icon: '&#9830;', desc: 'Gather contraband on foot' },
  { label: 'Extract', icon: '&#10003;', desc: 'Bank it — or lose it all' },
  { label: 'Recover', icon: '&#8635;', desc: 'Died? Race back to your corpse' },
];
---

<div class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-6 gap-4">
  {steps.map((step, i) => (
    <div class="text-center">
      <div class:list={[
        'w-14 h-14 mx-auto mb-3 flex items-center justify-center border rounded-full text-xl',
        step.label === 'Extract' ? 'border-crimson text-crimson' : 'border-border-subtle text-text-dim'
      ]}>
        <span set:html={step.icon} />
      </div>
      <div class="font-display text-xs font-bold uppercase tracking-wide mb-1">{step.label}</div>
      <p class="text-text-dim text-[11px] leading-snug">{step.desc}</p>
      {i < steps.length - 1 && (
        <div class="hidden lg:block font-mono text-text-faint text-xs mt-2">&rarr;</div>
      )}
    </div>
  ))}
</div>
```

- [ ] **Step 9: Build and verify**

```bash
npm run build
```

Expected: Build succeeds with no errors.

- [ ] **Step 10: Commit**

```bash
git add src/components/
git commit -m "feat: add StoreCta, MapCard, FactionCard, WeaponCard, MediaPlaceholder, FaqItem, CraftingTier, GameplayLoop components"
```

---

## Task 5: Home Page

**Files:**
- Create: `src/components/Hero.astro`
- Modify: `src/pages/index.astro`

- [ ] **Step 1: Create Hero component**

Write `src/components/Hero.astro`:

```astro
<section class="relative h-screen flex flex-col items-center justify-center overflow-hidden bg-base">
  <!-- Background cityscape silhouette -->
  <div class="absolute inset-0">
    <svg viewBox="0 0 1440 810" preserveAspectRatio="xMidYMax slice" class="w-full h-full">
      <!-- Noise texture -->
      <filter id="heroNoise"><feTurbulence baseFrequency="0.5" numOctaves="3" seed="2"/><feColorMatrix type="saturate" values="0"/></filter>
      <rect width="1440" height="810" fill="#050505"/>
      <rect width="1440" height="810" filter="url(#heroNoise)" opacity="0.05"/>

      <!-- Distant skyline -->
      <g fill="#0a0a0a">
        <rect x="100" y="450" width="40" height="360"/>
        <rect x="160" y="380" width="60" height="430"/>
        <rect x="240" y="420" width="35" height="390"/>
        <rect x="300" y="350" width="80" height="460"/>
        <rect x="400" y="400" width="50" height="410"/>
        <rect x="470" y="300" width="70" height="510"/>
        <rect x="560" y="360" width="45" height="450"/>
        <rect x="620" y="280" width="90" height="530"/>
        <rect x="730" y="340" width="55" height="470"/>
        <rect x="800" y="250" width="100" height="560"/>
        <rect x="920" y="320" width="60" height="490"/>
        <rect x="1000" y="370" width="75" height="440"/>
        <rect x="1090" y="290" width="85" height="520"/>
        <rect x="1190" y="350" width="50" height="460"/>
        <rect x="1260" y="400" width="70" height="410"/>
        <rect x="1350" y="330" width="90" height="480"/>
      </g>

      <!-- Closer buildings (darker, larger) -->
      <g fill="#080808">
        <rect x="0" y="550" width="120" height="260"/>
        <rect x="180" y="500" width="100" height="310"/>
        <rect x="350" y="520" width="130" height="290"/>
        <rect x="550" y="480" width="110" height="330"/>
        <rect x="750" y="530" width="90" height="280"/>
        <rect x="900" y="490" width="140" height="320"/>
        <rect x="1100" y="510" width="100" height="300"/>
        <rect x="1280" y="540" width="160" height="270"/>
      </g>

      <!-- Ground plane -->
      <rect x="0" y="720" width="1440" height="90" fill="#060606"/>
      <line x1="0" y1="720" x2="1440" y2="720" stroke="#111" stroke-width="1"/>

      <!-- Single crimson accent — distant neon sign -->
      <rect x="835" y="270" width="30" height="6" rx="1" fill="#dc2626" opacity="0.8"/>
      <rect x="835" y="270" width="30" height="6" rx="1" fill="#dc2626" opacity="0.15">
        <animate attributeName="opacity" values="0.15;0.3;0.15" dur="4s" repeatCount="indefinite"/>
      </rect>

      <!-- Rain -->
      <g stroke="white" stroke-width="0.5" opacity="0.04">
        <line x1="100" y1="0" x2="85" y2="810"/><line x1="250" y1="0" x2="235" y2="810"/>
        <line x1="400" y1="0" x2="385" y2="810"/><line x1="550" y1="0" x2="535" y2="810"/>
        <line x1="700" y1="0" x2="685" y2="810"/><line x1="850" y1="0" x2="835" y2="810"/>
        <line x1="1000" y1="0" x2="985" y2="810"/><line x1="1150" y1="0" x2="1135" y2="810"/>
        <line x1="1300" y1="0" x2="1285" y2="810"/><line x1="175" y1="0" x2="160" y2="810"/>
        <line x1="475" y1="0" x2="460" y2="810"/><line x1="775" y1="0" x2="760" y2="810"/>
        <line x1="1075" y1="0" x2="1060" y2="810"/>
      </g>

      <!-- Figure silhouette on rooftop -->
      <g transform="translate(720, 440)">
        <circle cx="0" cy="-30" r="10" fill="#080808"/>
        <rect x="-8" y="-20" width="16" height="35" fill="#080808"/>
        <polygon points="-12,-10 -20,25 -6,25" fill="#080808"/>
        <polygon points="12,-10 20,25 6,25" fill="#080808"/>
        <rect x="-12" y="15" width="8" height="25" fill="#080808"/>
        <rect x="4" y="15" width="8" height="25" fill="#080808"/>
      </g>
    </svg>
  </div>

  <!-- Title treatment -->
  <div class="relative z-10 text-center px-6">
    <div class="font-mono text-[10px] text-text-dim tracking-[6px] uppercase mb-6">PERSISTENT EXTRACTION WARZONE</div>
    <h1 class="font-display text-5xl sm:text-7xl lg:text-8xl font-black uppercase tracking-tight leading-[0.9]">
      CONTRABAND<br>
      <span class="text-crimson">SKIES</span>
    </h1>
    <div class="w-16 h-[3px] bg-crimson mx-auto mt-6 mb-6"></div>
    <p class="font-display text-lg sm:text-xl text-text-muted italic max-w-md mx-auto">
      Fly fast. Fight grounded. Extract everything.
    </p>
  </div>

  <!-- Scroll indicator -->
  <div class="absolute bottom-8 left-1/2 -translate-x-1/2 z-10">
    <div class="w-px h-10 bg-gradient-to-b from-transparent to-text-faint mx-auto mb-2"></div>
    <div class="font-mono text-[8px] text-text-faint tracking-[3px] uppercase">SCROLL</div>
  </div>
</section>
```

- [ ] **Step 2: Build the full home page**

Replace `src/pages/index.astro` with:

```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
import Hero from '../components/Hero.astro';
import PanelSection from '../components/PanelSection.astro';
import FeatureCard from '../components/FeatureCard.astro';
import GameplayLoop from '../components/GameplayLoop.astro';
import StoreCta from '../components/StoreCta.astro';
---

<BaseLayout title="Contraband Skies — Fly Fast. Fight Grounded. Extract Everything.">
  <Hero />

  <!-- Three Pillars -->
  <PanelSection variant="panel-alt" cut="top">
    <div class="text-center mb-12">
      <div class="font-mono text-[10px] text-crimson tracking-[4px] uppercase mb-3">CORE PILLARS</div>
      <h2 class="font-display text-3xl sm:text-4xl font-bold uppercase tracking-tight">What Defines the Fight</h2>
    </div>
    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
      <FeatureCard
        title="Flight"
        description="Omnidirectional flight rigs for rapid traversal, scouting, and evasion. Boost, dive, climb, disengage — the sky is your escape route, not your fortress."
        icon='<svg viewBox="0 0 40 40" class="w-10 h-10"><path d="M20 4 L36 28 L28 26 L20 36 L12 26 L4 28 Z" fill="none" stroke="#dc2626" stroke-width="1.5"/></svg>'
      />
      <FeatureCard
        title="Combat"
        description="All gunfights happen on the ground. Third-person, over-the-shoulder. Positioning, cover, and sightline control matter more than raw damage."
        icon='<svg viewBox="0 0 40 40" class="w-10 h-10"><rect x="4" y="18" width="32" height="6" rx="1" fill="none" stroke="#dc2626" stroke-width="1.5"/><rect x="10" y="14" width="4" height="14" rx="1" fill="none" stroke="#dc2626" stroke-width="1.5"/><circle cx="34" cy="21" r="3" fill="none" stroke="#dc2626" stroke-width="1.5"/></svg>'
      />
      <FeatureCard
        title="Extraction"
        description="Nothing is yours until you extract it. Extraction is visible, noisy, and contestable. Bank your contraband — or lose it all."
        icon='<svg viewBox="0 0 40 40" class="w-10 h-10"><polygon points="20,4 36,30 4,30" fill="none" stroke="#dc2626" stroke-width="1.5"/><line x1="20" y1="14" x2="20" y2="22" stroke="#dc2626" stroke-width="1.5"/><circle cx="20" cy="26" r="1.5" fill="#dc2626"/></svg>'
      />
    </div>
  </PanelSection>

  <!-- The Contraband Loop -->
  <PanelSection variant="panel" cut="both">
    <div class="text-center mb-12">
      <div class="font-mono text-[10px] text-text-dim tracking-[4px] uppercase mb-3">THE LOOP</div>
      <h2 class="font-display text-3xl sm:text-4xl font-bold uppercase tracking-tight">Deploy. Loot. Extract. Repeat.</h2>
      <p class="text-text-muted mt-4 max-w-xl mx-auto text-sm leading-relaxed">
        Freedom while traveling. Tension while looting. Fear while carrying high-value contraband. Triumph when extracting. Desperation when racing back to a corpse.
      </p>
    </div>
    <GameplayLoop />
  </PanelSection>

  <!-- Store CTAs -->
  <PanelSection variant="base" cut="top">
    <div class="text-center mb-10">
      <h2 class="font-display text-3xl sm:text-4xl font-bold uppercase tracking-tight">Enter the Warzone</h2>
    </div>
    <StoreCta />
  </PanelSection>
</BaseLayout>
```

- [ ] **Step 3: Run dev server, verify all sections**

```bash
npm run dev
```

Verify: hero with cityscape silhouette and title, three pillar cards, gameplay loop diagram, store CTAs with platform info. Check mobile responsiveness. Then build:

```bash
npm run build
```

Expected: Build succeeds.

- [ ] **Step 4: Commit**

```bash
git add src/components/Hero.astro src/pages/index.astro
git commit -m "feat: build complete home page with hero, pillars, gameplay loop, and store CTAs"
```

---

## Task 6: Game Page

**Files:**
- Create: `src/pages/game.astro`

- [ ] **Step 1: Build the game page**

Write `src/pages/game.astro`:

```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
import PanelSection from '../components/PanelSection.astro';
import FeatureCard from '../components/FeatureCard.astro';
import WeaponCard from '../components/WeaponCard.astro';
import CraftingTier from '../components/CraftingTier.astro';
---

<BaseLayout title="Game — Contraband Skies" description="Flight, combat, extraction, crafting, and insurance — every system in Contraband Skies explained.">

  <!-- Page header -->
  <section class="pt-28 pb-16 px-6 bg-base noise-overlay">
    <div class="max-w-7xl mx-auto text-center">
      <div class="font-mono text-[10px] text-crimson tracking-[4px] uppercase mb-3">SYSTEMS</div>
      <h1 class="font-display text-4xl sm:text-6xl font-black uppercase tracking-tight">The Game</h1>
      <p class="text-text-muted mt-4 max-w-lg mx-auto text-sm">Every system. Every risk. Every reward.</p>
    </div>
  </section>

  <!-- FLIGHT -->
  <PanelSection variant="panel-alt" cut="top">
    <div class="grid grid-cols-1 lg:grid-cols-2 gap-12 items-start">
      <div class="reveal">
        <div class="font-mono text-[10px] text-crimson tracking-[4px] uppercase mb-3">TRAVERSAL</div>
        <h2 class="font-display text-3xl font-bold uppercase tracking-tight mb-4">Flight System</h2>
        <p class="text-text-muted text-sm leading-relaxed mb-6">
          High-speed omnidirectional flight rigs are your primary way across the map. But the sky is not safe — grounded enemies can shoot you down. Flight is about repositioning, not domination.
        </p>
        <div class="border border-crimson/20 bg-surface/30 p-4">
          <div class="font-mono text-[9px] text-text-dim tracking-[2px] uppercase mb-2">ENERGY SYSTEM</div>
          <p class="text-text-muted text-xs leading-relaxed">
            Boosting drains energy. Normal flight regenerates it. Diving improves efficiency. Exhaustion weakens pursuit without making you helpless.
          </p>
        </div>
      </div>
      <div class="grid grid-cols-2 gap-3 reveal">
        <FeatureCard title="Cruise" description="Baseline directional flight" icon='<svg viewBox="0 0 32 32" class="w-8 h-8"><path d="M6 16h20M22 12l4 4-4 4" fill="none" stroke="#dc2626" stroke-width="1.5"/></svg>' />
        <FeatureCard title="Boost" description="High-speed energy burst" icon='<svg viewBox="0 0 32 32" class="w-8 h-8"><path d="M8 16h16M20 10l6 6-6 6M4 12l4 4-4 4" fill="none" stroke="#dc2626" stroke-width="1.5"/></svg>' />
        <FeatureCard title="Climb" description="Vertical gain for escape" icon='<svg viewBox="0 0 32 32" class="w-8 h-8"><path d="M16 26V6M12 10l4-4 4 4" fill="none" stroke="#dc2626" stroke-width="1.5"/></svg>' />
        <FeatureCard title="Dive" description="Rapid forced descent" icon='<svg viewBox="0 0 32 32" class="w-8 h-8"><path d="M16 6v20M12 22l4 4 4-4" fill="none" stroke="#dc2626" stroke-width="1.5"/></svg>' />
        <FeatureCard title="Brake" description="Responsive deceleration" icon='<svg viewBox="0 0 32 32" class="w-8 h-8"><rect x="10" y="10" width="12" height="12" rx="2" fill="none" stroke="#dc2626" stroke-width="1.5"/></svg>' />
        <FeatureCard title="Touchdown" description="Return to ground" icon='<svg viewBox="0 0 32 32" class="w-8 h-8"><path d="M16 6v16M10 18l6 6 6-6M6 26h20" fill="none" stroke="#dc2626" stroke-width="1.5"/></svg>' />
      </div>
    </div>
  </PanelSection>

  <!-- COMBAT -->
  <PanelSection variant="panel" cut="both">
    <div class="reveal">
      <div class="font-mono text-[10px] text-crimson tracking-[4px] uppercase mb-3">GUNPLAY</div>
      <h2 class="font-display text-3xl font-bold uppercase tracking-tight mb-2">Combat</h2>
      <p class="text-text-muted text-sm leading-relaxed mb-4 max-w-2xl">
        Third-person, over-the-shoulder. Shields plus health. Mid-range engagements dominate. Positioning, cover, and sightline control are as important as raw DPS. Players in the air are vulnerable — flight is not fire superiority.
      </p>
    </div>
    <div class="grid grid-cols-2 lg:grid-cols-4 gap-4 mt-8 reveal">
      <WeaponCard name="Pulse Carbines" role="Versatile mid-range workhorse. Reliable damage, balanced handling." icon='<svg viewBox="0 0 40 20" class="w-full h-8"><rect x="4" y="7" width="28" height="6" rx="1" fill="none" stroke="#888" stroke-width="1"/><rect x="10" y="5" width="3" height="10" rx="1" fill="none" stroke="#888" stroke-width="1"/></svg>' />
      <WeaponCard name="Rail Rifles" role="Long-range precision. High damage per shot, slow follow-up." icon='<svg viewBox="0 0 40 20" class="w-full h-8"><rect x="2" y="8" width="36" height="4" rx="1" fill="none" stroke="#888" stroke-width="1"/><circle cx="36" cy="10" r="3" fill="none" stroke="#888" stroke-width="1"/></svg>' />
      <WeaponCard name="Shotguns" role="Devastating up close. Forces aggressive positioning plays." icon='<svg viewBox="0 0 40 20" class="w-full h-8"><rect x="4" y="7" width="24" height="6" rx="1" fill="none" stroke="#888" stroke-width="1"/><rect x="28" y="5" width="8" height="10" rx="2" fill="none" stroke="#888" stroke-width="1"/></svg>' />
      <WeaponCard name="SMGs" role="High rate of fire, short-to-mid pressure. Punishes hesitation." icon='<svg viewBox="0 0 40 20" class="w-full h-8"><rect x="6" y="7" width="20" height="6" rx="1" fill="none" stroke="#888" stroke-width="1"/><rect x="12" y="5" width="3" height="10" rx="1" fill="none" stroke="#888" stroke-width="1"/><rect x="14" y="13" width="6" height="5" rx="1" fill="none" stroke="#888" stroke-width="1"/></svg>' />
    </div>
    <p class="font-mono text-[9px] text-text-faint tracking-[2px] uppercase mt-6 text-center reveal">
      EXPANDING POST-LAUNCH: STATUS WEAPONS &bull; SHIELD BREAKERS &bull; UTILITY LAUNCHERS
    </p>
  </PanelSection>

  <!-- EXTRACTION -->
  <PanelSection variant="panel-alt" cut="both">
    <div class="reveal">
      <div class="font-mono text-[10px] text-crimson tracking-[4px] uppercase mb-3">HIGH STAKES</div>
      <h2 class="font-display text-3xl font-bold uppercase tracking-tight mb-4">Extraction</h2>
      <p class="text-text-muted text-sm leading-relaxed mb-8 max-w-2xl">
        Nothing enters your permanent stash until you extract it. Extraction happens at zones, pads, or called uplinks — it takes time, it's visible, it's noisy, and it can be contested.
      </p>
    </div>
    <div class="grid grid-cols-1 md:grid-cols-2 gap-6 reveal">
      <div class="border border-border-subtle bg-surface/30 p-6">
        <h3 class="font-display text-sm font-bold uppercase tracking-wide text-crimson mb-3">Active Run Inventory</h3>
        <ul class="text-text-muted text-sm space-y-2">
          <li>&bull; Holds current-session loot</li>
          <li>&bull; Lost on death unless recovered from corpse</li>
          <li>&bull; Loot must be physically collected while grounded</li>
          <li class="text-crimson font-bold">&bull; At risk until extraction</li>
        </ul>
      </div>
      <div class="border border-border-subtle bg-surface/30 p-6">
        <h3 class="font-display text-sm font-bold uppercase tracking-wide mb-3">Permanent Stash</h3>
        <ul class="text-text-muted text-sm space-y-2">
          <li>&bull; Stores extracted loot and currencies</li>
          <li>&bull; Stores crafting materials and recipes</li>
          <li>&bull; Stores bio-bound weapons</li>
          <li class="text-text-dim">&bull; Safe between runs</li>
        </ul>
      </div>
    </div>
  </PanelSection>

  <!-- CRAFTING -->
  <PanelSection variant="panel" cut="both">
    <div class="reveal">
      <div class="font-mono text-[10px] text-crimson tracking-[4px] uppercase mb-3">LONG-TERM MASTERY</div>
      <h2 class="font-display text-3xl font-bold uppercase tracking-tight mb-4">Crafting</h2>
      <p class="text-text-muted text-sm leading-relaxed mb-8 max-w-2xl">
        You're not just finding guns. You're slowly assembling personally attuned weapons through rare materials, research, and fabrication. Crafted weapons are bio-bound — permanently yours, impossible to steal.
      </p>
    </div>

    <!-- Crafting pipeline -->
    <div class="grid grid-cols-1 sm:grid-cols-5 gap-4 mb-10 reveal">
      {['Material\nAcquisition', 'Research\nUnlocks', 'Fabrication', 'Bio-\nBinding', 'Insurance\nAttunement'].map((step, i) => (
        <div class="text-center">
          <div class:list={[
            'w-12 h-12 mx-auto mb-2 flex items-center justify-center border rounded text-sm font-bold font-display',
            i === 3 ? 'border-crimson text-crimson' : 'border-border-subtle text-text-dim'
          ]}>
            {i + 1}
          </div>
          <div class="font-mono text-[9px] text-text-muted tracking-[1px] uppercase whitespace-pre-line">{step}</div>
        </div>
      ))}
    </div>

    <!-- Weapon tiers -->
    <div class="reveal">
      <div class="font-mono text-[9px] text-text-dim tracking-[2px] uppercase mb-4">WEAPON TIERS</div>
      <div class="space-y-4">
        <CraftingTier tier={1} totalTiers={4} name="Fieldmade" description="Early crafted weapons. Low prestige, immediately useful. Your first taste of building." />
        <CraftingTier tier={2} totalTiers={4} name="Specialized" description="Mid-tier role-defining weapons. Built for specific playstyles and combat roles." />
        <CraftingTier tier={3} totalTiers={4} name="Signature" description="Long-grind build-around weapons. The centerpiece of a loadout." />
        <CraftingTier tier={4} totalTiers={4} name="Legacy" description="Very rare prestige projects. Major time investment. The weapons people remember." />
      </div>
    </div>
  </PanelSection>

  <!-- INSURANCE -->
  <PanelSection variant="panel-alt" cut="both">
    <div class="reveal">
      <div class="font-mono text-[10px] text-crimson tracking-[4px] uppercase mb-3">PROTECTION</div>
      <h2 class="font-display text-3xl font-bold uppercase tracking-tight mb-4">Insurance</h2>
      <p class="text-text-muted text-sm leading-relaxed mb-8 max-w-2xl">
        Insurance is not a cash shop convenience. It's a long-term progression goal earned through faction reputation, rare tokens, and repeated successful extractions.
      </p>
    </div>
    <div class="grid grid-cols-1 md:grid-cols-3 gap-6 reveal">
      <div class="border border-border-subtle bg-surface/30 p-6">
        <div class="font-mono text-[9px] text-text-dim tracking-[2px] uppercase mb-2">TIER 1</div>
        <h3 class="font-display text-lg font-bold uppercase mb-2">Partial Attunement</h3>
        <p class="text-text-muted text-sm">Faster recovery after loss. Reduced restoration cost. The first step toward protection.</p>
      </div>
      <div class="border border-crimson/30 bg-surface/30 p-6">
        <div class="font-mono text-[9px] text-crimson tracking-[2px] uppercase mb-2">TIER 2</div>
        <h3 class="font-display text-lg font-bold uppercase mb-2">Core Preservation</h3>
        <p class="text-text-muted text-sm">Weapon returns automatically after death with a meaningful timer. Attached mods may still be lost.</p>
      </div>
      <div class="border border-border-subtle bg-surface/30 p-6">
        <div class="font-mono text-[9px] text-text-dim tracking-[2px] uppercase mb-2">TIER 3</div>
        <h3 class="font-display text-lg font-bold uppercase mb-2">Legacy Policy</h3>
        <p class="text-text-muted text-sm">Highest-tier protection. Only for top-end long-grind builds. Takes a very long time to earn.</p>
      </div>
    </div>
  </PanelSection>

  <!-- DEATH & RECOVERY -->
  <PanelSection variant="panel" cut="top">
    <div class="reveal">
      <div class="font-mono text-[10px] text-crimson tracking-[4px] uppercase mb-3">CONSEQUENCES</div>
      <h2 class="font-display text-3xl font-bold uppercase tracking-tight mb-4">Death &amp; Recovery</h2>
      <p class="text-text-muted text-sm leading-relaxed mb-8 max-w-2xl">
        Death is not the end — it's a race. Your corpse creates a beacon in the live world. Redeploy, get there first, and recover your gear. Or don't, and face the cost.
      </p>
    </div>
    <div class="grid grid-cols-2 md:grid-cols-4 gap-4 reveal">
      {[
        { label: 'Death', desc: 'All non-protected items drop at your corpse' },
        { label: 'Beacon', desc: 'Corpse creates a recoverable beacon in the live world' },
        { label: 'Redeploy', desc: 'Re-enter the same world and race to your drop' },
        { label: 'Recover', desc: 'Get there first — or others loot it' },
      ].map(step => (
        <div class="text-center border border-border-subtle bg-surface/30 p-4">
          <div class="font-display text-sm font-bold uppercase tracking-wide mb-2">{step.label}</div>
          <p class="text-text-dim text-xs leading-snug">{step.desc}</p>
        </div>
      ))}
    </div>
    <p class="text-center font-display text-sm italic text-text-muted mt-8 reveal">
      "Desperation when racing back to a corpse."
    </p>
  </PanelSection>

</BaseLayout>
```

- [ ] **Step 2: Verify in browser**

```bash
npm run dev
```

Navigate to `/game`. Verify: page header, flight grid, combat/weapons, extraction comparison, crafting pipeline + tiers, insurance tiers, death flow. Check mobile. Then build:

```bash
npm run build
```

- [ ] **Step 3: Commit**

```bash
git add src/pages/game.astro
git commit -m "feat: build game page with flight, combat, extraction, crafting, insurance, and death sections"
```

---

## Task 7: World Page

**Files:**
- Create: `src/pages/world.astro`

- [ ] **Step 1: Build the world page**

Write `src/pages/world.astro`:

```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
import PanelSection from '../components/PanelSection.astro';
import MapCard from '../components/MapCard.astro';
import FactionCard from '../components/FactionCard.astro';
---

<BaseLayout title="World — Contraband Skies" description="Three distinct maps, five hostile factions, and a world that doesn't wait for you.">

  <!-- Page header -->
  <section class="pt-28 pb-16 px-6 bg-base noise-overlay">
    <div class="max-w-7xl mx-auto text-center">
      <div class="font-mono text-[10px] text-crimson tracking-[4px] uppercase mb-3">THE WARZONE</div>
      <h1 class="font-display text-4xl sm:text-6xl font-black uppercase tracking-tight">The World</h1>
      <p class="text-text-muted mt-4 max-w-lg mx-auto text-sm">Three distinct maps. Same noir grammar. Different combat rhythm.</p>
    </div>
  </section>

  <!-- MAPS -->
  <PanelSection variant="panel-alt" cut="top">
    <div class="font-mono text-[10px] text-text-dim tracking-[4px] uppercase mb-8 reveal">THEATRES OF OPERATION</div>
    <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
      <div class="reveal">
        <MapCard
          name="Urban"
          description="Dense city core. Towers, alleys, elevated rail, neon avenues, plazas, and warehouses. Vertical combat at its most intense — rooftops become both refuge and killing ground."
          callout="Strongest vertical escape play and rooftop ambush potential"
          accent="gold"
          landmarks={['Rooftops', 'Alleys', 'Elevated Rail', 'Neon Avenues', 'Towers', 'Warehouses']}
          mapArt='<svg viewBox="0 0 200 100" class="w-full h-24"><g fill="none" stroke="#d4a017" stroke-width="0.5" opacity="0.6"><rect x="10" y="30" width="20" height="70"/><rect x="35" y="15" width="30" height="85"/><rect x="70" y="40" width="15" height="60"/><rect x="90" y="10" width="25" height="90"/><rect x="120" y="35" width="20" height="65"/><rect x="145" y="20" width="35" height="80"/><line x1="0" y1="98" x2="200" y2="98"/></g></svg>'
        />
      </div>
      <div class="reveal">
        <MapCard
          name="Rural"
          description="Open farmland, ridgelines, creek beds, timber edges, mills, and quarries. Longer sightlines mean more exposure during flight and stronger marksman pressure on the ground."
          callout="Punishes exposed movement"
          accent="crimson"
          landmarks={['Ridgelines', 'Creek Beds', 'Timber Edges', 'Mills', 'Quarries', 'Farmland']}
          mapArt='<svg viewBox="0 0 200 100" class="w-full h-24"><g fill="none" stroke="#dc2626" stroke-width="0.5" opacity="0.6"><path d="M0,70 Q50,40 100,65 T200,55"/><path d="M0,80 Q60,60 120,75 T200,70"/><line x1="40" y1="45" x2="40" y2="70"/><line x1="42" y1="45" x2="48" y2="55"/><line x1="38" y1="45" x2="32" y2="55"/><line x1="140" y1="55" x2="140" y2="72"/><line x1="142" y1="55" x2="148" y2="63"/><line x1="138" y1="55" x2="132" y2="63"/><line x1="0" y1="98" x2="200" y2="98"/></g></svg>'
        />
      </div>
      <div class="reveal">
        <MapCard
          name="Suburbs"
          description="Housing blocks, school zones, strip malls, drainage channels, cul-de-sacs, and backyard routes. Medium sightlines with endless flanking lanes for house-to-house pressure."
          callout="Ambushes, lane control, house-to-house gunfights"
          accent="crimson"
          landmarks={['Housing Blocks', 'Strip Malls', 'Drainage Channels', 'Cul-de-sacs', 'School Zones', 'Backyard Routes']}
          mapArt='<svg viewBox="0 0 200 100" class="w-full h-24"><g fill="none" stroke="#999" stroke-width="0.5" opacity="0.6"><rect x="15" y="55" width="25" height="20"/><polygon points="15,55 27,42 40,55"/><rect x="55" y="50" width="30" height="25"/><polygon points="55,50 70,38 85,50"/><rect x="100" y="55" width="20" height="20"/><polygon points="100,55 110,44 120,55"/><rect x="135" y="48" width="35" height="27"/><polygon points="135,48 152,36 170,48"/><line x1="0" y1="98" x2="200" y2="98"/></g></svg>'
        />
      </div>
    </div>
  </PanelSection>

  <!-- FACTIONS -->
  <PanelSection variant="panel" cut="both">
    <div class="reveal">
      <div class="font-mono text-[10px] text-crimson tracking-[4px] uppercase mb-3">HOSTILES</div>
      <h2 class="font-display text-3xl font-bold uppercase tracking-tight mb-2">PvE Factions</h2>
      <p class="text-text-muted text-sm leading-relaxed mb-8 max-w-2xl">
        The world is not empty between players. Five factions control territory, guard resources, and hunt anyone who enters their domain.
      </p>
    </div>
    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 reveal">
      <FactionCard
        name="Scav Raiders"
        role="Baseline armed roamers. Camp defenders, convoy escorts, and opportunistic hunters."
        icon='<svg viewBox="0 0 40 40" class="w-10 h-10"><circle cx="20" cy="14" r="6" fill="none" stroke="#888" stroke-width="1.5"/><path d="M10 34 C10 26 30 26 30 34" fill="none" stroke="#888" stroke-width="1.5"/></svg>'
      />
      <FactionCard
        name="Blacksite Security"
        role="Tougher tech guardians. They defend research zones and high-value installations."
        icon='<svg viewBox="0 0 40 40" class="w-10 h-10"><rect x="10" y="8" width="20" height="24" rx="2" fill="none" stroke="#888" stroke-width="1.5"/><line x1="15" y1="16" x2="25" y2="16" stroke="#888" stroke-width="1.5"/><line x1="15" y1="22" x2="25" y2="22" stroke="#888" stroke-width="1.5"/></svg>'
      />
      <FactionCard
        name="Bioforms"
        role="Mutated organic threats. Source of rare biochemical crafting components."
        accent="toxic"
        icon='<svg viewBox="0 0 40 40" class="w-10 h-10"><circle cx="20" cy="20" r="10" fill="none" stroke="#22c55e" stroke-width="1.5"/><circle cx="16" cy="17" r="2" fill="#22c55e"/><circle cx="24" cy="17" r="2" fill="#22c55e"/><path d="M14 25 Q20 30 26 25" fill="none" stroke="#22c55e" stroke-width="1.5"/></svg>'
      />
      <FactionCard
        name="Stormbound Drones"
        role="Airborne machine patrols. They own dangerous skies and make flight costly in their territory."
        icon='<svg viewBox="0 0 40 40" class="w-10 h-10"><rect x="14" y="16" width="12" height="8" rx="1" fill="none" stroke="#888" stroke-width="1.5"/><line x1="8" y1="20" x2="14" y2="18" stroke="#888" stroke-width="1.5"/><line x1="26" y1="18" x2="32" y2="20" stroke="#888" stroke-width="1.5"/><circle cx="10" cy="20" r="2" fill="none" stroke="#888" stroke-width="1"/><circle cx="30" cy="20" r="2" fill="none" stroke="#888" stroke-width="1"/></svg>'
      />
      <FactionCard
        name="Rogue Harvesters"
        role="Extraction-focused AI competitors. They race you to resource sites and extract for themselves."
        icon='<svg viewBox="0 0 40 40" class="w-10 h-10"><polygon points="20,8 32,28 8,28" fill="none" stroke="#888" stroke-width="1.5"/><line x1="20" y1="16" x2="20" y2="22" stroke="#888" stroke-width="1.5"/><circle cx="20" cy="25" r="1.5" fill="#888"/></svg>'
      />
    </div>
  </PanelSection>

  <!-- WORLD EVENTS -->
  <PanelSection variant="panel-alt" cut="top">
    <div class="text-center reveal">
      <div class="font-mono text-[10px] text-text-dim tracking-[4px] uppercase mb-3">PERSISTENT</div>
      <h2 class="font-display text-3xl font-bold uppercase tracking-tight mb-4">The World Doesn't Wait</h2>
      <p class="text-text-muted text-sm leading-relaxed max-w-xl mx-auto">
        Resource nodes replenish. AI factions roam and defend. Contracts rotate. Corpse drops expire. Event zones pulse on and off. Storm anomalies tear through the map. Whether you're online or not, the warzone keeps moving.
      </p>
    </div>
  </PanelSection>

</BaseLayout>
```

- [ ] **Step 2: Verify and build**

```bash
npm run dev
```

Navigate to `/world`. Verify: three map cards with SVG art and correct accents (gold for Urban), five faction cards (toxic green for Bioforms), world events section. Check mobile. Build:

```bash
npm run build
```

- [ ] **Step 3: Commit**

```bash
git add src/pages/world.astro
git commit -m "feat: build world page with three maps, five PvE factions, and world events"
```

---

## Task 8: Media Page

**Files:**
- Create: `src/pages/media.astro`

- [ ] **Step 1: Build the media page**

Write `src/pages/media.astro`:

```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
import PanelSection from '../components/PanelSection.astro';
import MediaPlaceholder from '../components/MediaPlaceholder.astro';
---

<BaseLayout title="Media — Contraband Skies" description="Screenshots, trailers, and concept art from Contraband Skies.">

  <!-- Page header -->
  <section class="pt-28 pb-16 px-6 bg-base noise-overlay">
    <div class="max-w-7xl mx-auto text-center">
      <div class="font-mono text-[10px] text-crimson tracking-[4px] uppercase mb-3">INTELLIGENCE</div>
      <h1 class="font-display text-4xl sm:text-6xl font-black uppercase tracking-tight">Media</h1>
      <p class="text-text-muted mt-4 max-w-lg mx-auto text-sm">Classified assets. Clearance pending.</p>
    </div>
  </section>

  <!-- TRAILER -->
  <PanelSection variant="panel-alt" cut="top">
    <div class="reveal">
      <div class="font-mono text-[10px] text-text-dim tracking-[4px] uppercase mb-6">FOOTAGE</div>
      <div class="max-w-4xl mx-auto">
        <MediaPlaceholder label="CLASSIFIED // FOOTAGE PENDING" aspect="video" />
      </div>
    </div>
  </PanelSection>

  <!-- SCREENSHOTS -->
  <PanelSection variant="panel" cut="both">
    <div class="reveal">
      <div class="font-mono text-[10px] text-text-dim tracking-[4px] uppercase mb-6">FIELD CAPTURES</div>
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
        <MediaPlaceholder label="INTEL REDACTED" aspect="screenshot" />
        <MediaPlaceholder label="INTEL REDACTED" aspect="screenshot" />
        <MediaPlaceholder label="INTEL REDACTED" aspect="screenshot" />
        <MediaPlaceholder label="INTEL REDACTED" aspect="screenshot" />
        <MediaPlaceholder label="INTEL REDACTED" aspect="screenshot" />
        <MediaPlaceholder label="INTEL REDACTED" aspect="screenshot" />
      </div>
    </div>
  </PanelSection>

  <!-- CONCEPT ART -->
  <PanelSection variant="panel-alt" cut="both">
    <div class="reveal">
      <div class="font-mono text-[10px] text-text-dim tracking-[4px] uppercase mb-6">RECONNAISSANCE SKETCHES</div>
      <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
        <MediaPlaceholder label="ASSET CLASSIFIED" aspect="art" />
        <MediaPlaceholder label="ASSET CLASSIFIED" aspect="art" />
        <MediaPlaceholder label="ASSET CLASSIFIED" aspect="art" />
        <MediaPlaceholder label="ASSET CLASSIFIED" aspect="art" />
      </div>
    </div>
  </PanelSection>

  <!-- PRESS KIT -->
  <PanelSection variant="panel" cut="top">
    <div class="text-center reveal">
      <div class="border border-border-subtle bg-surface/30 inline-block px-10 py-8">
        <svg viewBox="0 0 32 32" class="w-8 h-8 mx-auto mb-3">
          <rect x="6" y="4" width="20" height="24" rx="2" fill="none" stroke="#666" stroke-width="1.5"/>
          <line x1="10" y1="12" x2="22" y2="12" stroke="#666" stroke-width="1"/>
          <line x1="10" y1="16" x2="22" y2="16" stroke="#666" stroke-width="1"/>
          <line x1="10" y1="20" x2="18" y2="20" stroke="#666" stroke-width="1"/>
        </svg>
        <div class="font-display text-sm font-bold uppercase tracking-wide mb-1">Press Kit</div>
        <p class="font-mono text-[9px] text-text-dim tracking-[2px] uppercase">AVAILABLE ON REQUEST</p>
      </div>
    </div>
  </PanelSection>

</BaseLayout>
```

- [ ] **Step 2: Verify and build**

```bash
npm run dev
```

Navigate to `/media`. Verify: trailer placeholder (video aspect), 6 screenshot placeholders (3-col grid), 4 concept art placeholders (2-col), press kit card. All should show redaction treatment. Build:

```bash
npm run build
```

- [ ] **Step 3: Commit**

```bash
git add src/pages/media.astro
git commit -m "feat: build media page with classified placeholder treatment for all assets"
```

---

## Task 9: FAQ Page

**Files:**
- Create: `src/pages/faq.astro`

- [ ] **Step 1: Build the FAQ page**

Write `src/pages/faq.astro`:

```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
import PanelSection from '../components/PanelSection.astro';
import FaqItem from '../components/FaqItem.astro';

const faqs = [
  {
    question: 'What platforms is Contraband Skies on?',
    answer: 'PC and Mac at launch on July 1, 2026. iOS is on the roadmap for a future release.',
  },
  {
    question: 'How much does it cost?',
    answer: '$50 at launch. One purchase, full game.',
  },
  {
    question: 'When does it launch?',
    answer: 'July 1, 2026. Available on Steam and Epic Games Store.',
  },
  {
    question: 'Is this a battle royale?',
    answer: 'No. Contraband Skies is a persistent extraction warzone — always-on shared worlds across three distinct maps. Players drop in and out of the same live ecosystem. No shrinking circles, no short isolated matches.',
  },
  {
    question: 'Can I play solo?',
    answer: 'Yes. A full PvE progression path exists — you can farm materials, unlock schematics, forge bio-bound weapons, and eventually insure them without ever engaging in PvP. The PvE route is intentionally slower, but everything is achievable.',
  },
  {
    question: 'What about controller support?',
    answer: 'Details on controller support will be announced closer to launch.',
  },
  {
    question: 'What are the system requirements?',
    answer: 'System requirements will be published ahead of launch. Stay tuned.',
  },
  {
    question: 'Will there be microtransactions?',
    answer: 'Details on post-launch monetization will be shared before release.',
  },
];

const socialSlots = [
  { name: 'Discord', icon: '<svg viewBox="0 0 24 24" class="w-5 h-5"><path d="M20.3 4.7a19.5 19.5 0 00-4.8-1.5 14.2 14.2 0 00-.6 1.3 18 18 0 00-5.4 0 13 13 0 00-.7-1.3A19.4 19.4 0 004 4.7 20.6 20.6 0 00.5 17a19.7 19.7 0 006 3 14.8 14.8 0 001.3-2.1 12.7 12.7 0 01-2-.9l.5-.4a14 14 0 0012 0l.5.4a12.8 12.8 0 01-2 .9A14.8 14.8 0 0018 20a19.6 19.6 0 006-3A20.5 20.5 0 0020.3 4.7zM8.3 14.5c-1 0-2-.9-2-2.1s.8-2.1 2-2.1 2 1 2 2.1-.8 2.1-2 2.1zm7.4 0c-1 0-2-.9-2-2.1s.8-2.1 2-2.1 2 1 2 2.1-.8 2.1-2 2.1z" fill="currentColor"/></svg>' },
  { name: 'X', icon: '<svg viewBox="0 0 24 24" class="w-5 h-5"><path d="M18.2 2h3.5l-7.6 8.7L23 22h-7l-5.5-7.1L4.6 22H1l8.1-9.3L.8 2h7.2l4.9 6.5L18.2 2zm-1.2 18h2L6.9 4H4.7l12.3 16z" fill="currentColor"/></svg>' },
  { name: 'YouTube', icon: '<svg viewBox="0 0 24 24" class="w-5 h-5"><path d="M23.5 6.2a3 3 0 00-2.1-2.1C19.5 3.5 12 3.5 12 3.5s-7.5 0-9.4.6A3 3 0 00.5 6.2 31.3 31.3 0 000 12a31.3 31.3 0 00.5 5.8 3 3 0 002.1 2.1c1.9.6 9.4.6 9.4.6s7.5 0 9.4-.6a3 3 0 002.1-2.1A31.3 31.3 0 0024 12a31.3 31.3 0 00-.5-5.8zM9.6 15.6V8.4L15.8 12l-6.2 3.6z" fill="currentColor"/></svg>' },
  { name: 'Reddit', icon: '<svg viewBox="0 0 24 24" class="w-5 h-5"><circle cx="12" cy="12" r="10" fill="none" stroke="currentColor" stroke-width="1.5"/><circle cx="8.5" cy="11" r="1.5" fill="currentColor"/><circle cx="15.5" cy="11" r="1.5" fill="currentColor"/><path d="M8 15c1.3 1 2.5 1.5 4 1.5s2.7-.5 4-1.5" fill="none" stroke="currentColor" stroke-width="1.2"/></svg>' },
];
---

<BaseLayout title="FAQ — Contraband Skies" description="Frequently asked questions about Contraband Skies — platforms, price, release date, and more.">

  <!-- Page header -->
  <section class="pt-28 pb-16 px-6 bg-base noise-overlay">
    <div class="max-w-7xl mx-auto text-center">
      <div class="font-mono text-[10px] text-crimson tracking-[4px] uppercase mb-3">BRIEFING</div>
      <h1 class="font-display text-4xl sm:text-6xl font-black uppercase tracking-tight">FAQ</h1>
      <p class="text-text-muted mt-4 max-w-lg mx-auto text-sm">Everything you need to know before you drop in.</p>
    </div>
  </section>

  <!-- FAQ ACCORDION -->
  <PanelSection variant="panel-alt" cut="top">
    <div class="max-w-3xl mx-auto reveal">
      {faqs.map(faq => (
        <FaqItem question={faq.question} answer={faq.answer} />
      ))}
    </div>
  </PanelSection>

  <!-- ABOUT / STUDIO -->
  <PanelSection variant="panel" cut="both">
    <div class="text-center reveal">
      <div class="font-mono text-[10px] text-text-dim tracking-[4px] uppercase mb-3">THE STUDIO</div>
      <h2 class="font-display text-3xl font-bold uppercase tracking-tight mb-4">About</h2>
      <div class="border border-border-subtle bg-surface/30 inline-block px-10 py-8">
        <p class="font-mono text-[10px] text-text-dim tracking-[2px] uppercase">MORE INFO COMING SOON</p>
      </div>
    </div>
  </PanelSection>

  <!-- COMMUNITY LINKS -->
  <PanelSection variant="panel-alt" cut="both">
    <div class="reveal">
      <div class="text-center mb-8">
        <div class="font-mono text-[10px] text-text-dim tracking-[4px] uppercase mb-3">COMMS</div>
        <h2 class="font-display text-3xl font-bold uppercase tracking-tight">Community</h2>
      </div>
      <div class="grid grid-cols-2 md:grid-cols-4 gap-4 max-w-2xl mx-auto">
        {socialSlots.map(slot => (
          <div class="border border-border-subtle bg-surface/30 p-5 text-center">
            <div class="text-text-dim mx-auto mb-3 flex justify-center" set:html={slot.icon} />
            <div class="font-mono text-[10px] text-text-muted tracking-[1px] uppercase mb-1">{slot.name}</div>
            <div class="font-mono text-[8px] text-text-faint tracking-[2px] uppercase">COMING SOON</div>
          </div>
        ))}
      </div>
    </div>
  </PanelSection>

  <!-- CONTACT -->
  <PanelSection variant="panel" cut="top">
    <div class="text-center reveal">
      <div class="font-mono text-[10px] text-text-dim tracking-[4px] uppercase mb-3">REACH OUT</div>
      <h2 class="font-display text-2xl font-bold uppercase tracking-tight mb-4">Contact</h2>
      <p class="text-text-muted text-sm">
        For press, partnership, or general inquiries —
        <span class="font-mono text-[10px] text-text-dim tracking-[2px] uppercase border border-border-subtle px-3 py-1 ml-1">EMAIL PLACEHOLDER</span>
      </p>
    </div>
  </PanelSection>

</BaseLayout>
```

- [ ] **Step 2: Verify and build**

```bash
npm run dev
```

Navigate to `/faq`. Verify: accordion opens/closes with crimson chevron, 8 questions render, studio placeholder, 4 community cards, contact section. Check mobile. Build:

```bash
npm run build
```

- [ ] **Step 3: Commit**

```bash
git add src/pages/faq.astro
git commit -m "feat: build FAQ page with accordion, studio placeholder, community links, and contact"
```

---

## Task 10: Final Polish and Full Verification

**Files:**
- Modify: `src/styles/global.css` (if needed)
- Modify: any components with issues found during verification

- [ ] **Step 1: Run full build**

```bash
npm run build
```

Expected: Clean build, no warnings, no errors.

- [ ] **Step 2: Full site walkthrough in dev mode**

```bash
npm run dev
```

Verify each page in browser at desktop and mobile widths:

1. **Home** (`/`) — Hero renders with cityscape, title, tagline. Three pillars show. Gameplay loop diagram. Store CTAs.
2. **Game** (`/game`) — All 6 sections render. Flight grid, weapon cards, extraction comparison, crafting pipeline, insurance tiers, death flow.
3. **World** (`/world`) — Three map cards with correct accents. Five faction cards. World events.
4. **Media** (`/media`) — Trailer placeholder, 6 screenshot slots, 4 concept art slots, press kit.
5. **FAQ** (`/faq`) — Accordion works. Studio placeholder. Community cards. Contact.
6. **Nav** — Active page highlighted crimson. Mobile hamburger works. Scroll behavior changes opacity.
7. **Footer** — Renders on all pages with social placeholders and copyright.
8. **Scroll reveals** — Sections fade in on scroll across all pages.
9. **Panel cuts** — Diagonal cuts visible between sections, no content clipping issues.

- [ ] **Step 3: Fix any issues found**

Address any rendering, layout, or interaction issues discovered during walkthrough.

- [ ] **Step 4: Final commit**

```bash
git add -A
git commit -m "feat: complete Contraband Skies marketing site — all 5 pages, panel-cut noir design system, FTP deploy pipeline"
```
