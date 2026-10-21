# Personal Git Aliases

Git shortcuts for everyday work, staging files by partial name, and checking
multiple repositories. Definitions live in [aliases.gitconfig](aliases.gitconfig).
This file contains aliases only: no credentials, user identity, or remote settings.

## Install

```sh
git clone https://github.com/mlc4050/git-aliases.git D:/git/git-aliases
git config --global --add include.path D:/git/git-aliases/aliases.gitconfig
```

Keep the checkout in a permanent location. On another machine, replace the paths
with the absolute location of your checkout. Add the include once; your other Git
settings remain intact. Later definitions of the same alias take precedence.

Shell aliases require Git Bash/Unix utilities. `fadd` and `pending` require a
Bash-compatible shell; `k` and `g` require Gitk and Git GUI respectively.

## Alias reference

Use every shortcut after `git`, as shown below.

| Alias | Example | What it does |
| --- | --- | --- |
| **st** | `git st` | Show working-tree and staging status. |
| **sts** | `git sts` | Show compact status. |
| **co** | `git co main` | Check out a branch or path using Git checkout. |
| **sw** | `git sw main` | Another shortcut for checkout; this uses `checkout`, not Git's newer `switch` command. |
| **br** | `git br` | List branches. Accepts branch options, such as `git br -a`. |
| **nb** | `git nb feature-name` | Create a branch and check it out. |
| **fadd** | `git fadd ccd` | Stage changed, deleted, or untracked files whose filename contains the literal text `ccd`. Prints each staged path. |
| **unstage** | `git unstage path/to/file` | Remove changes from staging while keeping file edits. With no path, unstage everything. |
| **amend** | `git amend` | Edit the latest commit, including staged changes. `git amend --no-edit` keeps its message. Rewrites that commit. |
| **mrg** | `git mrg feature-name` | Merge with `--no-ff --no-commit --edit`, leaving the merge ready to review and commit. |
| **ff** | `git ff feature-name` | Merge with `--ff --no-commit`. Allows fast-forwarding but does not require it. A fast-forward completes immediately; a merge commit is paused. |
| **sup** | `git sup` | Initialize and update submodules recursively to the commits recorded by their parent repositories. |
| **gl** | `git gl` | Show a compact, decorated commit graph for the current history. |
| **gla** | `git gla` | Show the same graph across all refs. |
| **glw** | `git glw` | Show all-ref history with commit hash, committer, relative date, decorations, and subject. |
| **k** | `git k` | Launch Gitk for all refs in date order. |
| **g** | `git g` | Launch Git GUI. |
| **deepfetch** | `git deepfetch` | Recursively find `.git` directories beneath the alias execution directory and fetch each repository. Does not merge updates. |
| **pending** | `git pending` | Report repositories behind their configured upstream, including ahead counts when diverged. Uses existing remote-tracking refs; fetch first for fresh results. |
| **shleemypants** | `git shleemypants 3` | Start an interactive rebase of the last three commits. Rewrites history. |
| **revert** | `git revert path/to/file` | Defined as checkout-from-index, which would discard unstaged file edits; however, Git's built-in `revert` takes precedence over this alias. See notes below. |
| **fuckit** | `git fuckit` | Run `git reset --hard`, discarding staged and unstaged changes to tracked files. Does not generally clean untracked files. |
| **dump** | `git dump` | Currently defines a stash/drop shell function but never calls it, so this exported definition performs no stash or deletion. |
| **publish** | `git publish` | Intended to push the current branch to origin and set its upstream. The exported definition contains non-breaking spaces and needs correction before use. |

## Stage by part of a filename

```text
git fadd ccd
Staged: releases/ccd_output_routine.vhd
```

Matching is case-sensitive and uses the filename, not its parent directories.
Quote text containing spaces: `git fadd "output routine"`. All matching files
are staged in full; this does not choose individual hunks. Ignored untracked files
are excluded. Existing staged changes remain staged. No matches means no change.
Shell aliases execute from the repository root, so this checks the whole repository.

To undo staging without losing edits:

```sh
git unstage releases/ccd_output_routine.vhd
# Or unstage everything:
git unstage
```

## Export notes

The original aliases are preserved, including two `mrg` definitions and two
identical `revert` definitions. For `mrg`, the last value is effective. Git aliases
do not replace built-in commands, so `git revert` still runs Git's real revert
command, not the configured `checkout --` value.

`dump` and `publish` have the issues documented above and have not been silently
repaired. For publishing a branch directly, use:

```sh
git push -u origin HEAD
```

`deepfetch` and `pending` discover `.git` directories rather than worktree `.git`
files. Git shell aliases start at the repository root, not necessarily the folder
you invoked them from. They are not general parent-folder scanners outside a Git
repository. `pending` lists behind/diverged repositories, not ahead-only ones.

`exec`, `ci`, and `com` were intentionally removed from the export and this machine.

## Inspect your installed aliases

```sh
git config --show-origin --get-regexp '^alias\.'
git config --get alias.fadd
```

Inspect destructive commands before using them. In particular, `fuckit` discards
tracked-file changes, while `amend` and `shleemypants` rewrite commit history.
