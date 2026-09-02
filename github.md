# Git Branch & Cherry-Pick Workflow

This document describes the Git workflow used for maintaining branches, cherry-picking selected commits, and cleaning up local/remote branches.

## Repository

```text
https://github.com/l1068/media
```

## Check Current Branch

```bash
git status
git branch -a (To list both local and remote tracking branches)
```

Example:

```text
* CI
  main
  main1
  remotes/origin/CI
  remotes/origin/HEAD -> origin/main
  remotes/origin/build
  remotes/origin/main
  remotes/origin/main1
  remotes/origin/test
```

If you want to remove both local and remote main1, run:
```
git branch -D main1
git push origin --delete main1
git fetch --prune
```

---

## Fetch Latest Changes

Always fetch the latest remote information before starting:

```bash
git fetch origin --tags
git fetch --prune
```

`--prune` removes stale remote-tracking branches that no longer exist on GitHub.

---

## Create a Branch

Create a new branch from a specific tag:

```bash
git switch -c cherry-pick-selected r1.11.0-0.0.2
```

Check the branch:

```bash
git status
git branch
```

---

## Cherry-Pick Selected Commits

Cherry-pick multiple commits:

```bash
git cherry-pick 17d0953 a7a405a 52c2877 c010d43 202a609 505455c
```

Check the resulting commits:

```bash
git log --oneline --reverse r1.11.0-0.0.2..HEAD
```

---

## Push the New Branch

```bash
git push -u origin cherry-pick-selected
```

After this, the branch will be available on GitHub.

---

## Delete a Local Branch

First make sure you are **not currently on the branch you want to delete**.

For example, if you are currently on `CI`:

```bash
git branch -d main1
```

If Git reports that the branch has not been merged and you intentionally want to remove it:

```bash
git branch -D main1
```

---

## Delete a Remote Branch

To delete a branch from GitHub:

```bash
git push origin --delete main1
```

If Git reports:

```text
error: unable to delete 'main1': remote ref does not exist
```

the remote branch has already been deleted.

Your local repository may still show:

```text
remotes/origin/main1
```

Clean the stale reference with:

```bash
git fetch --prune
```

Then verify:

```bash
git branch -a
```

---

## Remove Stale Remote Branches

Use:

```bash
git fetch --prune
```

This updates your local remote-tracking references and removes branches that no longer exist on the remote.

You can also use:

```bash
git remote prune origin
```

---

## Check Commit Order

To display commits in chronological order:

```bash
git log --oneline --reverse
```

For commits after a specific tag:

```bash
git log --oneline --reverse r1.11.0-0.0.2..HEAD
```

---

## Cherry-Pick a Single Commit

```bash
git cherry-pick <commit>
```

Example:

```bash
git cherry-pick 17d0953
```

---

## Abort a Cherry-Pick

If a conflict occurs and you want to cancel the operation:

```bash
git cherry-pick --abort
```

---

## Continue After Resolving Conflicts

After resolving the conflicted files:

```bash
git add .
git cherry-pick --continue
```

---

## View Cherry-Pick Status

```bash
git status
```

You can also inspect the current commit history:

```bash
git log --oneline --decorate -20
```

---

## Useful Commands

### List local branches

```bash
git branch
```

### List local and remote branches

```bash
git branch -a
```

### List remote branches

```bash
git branch -r
```

### Show remotes

```bash
git remote -v
```

### Fetch everything

```bash
git fetch --all --tags
```

### Clean stale remote branches

```bash
git fetch --prune
```

### Show recent commits

```bash
git log --oneline -20
```

### Show all tags

```bash
git tag
```

### Show the current branch

```bash
git branch --show-current
```

---

## Recommended Cleanup

If you have deleted a branch from GitHub and it still appears under `remotes/origin/`, run:

```bash
git fetch --prune
```

For example:

```bash
git branch -a

git fetch --prune

git branch -a
```

The stale branch:

```text
remotes/origin/main1
```

will then disappear if `main1` no longer exists on the remote.

---

## Important

Do **not** run:

```bash
git push origin --delete main1
```

if the remote branch has already been deleted.

If you receive:

```text
remote ref does not exist
```

simply run:

```bash
git fetch --prune
```

to synchronize your local remote-tracking branches.
