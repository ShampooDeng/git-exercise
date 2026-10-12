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

create a new working copy (after a given parent).

```sh
jj new
jj new <bookmark>
jj new -r <rev>
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

Stepping Forward in a Stack.

```sh
jj prev
```
