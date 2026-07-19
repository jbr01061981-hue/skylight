# Skylight AI Agent Guide

This repository is a pnpm workspace for the Skylight project. The goal of this file is to help AI coding agents quickly understand the main packages, command-line workflows, and where source code should be changed.

## Workspace layout

- `shared/` — shared TypeScript types, config schema, geo/math, and pure utility logic.
- `server/` — Express + WebSocket backend with REST helpers, data enrichment, and aircraft source handling.
- `web/` — React + Vite frontend apps for display, control panel, tracker UI, and TV dashboard.
- `tracker/` — PTZ camera tracker process, vision pipeline, and tracker-side CLI/helper scripts.
- `scripts/` — shell helpers for Raspberry Pi setup, radio setup, deployment, and model fetching.
- `pi-setup/` — Raspberry Pi appliance provisioning docs and systemd service definitions.
- `compose.yaml` — Docker compose setup for the server/display container.

## Key commands

Use `pnpm` from the workspace root.

- `pnpm install` — install dependencies for all workspace packages.
- `pnpm dev` — run `server`, `web`, and `tracker` together via `concurrently`.
- `pnpm dev:server` — run `server` in watch mode.
- `pnpm dev:web` — run `web` via Vite.
- `pnpm dev:tracker` — run `tracker` in watch mode.
- `pnpm build` — build only the `web` app.
- `pnpm start` — start the `server` package.
- `pnpm test` — run `vitest` from the root workspace.
- `pnpm typecheck` — run TypeScript checks across `shared`, `server`, `web`, and `tracker`.

## Package-specific notes

### `shared`
- Holds common typed contracts, geodesy, celestial math, aircraft/route/camera models, and config normalization.
- Changes here affect both `server`, `web`, and `tracker`.

### `server`
- Entry point: `server/src/index.ts`.
- Implements aircraft data sources, REST API helpers, and WebSocket hub logic.
- Common edit targets: `datasource.ts`, `hub.ts`, `geocode.ts`, `tle.ts`, and `enrich/*`.

### `web`
- Entry points: `web/src/display/main.tsx`, `web/src/control/main.tsx`, `web/src/tracker/main.tsx`, `web/src/tv/main.tsx`.
- Use `web/src` for source changes; `web/dist` is generated build output and should not be edited.
- The UI is split into display, control, tracker, and TV/stream views.

### `tracker`
- Entry point: `tracker/src/index.ts`.
- Contains mount modeling, PTZ camera tracking, vision triage, frame handling, and stabilization.
- Includes optional `onnxruntime-node` support for the neural detector.
- `tracker/scripts/` contains CLI utilities for offline camera/vision maintenance and diagnostics.

## Command-line / CLI guidance

- The project uses shell scripts in `scripts/` for deployment and local radio setup.
- `tracker/scripts/*.ts` are command-line helpers and offline tools. They are typically run with `pnpm exec tsx tracker/scripts/<script>.ts`.
- CLI-related entrypoints are often `tsx`-driven; the workspace packages are configured as ESM.
- For Pi deployment and appliance setup, consult `pi-setup/README.md`.

## Recommended behavior for AI agents

- Prefer editing inside `shared/src`, `server/src`, `web/src`, and `tracker/src`.
- Do not modify generated build output in `web/dist`.
- Preserve existing TypeScript strictness and the root `pnpm` workspace conventions.
- For changes involving runtime config or environment, refer to the root `README.md` and `pi-setup/README.md` rather than repeating long setup instructions.

## Useful docs

- Repository overview: `README.md`
- Raspberry Pi appliance: `pi-setup/README.md`
- Docker/container workflow: `compose.yaml`
- Root workspace scripts and build commands: `package.json`
