# Pull / Clone Scenarios

These are the detailed step-by-step commands for each pull/clone scenario.
Match the scenario from environment detection, then run the corresponding flow.

---

## Scenario A: Fresh Clone (empty directory)

Local: directory exists but is empty (or doesn't exist yet)
Remote: exists on GitHub

```
git clone https://github.com/<user>/<repo>.git
cd <repo>
```

git clone creates a subfolder by default. If user wants files in current dir, ask.

---

## Scenario B: Empty Dir with .git (git init done, no remote)

Local: directory is empty and has .git initialized
Remote: exists on GitHub

```
git remote add origin https://github.com/<user>/<repo>.git
git pull origin <branch>
```

Detect branch first (main vs master).
If pull just fetches but doesn't merge (no local branch):
```
git checkout <branch>
```
or
```
git branch --set-upstream-to=origin/<branch> <branch>
```

---

## Scenario C: Has Code, No Git At All (connect existing folder)

Local: has project files, no .git
Remote: exists on GitHub, has code

```
git init
git remote add origin https://github.com/<user>/<repo>.git
git pull origin <branch> --allow-unrelated-histories
```

If conflicts -> resolve.
Merge may succeed automatically if files don't overlap.

---

## Scenario D: Everything Ready, Daily Pull

Local: has .git, has remote, clean working tree
Remote: exists, remote configured

```
git pull
```

---

## Scenario E: Dirty Working Tree Before Pull

Local: has .git, has remote, but has uncommitted changes

```
git stash push -m "auto-stash before pull"
git pull
git stash pop
```

After stash pop, if conflicts -> resolve.

---

## Notes

- Always detect branch (main vs master) before pulling
- On Windows, watch for line ending issues (CRLF vs LF) - git usually handles this automatically
- If git clone fails with auth errors, user needs GitHub authentication
