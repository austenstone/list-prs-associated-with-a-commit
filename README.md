# Action

It's hard to get the PR associated with a merge because of rebase or squash.

This action uses the [List pull requests associated with a commit](https://docs.github.com/en/rest/commits/commits?apiVersion=2022-11-28#list-pull-requests-associated-with-a-commit) API to get the PRs associated with a commit.

## Usage
Create a workflow (eg: `.github/workflows/usage.yml`). See [Creating a Workflow file](https://help.github.com/en/articles/configuring-a-workflow#creating-a-workflow-file).

<!-- 
### PAT(Personal Access Token)

You will need to [create a PAT(Personal Access Token)](https://github.com/settings/tokens/new?scopes=admin:org) that has `admin:org` access.

Add this PAT as a secret so we can use it as input `github-token`, see [Creating encrypted secrets for a repository](https://docs.github.com/en/enterprise-cloud@latest/actions/security-guides/encrypted-secrets#creating-encrypted-secrets-for-a-repository). 
### Organizations

If your organization has SAML enabled you must authorize the PAT, see [Authorizing a personal access token for use with SAML single sign-on](https://docs.github.com/en/enterprise-cloud@latest/authentication/authenticating-with-saml-single-sign-on/authorizing-a-personal-access-token-for-use-with-saml-single-sign-on).
-->

#### Example
```yml
name: Usage
on:
  push:
  pull_request:
  workflow_dispatch:

jobs:
  run:
    name: Run Action
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - id: get-pr
        uses: austenstone/list-prs-associated-with-a-commit@main
      - run: echo '${{ fromJson(steps.get-pr.outputs.prs)[0].number }}'
      - run: echo '${{ fromJson(steps.get-pr.outputs.pr-numbers)[0] }}'
```

## ➡️ Inputs
Various inputs are defined in [`action.yml`](action.yml):

| Name | Description | Default |
| --- | - | - |
| github&#x2011;token | Token to use to authorize. | ${{&nbsp;github.token&nbsp;}} |
| owner | Owner of the repository. | N/A |
| repo | Name of the repository. | N/A |
| sha | The commit SHA. | ${{&nbsp;github.event.after&nbsp; || &nbsp;github.sha&nbsp;}} |


## ⬅️ Outputs
| Name | Description |
| --- | - |
| prs | The PRs associated with the commit as JSON. |
| pr-numbers | The PR numbers associated with the commit as JSON. |

## Further help
To get more help on the Actions see [documentation](https://docs.github.com/en/actions).
