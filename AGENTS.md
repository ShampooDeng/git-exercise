# General rules

This repo is configuared with both `git` and `jujutsu`:
- Use `git` only when required, otherwise use `jujutsu`
- Never push to remote local changes to remote repository without explicit user request
- Ask for second approval before pushing to remote

# Review commit and provide commit message

When asked to review current commit and provide commit message:
- Restrictly follow the rules of `Conventional Commits`
- Always include a detailed description (body) with the commit message, not just a subject line.
- Use `jj describe --message "{MESSAGE}"` in default.
- If`git` is requested, directly output message without any tab or space, so it's easier to copy

# Move bookmarks

When asked to `update bookmark <bookmark>`:
- Always move `<bookmark>` to the latest non-empty revision of the current branch
    - i.e. the newest non-empty ancestor of `@` (`master` here).
    - Only empty *non-merge* commits (working copy, WIP, placeholder) are skipped
    - Empty merge commits still count, i.e. `Merge pull request #2`, whose tree matches one side).
- If `<bookmark>` is left empty, ask the user for it instead of guessing.

```shell
# 1. inspect the target revision (read-only)
jj log -r 'latest((::@ ~ empty()) | (::@ & merges()), 1)' -T 'commit_id.short() ++ " " ++ description.first_line() ++ " empty=" ++ empty ++ " bookmarks=" ++ bookmarks ++ "\n"'

# 2. move the tracked bookmark to that revision
jj bookmark set <bookmark> -r 'latest((::@ ~ empty()) | (::@ & merges()), 1)' --allow-backwards
```

- `::@ ~ empty()` keeps non-empty ancestors; `::@ & merges()` adds empty merge commits back in; `latest(..., 1)` picks exactly one revision.
- Do not use `latest(::@ & ~empty(), 1)`: it walks past an empty merge head and lands on one of its parents.
- Do not use `heads(~empty())` or `heads(::@ & ~empty())` as the move target: the former is repository-wide, the latter can resolve to several revisions (`Error: Revset ... resolved to more than one revision`).

