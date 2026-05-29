# Changelog

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
