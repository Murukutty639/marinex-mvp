# MariNex MVP

MariNex is a local, runnable prototype that demonstrates how one senior maritime professional's judgement can be captured, sanitised, verified, converted into an interactive scenario and transferred to one newcomer.

The MVP proves: capture -> anonymise -> retrieve evidence -> author scenario -> expert review -> publish -> learner decision -> immediate feedback -> Expert Replay -> learning map -> analytics -> traceability.

Training prototype only. Follow approved vessel/company procedures and authorised instruction in real operations.

## Architecture

- `apps/api`: Express, TypeScript, SQLite, sessions, RBAC, retrieval and workflow APIs.
- `apps/web`: React, Vite and responsive learner/expert/admin UI.
- `packages/shared`: shared schemas and types.
- `docs`: security, privacy, traceability, UX, data model, API, mockups and diagrams.

## Prerequisites

- Node.js 22 recommended.
- npm 10+.
- Docker Desktop optional for Compose verification.

## Install

```bash
npm install
```

## Environment

```bash
cp .env.example .env
```

The app does not require an LLM API key.

## Migrate And Seed

```bash
npm run migrate
npm run seed
```

The seed command resets the demo database and creates the complete story.

## Run Development

```bash
npm run dev
```

- Web: `http://localhost:5173`
- API health: `http://localhost:4000/api/health`

## Temporary Live Sharing

`npm run share` builds and serves the complete demo on `http://127.0.0.1:4173`. The share server uses an isolated `storage/live-demo.sqlite` database, seeded only on first use, and a separate upload directory. Local learning records are unaffected. Visitors using the same demo account share that account's progress.

In another terminal, run Cloudflare's official tunnel utility:

```powershell
.\.share\cloudflared.exe tunnel --url http://127.0.0.1:4173 --protocol http2
```

Share the HTTPS URL printed by the utility. Keep the computer awake and online and both processes running. Stopping the tunnel disables the link. Production-mode secure cookies require HTTPS for sign-in; local HTTP can be used to check the public page and API health.

Cloudflare Quick Tunnel setup and download: https://developers.cloudflare.com/tunnel/get-started/quick-tunnels/ and https://developers.cloudflare.com/tunnel/downloads/.

## Demo Credentials

All demo users use `Password123!`.

- Public demo: `public@marinex.test`
- Newcomer: `newcomer@marinex.test`
- Expert: `expert@marinex.test`
- Admin: `admin@marinex.test`

## Demo Walkthrough

1. Open the public home page and review skill-to-maritime cards.
2. Sign in as the newcomer.
3. Open `The Conflicting Reading`.
4. Choose option B.
5. Read immediate feedback.
6. Open Expert Replay and traceability.
7. Complete the lesson and observe XP/streak progress.
8. Sign in as expert and inspect capture, sanitisation, review and evidence search.
9. Sign in as admin and inspect analytics, audit and traceability.

## Upgraded Learning Experience

The public Explore screen includes 80 independently authored synthetic questions across five tracks and 20 four-question lessons. Computing, engineering and business pathways connect prior skills with maritime concepts. Interactions include single choice, scenario decisions, multi-select, ordering and short reflection. Reflections are acknowledged, not automatically assessed for expertise or mastery.

Guest completion is stored under `marinex.publicProgress.v1` on the device. Account progress is stored separately in SQLite and is never inferred from guest storage. Complete every question successfully before finishing a signed-in lesson. First-time public lessons award 20 XP; the core company scenario awards 25 XP once.

The five-stage voyage contains ten required activities. Its percentage reflects completed activities, not XP, streak length or mastery. Map navigation locks later stages until preceding stages are complete. Public introductory tracks remain freely explorable independently of the voyage map.

Learning completions use a validated IANA timezone from the browser. The API stores real activity dates, derives current and longest streaks, and returns the last seven days and history for the calendar. Multiple activities on one local day give one streak-day credit. A gap resets the current streak but never the longest. Reviewing a lesson on another day qualifies after answering its questions again, without repeat XP. Repeated completion requests on the same day do not create extra activity records. Old XP and scenario completions remain; historical streak days are not invented from a legacy counter.

Listen controls use browser speech synthesis and support pause, resume, restart, stop, voice selection and stored speed preference. Optional reflection dictation requires explicit microphone activation and an explicit submission. Speech and recognition stop on navigation. Browser support varies; typing and reading always work. No paid speech API or LLM key is required.

### Four-Minute Demo

1. Open Explore and select Computing & Data to show its skill translation.
2. Start From Data to Decisions. Use Listen to Question, check an answer and reveal its maritime explanation. Continue through three choices and a typed reflection to finish the lesson.
3. Sign in as `newcomer@marinex.test` with `Password123!`. Show the Learning Map, current-stage marker and blue progress bar.
4. Continue or review The Conflicting Reading, choose B, open Expert Replay and use Listen to Expert Replay.
5. Complete the scenario and show the updated activity, streak and calendar. Complete The Maritime World to unlock Verify.

### Browser Verification

API tests use a separate `storage/tests.sqlite` database. Playwright uses the running demo API and demo account; it can add genuine demo completions. Tests cover the full flow, voice support and permission fallbacks with mocks, keyboard/interaction states and layouts at 375, 430, 768, 1024 and 1440 pixels.

Install Playwright Chromium with `npx playwright install chromium`, or use installed Chrome in PowerShell:

```powershell
$env:PLAYWRIGHT_CHANNEL = 'chrome'
npm.cmd run e2e
```

The existing lint scripts run TypeScript checks rather than a separate ESLint rule set. All public content is synthetic introductory training, not expert accreditation or operational advice. Real voice output and recognition quality depend on browser/platform services and need manual testing on the presentation device.

## Tests

```bash
npm run typecheck
npm run test
npm run build
npm run e2e
npm run screenshots
```

Playwright requires browser binaries. If they are unavailable locally, SVG mockups remain in `docs/mockups` and the screenshot command is still documented.

## Docker

```bash
docker compose up --build
```

The web app is served on `http://localhost:8080` and proxies API calls to the API container.

## Mock RAG

The MVP uses approved local knowledge chunks and deterministic lexical retrieval. A support score above `RAG_CONFIDENCE_THRESHOLD` passes the safety gate; weak evidence returns `FLAG` and is blocked from approval.

To replace this later, keep the same retrieval interface and swap the lexical scorer for embeddings/vector search. Scenario publication should still require provenance and expert review.

## Privacy And Security Caveats

Automated anonymisation is only a suggestion step. A production version needs stronger tenant isolation, managed database, backups, encryption/key management, object storage, malware scanning, enterprise identity, retention/deletion policy, legal review, training governance, monitoring, TLS, secrets management, disaster recovery and external security review.

This MVP is not production-ready and is not an STCW certification, approved simulator or operational guidance tool.
