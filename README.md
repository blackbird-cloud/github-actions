# Blackbird Cloud Github Actions

Collection of github actions to make it easier to deploy terraform code.

## Providers

Getting state from the following providers

- AWS
- Azure
- Google Cloud
- Terraform cloud

## List of actions

Some extra actions to setup all the tooling

- Changed files
- Git checkout
- Decrypt sops
- Setup
  - Git checkout
  - Changed files


## Terragrunt actions
- format
- plan
- apply
- destroy

## Example job

As most projects use multiple folders for different envirionments a matrix strategy is needed to deploy only changes. In the following example the environments are in the `cloud` folder. Use this example in you main workflow to plan your terraform.
```yaml
jobs:
  setup:
    runs-on: ubuntu-latest
    outputs:
      matrix: ${{ steps.setup.outputs.files }}
    steps:
      - uses: blackbird-cloud/github-actions/setup@v1
        id: setup
        with:
          folder: cloud
  terraform-plan:
    runs-on: ubuntu-latest
    needs: setup
    if: ${{ needs.setup.outputs.matrix != '[]' && needs.setup.outputs.matrix != '' }}
    strategy:
      matrix:
        target: ${{ fromJSON(needs.setup.outputs.matrix) }}
    steps:
      - uses: blackbird-cloud/github-actions/terragrunt/plan@v1
        with:
          cloud: AWS
          matrix: ${{ matrix.target }}
          aws-region: eu-central-1
          aws-role-to-assume: arn:aws:iam::1234567890:role/GitHub
```
