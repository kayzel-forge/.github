# Kayzel Forge shared workflows

## Coolify deploy

`coolify-deploy.yml` deploys one Coolify app from a job on the homelab runner pool and records it as a GitHub deployment. The job's `environment` creates the deployment row, and its `url` is the link on that row. Callers run it from `workflow_dispatch` only: a push deploys nothing. Agents and Updates start those runs through the homelab-coolify skill's `deploy` command.

The job runs on `homelab-deploy`, a label every pool runner carries and one runner carries alone, so a deploy never waits behind CI.

Coolify has no commit parameter, so it builds the head of the app's branch. Before it builds, the job reads the commit of the image Coolify runs now. When that commit already contains the run's commit, the job ends green as already live and builds nothing, so agents that finish together do not rebuild the same app. With `rollback-to`, the job asks Coolify to run an image it still holds for that commit, which takes seconds, not a build. The job summary names the commit Coolify deployed, and a warning marks a run whose commit was not the one deployed.

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
