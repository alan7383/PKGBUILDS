# PKGBUILDS

Automated AUR packages, kept up to date by GitHub Actions.

Every 6 hours the [update workflow](.github/workflows/update.yml) checks each
package's upstream for a new release. When one is found it:

1. updates the `PKGBUILD` (`pkgver`, `pkgrel`, `sha256sums`),
2. test-builds it with `makepkg` and regenerates `.SRCINFO`,
3. commits the changes back to this repository, and
4. pushes the package files to the AUR.

If a package fails to update in two runs in a row, the
[notify workflow](.github/workflows/notify.yml) opens an issue for it. The issue
closes once that package updates again.

## Ground rule

**This repository never hosts a binary.** Every `PKGBUILD` sources what
*upstream* published — a release tarball, `.deb`, wheel or AppImage — and
either repackages it (`-bin`) or builds it (no suffix), exactly like any
hand-written AUR package would. CI only bumps `pkgver` and the checksums.

This aligns with the [AUR submission guidelines](https://wiki.archlinux.org/title/AUR_submission_guidelines#Rules_of_submission).

## Packages

| Package | Upstream | Upstream deliverable | AUR |
|---|---|---|---|
| [`anker-launcher-bin`](anker-launcher-bin/) | [zororver/anker-launcher](https://github.com/zororver/anker-launcher) | AppImage (`AnkerLauncher-*-x86_64.AppImage`) | [anker-launcher-bin](https://aur.archlinux.org/packages/anker-launcher-bin) |

## Setup & AUR Publishing

To enable pushing packages to the AUR from GitHub Actions:

1. Generate an SSH keypair for the AUR if you don't have one:
   ```bash
   ssh-keygen -t ed25519 -C "aur" -f ~/.ssh/aur_ed25519
   ```
2. Add the public key (`~/.ssh/aur_ed25519.pub`) to your [AUR Account Settings](https://aur.archlinux.org/account).
3. In this repository's GitHub Settings -> **Secrets and variables** -> **Actions**:
   - Add Secret `AUR_SSH_PRIVATE_KEY` with the full private key contents (`~/.ssh/aur_ed25519`).
   - (Optional) Add Variable `AUR_GIT_NAME` (e.g. `alan7383`) and Secret `AUR_GIT_EMAIL`.
4. Set `AUR_PUBLISH=true` in `pkg.sh` (already enabled).
