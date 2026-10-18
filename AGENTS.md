# General rules

This repo is configuared with both `git` and `jujustu`:

- Use `git` only when required, otherwise use `jujustu`
- **DO NOT** push to remote local changes to remote repository, if not requested
- If requested to push to remote, ask for second approval before executing

# Review commit and provide commit message

Following the rules below when asked to review current commit and provide commit message:

- Restrictly follow the rules of `Conventional Commits`
- Always include a detailed description (body) with the commit message, not just a subject line.
- When choose `jujustu`, use `jj describe --message "{MESSAGE}"`
- When choose `git`, directly output message without any tab or space, so it's easier to copy

# Sync bookmarks

- When asked to `sync bookmark`, always move the currently tracked bookmark to the latest non-empty revision.


