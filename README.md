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
[aliases.gitconfig](aliases.gitconfig). They are exported unchanged, including
duplicate definitions, and have not been executed or repaired. Some commands
change history or discard work: inspect their definitions before using them.

## Aliases

`amend`, `br`, `ci`, `co`, `com`, `deepfetch`, `dump`, `exec`, `ff`, `fuckit`, `g`, `gl`, `gla`, `glw`, `k`, `mrg`, `nb`, `pending`, `publish`, `revert`, `shleemypants`, `st`, `sts`, `sup`, `sw`, `unstage`.

`fadd` was not configured when this export was created.
