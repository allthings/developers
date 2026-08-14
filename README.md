# Allthings Developer Docs

This is Allthings' only **public** documentation repository. It exists to host
specifications that we hand to people outside the organisation, at URLs that
must keep working.

It is plain Markdown, rendered by GitHub. There is no build step, no generator
and no website — deliberately. See [CLAUDE.md](CLAUDE.md) before adding or
moving anything.

## Contents

| Document | Audience | Implemented by |
| --- | --- | --- |
| [ERP Data Exchange Specification](erp-data-exchange-specification.md) | External ERP / consumption-data / document providers | [`nisaba`](https://github.com/allthings/nisaba) |

## What used to be here

Until 2024 this repo was the content source for a Gatsby site at
`developers.allthings.me`. That site is gone — the hostname now serves the
Swagger UI from [`api-doc`](https://github.com/allthings/api-doc), and the
generator repo (`allthings/documentation`) is archived.

The remaining guides were either obsolete or had an internal-only audience, so
they moved next to the code they describe:

| Was | Now |
| --- | --- |
| `oauth.md` | [`accounts`](https://github.com/allthings/accounts) → `docs/oauth.md` |
| `guides/master-data.md` | [`node-sdk`](https://github.com/allthings/node-sdk) → `examples/erp-master-data-interface/README.md` |
| `guides/micro-app.md` | [`connect`](https://github.com/allthings/connect) → `docs/FrontendBundles/` |
| `s3-access-for-external-data-providers.md` | [`nisaba`](https://github.com/allthings/nisaba) → `doc/s3-access-for-external-data-providers.md` |
| `guides/quickstart.md` | Removed. Superseded by connector solutions — OAuth clients are declared in a solution manifest (`frontendOauthClient`), not created by hand. |
| `apis/core-data.md` | Removed. Canonical: <https://api-doc.allthings.me/> |
| `apis/webhooks.md`, `webhooks/**` | Removed. The public webhooks API (`webhooks.allthings.me`) no longer exists. |
| `apis/dataamp.md` | Removed. Was a stale generated GraphQL schema dump; regenerate from [`dataamp`](https://github.com/allthings/dataamp) if needed. |
| `design-guide/**` | Removed. Last touched 2020, described `@allthings/elements@3`. |
| `urns.md` | Removed. Was an empty stub. |
