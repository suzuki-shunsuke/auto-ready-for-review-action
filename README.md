# auto-ready-for-review-action

GitHub Action to make draft pull requests ready for review when CI succeeds

[action.yaml](action.yaml)

## Motivation

When we create a pull request, instead of opening it as ready for review from the beginning, we open a pull request as draft first and change the status of the pull request `Ready for review` after confirming CI passes.

1. Open a pull request as draft
2. Wait until CI finishes and confirm CI passes
3. Change the status of the pull request to `Ready for review`

We think this is a manner to collaborate with reviewers comfortably.
But if it takes a long time to finish CI, we have to wait for a long time and sometimes forget to make the pull request `Ready for review`.

To solve the issue, we create this action to make pull requests `Ready for review` after their CI succeed.
Please add a pull request label `auto-ready-for-review` (You can change this label) to draft pull requests, and run this action via [schedule event](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#schedule).
Then this action would make pull requests `Ready for review` automatically.

## Requiements

- [GitHub CLI](https://cli.github.com/)
- GitHub Access Token: The permission `pull-requests: write` is required. `GITHUB_TOKEN` secret is available

## Usage

Please run this action via [schedule event](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows#schedule).
And please add a pull request label `auto-ready-for-review` (You can change this label) to draft pull requests.

```yaml
name: Auto Ready for Review
on:
  schedule:
    - cron: '*/5 * * * *' # Execute every 5 minutes
  workflow_dispatch: {} # Enable us to run this workflow manually
jobs:
  auto-ready-for-review:
    runs-on: ubuntu-latest
    permissions:
      pull-requests: write # To update pull requests
    steps:
      - uses: suzuki-shunsuke/auto-ready-for-review-action@main
```

## Inputs

### Required

Nothing.

### Optional

- `github_token`: GitHub Access Token to update pull requests. The permission `pull-requests:write` is required. `GITHUB_TOKEN` secret is used by default
- `label`: A pull request label to be set to target pull requests. The default is `auto-ready-for-review`
- `comment`: A comment template. The default is `CI succeeded so this pull request got ready for review!`

## Oputputs

Nothing.

## LICENSE

[MIT](LICENSE)
