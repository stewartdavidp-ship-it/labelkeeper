# CLAUDE.md — Label Keeper

## Claude Code Bootstrap

**Before making any changes, do the following:**

1. **Read ARCHITECTURE.md** in this repo — it documents the system architecture, deployment procedures, data model, and key patterns. Do NOT assume hosting, deploy process, or auth flow.
2. **Read memory files** — Check `~/.claude/projects/-Users-davidstewart-Downloads/memory/MEMORY.md` for cross-project context, active jobs, and deployment procedures.
3. **Deploy** — Label Keeper client deploys via GitHub Pages (push to main). The API deploys via `cd ~/Developer/labelkeeper-api && bash deploy.sh`.
4. **After context compaction** — Re-read this bootstrap section, ARCHITECTURE.md, and memory files. Never improvise from summary alone.

## What This App Is

Label Keeper is a label/tag management application. Single-file HTML app (`index.html`). Firebase RTDB for persistence (`labelkeeper-e8a79` project). Deployed via GitHub Pages. Has a companion API service (`labelkeeper-api`) deployed to Cloud Run on the same Firebase project.

## Current Idea

Future direction: multi-user support. Sheets/content/templates will move to Firebase under `labelkeeper/{uid}/`. Auth will flow from consuming apps (e.g., Shir admin passes Firebase ID token).

## RULEs — Do not violate these.

- No unbounded Firebase listeners. All reads must use limitToLast(N) or .once('value').

## CONSTRAINTs — External realities. Work within these.

- Single-file HTML app — all UI in `index.html`, no build system.
- Firebase project: `labelkeeper-e8a79` (RTDB: `https://labelkeeper-e8a79-default-rtdb.firebaseio.com`).
- GCP project: `labelkeeper-e8a79` (project number `1075204398975`). Cloud Run, Secret Manager, RTDB all in this project.
- Shir Glassworks admin app calls POST /sessions for print cards — LK is a dependency for SG.

## DECISIONs — Current direction for this phase.

_(none active)_

## OPENs — Unresolved. Flag if you encounter these during build.

- Multi-user data model: how does `labelkeeper/{uid}/` work with shared templates?
