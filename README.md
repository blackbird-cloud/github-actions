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
    steps:
      - uses: blackbird-cloud/github-actions/setup@v1    
      - uses: blackbird-cloud/github-actions/changed-files
  terraform:
    runs-on: ubunut-latest
    strategy:
      matrix:
        target: ${{ jobs.setup.output.matrix }}
    steps:
      - uses: blackbird-cloud/github-actions/terragrunt-aws
      - uses: blackbird-cloud/github-actions/terragrunt-plan@v1    
        with:
          matrix: ${{ matrix.target }}
```