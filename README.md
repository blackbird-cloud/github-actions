# Blackbird Cloud Github Actions

## Terragrunt

Getting state from the following supported providers

- AWS
- Azure
- Google Cloud
- Terraform cloud

## Example job
```yaml
jobs:
  setup:
    runs-on: ubuntu-latest
    outputs:
      matrix: ${{ steps.setup.outputs.matrix }}
    steps:
      - uses: blackbird-cloud/github-actions/setup@v1
        id: setup
  terraform:
    runs-on: ubuntu-latest
    needs: setup
    strategy:
      matrix:
        target: ${{ needs.setup.output.matrix }}
    steps:
      - uses: blackbird-cloud/github-actions/terragrunt/apply@v1
        with:
          cloud: AWS
          matrix: ${{ matrix.target }}
```
