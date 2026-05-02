# guardrailsfirst

Landing page for [guardrailsfirst.com](https://guardrailsfirst.com), a content brand under [Real Agents Academy](https://realagents.academy). Single-page Astro site, deployed to Azure Static Web Apps.

> **First time on a new machine?** Skip to [Setup from scratch](#setup-from-scratch). For project context, decisions, and brand tokens, see [CLAUDE.md](./CLAUDE.md). For the full implementation plan, see [docs/PLAN.md](./docs/PLAN.md). For the running list of pending work and known debt, see [docs/TECHNICAL_DEBT.md](./docs/TECHNICAL_DEBT.md).

## What this is

A static, single-page landing site whose only job is to drive newsletter subscriptions to [guardrailsfirst.substack.com](https://guardrailsfirst.substack.com). It must render correctly in social previews (LinkedIn, X, Bluesky, Substack) and hit Lighthouse 95+ on mobile.

Stack:

- **Astro** (static-site generator, zero client JS by default)
- **Hand-written CSS** with design tokens (no framework)
- **Inter** typeface, self-hosted
- **Azure Static Web Apps** (free tier) for hosting
- **Namecheap** for DNS

## Setup from scratch

For a fresh Windows 10/11 machine. Adapt for macOS/Linux as needed (the only Windows-specific bit is the PowerShell profile path).

### 1. Install prerequisites

```powershell
# Git for Windows
winget install Git.Git

# fnm (Node version manager — preferred over a global Node install)
winget install Schniz.fnm

# (optional) VS Code
winget install Microsoft.VisualStudioCode
```

### 2. Wire up fnm

After installing fnm, fully close and reopen PowerShell so PATH is refreshed. Then add fnm shell integration to your PowerShell profile so it survives across sessions:

```powershell
if (-not (Test-Path $PROFILE)) { New-Item -ItemType File -Path $PROFILE -Force }
Add-Content -Path $PROFILE -Value "`nfnm env --use-on-cd --shell powershell | Out-String | Invoke-Expression"
. $PROFILE
```

### 3. Clone the repo

```powershell
git clone https://github.com/realagents-academy/guardrailsfirst.git
cd guardrailsfirst
```

### 4. Install Node + dependencies

The repo pins a Node version via `.node-version`. fnm reads it automatically:

```powershell
fnm install              # installs the version from .node-version
fnm use                  # activates it for this shell
node --version           # verify (should match .node-version)
npm install              # install project dependencies
```

### 5. Run the dev server

```powershell
npm run dev
```

Opens at <http://localhost:4321>.

## Daily workflow

```powershell
npm run dev              # start dev server with hot reload
npm run build            # build static site to dist/
npm run preview          # preview the production build locally
npm run check            # type-check + Astro diagnostics (run before pushing)
```

Pushes to `main` trigger a deploy to Azure Static Web Apps via the GitHub Action in `.github/workflows/`. SWA-managed TLS auto-provisions for `guardrailsfirst.com` and `www.guardrailsfirst.com`.

## Where to edit

| Want to change… | Edit this file |
| --- | --- |
| Hero headline / tagline / CTA copy | `src/components/Hero.astro` |
| Footer copy / bio / © line | `src/components/Footer.astro` |
| Social icon set (links + which icons render) | `src/components/SocialIcons.astro` |
| Page `<title>` / OG description / Twitter card | `src/layouts/Base.astro` |
| OG share image (1200×630) | `public/og-image.png` |
| Color tokens, type scale, spacing | `src/styles/global.css` |
| Site URL (used for absolute OG/sitemap URLs) | `astro.config.mjs` |
| Security headers, HTTPS enforcement | `staticwebapp.config.json` |

## Project structure

See [docs/PLAN.md](./docs/PLAN.md#repo-structure) for the full tree and what lives where.

Top level:

```text
.
├── src/                       Astro components, layouts, pages, CSS
├── public/                    Static assets served as-is (favicon, og-image, logo)
├── pictures/                  Source-of-truth assets (logo PNGs, brand banners) — versioned, NOT deployed
├── docs/
│   ├── PLAN.md                Frozen implementation plan from project kickoff
│   └── TECHNICAL_DEBT.md      Living backlog: pending work, deferred items, future architecture
├── astro.config.mjs           Astro build config
├── staticwebapp.config.json   SWA security headers + routing
├── package.json
├── .node-version              Node version pin (read by fnm/nvm)
├── README.md                  This file
└── CLAUDE.md                  Context for Claude Code sessions
```

## License

[MIT](./LICENSE)
