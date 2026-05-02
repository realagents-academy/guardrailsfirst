# CLAUDE.md

Auto-loaded by Claude Code when working in this repo. Captures decisions, voice, brand tokens, and project state so a fresh Claude session (e.g. after a machine format or by a new collaborator) can pick up cold without re-deriving everything.

## What this project is

**Guardrails First** is a content brand owned by Alex Araujo (Microsoft AI architect, MCT, based in Switzerland). It sits under the umbrella brand **Real Agents Academy** (`realagents.academy`, currently dormant — future paid courses will live there).

This repo is the single-page landing site at **guardrailsfirst.com**. Its only job is to drive newsletter subscriptions to **guardrailsfirst.substack.com** and render correctly in social previews (LinkedIn, X, Bluesky, Substack).

Stack: **Astro** (SSG, static HTML/CSS, zero client JS by default). Hosted on **Azure Static Web Apps** (free tier). DNS at **Namecheap**. Custom domain auto-issues TLS via SWA.

## Current state

See `PLAN.md` for the full implementation plan. As of the last working session, we're partway through:

- ✅ GitHub org `realagents-academy` created; `guardrailsfirst.com` + `realagents.academy` verified at the org level.
- ✅ Repo `realagents-academy/guardrailsfirst` created (this one).
- ✅ Logo asset committed at `pictures/GuardrailsFirstLogo.png` (square GF monogram, navy bg, teal strokes). No SVG yet.
- ⏳ Astro scaffolding — pending. Next concrete step.
- ⏳ Site build (Hero, Footer, OG metadata, favicons).
- ⏳ SWA provisioning, custom domain DNS.

## Brand identity (use these for ALL copy and code)

### Color tokens
- `--color-bg`: `#0B1F3A` (navy) — primary background
- `--color-accent`: `#5EEAD4` (teal) — CTA, logo highlights
- `--color-text`: `#F8FAFC` (off-white)
- `--color-text-muted`: `#94A3B8` (slate-400)

Contrast: teal on navy ≈ 10:1, well above WCAG AA.

### Typography
- **Inter** (preferred) or Space Grotesk — geometric sans-serif
- Self-host via `@fontsource/inter` — no third-party Google Fonts request
- System stack fallback

### Aesthetic
Linear / Stripe / Vercel minimalism. Clean Swiss design. Generous whitespace. **Not** consumer-playful, **not** corporate-stiff. Adult, technical, confident.

### Voice for any user-facing copy
Sharp, opinionated, practitioner-grade. No hype. No corporate fluff.

**On-brand phrasing**: "AI that ships in regulated enterprises." "Real scenarios, real blockers, real patterns." "By Alex Araujo."

**Avoid**: "empower," "synergy," "leverage AI to unlock," "exciting journey," "we're thrilled," any LinkedIn-esque buzzword.

## Brand architecture

| Layer | Real Agents Academy | Guardrails First |
|---|---|---|
| Role | Umbrella brand | Content show under umbrella |
| Domain | `realagents.academy` (dormant) | `guardrailsfirst.com` (live target) |
| Audience | Future course buyers | Practitioners, MCTs, enterprise AI architects |
| Content | Future paid courses | Newsletter, YouTube, essays |
| Visual identity | TBD (current placeholder: orange "RA" mark) | Defined: navy + teal, GF monogram |

The two layers are intentionally distinct in visual identity. Don't conflate them. The umbrella org operates both; the brands stay separate to consumers.

## DNS constraints — DO NOT BREAK

`guardrailsfirst.com` already has live records that must remain untouched when adding web records:

- MX `@` — Namecheap email forwarding (`hello@guardrailsfirst.com` → `alejandro.araujo@araujoinnovations.ch`)
- TXT `@` — M365 SPF (`v=spf1 include:spf.protection.outlook.com ...`)
- TXT `selector1._domainkey` and `selector2._domainkey` — M365 DKIM
- TXT `_dmarc` — DMARC policy
- CNAME `autodiscover` — M365 (if present)
- TXT `_github-challenge-realagents-academy` — GitHub org verification

Mail must keep working through any DNS change. Any new web record (SWA's apex A, www CNAME, dnsauth TXT) sits on different host labels — no conflict — but always glance at the host column before saving in Namecheap.

## Live external systems

- Newsletter: https://guardrailsfirst.substack.com (live — primary CTA)
- YouTube: https://www.youtube.com/@guardrailsfirst (live, empty)
- Email: `hello@guardrailsfirst.com` (forwards)

Planned (do NOT link until live):
- X/Twitter at `https://x.com/guardrailsfirst` (in progress)
- Bluesky at `https://bsky.app/profile/guardrailsfirst.com` (planned, will use domain-as-handle via `_atproto` TXT)
- LinkedIn (Real Agents Academy company page), Instagram, Threads — deferred

## Working with Alex

Working style notes (so you match his cadence):

- **Senior practitioner**, not a beginner — no hand-holding on Azure/TS/DNS basics.
- **Prefers recommendations with reasoning, not menus of options.** When you see a fork, pick a side and justify in one sentence.
- **Wants exact specifics**: file paths, exact DNS records, exact commands, exact time estimates. Generic instructions get pushback.
- **Plan before code** for anything non-trivial. Use plan mode.
- **Tight scope discipline.** "Don't overengineer — match the simplicity of the v1 scope."
- **Voice**: terse, opinionated, dry. Don't grovel. Don't pad. Match his cadence.

## Out of scope for v1

CMS, blog engine, essay archive, Substack API integration, analytics, cookie banners, LinkedIn/Bluesky/Instagram/Threads/GitHub icons (until handles are live), Real Agents Academy umbrella site (stays dormant).

## Where context lives

- **In this repo**: PLAN.md (full implementation plan), README.md (setup + workflow), this file.
- **Outside this repo (may not survive a machine format)**:
  - `~/.claude/plans/` — original plan file
  - `~/.claude/projects/<project-hash>/memory/` — auto-memory (user profile, brand, DNS, planning style memories)
  - Tell Alex to back up his entire `~/.claude/` directory before formatting if he wants those preserved.

If memory is lost, this CLAUDE.md + PLAN.md should be enough to restore working context.
