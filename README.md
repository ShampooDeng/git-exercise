# Repo summary

The purpose of this repository is to help me to learn how to write
commit message, mainly based on `conventional commit`.

# Useful commands

check status of current working copy.

```sh
jj st
```

move bookmark to certian working copy.

```sh
jj bookmark set <bookmark> -r <rev>
jj bookmark set <bookmark> -r @  # @ represent curent working copy.
```

create a new working copy.
```sh
jj new
```

push/pull/fetch local changes to remote.

```sh
jj git push
jj git pull
jj git fetch
```

edit certian commit.

```sh
jj edit <rev>
```

Stepp back/forward in a Stack and create a new working copy.

```sh
jj prev
jj prev 2 # step 2 revison back.
jj next
jj next 2 # step 2 revisoin forward.
```

Jump to a certain revision and create a new working copy.

```sh
jj new <rev>
jj new <bookmark>
```

diff with parent working copy.

```sh
jj diff
```
 
squash current working copy into parent revision.

```sh
jj squash
```

merage two branches.

```sh
jj new <latest rev of branche1> <latest rev of branche2>
```

rebase one branch to another branch.

```sh
# Connect branch1 onto the end of branch2
jj rebase <start rev of branch1> --onto <latest rev of branch2>
```

resolve conflicts.

```sh
jj status
jj resolve
```

list all bookmarks.

```she
jj bookmark list
jj bookmark list --all-remotes
```

# Jujutsu (`jj`): Merge vs. Rebase

## 1. Scenario

Two branches diverge from commit C:

```text
       E  ← Colleague's feature (pushed)
       │
       D
       │
       C
        \
         F
         │
         H
         │
         I  ← My working branch (@)
```

**Goal:** Incorporate the colleague's changes (D → E) into my working branch (F → H → I).

First, fetch the remote changes:

```bash
jj git fetch
jj bookmark list --all-remotes
```

Assume E is referenced by `feature-alice@origin`.

## 2. Option A: Merge — Preserve Both Histories

```bash
jj new @ feature-alice@origin
```

Creates a new working-copy commit with two parents, I and E.

- Preserves both branches' existing commits.
- Does not rewrite commit history.
- Suitable when commits have already been shared.

## 3. Option B: Rebase — Create Linear History

```bash
jj rebase -s F --onto feature-alice@origin
```

Moves F and its descendants (H, I) onto E:

```text
@ I'
  │
  H'
  │
  F'
  │
  E
  │
  D
  │
  C
```

- Produces a linear history.
- Rewrites local commits F, H, I (new commit IDs).
- Leaves the colleague's commits unchanged.
- Best for unpublished local work.

If currently at I, the shorthand is:

```bash
jj rebase --onto feature-alice@origin
```

## 4. Handling Conflicts

Jujutsu can record conflicts without immediately interrupting a merge or rebase.

```bash
jj status    # Inspect conflicts
jj resolve   # Resolve with configured merge tool
```

Conflicted files can also be edited manually.

## 5. Merge vs. Rebase

| Consideration | Merge (`jj new`) | Rebase (`jj rebase`) |
|---|---|---|
| Preserves existing commit IDs | Yes | No, for rebased commits |
| Creates a merge commit | Yes | No |
| Produces linear history | No | Yes |
| Suitable when your branch is already shared | Yes | Generally avoid |
| Suitable for unpublished local changes | Yes | Often preferred |
| Rewrites colleague's commits | No | No |

**Rule of thumb:**

- **Merge:** Preserve shared history.
- **Rebase:** Integrate upstream changes into unpublished local work while keeping history linear.

If pushing afterward, ensure your bookmark points to the intended commit:

```bash
jj bookmark set my-feature -r @
jj git push --bookmark my-feature
```
