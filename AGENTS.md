# flux

The GUI for Nanite: a Vite + React 19 + Tailwind/shadcn (Radix) single-page app — first-run setup, model picker, Settings, chat surface, plugin panels. It talks to Nanite's headless HTTP API; it has no backend of its own.

It is not: a Go project (the `go.mod` exists only to pin the envelope catalog exporter), the owner of envelope schemas (they live in `go-envelopes`), or a place for server logic. Backend behavior belongs in Nanite.

## Start Here

- `README.md` — run instructions, ports, envelope-type workflow.
- `src/App.tsx`, `src/components/AppGate.tsx` — shell and first-run gating.
- `src/lib/api.ts` — the API client (`/api` base); `src/hooks/useChat.ts` — SSE chat stream.
- `src/components/chat/envelopes/` — envelope card components; `src/generated/` — generator output.
- `src/_host/` — host re-exports (React, shadcn primitives) that runtime-loaded plugins resolve through the importmap built in `vite.config.ts`.
- `scripts/` — `generate-envelope-types.mjs`, `generate-plugin-imports.mjs`.
- `src/__tests__/` and colocated `*.test.*` — vitest suite (happy-dom).

## Commands

```sh
npm ci
npm run dev                # http://localhost:5179; /api -> Nanite :8090
npm run build              # tsc -b && vite build (runs generators first)
npm run test -- --run      # vitest, single pass
npm run lint               # biome check .
npm run check:envelopes    # fails if generated envelope types are stale
```

Env: `NANITE_UI_PORT` (default 5179), `NANITE_API_PORT` (default 8090). Envelope generation needs a Go toolchain (it runs `go tool envelopes-export`).

## Boundaries

- `src/generated/` is generator output — regenerate with `npm run generate:envelopes` / `generate:plugins`, never hand-edit. Guard: `npm run check:envelopes`.
- Envelope types come from the released `go-envelopes` module pinned in `go.mod`. A new core type lands there first; then bump the pin here (and in Nanite), add the component under `src/components/chat/envelopes/`, and regenerate.
- Plugins are loaded at runtime via `import(bundleUrl)` and share the host's React/shadcn through the importmap. Keep `HOST_ENTRIES` in `vite.config.ts` and `preserveEntrySignatures: 'strict'` intact, or plugin bundles lose exports.
- All server calls go through `/api` (proxied in dev). Do not hard-code backend hosts or ports in source.
- Nanite `main` still embeds its own `ui/` copy; this repo and that copy can diverge. Do not assume a change here reaches the shipped Nanite binary.
- No secrets in source, fixtures, or committed env files.
- Keep `package.json` `version` and `CHANGELOG.md` in step; the package is `private`, do not `npm publish`.
