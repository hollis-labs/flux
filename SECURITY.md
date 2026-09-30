# Security policy

## Supported versions

Flux is pre-1.0 software. Security fixes are made on `main` and released in the
next patch version. Older versions do not receive backports.

## Report a vulnerability

Do not include an exploit, token, conversation transcript, database, or
provider credential in a public issue.

Use GitHub's private vulnerability-reporting flow when the repository's
Security tab offers it. If it is unavailable, contact a repository maintainer
privately through a contact channel published on the Hollis Labs organization
or maintainer profile. Include:

- the affected commit or version, browser, and operating system
- how you reach the Nanite backend (local, reverse proxy, remote)
- reproduction steps and the security impact
- whether credentials or conversation data may have been exposed
- a safe way to contact you about coordination

Maintainers will acknowledge a private report, investigate it, and coordinate
disclosure; response times are best effort.

## Deployment boundary

Flux is a browser client for Nanite. It has no server component of its own and
performs no authentication; it sends `/api` requests to a Nanite backend and
renders the responses.

- The dev server (`npm run dev`) is for local development only. Do not expose
  it to a network.
- Whoever can reach the UI acts as the Nanite user: read transcripts, change
  settings and provider configuration, and drive agents that can run tools.
  Authentication and TLS are the job of the Nanite deployment and any reverse
  proxy. Nanite is designed as a single-user local application; read its
  security documentation before exposing it.
- Serve a built bundle from the same origin as `/api`.

## Plugins

Flux loads plugin UI bundles at runtime with a dynamic `import()` of a URL the
backend provides, and gives them the host's React and UI primitives. A loaded
plugin bundle runs with the full privileges of the page. Trust is established
by the backend's plugin verification (signed binaries/bundles in Nanite), not
by Flux. Only install plugins you trust.

## Data handling

Flux keeps some UI preferences in browser `localStorage` (layout, app state).
Conversation content is fetched from Nanite and held in memory. Model output is
untrusted: report any way it can execute script or load remote resources
through the Markdown, envelope-card, or plugin rendering paths.

## Current security limitations

- no built-in authentication, TLS, or access control
- plugin bundles are not sandboxed from the host page
- assumes a trusted, single-user Nanite backend
- pre-1.0 contracts; the backend API it depends on can change
