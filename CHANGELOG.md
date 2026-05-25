# Changelog

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
