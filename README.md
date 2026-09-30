# Flux (temp name)

Flux is the GUI for [Nanite](https://github.com/hollis-labs/nanite): first-run
setup, the model picker, Settings, and the chat surface. It was extracted from
Nanite's `ui/` directory (history preserved) as the first move in a
portfolio-wide shift — every app's GUI becomes its own repo, talking to its
headless backend over HTTP, instead of being built and embedded into the
backend's binary.

> **Status of the extraction.** As of this writing, Nanite's `main` branch still
> carries its own copy of this UI in `ui/` and still embeds the built output in
> the `nanite` binary (`internal/server/spa.go`, `make build-ui`). This repo is
> the standalone home the UI is moving to; until Nanite drops its embedded copy,
> the two can diverge. Run Flux against Nanite's API as below.

Run them side by side in development:

```bash
# Terminal 1, in nanite/
air   # or: go run ./cmd/nanite serve -port 8090 -db ./nanite.db -dev

# Terminal 2, here
npm ci
npm run dev   # http://localhost:5179, proxies /api to Nanite at :8090
```

The dev server's ports come from `vite.config.ts` and can be overridden:

| Variable | Default | Purpose |
|---|---|---|
| `NANITE_UI_PORT` | `5179` | Port for the Vite dev server |
| `NANITE_API_PORT` | `8090` | Port of the Nanite backend `/api` is proxied to |

## Envelope types

The chat surface renders structured "envelope" cards whose types and schemas
are owned by the released [`github.com/hollis-labs/go-envelopes`](https://github.com/hollis-labs/go-envelopes)
module, not by this repo or by Nanite. `go.mod` here pins the same module
version Nanite's `go.mod` pins; a Go toolchain is required at build time only
to run its exporter — this repo has no Go source of its own.

- `npm run generate:envelopes` — regenerates `src/generated/envelope-types.generated.ts`
  from the pinned module's catalog (`scripts/generate-envelope-types.mjs`).
- `npm run generate:plugins` — regenerates `src/generated/plugin-envelopes.ts`,
  the renderer registry (`scripts/generate-plugin-imports.mjs`).
- Both run automatically via `prebuild`/`predev`. `npm run check:envelopes`
  verifies the generated output isn't stale (CI use).
- Never hand-edit anything under `src/generated/`.

Adding a new core envelope type: land it in `go-envelopes` first, bump the
pinned version in both this repo's `go.mod` and Nanite's, then add the React
component under `src/components/chat/envelopes/` and regenerate. See Nanite's
`docs/ref-envelope-component.md` for the full walkthrough.

## Scripts

```bash
npm run dev              # vite dev server
npm run build             # tsc -b && vite build
npm run test               # vitest
npm run lint               # biome check .
```

## Project docs

[`AGENTS.md`](AGENTS.md) · [`CONTRIBUTING.md`](CONTRIBUTING.md) ·
[`SECURITY.md`](SECURITY.md) · [`CHANGELOG.md`](CHANGELOG.md) ·
[`TRADEMARK.md`](TRADEMARK.md)

## License

MIT — see [`LICENSE`](LICENSE). The name and branding are covered separately
by [`TRADEMARK.md`](TRADEMARK.md).
