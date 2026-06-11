# Changelog

## 2026.06.11

### What Changed
- **bash + zsh: SSH-scoped `TERM` fallback for Alacritty.** When you SSH from
  Alacritty (`TERM=alacritty`) to a host that lacks the `alacritty` terminfo
  entry — old NAS boxes, firewalls, appliances — the remote can't interpret the
  cursor-movement sequences, so the prompt wanders and you end up typing blind.
  Added an `ssh()` wrapper that forces `TERM=xterm-256color` for the SSH call
  only when the local terminal is `alacritty`/`alacritty-direct`; the local
  session keeps full Alacritty terminfo and any other `TERM` passes through
  unchanged. Reported by cyberagency on Discussions #45.

### Technical Details
- Identical POSIX `ssh()` function added to `etc/skel/.bashrc-latest` and
  `etc/skel/.zshrc`, placed after the existing `ex()` function. Uses a `case`
  on `$TERM` and `command ssh` to avoid recursion; only `ssh` is wrapped
  (`scp`/`sftp`/`rsync` allocate no remote pty, so they're unaffected).
- **fish needs no change** — its `config.fish` already sets
  `set TERM "xterm-256color"`, which fish exports to child processes (verified:
  `fish -c 'set TERM xterm-256color; env | grep ^TERM'`), so fish SSH sessions
  were never affected.
- Considered and rejected a single `~/.ssh/config` `SetEnv TERM=...` line: TERM
  rides the pty-req, not the env channel, so it's unreliable for this.
- Verified against the real edited files with a stubbed `ssh` on PATH: bash/zsh
  override to `xterm-256color` for `alacritty`/`alacritty-direct`, pass other
  TERMs through, and the bash function loads correctly past the
  `[[ $- != *i* ]] && return` interactive guard.
- Affects newly created users only (skel). Ship sequence: rebuild `kiro-shells`
  → install → `kiro-skell` → test.

### Files Modified
- etc/skel/.bashrc-latest
- etc/skel/.zshrc

## 2026.06.06

### What Changed
- **fish: add `~/.local/bin` and `~/.bin` to PATH.** The skel `config.fish`
  never put the user-local bin dirs on PATH, while `.zshrc` already did
  (its `$HOME/.bin` / `$HOME/.local/bin` guards). This fish/zsh parity gap
  meant tools that install into `~/.local/bin` (e.g. the Antigravity CLI
  self-updater, which drops `agy` there and patches the user's shell config
  itself) were not found on a default fish login. fish users now get the
  same PATH coverage out of the box — installers no longer need to hack
  `config.fish`.

### Technical Details
- Added a `for dir in $HOME/.bin $HOME/.local/bin` loop right after the
  interactive-shell guard in `etc/skel/.config/fish/config.fish`. Each dir is
  prepended only if it exists and is not already on PATH (`test -d` +
  `not contains`), yielding the same `~/.local/bin` → `~/.bin` precedence as
  `.zshrc`. Validated with `fish -n`.
- Affects newly created users only (skel) — existing homes keep their config.
  Ship sequence: rebuild `kiro-shells` → install → `kiro-skell` → test.

### Files Modified
- etc/skel/.config/fish/config.fish
- CHANGELOG.md

## 2026.05.31

### What Changed
- Fixed the stale skel-script path in the shell-config comments. `.zshrc`,
  `.bashrc-latest`, and `.config/fish/config.fish` all said the skel alias was
  "replaced with a script at `/usr/local/bin/skel`" — a path that no longer
  exists. They now point at `/usr/local/bin/kiro-skell`, the actual Kiro
  system tool. Part of the ecosystem-wide `skel` → `skell` / `kiro-skell`
  double-L naming consistency pass (convention recorded in
  `Kiro-HQ/ASSISTANT.md`). `/etc/skel` (the Linux path) is unchanged.
- Reviewed and **deliberately left single-L**: the `bupskel` alias and its
  `~/.skel-backup-*` folder. It backs up the `/etc/skel` *directory* (a path
  operation), not the restore command, so double-L would be incorrect here.

### Files Modified
- `etc/skel/.zshrc`
- `etc/skel/.bashrc-latest`
- `etc/skel/.config/fish/config.fish`

## 2026.05.29

### What Changed
- Fixed every shell-config alias that still pointed at the old `edu-*` helper
  script names. Those scripts were renamed to `kiro-*` (and now live in
  `edu-system-files/usr/local/bin/`, installed to `/usr/local/bin/`), so all
  these aliases had been silently broken. Applied identically across
  `.bashrc-latest`, `.zshrc`, and `config.fish`.
- Removed the dead `rvariety` / `rkmix` / `rconky` aliases (in `.bashrc-latest`
  and `.zshrc`) — they referenced `edu-remove-*` scripts that were dropped, not
  renamed, and have no `kiro-*` replacement.

### Technical Details
- Renames applied (old → new):
  - `edu-which-vga` → `kiro-which-vga`
  - `edu-fix-pacman-databases-and-keys` → `kiro-fix-pacman-keys` (7 alias variants)
  - `edu-fix-pacman-conf` → `kiro-fix-pacman-conf`
  - `edu-fix-pacman-gpg-conf` → `kiro-fix-gpg-conf`
  - `edu-fix-archlinux-servers` → `kiro-fix-mirrors`
  - `edu-probe` → `kiro-probe`
- Mappings verified against the script purpose headers in `edu-system-files`,
  not just name similarity. `kiro-fix-mirrors` (rate-mirrors) chosen over
  `kiro-get-mirrors` (reflector) to match the old `fix-archlinux-mirrors` intent.

### Files Modified
- `etc/skel/.bashrc-latest`
- `etc/skel/.zshrc`
- `etc/skel/.config/fish/config.fish`

## 2026.05.25

### What Changed
- Removed the `ngrub` / `nconfgrub` grub aliases from the skel `config.fish`. Kiro
  boots with systemd-boot, so grub helpers are dead weight. Mirrors the same
  removal in ATT's shipped `config.fish`.
- Removed grub aliases (`update-grub`, `grub-update`, `ngrub`, `nconfgrub`) from
  the skel `.bashrc-latest` and `.zshrc` too — the fish removal had left bash/zsh
  inconsistent. `.bashrc-latest` is what the ISO build ships as `.bashrc`. Both
  still parse clean (`bash -n` / `zsh -n`).

### Files Modified
- `etc/skel/.config/fish/config.fish`
- `etc/skel/.bashrc-latest`
- `etc/skel/.zshrc`

### Verified
- Rebuilt and confirmed live: `edu-shells-git` 26.05-10 (built 09:14). Installed
  `/etc/skel/.config/fish/config.fish` has no grub references.

## 2026.05.18

### What Changed
- Initial project setup; existing scripts: `setup.sh`, `up.sh`

### Technical Details
- Standard bash template with `set -euo pipefail`, color logging, error trap

### Files Modified
- `setup.sh`, `up.sh`
