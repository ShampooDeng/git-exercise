# General rules

This repo is configuared with both `git` and `jujustu`:

- Use `git` only when required, otherwise use `jujustu`
- Never push to remote local changes to remote repository without explicit user request
- Ask for second approval before pushing to remote

# Review commit and provide commit message

Following the rules below when asked to review current commit and provide commit message:

- Restrictly follow the rules of `Conventional Commits`
- Always include a detailed description (body) with the commit message, not just a subject line.
- When choose `jujustu`, use `jj describe --message "{MESSAGE}"`
- When choose `git`, directly output message without any tab or space, so it's easier to copy

# Move bookmarks

When asked to `update bookmark`:

- always move the currently tracked bookmark to the latest non-empty revision of the current branch
- 

```shell
# lists visible heads among non-empty revisions
jj log -r 'heads(~empty())' -T 'commit_id.short() ++ " " ++ description ++ " bookmarks=" ++ bookmarks ++ "\n"'
```

- To inspect non-empty revision heads, use `jj log -r 'heads(~empty())' -T 'commit_id.short() ++ " " ++ description ++ " bookmarks=" ++ bookmarks ++ "\n"'`. This lists visible heads among non-empty revisions; it does not move a bookmark, and it may return multiple heads when history has diverged. Choose the latest non-empty revision relevant to the tracked bookmark rather than assuming there is one global head.


