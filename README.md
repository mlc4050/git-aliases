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
| **br** | `git br` | List branches. Accepts branch options, such as `git br -a`. |
| **nb** | `git nb feature-name` | Create a branch and check it out. |
| **fadd** | `git fadd <text>` | Stage changed, deleted, or untracked files whose filename contains any substring you supply. Prints each staged path. |
| **unstage** | `git unstage path/to/file` | Remove changes from staging while keeping file edits. With no path, unstage everything. |
| **amend** | `git amend` | Add staged changes to the latest commit while keeping its message. Use `git amend --edit` to open the message editor. Rewrites that commit. |
| **mrg** | `git mrg feature-name` | Merge with `--no-ff --no-commit --edit`, leaving the merge ready to review and commit. |
| **sup** | `git sup` | Initialize and update submodules recursively to the commits recorded by their parent repositories. |
| **gl** | `git gl` | Show a compact, decorated commit graph for the current history. |
| **gla** | `git gla` | Show the same graph across all refs. |
| **glw** | `git glw` | Show all-ref history with commit hash, committer, relative date, decorations, and subject. |
| **k** | `git k` | Launch Gitk for all refs in date order. |
| **g** | `git g` | Launch Git GUI. |
| **deepfetch** | `git deepfetch` | Recursively find `.git` directories beneath the alias execution directory and fetch each repository. Does not merge updates. |
| **pending** | `git pending` | Report repositories behind their upstream and flag detached checkouts or branches without an upstream. Skips linked worktrees unless passed `--worktrees`. Includes ahead counts when diverged; fetch first for fresh results. |
| **shleemypants** | `git shleemypants <count>` | Start an interactive rebase of the last `<count>` commits. Supply a positive number, for example `git shleemypants 5` for five commits. Rewrites history. |
| **fuckit** | `git fuckit` | Run `git reset --hard`, discarding staged and unstaged changes to tracked files. Does not generally clean untracked files. |

## Stage by part of a filename

```sh
git fadd <text>
```

Replace `<text>` with any part of a filename. There is nothing special about
`ccd`; it is just one example:

```text
git fadd ccd
Staged: releases/ccd_output_routine.vhd
```

Other examples are `git fadd routine`, `git fadd .vhd`, or `git fadd README`.
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

The `mrg` alias has one definition: `merge --no-ff --no-commit --edit`.

To push the current branch and set its upstream, use:

```sh
git push -u origin HEAD
```

`exec`, `ci`, `com`, `dump`, `publish`, `revert`, `ff`, and `sw` were intentionally removed from the export and this machine.

## Check for pending updates

Run these from your parent repository folder, such as `D:/git`:

```sh
git deepfetch             # Refresh remote-tracking refs
git pending               # Check repositories, skipping linked worktrees
git pending --worktrees   # Include separate build/development worktrees
```

Example output:

```text
./Arcade-Seta_MiSTer       3 behind
./example                 0 behind [DETACHED; vs origin/main]
./another                 2 behind / 1 ahead (DIVERGED)
```

`DETACHED` means the checkout is at a commit rather than on a named branch.
`NO UPSTREAM` means its branch has no configured tracking branch. The reported
comparison is informational; these commands do not attach branches or merge updates.

`deepfetch` discovers `.git` directories. `pending` also discovers worktree and
submodule `.git` files. Linked worktrees are skipped by default so separate build
copies do not clutter the report. Use `git pending --worktrees` to include them.
Submodules are still checked. From a parent folder outside a repository, both scan
beneath that folder; inside a repository, Git shell aliases start at its root.

`pending` uses each checked-out branch's configured upstream. A detached checkout
or branch without an upstream is always reported when included in the scan, even
if up to date. For those, it compares against `origin/HEAD`, or `origin/main` /
`origin/master` if exactly one exists. The output labels that fallback explicitly; it does not configure
an upstream or change any checkout. If no unambiguous comparison exists, it
reports that instead of silently skipping the repository. A submodule or build
worktree may be intentionally detached.

Normal branches that are only ahead are omitted. Results use the remote-tracking
refs already on disk; run `git deepfetch` first for fresh results. Fetching alone
does not update your checked-out files.

To stay on a branch when updating, use `git switch main` (or `master`) followed
by `git pull --ff-only`. `git checkout origin/main` detaches HEAD.

## Inspect your installed aliases

```sh
git config --show-origin --get-regexp '^alias\.'
git config --get alias.fadd
```

Inspect destructive commands before using them. In particular, `fuckit` discards
tracked-file changes, while `amend` and `shleemypants` rewrite commit history.
