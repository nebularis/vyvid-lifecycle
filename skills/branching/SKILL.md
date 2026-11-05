---
name: branching
description: Working across branches, worktrees and repositories. Use when merging or measuring a branch, resolving a conflict in a register or in a generated index, renumbering a register row, working in a git worktree, running or bypassing a git hook, or doing a task that writes to more than one repository.
---

# Branches, worktrees and repositories

The lifecycle skill says how a unit of work runs and when a branch is due. This skill is about the git mechanics around it: what the hooks do at each moment, how a branch is merged and measured, what a worktree changes, and what a task that spans repositories must decide first.

## The hooks

{{hooks_install}} installs the hooks into the shared hooks directory, which covers the main checkout and every worktree. Install never replaces a hook it did not write. Four moments.

| Moment | Hook | Runs |
|---|---|---|
| a commit | `pre-commit` | the commit checks over the staged files, each check only where the change touches what it governs |
| a merge into `{{trunk}}`, automatic | `pre-merge-commit` | the commit checks over every file the merge changes, plus the merge gates |
| a merge into `{{trunk}}` completed by `git commit`, after conflicts or a squash | `pre-commit`, which detects the merge | the same |
| after any merge | `post-merge` | the rebuild of the derived indexes, so they are never stale. Never fails |

A push is never refused, so a branch always reaches {{maintainer}}, and a cloud agent's push does too. A merge made on the hosting service runs no local hook, so run {{hooks_merge}} first. A check judges only what the change touches, so a failure elsewhere in the tree never blocks a commit, and the full check still finds it. A check whose tool cannot run warns and asks for the install. Never bypass a hook (`--no-verify`, or the variable that skips named checks) without recording which and why in the slice's Validation Pack, and never report a passing hook as a passing {{check_all}}.

## Measuring a branch

Where branches are squash-merged, ancestry says nothing about whether a branch is merged. `git log {{trunk}}..branch` lists commits whose content may be on `{{trunk}}` already, and `git cherry` sees none of them, since a squash changes every patch id. Measure by content. For each file the branch changed since its merge-base, compare the branch's blob with `{{trunk}}`'s, and count how many of the branch's added lines are present in `{{trunk}}` today. A branch at or above the whole is merged. Before analysing a set of branches, ask {{maintainer}} which are live. `{{board}} --measure` computes this share for every branch, and the board's Branches table says which status records each branch moves and which branch marks nothing.

A trial merge, `git merge-tree --write-tree {{trunk}} branch`, reports textual conflicts without touching the tree. It does not see a branch that changes a term another branch renames in a file the rename does not touch, so the full check on the merged tree is still the check.

## Merging

- **Merge `{{trunk}}` into a paused branch, never rebase it.** A branch is pushed, and a cloud agent may hold it. A rebase rewrites what they have.
- **Register rows collide.** Every branch numbers its rows from the highest it can see, so two branches assign one identifier to two rows, and `{{check_identifiers}}` fails the merged tree with every site, the time each line was committed to the second, and the site that keeps the number. First in wins: the row committed earlier keeps the number, and where two share the second {{maintainer}} decides, and the later row and every citation of it are renumbered, which the same check then verifies. No provisional identifiers and no reserved ranges, since a long-lived branch must keep its own index fresh.
- **Rebuild the derived indexes after the merge** (`{{refresh_index}}`), which the `post-merge` hook does for a merge made with `git merge`. A squash completed by `git commit` runs no post-merge hook, so run it by hand.
- **After merging `{{trunk}}` into a branch, bring its boards onto the grammar.** `{{normalise_apply}}`, `{{rename_apply}}` and `{{build_work}}`, each idempotent, so a record the branch still holds in an older shape, and {{index}}'s region, agree with `{{trunk}}` before the next commit, which the `{{check_work}}` check otherwise refuses. A conflict in a generated region is resolved by taking `{{trunk}}`'s and regenerating. A conflict in a status record is a real one, resolved by hand with the join rule, the row furthest along the ladder, then the later day.
- **Versions move once, at the merge into `{{trunk}}`** (lifecycle skill), and tags are {{maintainer}}'s.
- **Record the merge.** The status record of each unit the merge touched says what moved, and a renumbering says which identifiers changed.

## Worktrees

A worktree shares the repository, its remotes, its hooks directory and its ignore rules with the main checkout. It has its own untracked local folder, so a draft left in one worktree is not in another. The project's tasks run against the worktree they are run in, and its bootstrap is needed once per worktree. A commit in a worktree runs the same hooks. Delete a worktree with `git worktree remove`, never by deleting its directory, so the repository forgets it. A session's presence in a worktree is a file under its local folder, written by `{{here}}` before the first edit and listed by the same command, so two sessions sharing one worktree see each other.

## A task that spans repositories

Before writing, decide for each repository the task writes to which guidance governs it: that repository's {{contract}}, and any private guidance it says exists. State the result once, in chat, when the task first writes to the repository. Never copy between a public and a private repository. Link to a tag instead. A change blocked in one repository is explained there by that repository's own rules.
