# Update Version Action

This GitHub Action determines and outputs the next version for your repository based on the current branch and latest Git tag.  
It is designed to be used in CI/CD pipelines to automate versioning for releases and snapshot builds.

## How It Works

- **On `main` branch:**  
  - Finds the latest tag (e.g., `v1.2.3`), bumps the patch version (e.g., `v1.2.4`), and outputs it as the next release version.
- **On other branches:**  
  - Uses the latest tag and appends a snapshot suffix (e.g., `v1.2.3-snapshot`).

## Inputs

| Name             | Description                              | Required | Default    |
|------------------|------------------------------------------|----------|------------|
| `snapshot_suffix`| Suffix for snapshot versions             | false    | snapshot   |

## Outputs

| Name    | Description                                   |
|---------|-----------------------------------------------|
| version | The computed version string (e.g., `v1.2.4` or `v1.2.3-snapshot`) |
| major   | The major version number (e.g., `1`)          |

## Usage Example

```yaml
jobs:
  set-version:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Set version variables
        id: vars
        uses: morphstack/devops-toolkit/.github/actions/update-version-action@v1
        with:
          snapshot_suffix: 'dev'

      - name: Use version output
        run: echo "Version is ${{ steps.vars.outputs.version }}, Major is ${{ steps.vars.outputs.major }}"