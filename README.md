# Use App Token

This GitHub Action can be used to impersonate a GitHub App when `secrets.GITHUB_TOKEN`'s limitations are too restrictive and a personal access token is not suitable.

[`secrets.GITHUB_TOKEN`](https://help.github.com/en/actions/configuring-and-managing-workflows/authenticating-with-the-github_token) has limitations such as [not being able to triggering a new workflow from another workflow](https://github.community/t5/GitHub-Actions/Triggering-a-new-workflow-from-another-workflow/td-p/31676). A workaround is to use a [personal access token](https://help.github.com/en/github/authenticating-to-github/creating-a-personal-access-token-for-the-command-line) from a [personal user/bot account](https://help.github.com/en/github/getting-started-with-github/types-of-github-accounts#personal-user-accounts). However, for organizations, GitHub Apps are [a more appropriate automation solution](https://developer.github.com/apps/differences-between-apps/#machine-vs-bot-accounts).

# Example Workflow

```yml
jobs:
  job:
    runs-on: ubuntu-latest
    steps:
      - name: Generate token
        id: generate_token
        uses: bubkoo/use-app-token@v1
        with:
          APP_ID: ${{ secrets.APP_ID }}
          PRIVATE_KEY: ${{ secrets.PRIVATE_KEY }}
      - name: Use token
        env:
          TOKEN: ${{ steps.generate_token.outputs.token }}
        run: |
          echo "The generated token is masked: ${TOKEN}"
```
