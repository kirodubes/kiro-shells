<p align="center">
  <img src="kiro.jpg" alt="Kiro" width="220" />
</p>

# kiro-shells

Meta-package for the Kiro shell configuration. Installing `kiro-shells` pulls in all
three per-shell config packages:

- [`kiro-bash-config`](https://github.com/kirodubes/kiro-bash-config) — bash (`/etc/skel/.bashrc`)
- [`kiro-zsh-config`](https://github.com/kirodubes/kiro-zsh-config) — zsh (`/etc/skel/.zshrc`)
- [`kiro-fish-config`](https://github.com/kirodubes/kiro-fish-config) — fish (`/etc/skel/.config/fish/`)

The shell configs used to live here directly; they were split into the three packages
above on 2026.06.26. This repo now ships no `/etc/skel` payload of its own — it exists only
to provide the `kiro-shells` meta-package and its maintenance scripts.

## Install

```bash
sudo pacman -S kiro-shells
```

> DO NOT JUST RUN THIS. EXAMINE AND JUDGE. RUN AT YOUR OWN RISK.

<!-- KIRO-FUNDING-FOOTER:START — managed by Kiro-HQ/cascade-readme-footer.sh -->
## Help fund Kiro

Everything I build here stays free and open — always. If Kiro or any of these
tools have ever saved you time or taught you something, a small monthly
contribution helps keep the work going. Donations target break-even, nothing
more — the core always stays free for everyone.

- GitHub Sponsors: https://github.com/sponsors/erikdubois
- Patreon: https://www.patreon.com/c/kiroproject
- YouTube memberships: https://www.youtube.com/@ErikDubois/join
- Ko-fi: https://ko-fi.com/erikdubois
- PayPal: https://www.paypal.me/erikdubois
<!-- KIRO-FUNDING-FOOTER:END -->
