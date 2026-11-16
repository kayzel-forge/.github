# Kayzel Forge shared workflows

## Coolify deploy

`coolify-deploy.yml` deploys one Coolify app from a job on the homelab runner pool and records it as a GitHub deployment. The job's `environment` creates the deployment row, and its `url` is the link on that row. Coolify has no commit parameter, so it builds the head of the app's branch. The job summary names the commit Coolify built, and a warning marks a run whose commit was not the one built.

The caller repo needs a `COOLIFY_TOKEN` repository secret. Org secrets do not reach private repos on the Free plan.

```yaml
jobs:
  deploy-aperture:
    uses: kayzel-forge/.github/.github/workflows/coolify-deploy.yml@main
    with:
      app: 9i6g81wudlfrdznd3irkrtjp
      environment: aperture
      url: https://aperture.lan
    secrets: inherit
```

This repo is public so the org profile and these workflows are readable by every org repo. It runs no jobs itself, and public repos cannot use the homelab runners.
