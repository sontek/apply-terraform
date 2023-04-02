# apply-terraform
Github action that applies terraform changes after they are planned


## Usage
To use it you can define an action that runs on pull requests:

```yaml
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
```