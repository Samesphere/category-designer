# Category-Designer

## What this project does
A single-page web app that guides founders and entrepreneurs through the **Category Design Game** — a structured brainstorming process from the Category Pirates framework. Users chat with an AI strategist that walks them through 5 sequential questions to design a new market category (instead of competing in an existing one), then builds a Brand Pyramid: a 7-element positioning framework summarising who they serve, what they promise, and how they're different.

## Current status
- Working: full chat UI, two-phase flow (Category Design Game → Brand Pyramid), Vercel deployment with server-side API proxy
- In progress: nothing
- Next: add session export/share, optional user API key fallback

## Architecture

**Single HTML file** (`index.html`) — all UI, styling, and client JS in one file. No build step, no framework.

**Vercel serverless proxy** (`api/chat.js`) — receives POST requests from the browser and forwards them to Anthropic's API with the key injected from environment variables. Keeps the API key off the client.

**Environment variable required:** `ANTHROPIC_API_KEY` — set in Vercel project settings (not in code).

## File Structure

```
Category-Designer/
├── index.html          # Full UI — chat interface, CSS, JS
├── api/
│   └── chat.js         # Vercel serverless function — Anthropic proxy
├── vercel.json         # Vercel config (route rewrites)
└── CLAUDE.md           # This file
```

## How the AI Flow Works

Two phases, one question at a time:

**Phase 1 — The Category Design Game (5 questions)**
1. Current reality — map the landscape, headwinds, tailwinds
2. Future business models — structural shifts 10-20 years out
3. Future products & demand — Superconsumers, new demand from trend intersections
4. The new category — what renders the old category irrelevant
5. Different future scenarios — incremental → world-changing (push toward Scenario 3+)

**Phase 2 — The Brand Pyramid (7 elements)**
Who it's for → Core Problem → Core Promise → Category Name → Category Statement → Tagline → POV

## Deployment

- **GitHub:** `category-designer` repo under the markr account
- **Vercel:** connected to the GitHub repo, auto-deploys on push to `main`
- **API key:** set as `ANTHROPIC_API_KEY` in Vercel environment variables

To deploy changes: commit and push to `main`. Vercel picks it up automatically.

## Key Design Decisions

- **No build step** — keeps it simple, fast to iterate, easy to hand off
- **Server-side proxy** — the original HTML called Anthropic directly from the browser (no API key, would fail CORS). The proxy fixes both problems.
- **Model:** `claude-sonnet-4-6` (updated from `claude-sonnet-4-20250514` in the original)
- **Max tokens:** 1500 per response (verbose mode — the system prompt explicitly asks for deep, expansive answers on tailwinds/trends)
