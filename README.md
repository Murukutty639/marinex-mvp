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
