# Personal Git Aliases

An alias-only export of the Git shortcuts configured on this machine.
User identity, credentials, remotes, and other Git settings are not included.

## Install

Clone or copy this repository to a permanent location, then include the alias file
in your global configuration. For this checkout on Windows:

```sh
git config --global --add include.path D:/git/git-aliases/aliases.gitconfig
```

On another machine, replace that path with the absolute path to its checkout.
Run the include command once. This loads the aliases without replacing your
existing global settings. Later definitions of the same alias take precedence.
Aliases beginning with `!` use a shell; several require Git Bash/Unix utilities
or optional tools such as `gitk` and Git GUI.

## Inspect

```sh
git config --show-origin --get-regexp '^alias\.'
git config --get alias.st
```

Use an alias as `git st`, `git co`, etc. The alias definitions are in
[aliases.gitconfig](aliases.gitconfig). Existing aliases are exported unchanged, including duplicate definitions.
The added `fadd` alias is tested separately. Some commands
change history or discard work: inspect their definitions before using them.

## Aliases

`amend`, `br`, `co`, `deepfetch`, `dump`, `fadd`, `ff`, `fuckit`, `g`, `gl`, `gla`, `glw`, `k`, `mrg`, `nb`, `pending`, `publish`, `revert`, `shleemypants`, `st`, `sts`, `sup`, `sw`, `unstage`.

## Stage files by part of their name

```sh
git fadd ccd
```

Stages changed, deleted, or untracked non-ignored files whose **filename** contains
`ccd`, such as `releases/ccd_output_routine.vhd`. Prints each staged path.
Matching is case-sensitive and treats the search text literally. All matching
files are staged, with their entire changes; this does not select individual hunks.
Existing staged changes are preserved. With no matches, nothing is staged.

Requires Git Bash/Unix shell utilities (`sort` and a shell with `read -d`).
