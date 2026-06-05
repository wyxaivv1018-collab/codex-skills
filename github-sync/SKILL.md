---
name: github-sync
description: "Push, pull, clone GitHub repos across all git states."
---

# GitHub Sync

## Trigger Announcement

When this skill triggers, say one short line like:
"Let me check the git environment first according to the github-sync skill..."

## Step 1: Environment Detection

Run these 5 checks before any operation:

1. Has .git? -> git rev-parse --is-inside-work-tree 2>$null
2. Has remote origin? -> git remote -v
3. Working tree clean? -> git status --porcelain
4. Default branch? -> git ls-remote --symref origin HEAD (skip if no remote)
5. Proxy configured? -> git config http.proxy

After checks, match scenario and choose push or pull flow.

## Proxy Config

User proxy: http://127.0.0.1:7892

If not set:
```
git config http.proxy http://127.0.0.1:7892
git config https.proxy http://127.0.0.1:7892
```

For npm, set env vars:
```
$env:HTTP_PROXY="http://127.0.0.1:7892"
$env:HTTPS_PROXY="http://127.0.0.1:7892"
```

## Branch Detection

main vs master: same thing, different default name.
Detect: git ls-remote --symref origin HEAD if remote exists.
If no remote (new repo): default to master unless user specifies another.

## Push to GitHub

Match scenario from references/push-scenarios.md after detection.

## Pull / Clone from GitHub

Match scenario from references/pull-scenarios.md after detection.

## Conflict Handling

If pull/push has conflicts:
1. Try git merge --no-edit for auto-merge
2. If fails, check conflict files, use git add to mark resolved
3. Or git merge --abort to roll back
4. If local changes unimportant, stash or reset first

## Auth

If auth fails: tell user they need GitHub authentication.
Recommend gh auth login (GitHub CLI) or SSH key.
