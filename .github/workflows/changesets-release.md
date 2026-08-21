# Reusable release workflow

Publish your package to npm with changesets.

## Inputs

### `node_version`
- **Type:** `string`
- **Required:** `false`
- **Default:** `"22"`
- **Description:** Node.js version to be used.

### `build_script`
- **Type:** `string`
- **Required:** `false`
- **Default:** `"build"`
- **Description:** The build script to run using `pnpm run --recursive <build_script> --if-present`. If the script does not exist, it will be skipped.

### `publish_docs`
- **Type:** `boolean`
- **Required:** `false`
- **Default:** `false`
- **Description:** Whether to publish docs using [reusable `docs` action](./docs.md) after publishing to npm.

### `docs_directory`
- **Type:** `string`
- **Required:** `false`
- **Default:** `"./docs"`
- **Description:** The directory (relative to project root) where your docs will be built to.


## Secrets

Pass these with `secrets: inherit`.

### `NPM_TOKEN`
- **Required:** yes (to publish)
- **Description:** npm token with publish rights to your package's scope.

### `REPO_PAT_TOKEN`
- **Required:** no
- **Description:** A personal access token used as `GH_TOKEN` for the publish step, so `gh` calls made by npm lifecycle scripts can reach **other private repos**. Only needed if a lifecycle script (e.g. `prepublishOnly`) fetches build inputs from a different private repo — the default `GITHUB_TOKEN` is scoped to the calling repo and can't read them. When it isn't set, `GH_TOKEN` falls back to `GITHUB_TOKEN`.


## Outputs

### `published`
- **Type:** `string`
- **Description:** Whether packages were published to npm. Returns `'true'` if packages were published, `'false'` otherwise.


## Example usage

### Setup

Add [changesets](https://github.com/changesets/changesets) to your project:

```console
pnpm i -D @changesets/cli
pnpm changeset init
```

Make sure your package.json has the `packageManager` field filled in and changesets scripts added:

```json
{
  "packageManager": "pnpm@9.6.0",
  "scripts": {
    "changeset:version": "changeset version",
    "changeset:publish": "git add --all && changeset publish"
  }
}
```

Finally, create a workflow file, e.g., `.github/workflows/release.yml`.


### Basic usage

```yaml
name: Release

on:
  push:
    branches:
      - main

jobs:
  release:
    uses: reuters-graphics/action-workflows/.github/workflows/changesets-release.yaml@main
    secrets: inherit
```

### Custom inputs

```yaml
name: Release

on:
  push:
    branches:
      - main

jobs:
  release:
    uses: reuters-graphics/action-workflows/.github/workflows/changesets-release.yaml@main
    secrets: inherit
    with:
      node_version: '22'
      build_script: 'build:lib'
      publish_docs: true
      docs_directory: './myDocs'
```

### Using outputs

You can use the `published` output to run additional jobs conditionally after publishing:

```yaml
name: Release

on:
  push:
    branches:
      - main

jobs:
  release:
    uses: reuters-graphics/action-workflows/.github/workflows/changesets-release.yaml@main
    secrets: inherit

  notify:
    needs: release
    if: needs.release.outputs.published == 'true'
    runs-on: ubuntu-latest
    steps:
      - name: Send notification
        run: echo "Packages were published to npm!"
```
