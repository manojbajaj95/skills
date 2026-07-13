# Install wiki CI

Install a CI workflow that runs **wiki update** on each push to the default branch. Detect the CI framework (GitHub Actions or GitLab CI), generate the config, and open a PR. Keeps `wiki/` in sync with small surgical edits — never a full wipe.

## When to use

Use when the user wants to:
- Set up automated wiki generation in CI
- Install a wiki refresh workflow
- Configure automatic documentation updates on push

## Prerequisites

- A GitHub or GitLab hosted repository
- Permission to add secrets/variables and merge PRs
- An agent CLI (or other runner) that can invoke this wiki skill in headless/non-interactive mode
- Credentials for that runner available as CI secrets (exact names depend on the chosen agent)

## Workflow

1. Detect the CI framework in use (GitHub Actions, GitLab CI, or other)
2. Detect or ask which agent runner the repo already uses (or prefers) for headless work
3. Generate the appropriate workflow configuration file
4. Open a pull request with the workflow file for review
5. Instruct the user to add the runner's required secrets/variables in CI settings
6. Once secrets are configured and the PR is merged, the wiki refreshes on every push to the default branch

Prefer committing the generated `wiki/` directory back into the repository so the knowledge base stays in-tree and vendor-neutral. Optional extras (GitHub wiki tab sync, external hosting) are add-ons — not required for install.

## Choose the runner

Do not hard-code a vendor. Infer from the repo when possible:

| Signal | Prefer |
| --- | --- |
| Existing workflow that already runs an agent CLI | Reuse that same install + auth pattern |
| User names a runner | Use that runner |
| Ambiguous | Ask before generating the workflow |

The generated job should:

1. Check out the repo (full history so update can diff against `.wiki-meta.json` `commitHash`)
2. Install or restore the chosen agent CLI
3. Invoke this wiki skill non-interactively with an **update** prompt (e.g. "Run wiki update — surgically refresh wiki/ for recent changes; never wipe")
4. Commit and push `wiki/` (and AGENTS/CLAUDE pointer changes if any) when the update writes files — or skip the commit when the skill reports a no-op

## Generated workflows

Replace the runner install/auth/invoke steps with whatever the chosen agent requires. Branch name should match the repo default if it is not `main`.

### GitHub Actions

Creates `.github/workflows/wiki-refresh.yml`:

```yaml
name: Wiki Refresh

on:
  push:
    branches: [main] # change to your default branch if not main

permissions:
  contents: write

jobs:
  wiki-refresh:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Install agent CLI
        run: |
          # Install the repo's chosen headless agent runner here.
          # Keep this step aligned with how the team already runs agents in CI.

      - name: Update wiki
        run: |
          # Invoke the wiki skill non-interactively, e.g.:
          #   <agent> -p "Run wiki update — surgically refresh wiki/ for recent code changes; never wipe the wiki"
          #
          # Pass auth via env from repository secrets — do not hard-code vendor keys.
        env:
          # Map secrets required by the chosen runner, e.g.:
          # API_KEY: ${{ secrets.AGENT_API_KEY }}

      - name: Commit wiki
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git add wiki/
          if git diff --staged --quiet; then
            echo "No wiki changes"
            exit 0
          fi
          git commit -m "docs(wiki): refresh from CI"
          git push
```

### GitLab CI

Appends a job to the existing `.gitlab-ci.yml` (or creates one if missing):

```yaml
wiki-refresh:
  stage: deploy
  rules:
    - if: $CI_COMMIT_BRANCH == $CI_DEFAULT_BRANCH
  before_script:
    - |
      # Install the repo's chosen headless agent runner here.
  script:
    - |
      # Invoke the wiki skill non-interactively (same prompt intent as GitHub Actions).
      # Auth via CI/CD variables — do not hard-code vendor keys.
    - |
      git config user.name "gitlab-ci"
      git config user.email "gitlab-ci@example.com"
      git add wiki/
      if git diff --staged --quiet; then
        echo "No wiki changes"
        exit 0
      fi
      git commit -m "docs(wiki): refresh from CI"
      git push "https://gitlab-ci-token:${CI_JOB_TOKEN}@${CI_SERVER_HOST}/${CI_PROJECT_PATH}.git" "HEAD:${CI_DEFAULT_BRANCH}"
  variables:
    # Map variables required by the chosen runner
```

Both workflows should:

- **Trigger on push** to the default branch
- **Install the chosen agent runner** (not a fixed vendor installer)
- **Run wiki update** headlessly (surgical edits; never wipe)
- **Persist `wiki/`** by committing it when files change (default), or skip commit on no-op

## Post-install instructions

After generating the workflow and opening the PR, tell the user:

1. Add the runner's required secrets/variables in CI settings:
   - **GitHub**: Repository **Settings > Secrets and variables > Actions** (or org-level equivalents)
   - **GitLab**: Repository **Settings > CI/CD > Variables** (or group-level equivalents)
2. Confirm the workflow's runner install + invoke steps match how they already run agents locally/in CI
3. Merge the workflow PR
4. The wiki will refresh on every push to the default branch (and land as a commit under `wiki/` unless they chose artifacts-only)

## Optional: GitHub wiki tab

If the user also wants the GitHub wiki tab updated, that is a separate post-step after `wiki/` is generated. The GitHub wiki must already be initialized (create the first page manually at `https://github.com/{owner}/{repo}/wiki`). Do not treat GitHub wiki sync as the primary storage — prefer the in-repo `wiki/` directory.

## Customization

The user can customize the workflow after installation:

- Change the trigger branch
- Add path filters so regeneration runs only when source files change
- Adjust timeout / machine size for large repos
- Skip the commit step and publish `wiki/` as a workflow artifact instead
- Narrow the agent prompt (e.g. force incremental-only)

## Troubleshooting

- Confirm the runner secret/variable names in the workflow match what is configured in CI settings
- Check that the workflow file path and branch trigger match the default branch
- Inspect the CI run logs for agent install, auth, and wiki generation failures
- If commits fail, verify the job has permission to push (`contents: write` on GitHub; push token/permissions on GitLab)
- If incremental mode never triggers, ensure checkout uses full history (`fetch-depth: 0`) so git diffs against `.wiki-meta.json`'s `commitHash` work
