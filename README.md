# repo-template

Template repository for new `smartwatermelon` repos. Created via:

```bash
gh repo create <name> --template smartwatermelon/repo-template
```

or, preferably, the wrapper script in `smartwatermelon/github-workflows` that also handles the
one repo-setting this template can't seed:

```bash
new-smartwatermelon-repo.sh <name>
```

## What this template gives you

- `.github/workflows/dependabot-auto-merge.yml` — auto-approves and merges patch/minor
  Dependabot PRs once CI passes.
- `.github/dependabot.yml` — weekly GitHub Actions dependency bumps.
- `CLAUDE.md` — starter file for repo-specific Claude Code guidance.

See `smartwatermelon/github-workflows`'s `README.md` for the full documentation on each
reusable workflow, including versioning and security invariants.

## Manual steps still required after creation

GitHub gives no way to seed these from a template repo or a `gh repo create` call — they need
one-time action per new repo:

1. **`can_approve_pull_request_reviews`** — required for `dependabot-auto-merge.yml`'s approval
   step to succeed. The `new-smartwatermelon-repo.sh` wrapper sets this automatically; if you
   used a bare `gh repo create --template`, set it yourself:

   ```bash
   gh api -X PUT repos/smartwatermelon/<name>/actions/permissions/workflow \
     -F can_approve_pull_request_reviews=true \
     -f default_workflow_permissions=read
   ```

2. **Branch protection** (optional but recommended) — add
   `standards-check / run-standards-check` as a required status check under
   Settings → Branches. (`claude-review / run-review` is retired in W3.)
