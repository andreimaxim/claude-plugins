---
name: organizing-commits
description: Reorganizes a branch's committed and uncommitted changes into logical, reviewable commits that each leave the code working, without changing the final code. Use when asked to restack, clean up, split, squash, or reorganize a branch's commits before review.
---

# Organizing commits

Reorganize all changes on the current branch into logical, easy-to-review commits. The changes are the commits since the merge base with the default branch, or with a base branch the user names, plus staged, unstaged, and untracked changes. Ask before committing untracked files that look unrelated to the work, such as local configuration or build output.

Inspect the complete diff before changing anything. Then reorder, split, squash, and reword commits so each commit makes one change a reviewer can follow. Follow the repository's commit message conventions, and keep each commit's author and trailers such as Co-Authored-By.

Each commit must leave the code working, so it builds and passes the repository's relevant checks on its own. When a split that is easier to review would leave a commit broken, combine or reorder the changes instead. Run the checks on every new commit, and report any commit you could not check.

Before rewriting, save the starting state, including uncommitted changes, under a backup ref. When you finish, confirm that the final tree matches the saved state exactly. If it doesn't, fix the commits or restore the backup.

Leave pushing to the user. Report the new commits, the backup ref, the check results for each commit, and the result of the tree check. If the branch was already pushed, say that publishing it needs a force push.
