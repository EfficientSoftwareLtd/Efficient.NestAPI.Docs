# Efficient.NestAPI.Docs

Source for the NestAPI developer documentation site, published with [Mintlify](https://mintlify.com).

The site combines **hand-written guides** (the `.mdx` files in this repo) with an
**auto-generated API reference** built from `openapi.json`.

## Branches / environments

| Branch        | API base URL                  | Docs URL                       |
|---------------|-------------------------------|--------------------------------|
| `development` | `https://dev.api.nestapi.com` | `https://dev-docs.nestapi.com` |
| `master`      | `https://api.nestapi.com`     | `https://docs.nestapi.com`     |

Each branch is a separate Mintlify project. Mintlify auto-deploys on push.

### Differences between the two `mint.json` files

`mint.json` on this (`development`) branch is the staging config. When promoting content to
`master`, the only two differences are:

1. Remove the `"topAnchor"` staging banner.
2. Change `"api.baseUrl"` from `https://dev.api.nestapi.com` to `https://api.nestapi.com`.

Everything else (navigation, groups, colours) is identical, so promote by merging
`development` → `master` and keeping `master`'s `mint.json` overrides for those two fields.

## `openapi.json` is generated — do not hand-edit

`openapi.json` is produced automatically by the API deployment pipeline:

1. The API pipeline fetches the live Swagger 2.0 spec from `/openapi` after each deploy.
2. Converts it to OpenAPI 3.0 with the `swagger2openapi` npm package.
3. Sets the `servers` URL to the matching environment.
4. Commits it to the corresponding branch of this repo.

Only endpoints tagged `nesting`, `importer`, `public`, or `account` appear — the API's
spec filter (`RemoveNonDocumentedEndpoints`) strips everything else. To add or remove an
endpoint from the reference, change its `[Tag(...)]` in the API repo, not this repo.

See `DOCUMENTATION_SITE_PLAN.md` in the API repo (`src/docs/`) for the full design.

## Local preview

```bash
npm i -g mintlify
mintlify dev
```

## Writing guides

Guides live at the repo root and under `guides/`, `concepts/`, and `account/`. Add new pages
to the `navigation` array in `mint.json`. Prose is authored by the dev/support team on the
`development` branch and promoted to `master` when ready for customers.
