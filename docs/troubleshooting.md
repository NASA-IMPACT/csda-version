# Troubleshooting Guide

## Permissions Issues

### Required Workflow Permissions

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

### Verify CSDA Release App Access

1. Visit the [CSDA Release App installations page](https://github.com/apps/csda-release-app/installations/105597643)
2. Verify your repository is listed under "Repository access"
3. If your repository is missing, add it to the installation

## Release-Please Stuck on Old Version

### Understanding the Release-Please Workflow

**Normal workflow:**

1. When a PR is merged to `main`, release-please creates a "Release PR" with the label `autorelease: pending`
2. When the Release PR is merged, release-please:
   - Creates a Git tag
   - Publishes a GitHub Release
   - Removes `autorelease: pending` and adds `autorelease: tagged`

**Common problem:**

If the release-please action crashes it never removes the `autorelease: pending` label. This causes release-please to ignore any workflow changes and continuously retry the failed release.

### Symptoms

- You update `release-as` in your workflow to a new version, but release-please keeps trying to create the old version
- Release-please workflow runs but ignores your configuration changes
- You see "Resource not accessible" or permission errors in workflow logs

-example image/image

### Solution

1. Find the Release PR with the `autorelease: pending` label
2. Manually remove the `autorelease: pending` label from the PR
3. Re-run the release-please workflow

This clears release-please's "memory" of the failed release and allows it to read your updated configuration.