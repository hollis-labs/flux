# Changelog

All notable changes to Flux are recorded here.

The format follows [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project uses [Semantic Versioning](https://semver.org/spec/v2.0.0.html). Pre-1.0: minor bumps for additive surface, patch bumps for fixes and documentation.

Flux was extracted from Nanite's `ui/` directory with history preserved, so the
git log reaches back to March 2026 and is mostly Nanite UI work. This file
starts at the extraction (September 2026) and summarises earlier history
briefly; it is good-faith, not exhaustive.

## [Unreleased]

## [0.0.1] - 2026-09-30

### Changed

- README corrected: the dev server runs on `http://localhost:5179` (not 5176),
  the `NANITE_UI_PORT` / `NANITE_API_PORT` overrides are documented, and the
  README no longer claims Nanite has stopped embedding the UI — Nanite's `main`
  still builds and embeds its own `ui/` copy.

### Added

- Open-source project documents: `AGENTS.md`, `CONTRIBUTING.md`,
  `SECURITY.md`, `TRADEMARK.md`, and this changelog.

## [0.0.0] - 2026-09-30

Everything up to the project's first tracked version, as it stood after
extraction.

### Added

- Standalone repository wired up from Nanite's `ui/` (2026-09-18), with the
  envelope catalog pinned to `go-envelopes` through `go.mod` and generated
  types/plugin registry scripts.
- Dev server port configurable via `NANITE_UI_PORT` / `NANITE_API_PORT`;
  default moved to 5179.
- MIT license.

### Earlier history (from Nanite's `ui/`)

- March 2026: chat MVP scaffold, agent modes, envelope cards, tool indicators,
  context broker, bookmarks, widgets, artifacts.
- April–May 2026: plugin SDK transport and registry, streaming/recovery fixes,
  provider failure explanations, large tool-result handling, workflow and
  memory surfaces.
- August–September 2026: first-run setup wizard, setup-gated app shell,
  real tool grants, skill approval, MCP server picker.

[Unreleased]: https://github.com/hollis-labs/flux/compare/v0.0.1...HEAD
[0.0.1]: https://github.com/hollis-labs/flux/compare/v0.0.0...v0.0.1
[0.0.0]: https://github.com/hollis-labs/flux/commits/main
