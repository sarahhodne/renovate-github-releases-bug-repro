# Renovate github-releases bug reproduction

This repository is a [minimal reproduction](https://github.com/renovatebot/renovate/blob/main/docs/development/minimal-reproductions.md) for a Renovate bug I have noticed.

## current behaviour

The logs pick up the goreleaser version in the `repro` file, and correctly parses the current version as v0.167.0, however, Renovate does not think there's a new version despite there being many new versions:

https://github.com/goreleaser/goreleaser/releases

## expected behaviour

Renovate opens a PR upgrading goreleaser to the latest version (v1.7.0 at the time of writing).

## noteworthy

I noticed that v0.167.0 is old enough that it doesn't show in the list of releases in the API without paginating:

```
❯ curl --silent 'https://api.github.com/repos/goreleaser/goreleaser/releases' | jq 'map(.name)'
[
  "v1.7.0",
  "v1.6.3",
  "v1.6.2",
  "v1.6.1",
  "v1.6.0",
  "v1.5.0",
  "v1.4.1",
  "v1.4.0",
  "v1.3.1",
  "v1.3.0",
  "v1.2.5",
  "v1.2.4",
  "v1.2.3",
  "v1.2.2",
  "v1.2.1",
  "v1.2.0",
  "v1.1.0",
  "v1.0.0",
  "v0.184.0",
  "v0.183.0",
  "v0.182.1",
  "v0.182.0",
  "v0.181.1",
  "v0.181.0",
  "v0.180.3",
  "v0.180.2",
  "v0.180.1",
  "v0.180.0",
  "v0.179.0",
  "v0.178.0"
]
```

I'm not 100% sure of what API calls Renovate call, but from the logs it seems like it's this one, so maybe the update doesn't get picked up because the old version is too old?
