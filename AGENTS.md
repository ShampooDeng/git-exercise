# General rules

This repo is configured with both `git` and `jujutsu`:
- Use `git` only when required, otherwise use `jujutsu`
- Never push to remote local changes to remote repository without explicit user request
- Ask for second approval before pushing to remote

# Review, fix and commit message

When asked to review current changes:
- Never make changes to existing files

When asked to perform a fix based on the review:
1. Use `jj new` to create a new working copy
2. Perform the fix and necessary tests
3. Ask for manual inspection

- Never `jj squash` without explictly user approval

When asked to provide commit message:
- Use `jj diff` to view and summary current changes
- Strictly follow the rules of `Conventional Commits`
- Always include a detailed description (body) with the commit message, not just a subject line.
- Use `jj describe --message "{MESSAGE}"` in default.
- If`git` is requested, directly output message without any tab or space, so it's easier to copy

# Move bookmark

When asked to `update bookmark <bookmark>`:
- Move `<bookmark>` to the newest counted ancestor of `@` (`master` here)
    - i.e. the head-most revision that is non-empty or a merge commit.
    - Empty non-merge commits (working copy, WIP) are skipped.
- Commit before moving: with uncommitted changes, `@` itself is the target.
- Read the step 1 output first, since `--allow-backwards` also allows a regression.
- If `<bookmark>` is left empty, ask instead of guessing.

```shell
# 1. inspect the target revision (read-only)
jj log -r 'latest((::@ ~ empty()) | (::@ & merges()), 1)' -T 'commit_id.short() ++ " " ++ description.first_line() ++ " empty=" ++ empty ++ " bookmarks=" ++ bookmarks ++ "\n"'

# 2. move the tracked bookmark to that revision
jj bookmark set <bookmark> -r 'latest((::@ ~ empty()) | (::@ & merges()), 1)' --allow-backwards
```
