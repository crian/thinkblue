# thinkblue

[![build-thinkblue](https://github.com/crian/thinkblue/actions/workflows/build.yml/badge.svg)](https://github.com/crian/thinkblue/actions/workflows/build.yml)
[![License](https://img.shields.io/badge/license-Apache--2.0-blue.svg)](LICENSE)

A slim, everyday-driver [BlueBuild](https://blue-build.org/) Silverblue image for my ThinkPad.

It is based on [`ghcr.io/blue-build/base-images/fedora-silverblue`](https://github.com/blue-build/base-images) and only changes what I actually care about: a few swapped apps, a different terminal, and Flatpaks for the rest. Everything else stays close to upstream.

## What's inside

- **Base:** `ghcr.io/blue-build/base-images/fedora-silverblue:latest`
- **Terminal:** [Ghostty](https://ghostty.org/), installed from the [Terra](https://terra.fyrox.dev/) repository
- **Removed:** Firefox, Ptyxis, GNOME Software, htop, nvtop, gnome-system-monitor, gnome-classic-session, GNOME Shell classic extensions
- **Added (DNF):** bat, btop, eza, fastfetch, fd-find, fish, ghostty, gnome-tweaks, lm_sensors, micro, nautilus-python, nethogs, ripgrep, starship, xdg-terminal-exec, zoxide
- **Flatpaks (system):** Google Chrome, Bazaar (app store), Flatseal, ExtensionManager, MissionCenter, GNOME core apps (Calculator, Calendar, Loupe, Papers, TextEditor, Weather), adw-gtk3 themes

The full definition lives in [`recipes/recipe.yml`](recipes/recipe.yml); additional files are in [`files/`](files/).

## Installation

> [!WARNING]
> Image-based Fedora is still [an experimental feature](https://www.fedoraproject.org/wiki/Changes/OstreeNativeContainerStable). Use at your own risk.

To rebase an existing Fedora Atomic installation to the latest build:

1. Switch to the unsigned image first to enroll the signing key via MOK:

   ```bash
   sudo bootc switch ghcr.io/crian/thinkblue:latest
   systemctl reboot
   ```

2. After reboot, enroll the MOK key when prompted (password you set during build), then switch to the signed image:

   ```bash
   sudo bootc switch --enforce-container-sigpolicy ghcr.io/crian/thinkblue:latest
   systemctl reboot
   ```

The `latest` tag always points to the most recent build.

## Updating

Updates are handled by [bootc](https://github.com/containers/bootc):

```bash
sudo bootc upgrade
systemctl reboot
```

Dependabot keeps the GitHub Actions used by the build workflow up to date.

## Verification

Images are signed with [Sigstore](https://www.sigstore.dev/)'s [cosign](https://github.com/sigstore/cosign). To verify a build, download [`cosign.pub`](cosign.pub) from this repository and run:

```bash
cosign verify --key cosign.pub ghcr.io/crian/thinkblue:latest
```

## Building your own

Want something similar? Fork this repo or start from the [BlueBuild template](https://github.com/blue-build/template) and follow the [setup guide](https://blue-build.org/how-to/setup/).

Offline ISOs can be generated on Fedora Atomic following the [BlueBuild ISO guide](https://blue-build.org/how-to/generate-iso/). They are too large to host on GitHub for free.

## License

[Apache-2.0](LICENSE)
