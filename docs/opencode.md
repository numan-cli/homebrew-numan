# OpenCode agent workflows

This repository runs [opencode-review-threads](https://github.com/marketplace/actions/opencode-review-threads)
via two reusable workflows pinned to the floating `v1` tag.

## Workflows

- `.github/workflows/opencode-review.yml` posts an incremental review on every
  PR open, reopen, synchronize, and ready-for-review. Findings arrive as
  individually resolvable inline threads plus one canonical summary comment.
- `.github/workflows/opencode.yml` answers `/oc` (or `/opencode`) comments on
  PRs and issues, and accepts ad-hoc prompts via **Actions > OpenCode > Run
  workflow**.

## Commands

| Command | Behavior |
|---|---|
| `/oc review` | Re-run the incremental PR review on demand |
| `/oc fix` | Run the merge-readiness pass: triage unresolved review threads, merge conflicts, then failing CI |
| `/oc fix <target>` | Same pass scoped to the named feedback or file |
| `/oc autopilot` / `prompt: /autopilot` | Alias for the same merge-readiness pass (also usable from `workflow_dispatch`) |
| `/oc <anything else>` | Free-form prompt run against the PR/issue context |

Only comments from `OWNER`, `MEMBER`, `COLLABORATOR`, or `CONTRIBUTOR`
associations trigger the bot; bot-authored comments are skipped.

## Secrets

The workflows read provider credentials from org/repo secrets:
`OPENCODE_API_KEY`, `CLOUDFLARE_ACCOUNT_ID`, `CLOUDFLARE_API_TOKEN`
(and optionally `CLOUDFLARE_API_KEY`, `CONTEXT7_API_KEY`). Model selection
probes `models-review` / `models-fix` chains in order and uses the first
reachable model.
