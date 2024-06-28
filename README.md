# Policy Push Action

## policy-push-action

Push (publish) policy image to registry.


## Inputs

### `tags`

**Required** The image tags to be pushed to the registry.

Default: empty

### `verbosity`

**Required** The logging verbosity level [ `info` | `error` | `debug` | `trace` ] used.

Default: `error`


## Outputs

None defined


## Example

```
name: policy-build-release

on:
  workflow_dispatch:
  push:
    tags:
    - '*'

jobs:
  release_policy:
    runs-on: ubuntu-latest
    name: build
    steps:

    - uses: actions/checkout@v4

    - name: Policy Login
      id: policy-login
      uses: opcr-io/policy-login-action@v3
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      with:
        username: policy-bot
        password: "$GITHUB_TOKEN"

    - name: Policy Build
      id: policy-build
      uses: opcr-io/policy-build-action@v3
      with:
        src: peoplefinder/src
        tags: datadude/peoplefinder:$(sver -n patch)
        revision: "$GITHUB_SHA"

    - name: Policy Push
      id: policy-push
      uses: opcr-io/policy-push-action@v3
      with:
        tag: datadude/peoplefinder:$(sver -n patch)

    - name: Policy Logout
      id: policy-logout
      uses: opcr-io/policy-logout-action@v3

```
