# kiro-shells — Project Instructions

## Purpose
`kiro-shells` is now a **meta-package**. It ships no `/etc/skel` payload of its own — it
`depends` on the three per-shell config packages so installing it pulls them all:
- [`kiro-bash-config`](https://github.com/kirodubes/kiro-bash-config) — `/etc/skel/.bashrc`
- [`kiro-zsh-config`](https://github.com/kirodubes/kiro-zsh-config) — `/etc/skel/.zshrc`
- [`kiro-fish-config`](https://github.com/kirodubes/kiro-fish-config) — `/etc/skel/.config/fish/`

It keeps `replaces`/`conflicts` on the legacy `edu-shells-git`. The `cachyos-fish-config`
conflict moved to `kiro-fish-config` (the package that actually ships a fish config).

## History
Until 2026.06.26 this package shipped all three shell configs directly under `etc/skel/`.
That payload was split into the three packages above; this repo was reduced to the meta.

## Current State
Meta only. `setup.sh` / `up.sh` are the standard repo-maintenance scripts. The build-clone
PKGBUILD has an empty `package()` and `depends` on the three config packages.

## Standards
- All scripts follow the standard bash template (see global CLAUDE.md)
- Scripts must start with `set -euo pipefail`
- Use the color/logging/error-trap boilerplate

## Next Steps
- (add tasks here)
