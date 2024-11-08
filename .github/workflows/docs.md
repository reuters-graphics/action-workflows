# Reusable docs workflow

Publish your docs to GitHub Pages via an action.

## Inputs

### `docs_directory`
- **Type:** `string`
- **Required:** `false`
- **Default:** `"./docs"`
- **Description:** The directory (relative to project root) where your docs will be built to.


## Example usage

### Setup

Create a workflow file, e.g., `.github/workflows/docs.yml`.

Make sure your package.json has the `packageManager` field filled in and, if needed, configure your docs to build via package script `build:docs`. For example:

```json
{
  "packageManager": "pnpm@9.4",
  "scripts": {
    "build:docs": "astro build"
  }
}
```

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
```