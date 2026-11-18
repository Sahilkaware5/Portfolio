# Sahil Kaware — Portfolio

> **AI Engineer in the Making.** A single-file, dependency-free portfolio that presents my projects, experience, certifications and learning journey, with a working contact form and an AI chat assistant.

Aspiring AI engineer (BCA → MCA) focused on Artificial Intelligence, Machine Learning, Generative AI and software development. This site is where I show what I build and how I think.

---

## Table of contents

1. [Overview](#overview)
2. [Sections](#sections)
3. [Projects featured](#projects-featured)
4. [Experience and certifications](#experience-and-certifications)
5. [Features](#features)
6. [Tech stack](#tech-stack)
7. [How the page is structured](#how-the-page-is-structured)
8. [Configuration](#configuration)
9. [Running and deploying](#running-and-deploying)
10. [Backend pieces (Supabase)](#backend-pieces-supabase)
11. [Accessibility and performance](#accessibility-and-performance)
12. [Customizing](#customizing)
13. [Contact](#contact)

---

## Overview

The whole site lives in one file, `portfolio-site.html`. HTML, CSS and JavaScript are inline, there is no build step and no framework, and the animated visuals are drawn with CSS and SVG instead of a 3D library. Open the file in a browser and it works.

Design direction: a dark, technical look with cyan accents, monospaced labels, glass-style panels and subtle motion, plus a light theme.

## Sections

| # | Section | What it shows |
|---|---------|---------------|
| – | **Hero** | Headline "Building intelligent solutions. Engineering the future.", an animated orbital core, and key actions |
| – | **About** | Who I am, my BCA → MCA journey, a portrait frame, and four guiding principles (Think → Build → …) |
| – | **At a glance** | A fractured-glass "shard" layout summarising education, current focus, stack, projects, experience and contact |
| – | **Skills** | An interactive "technology universe" of orbiting skills, with chips naming them all on narrow screens |
| – | **Projects** | Filterable project cards (All / AI·ML / Web / Systems) that fade in a real screenshot on hover |
| – | **Showcase** | A moving "project wall" with a switchable deck view |
| – | **GitHub** | Recent open-source activity (appears when data is available) |
| 04 | **Experience** | Xceed Beyond internship, with scope of work and viewable documents |
| 05 | **Certifications** | Certificate cards with thumbnails and a full-size viewer |
| 06 | **Learning Lab** | A five-step track: Exploring → Learning → Building → Experimenting → Improving, with the current focus highlighted |
| 07 | **Contact** | A validated contact form plus email, LinkedIn and GitHub links |
| – | **Footer** | Navigation, contact links, and a large name wordmark whose glow follows the cursor |

Field notes (a blog) and Testimonials sections also exist but stay hidden until content is added to the config.

## Projects featured

Each project has its own detail page (overview, role, technologies, key features, an architecture diagram that lights up in sequence, and a process timeline).

| Project | Category | Summary | Stack |
|---|---|---|---|
| **SignBridge** | AI / Computer Vision | Recognition studio for isolated Indian Sign Language signs. MediaPipe landmarks feed a pretrained ST-GCN model, the user confirms each prediction, and the text can be spoken. | Python, Computer Vision, ML |
| **PyPilot AI** | Generative AI | Local-first Python workspace. Ollama generates solutions, Docker tests them in isolation, the AI gets up to two repair attempts, and GitHub publishing waits for approval. | Python, Streamlit, Ollama, Docker, GitHub API |
| **CampusX** | Full-stack / Database | College management platform with role-based dashboards, student and faculty workflows and centralized data. | Supabase, JavaScript, SQL, Row-level security, Edge Functions |
| **Responda** | Web development | Browser-based disaster response platform. Citizens report emergencies with location, and responders triage, track and resolve them with a severity queue and map. | HTML, CSS, JavaScript, Leaflet, Geolocation API |
| **Database Engine** | Systems / Database | A database built from scratch: page-based storage, a custom SQL parser, a Flask REST API and a React interface. | Python, Flask, TypeScript, React, SQL |

## Experience and certifications

**Experience**

- **Digital Branding & Marketing Intern — Team Lead**, Xceed Beyond Innovation LLP (Pune), **July – September 2024**.
- Hybrid (on-campus and remote) internship on live projects with industry mentors; team lead across five projects.
- Scope of work: Digital Branding & Marketing, Graphic Design & Assets, Content Strategy.
- The offer letter and Certificate of Participation are viewable in the card.

**Certifications** (click a thumbnail to open the full certificate)

- **Virtual Internship — Google Cloud Generative AI**, SmartBridge & SmartInternz, 15 Sep – 31 Oct 2025 (ID `VIP-AI-2025-26244`).
- **Getting Started with AI and Machine Learning**, LinkedIn Learning path, completed 11 Nov 2025 (9 hrs 49 min).
- **Internship — Digital Branding & Communication**, Xceed Beyond, Jul – Sep 2024.
- **Smart India Hackathon — Participation** (credential pending).

## Features

- **Single-file architecture** with no build tooling.
- **Dark and light themes**, toggled in the navigation and remembered in `localStorage`.
- **Project detail pages** rendered from `<template>` elements inside the same file, with a sidebar for in-page navigation.
- **Config-driven content.** Contact details, projects' extras, certificates, notes and testimonials all come from one `PORTFOLIO_CONFIG` object; anything left empty stays hidden.
- **Working contact form** backed by Supabase, with client-side validation and a honeypot field against bots. Without a backend it falls back to an honest demo mode and an "Open in email app" option.
- **"Doraemon" AI assistant** (floating chat) that answers questions about the portfolio through a server-side endpoint.
- **WhatsApp click-to-chat** floating button.
- **Certificate and document viewer**: any element with `data-zoom` opens its image in an accessible modal dialog.
- **Cursor-following footer glow**: only the part of the name under the pointer lights up (disabled on touch devices).
- **Motion layer**: staggered entrance, scroll reveals, heading word reveals, floating tiles and a custom cursor glow.
- **Smart chat actions**: the assistant can point visitors to sections such as Experience or Certifications.

## Tech stack

- **HTML5**, **CSS3** (custom properties, grid, clip-path, masks, `backdrop-filter`) and **vanilla JavaScript** (ES modules-free IIFEs).
- **Inline SVG** for icons, diagrams and the orbital visuals.
- **Supabase** for the contact form (`contact_messages` table with row-level security) and the chat Edge Function. The Supabase JS client loads lazily from jsDelivr only when needed.
- No frameworks, bundlers or runtime dependencies.

## How the page is structured

```
portfolio-site.html
├── <head>            SEO + Open Graph tags, theme colour, inline favicon, theme bootstrap
├── <style>           All CSS, grouped by feature (shell, nav, hero, projects, experience,
│                     footer, project pages, themes, animations, breakpoints)
├── <body>
│   ├── #view-home    The main single-page site (all sections above)
│   ├── #view-project Mount point for project detail pages
│   ├── <template>    One per project: tpl-signbridge, tpl-pypilot-ai, tpl-campusx,
│   │                 tpl-responda, tpl-database-engine
│   ├── Floating UI   WhatsApp button, Doraemon chat launcher and panel, toast region
│   └── <script>      PORTFOLIO_CONFIG, then feature scripts: theme toggle, navigation,
│                     scroll reveals, config rendering (filters, certificates, notes,
│                     testimonials), Supabase form, chatbot, cursor effects, document viewer
```

Breakpoints are tuned at 1100, 900, 640 and 400 px, and the layouts reflow for tablet and phone (for example the lab track becomes a vertical stack and the glass shards become a grid).

## Configuration

All editable settings live in `window.PORTFOLIO_CONFIG` near the bottom of the file.

| Key | Purpose |
|---|---|
| `email`, `linkedin`, `github` | Contact and social links used across the site |
| `resume` | Path to the resume PDF (default `assets/resume/Sahil-Kaware-Resume.pdf`) |
| `formEndpoint`, `formAccessKey` | Optional third-party form backend (Formspree or Web3Forms) |
| `supabaseUrl`, `supabaseKey` | Supabase project URL and **publishable (anon)** key for the contact form |
| `whatsappNumber` | Country code plus number, digits only, for the WhatsApp button |
| `portrait`, `portraitAlt` | Portrait image for the About section (a placeholder shows if missing) |
| `chatEndpoint` | Chat endpoint; empty uses `<supabaseUrl>/functions/v1/chat` |
| `projects` | Per-project extras: `repo`, `demo`, `screenshots`, `video`, `problem`, `approach`, `result` |
| `notes` | Field notes / blog entries (section appears when non-empty) |
| `achievements` | Certifications: `title`, `issuer`, `year`, `detail`, `tags`, `image`, `url` |
| `testimonials` | Quotes with name, role and optional photo |

Example certificate entry:

```js
{ title: "Getting Started with AI and Machine Learning",
  issuer: "LinkedIn Learning · Learning Path",
  year: "2025",
  detail: "Completed 11 Nov 2025 · 9 hrs 49 min",
  tags: ["Machine Learning", "Neural Networks"],
  url: "",            // optional credential link
  image: "data:image/jpeg;base64,..." }   // or "assets/images/certs/name.jpg" or an https link
```

> **Security note:** only ever put the Supabase **publishable (anon)** key in the page. Never paste the `service_role` / secret key into browser code. Row-level security restricts visitors to inserting contact messages.

## Running and deploying

**Locally**

```bash
# Option 1: just open the file
open portfolio-site.html

# Option 2: serve it (needed if you add assets/ files and want paths to resolve cleanly)
python3 -m http.server 8000
# then visit http://localhost:8000/portfolio-site.html
```

**Deploying** works on any static host: GitHub Pages, Netlify, Vercel or Cloudflare Pages. For most hosts, rename the file to `index.html` so it loads at the site root.

**Optional asset folders** referenced by the config (create them next to the HTML file):

```
assets/
├── images/
│   ├── portrait.jpg          # About section photo (~800×1000)
│   ├── certs/                # certificate thumbnails, if you prefer files over inline images
│   └── projects/             # project screenshots
└── resume/
    └── Sahil-Kaware-Resume.pdf
```

The current certificate and document images are embedded directly in the HTML, so they work without these folders.

## Backend pieces (Supabase)

| Piece | Used for |
|---|---|
| Table `contact_messages` (`name`, `email`, `subject`, `message`) | Stores contact-form submissions. Row-level security allows insert-only access for visitors. |
| Edge Function `chat` | Powers the Doraemon assistant at `<supabaseUrl>/functions/v1/chat`. Model keys stay server-side and never reach the browser. |

The portfolio degrades gracefully: with no Supabase settings the form runs in demo mode and the chat endpoint is unavailable, while the rest of the site is unaffected.

## Accessibility and performance

- Skip-to-content link, semantic landmarks, labelled navigation and `aria-live` regions for status messages.
- Keyboard-reachable controls with visible focus; project cards reveal their screenshot on focus as well as hover.
- The certificate viewer uses the native `<dialog>` element (focus trap, Esc to close).
- `prefers-reduced-motion` disables non-essential animation.
- Content stays visible without JavaScript; reveal animations only hide content when JS is running.
- Images use lazy loading, and the Supabase client is loaded only when the contact form needs it.
- The file is large (about 2.5 MB) because screenshots and certificates are embedded. Moving them into `assets/` files will shrink it and speed up first load.

## Customizing

- **Add a project:** add a card in the Projects section, a matching `<template id="tpl-your-slug">`, and an entry under `projects` in the config.
- **Add a certificate:** add an object to `achievements`. It appears automatically.
- **Change colours:** edit the CSS variables at the top of the stylesheet, plus the `html[data-theme="light"]` overrides for the light theme.
- **Update experience:** edit the `exp-card` block in the Experience section. The "At a glance" experience tile has its own text to keep in sync.

## Contact

- **Email:** sahilkaware2310@gmail.com
- **LinkedIn:** https://www.linkedin.com/in/sahil-kaware
- **GitHub:** https://github.com/Sahilkaware5

---

© 2026 Sahil Kaware. All rights reserved.
