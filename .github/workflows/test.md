# Reusable test workflow

Run your repo's test suite on pull requests and other workflow triggers. Supports pnpm [workspaces](https://pnpm.io/workspaces).

## Inputs

### `node_versions`
- **Type:** `string`
- **Required:** `false`
- **Default:** `"20"`
- **Description:** A JSON-parsable string of Node.js versions to be used in a matrix strategy. This allows testing across multiple Node versions. Example input: `'[18,20,22]'`.

### `build_script`
- **Type:** `string`
- **Required:** `false`
- **Default:** `"build"`
- **Description:** The build script to run using `pnpm run --recursive <build_script> --if-present`. If the script does not exist, it will be skipped.


## Example usage

### Setup

Create a workflow file, e.g., `.github/workflows/test.yml`.

Make sure your package.json has the `packageManager` field filled in and configure your test scripts to run via package script. For example:

```json
{
  "packageManager": "pnpm@9.4",
  "scripts": {
    "test": "vitest run"
  }
}
```

### Basic usage

```yaml
name: Test

on:
  pull_request:
    branches:
      - main

jobs:
  test:
    uses: reuters-graphics/action-workflows/.github/workflows/test.yaml@main
    secrets: inherit
    env:
      TESTING: true
```

### Custom Node versions and build script

```yaml
name: Test

on:
  pull_request:
    branches:
      - main

jobs:
  test:
    uses: reuters-graphics/action-workflows/.github/workflows/test.yaml@main
    secrets: inherit
    env:
      TESTING: true
    with:
      node_versions: '[20,22]'
      build_script: 'build:lib'
```
