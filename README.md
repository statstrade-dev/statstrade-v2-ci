## Statstrade CI

This repository owns the central CI workflows for `statstrade-v2`.

### Web deployment

`.github/workflows/deploy-web.yml` receives `app-changed` events from the
parent repository and the frontend submodule. It checks out the requested
source revisions, generates the frontend with:

```sh
lein run -m statsbuild.project.main libs pages
```

and deploys the two Next.js workspaces through the local Netlify CLI.

Configure these values in the `statstrade-v2-ci` repository:

- Secret `GH_TOKEN`: read access to the source repositories and dispatch access.
- Secret `GH_USER`: GitHub Container Registry username.
- Secret `NETLIFY_AUTH_TOKEN`: Netlify production deployment token.
- Variable `NETLIFY_WEB_MAIN_SITE_ID`: Netlify project ID for `v2.statstrade.io`.
- Variable `NETLIFY_WEB_SUPERADMIN_SITE_ID`: Netlify project ID for
  `super.statstrade.io`.

The workflow does not store application or Supabase secrets. Those values are
configured in each Netlify project’s production environment.
