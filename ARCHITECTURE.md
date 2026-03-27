# ARCHITECTURE.md — Label Keeper

## Mast Platform Context

Label Keeper is a standalone application — not part of the Mast tenant architecture. However, it integrates with Mast tenants as a service: Shir Glassworks admin calls the Label Keeper API for print card sessions.

## Overview

Single-file HTML app for label/tag management.

**Repo:** `stewartdavidp-ship-it/labelkeeper` (prod), `labelkeepertest` (test)
**Firebase project:** `labelkeeper-e8a79`
**RTDB:** `https://labelkeeper-e8a79-default-rtdb.firebaseio.com`
**GCP project:** `labelkeeper-e8a79` (project number `1075204398975`)
**Hosting:** GitHub Pages (`stewartdavidp-ship-it/labelkeeper`)
**API:** Cloud Run `https://labelkeeper-api-1075204398975.us-central1.run.app`

## Deployment

### Client (Label Keeper app)
- GitHub Pages from `main` branch
- Push to `stewartdavidp-ship-it/labelkeeper` → auto-deploys

### API
- Cloud Run service on `labelkeeper-e8a79` GCP project
- Deploy: `cd ~/Developer/labelkeeper-api && bash deploy.sh`
- API key in GCP Secret Manager (`lk-api-key`)

### Test
- Test repo: `stewartdavidp-ship-it/labelkeepertest`
- Test app at: `/Users/davidstewart/Developer/labelkeepertest/`

## Data Model

**Firebase RTDB paths:**
- `/labelkeeper_sync/` — Sync data
- `/labelkeeper_api/sessions/` — Print card sessions

## API Endpoints

| Method | Path | Auth | Purpose |
|--------|------|------|---------|
| GET | /health | None | Health check |
| POST | /sessions | API key | Create print card session |
| GET | /sessions/:id | None | Get session data |
| POST | /sync/share | API key | Share sync data |
| POST | /sync/receive | API key | Receive sync data |
| POST | /sync/cleanup | API key | Clean up stale sync data |

## External Integrations

- **Shir Glassworks admin** — Calls POST /sessions for print cards. LK is a dependency.
- **Firebase REST API** — LK client uses API wrappers with Firebase REST fallback.

## Key Patterns

- **Single-file architecture.** All UI in `index.html`, no framework, no build system.
- **Separate API service.** Business logic in `labelkeeper-api` Cloud Run service, client is purely UI.
- **Firebase REST fallback.** Client can operate via Firebase REST API when SDK isn't available.
