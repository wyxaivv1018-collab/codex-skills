# Push Scenarios

These are the detailed step-by-step commands for each push scenario.
Match the scenario from environment detection, then run the corresponding flow.

---

## Scenario A: Brand New Project (no .git, no remote repo)

Local: no .git, no code yet (or just started)
Remote: does not exist on GitHub

```
git init
git add .
git commit -m "init: initial commit"
```

Ask user: "This repo is not on GitHub yet. Create one via gh command line or on the web?"

If via gh:
```
gh repo create <repo-name> --public --source=. --remote=origin --push
```

If already created on web:
```
git remote add origin https://github.com/<user>/<repo>.git
git branch -M master
git push -u origin master
```

---

## Scenario B: Has Code, No Git At All

Local: has project files, no .git directory
Remote: exists on GitHub, has code

```
git init
git remote add origin https://github.com/<user>/<repo>.git
git add .
git commit -m "init: initial commit"
git pull origin <branch> --allow-unrelated-histories
```

If conflicts during pull -> resolve.
```
git push -u origin <branch>
```

---

## Scenario C: Has .git, No Remote Configured

Local: git init already done, maybe some commits too
Remote: exists on GitHub

```
git remote add origin https://github.com/<user>/<repo>.git
git pull origin <branch> --allow-unrelated-histories
git push -u origin <branch>
```

---

## Scenario D: Everything Ready, Daily Push

Local: has .git, has remote, working tree may have changes
Remote: exists, remote configured

```
git add .
git commit -m "<descriptive message>"
git push
```

If no changes staged -> alert user.

---

## Scenario E: Local Behind Remote

Local: has .git, has remote, but behind remote history
Remote: has new commits not in local

```
git pull
git push
```

If conflicts -> resolve.

---

## Scenario F: Dirty Working Tree Before Push

Local: has .git, has remote, but has uncommitted changes

```
git stash push -m "auto-stash before push"
git pull
git push
git stash pop
```

Or ask user if they want to commit first instead of stashing.
