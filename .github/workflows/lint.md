# Lint node projects

Run ESLint and Prettier in CI.

## Inputs

### `node_version`
- **Type:** `string`
- **Required:** `false`
- **Default:** `"20"`
- **Description:** Node.js version to be used.

## Example usage

### Setup

Create a workflow file, e.g., `.github/workflows/release.yml`.

Make sure ESLint and Prettier are configured for your project (probably using [yaks](https://reuters-graphics.github.io/yaks/)) and your package.json has the `packageManager` field filled in.

```json
{
  "packageManager": "pnpm@9.6.0"
}
```

### Basic usage

```yaml
name: Lint

on:
  pull_request:
    branches:
      - main

jobs:
  lint:
    uses: reuters-graphics/action-workflows/.github/workflows/lint.yaml@main
    secrets: inherit
```

### Custom Node version

```yaml
name: Lint

on:
  pull_request:
    branches:
      - main

jobs:
  lint:
    uses: reuters-graphics/action-workflows/.github/workflows/lint.yaml@main
    secrets: inherit
    with:
      node_version: '22'
```
