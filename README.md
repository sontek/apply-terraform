# apply-terraform
Github action that applies terraform changes after they are planned.

If you are looking for `terraform plan` check out:

https://github.com/sontek/plan-terraform


## Usage
To use it you can define an action that runs on pull requests:

# plan-terraform
Github action that validates and plans terraform and provides
a nice output.


## Usage
To use it you can define an action that runs on pull requests:

```yaml
name: pr_build

on:
  pull_request:
    branches:
      - main

jobs:
  Terraform:
    name: Terraform
    runs-on: ubuntu-22.04
    permissions:
      contents: read
      pull-requests: write
    strategy:
      fail-fast: false
    steps:
      - uses: actions/checkout@v3

      - uses: sontek/apply-terraform@v2
        with:
          path: "."
          terraform-token: ${{ secrets.TFE_TOKEN }}
```

You can also a `matrix` and iterate over multiple paths if necessary.

By default it'll use the `latest` terraform version available but you can create a
`.tool-versions` file in the root of the repository and define a different version
like this:

```
terraform 1.4.2
```

## Inputs
`path` (optional): This is the path where the terraform configuration lives
`terraform-token` (optional): This is the token to communicate with terraform cloud if you are using it

## Auto-merge
If you would like to also automatically merge the PR when it has been applied you can do the following:

```yaml
name: pr_apply

on:
  pull_request:
    types: [ labeled ]

jobs:
  TerraformApply:
    if: ${{ github.event.label.name == 'tfc-apply' }}
    runs-on: ubuntu-22.04
    permissions:
      contents: read
      pull-requests: write
    strategy:
      fail-fast: false
      matrix:
        folder:
          - root
    steps:
      - uses: actions/checkout@v3
      - uses: sontek/apply-terraform@v1.1
        with:
          path: ${{ matrix.folder }}
          terraform-token: ${{ secrets.TFE_TOKEN }}
```