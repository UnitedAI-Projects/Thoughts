# AGENTS.md

## Project
Thoughts is a browser extension plus a Next.js website that turns a student's AI chats into an organized task list. Each task has a confidence score, a diagram, and a Golden Path (recommended steps).

**The main thing:** extension → API → task → page. Build only what is on this path. Do not build ideas listed under "Parked Ideas" in README.md.

## Structure
- `/web`: Next.js (TypeScript) app, pages and API routes. Deployed on Vercel.
- `/extension`: Manifest V3 browser extension (TypeScript).
- `/docs`: PLAN.md and sample data.
- README.md has the full plan. Read it before making structural decisions.

## Brand and design
- Colors are CSS variables, defined once, never hard-coded in components:
  - `--color-black: #000000` (True Black)
  - `--color-gold: #C9A227` (Rich Gold)
  - `--color-white: #FFFFFF`
  - `--color-surface: #F5F5F5`
- Gold on white fails contrast. On light backgrounds use black text and use gold only for fills, borders, and bars. Text on a gold fill is black.
- Style: clean, minimal, square-edged, matching the low-fidelity wireframes. Header on every page: logo "T Thoughts", nav (Dashboard, Settings), a "Capture on/off" pill, and a profile icon.

## Data
- Task status is one of `processing`, `ready`, `failed`.
- Task fields: id, title, summary, status, created_at, updated_at, confidence {score, band, reason}, diagram {nodes, edges}, golden_path [{title, description}].
- Confidence means "how clearly this task was identified", not how correct the original AI was. Band: High, Medium, Low.

## Rules
- Use mock data by default. `ANALYSIS_PROVIDER=mock` unless told otherwise.
- Only the backend may call Azure OpenAI. Never put an AI key in the extension or in client-side code.
- Never commit secrets or `.env`. Use `.env.example` with fake values only.
- Keep changes small and limited to the task given. Do not refactor unrelated files.
- Do not add new dependencies without saying why.
- TypeScript everywhere. Keep components small and put mock data in one file.
- Configure CORS on API routes, since the extension and website call from different origins.
- If a request conflicts with this file or the README, stop and ask.
