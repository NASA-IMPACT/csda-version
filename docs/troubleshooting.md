# Troubleshooting Guide

#### Required Workflow Permissions

Ensure your `.github/workflows/release-please.yml` file includes the following permissions:

```yaml
permissions:
  contents: write
  pull-requests: write
```

These permissions are required for release-please to:
- Create and update release PRs
- Create tags and GitHub releases
- Update changelog files

#### Verify CSDA Release App Access

- Visit the [CSDA Release App installations page](https://github.com/apps/csda-release-app/installations/105597643)
- Verify your repository is listed under `Repository access`
- If your repository is missing select the one you would like to add

### Resolving Stale Versions in Release Please

**Release-Please Workflow:**

- When a PR is merged to `main`, release-please creates a "Release PR" with the label `autorelease: pending`
- When the Release PR is merged, release-please:
   - Creates a Git tag
   - Publishes a GitHub Release
   - Removes `autorelease: pending` and adds `autorelease: tagged`

**Common problem:**

If the release-please action fails, it never removes the `autorelease: pending` label. This causes release-please to ignore any workflow changes and continuously retries the failed release.

#### Examples

- You update `release-as` in your workflow to a new version, but release-please keeps trying to create the old version
- Release-please workflow runs but ignores your configuration changes
- You see the error message `Resource not accessible by integration` or permission errors in the workflow logs

#### Solution

- Find the Release PR that failed with the `autorelease: pending` label
- Manually remove the `autorelease: pending` label from the PR
- Re-run the release-please workflow
- This clears release-please's "memory" of the failed release and allows it to read your updated configuration