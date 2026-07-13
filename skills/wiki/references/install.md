# Install wiki CI

Install a CI workflow that runs **wiki update** on each push to the default branch. Like [release-please](https://github.com/googleapis/release-please), keep a single open PR for wiki changes — update it on each run instead of pushing to the default branch.

## Prerequisites

- GitHub repository
- Headless agent CLI with CI secrets configured

## GitHub Actions

`.github/workflows/wiki-refresh.yml`:

```yaml
name: Wiki Refresh

on:
  push:
    branches: [main] # change to your default branch

permissions:
  contents: write
  pull-requests: write

jobs:
  wiki-refresh:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Install agent CLI
        run: |
          # Install the repo's headless agent runner

      - name: Install wiki skill
        run: npx skills add manojbajaj95/skills --skill wiki -y

      - name: Update wiki
        run: |
          # <agent> -p "wiki update"
        env:
          # API_KEY: ${{ secrets.AGENT_API_KEY }}

      - name: Open or update wiki PR
        env:
          GH_TOKEN: ${{ github.token }}
          BRANCH: wiki-refresh/update
          LABEL: autowiki: pending
          BASE: ${{ github.event.repository.default_branch }}
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"

          if git diff --quiet -- wiki/; then
            echo "No wiki changes"
            exit 0
          fi

          gh label create "$LABEL" --color "1d76db" --description "Pending automated wiki refresh" 2>/dev/null || true

          EXISTING_PR=$(gh pr list --head "$BRANCH" --base "$BASE" --state open --json number --jq '.[0].number // empty')
          if [ -z "$EXISTING_PR" ]; then
            EXISTING_PR=$(gh pr list --label "$LABEL" --state open --json number --jq '.[0].number // empty')
          fi

          git checkout -B "$BRANCH"
          git add wiki/
          git commit -m "docs(wiki): refresh from CI"
          git push -u origin "$BRANCH" --force

          BODY="Automated wiki refresh. Merge to sync \`wiki/\` with recent changes on \`$BASE\`."

          if [ -n "$EXISTING_PR" ]; then
            gh pr edit "$EXISTING_PR" --title "docs(wiki): refresh wiki" --body "$BODY"
            echo "Updated existing PR #$EXISTING_PR"
          else
            gh pr create --base "$BASE" --head "$BRANCH" --title "docs(wiki): refresh wiki" --body "$BODY" --label "$LABEL"
            echo "Created wiki refresh PR"
          fi
```

## Post-install

1. Add runner secrets/variables in CI settings
2. Merge the workflow PR
3. Each push to the default branch opens or updates a wiki refresh PR — merge that PR to land `wiki/` changes
