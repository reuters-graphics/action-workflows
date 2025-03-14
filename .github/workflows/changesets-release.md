# Reusable release workflow

Publish your package to npm with changesets.

## Inputs

### `node_version`
- **Type:** `string`
- **Required:** `false`
- **Default:** `"20"`
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
      node_version: '20'
      build_script: 'build:lib'
      publish_docs: true
      docs_directory: './myDocs'
```
