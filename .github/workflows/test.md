# Test workflow

Run your repo's test suite on pull requests and other workflow triggers. Supports pnpm [workspaces](https://pnpm.io/workspaces).

## Inputs

### `node_versions`
- **Type:** `string`
- **Required:** `false`
- **Default:** `"20"`
- **Description:** A comma-separated string of Node.js versions to be used in a matrix strategy. This allows testing across multiple Node versions. The default value is `"20"`. Example input: `'18,20,22'`.

### `build_script`
- **Type:** `string`
- **Required:** `false`
- **Default:** `"build"`
- **Description:** The build script to run using `pnpm run --recursive <build_script> --if-present`. If the script does not exist, it will be skipped.


## Example usage

### Setup

Create a workflow file (e.g., `.github/workflows/test.yml`).

Configure your test scripts to run via package script. For example:

```json
// package.json
{
  "scripts": {
    "test": "c8 mocha test/**/*.ts"
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
  use-reusable:
    uses: reuters-graphics/action-workflows/.github/workflows/test.yml@main
    secrets: inherit
```

### Custom Node versions and build script

```yaml
name: Test

on:
  pull_request:
    branches:
      - main

jobs:
  use-reusable:
    uses: reuters-graphics/action-workflows/.github/workflows/test.yml@main
    secrets: inherit
    with:
      repository: ${{ github.repository }}
      node_versions: '20,22'
      build_script: 'build:lib'
```
