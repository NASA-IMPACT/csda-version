# csda-version

A Github action to calculate the next [CSDA version](#csda-version) for the checked-out repository.

To use with [release-please](https://github.com/googleapis/release-please):

```yaml
  steps:
    - name: CSDA Version
      id: csda-version
      uses: NASA-IMPACT/csda-version@<hash-version> # hash-version is recommended for extra security 
    - uses: googleapis/release-please-action@<hash-version> # hash-version is recommended for extra security
      with:
        token: ${{ secrets.GITHUB_TOKEN }}
        release-type: simple
        release-as: ${{ steps.csda-version.outputs.version }}
        config-file: ${{ steps.csda-version.outputs.release-please-config }}
```


> [!IMPORTANT]
> You must use `release-type` in your Github Action YAML, you cannot use [Manifest Driven](https://github.com/googleapis/release-please/blob/main/docs/manifest-releaser.md) releasing.
> This is because `release-as` is ignored for manifest-driven releases.

To see this _in_ action, check out https://github.com/NASA-IMPACT/csdap-frontend/blob/main/.github/workflows/release-please.yml.

## CSDA version

A CSDA version is formatted like `vYY.PI.SP-X`, where:

- `YY` is the last two digits of the year,
- `PI` is the "program increment", which is like a normal calendar quarter except that PI `1` starts Oct 1
- `SP` is the sprint number
- `X` is the release number in this sprint

> [!NOTE]
> While the CSDA `version` _can_ be updated manually (see [example commit](https://github.com/NASA-IMPACT/csda-version/commit/d10da9054f4229fd7c7769066520b14d61ce7c08)), there is [a Github cron job](https://github.com/NASA-IMPACT/csda-version/blob/main/.github/workflows/bump-version.yml) that runs weekly to handle routine [updates](https://github.com/NASA-IMPACT/csda-version/blob/main/src/csda_version/__init__.py).

## Included Commit Prefixes

The following commit prefixes are included in the CHANGELOG:

- **feat:** Features
- **fix:** Bug Fixes
- **refactor:** Refactor
- **chore:** Chores
- **docs:** Documentation
- **revert:** Revert

Commits with other prefixes will not appear in the generated changelog, but will still be included in the release.
