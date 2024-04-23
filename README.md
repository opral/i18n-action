# 🥷 Ninja i18n [<img src="https://cdn.jsdelivr.net/gh/opral/monorepo@main/inlang/assets/md-badges/inlang.svg" alt="inlang ecosystem compatible badge" align="right" width="163" height="36">](https://inlang.com)

Automate your i18n workflow and ensure that your i18n files are always up to date with the latest changes in your source code.

![Ninja i18n comment cover image](https://raw.githubusercontent.com/opral/monorepo/main/inlang/source-code/github-lint-action/assets/ninja-i18n-cover.png)

## Workflow

https://github.com/opral/monorepo/assets/59048346/99049121-11db-4672-a7a8-e0d606426f4b

- When a pull request is opened or updated, the action will run and check for new i18n issues.
- If issues are found, the action creates a comment on the pull request with an issues report including links to the Fink localization editor to resolve the issues.
- After the issues are resolved, the action will automatically update the pull request comment to reflect the changes.
- If no issues are found, the action will simply not create any comments.

## Features

- Automated i18n linting for pull requests
- Detection of new broken inlang projects
- Comment creation with issue report
- Link creation to fix issues directly in [Fink localization editor](https://inlang.com/m/tdozzpar/app-inlang-finkLocalizationEditor)

## Getting Started

Add the following workflow file to the `main` branch of your repository in this path `.github/workflows/ninja_i18n.yml`

```yml
name: Ninja i18n action

on:
  pull_request_target:

# explicitly configure permissions, in case your GITHUB_TOKEN workflow permissions are set to read-only in repository settings
permissions: 
  pull-requests: write

jobs:
  ninja-i18n:
    name: Ninja i18n - GitHub Lint Action
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        id: checkout
        uses: actions/checkout@v4

      - name: Run Ninja i18n
        id: ninja-i18n
        # @main ensures that the latest version of the action is used
        uses: opral/ninja-i18n-action@main
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          
```

**Note:** Commit the workflow file to the `main` branch of your repository before testing.

### Test if it works

Create a pull request with changes to your i18n files and see if the action runs and creates a comment with the issues report:

1. Create a new `test-ninja` branch in your repository
2. Open the `test-ninja` branch in [Fink localization editor](https://fink.inlang.com) and delete a translation for testing purposes
3. Commit and push the change
4. Create a pull request from the `test-ninja` branch to `main`
5. Check if the action runs and creates a comment with the issues report

[Click here to watch a video showing the below steps in GitHub](https://www.loom.com/share/c4d15fefb0854ca4b75a85cdb0d2c7e3)

### Environmental Variables:

| Name                  | Requirement | Description |
| --------------------- | ----------- | ----------- |
| `GITHUB_TOKEN`        | _required_ | Usage: `GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}`,  Ninja i18n action uses this in-built GitHub token to make the API calls for interacting with GitHub. It is built into Github Actions and does not need to be manually specified in your secrets store. [More Info](https://help.github.com/en/actions/configuring-and-managing-workflows/authenticating-with-the-github_token)|

## Troubleshooting

Please make sure that:
1. you have set up an [inlang project](https://inlang.com//documentation/concept/project) with [lint rules](https://inlang.com/c/lint-rules) in your repository
2. you have activated actions for your repository:
    1. On GitHub.com, navigate to the main page of the repository
    2. Click **Actions** tab
    3. Click the button **Enable Actions on this repository**

3. actions and workflows are allowed in your repository:
    1. On GitHub.com, navigate to the main page of the repository
    2. Click **Settings** tab
    3. Click **Actions** in the left sidebar, then click **General**
    4. Select the desired option (e.g. **Allow all actions and workflows**) under **Actions permissions**
    5. Click **Save** to apply the settings
