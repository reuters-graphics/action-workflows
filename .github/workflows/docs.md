# Reusable docs workflow

Publish your docs to GitHub Pages via an action.

## Inputs

### `docs_directory`
- **Type:** `string`
- **Required:** `false`
- **Default:** `"./docs"`
- **Description:** The directory (relative to project root) where your docs will be built to.

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

## Example usage

### Setup

Setup GitHub Pages on your repo and choose [GitHub Actions as the publishing source](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-with-a-custom-github-actions-workflow).

Create a workflow file, e.g., `.github/workflows/docs.yml`.

Make sure your package.json has the `packageManager` field filled in and, if needed, configure your docs to build via package script `build:docs`. For example:

```json
{
  "packageManager": "pnpm@9.6.0",
  "scripts": {
    "build:docs": "astro build"
  }
}
```

It's alse generally recommended to add the `docs/` directory (or whichever directory your docs are built to) to your `.gitignore`.

### Basic usage

```yaml
name: Docs

on:
  push:
    branches:
      - main

jobs:
  docs:
    uses: reuters-graphics/action-workflows/.github/workflows/docs.yaml@main
    secrets: inherit
```

### Custom docs directory

```yaml
name: Docs

on:
  push:
    branches:
      - main

jobs:
  docs:
    uses: reuters-graphics/action-workflows/.github/workflows/docs.yaml@main
    secrets: inherit
    with:
      docs_directory: './dist'
      node_version: '22'
```
