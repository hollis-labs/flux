# Contributing

How a change gets from your clone into `main`. This is deliberately short: most
of what you need is written somewhere closer to the thing it describes.

## Before your first change

`README.md` has the run steps. You need a Nanite backend running to exercise
most screens, and a Go toolchain because the envelope generators shell out to
`go tool`.

`AGENTS.md` is the fastest orientation to the layout and to the boundaries that
are not obvious from reading the code.

## The sequence

1. **Branch.** `<type>/<short-slug>`, where the type matches the change —
   `feat`, `fix`, `docs`, `chore`. Nothing enforces this; it is what the
   history does.
2. **Change one thing.** A branch carrying two unrelated changes costs the
   reviewer the ability to accept one and question the other.
3. **Run the checks** before you push:
   ```
   npm run lint && npm run test -- --run && npm run check:envelopes && npm run build
   ```
4. **Push and open a pull request.** A maintainer will review it.

Commit subjects follow the conventional-commit shape — a type, an optional
scope, a colon, then the summary.

## What a pull request should carry

The reviewer was not there when you made the decisions. State what the change
does, what it deliberately leaves alone, and the evidence that it works — the
commands you ran and what came back, not a claim that it passes. For a visible
change, describe what you saw in the browser.

Add a line to `CHANGELOG.md` under `[Unreleased]`.

## The one that cannot be undone

**Do not commit credentials or real conversation data.** Flux renders prompts,
tool output, and provider settings from a live backend. Screenshots, fixtures,
and logs attached to an issue or PR must be scrubbed; a secret pushed to a
public branch should be treated as leaked even after you delete it.

## Things that surprise people

- **`src/generated/` is generator output.** Regenerate it; never hand-edit.
- **The envelope catalog lives in a released module** (`go-envelopes`), not in
  this repo. New core types are released there first.
- **Plugins share the host's React through an importmap.** Changes to
  `vite.config.ts` host entries can break runtime-loaded plugins without a
  build error.
- **Nanite's `main` still embeds its own copy of this UI**, so a change here
  does not automatically reach the `nanite` binary.

## What this does not cover

- **Which change is worth making.** Open an issue to discuss larger features
  before building them.
- **Backend bugs.** Those belong in the Nanite repository.
- **Releases.** Maintainers cut those.
