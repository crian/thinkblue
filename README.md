# thinkblue

[![build-thinkblue](https://github.com/crian/thinkblue/actions/workflows/build.yml/badge.svg)](https://github.com/crian/thinkblue/actions/workflows/build.yml)
[![License](https://img.shields.io/badge/license-Apache--2.0-blue.svg)](LICENSE)

A slim, everyday-driver [Universal Blue](https://universal-blue.org/) Silverblue image for my ThinkPad, built with [BlueBuild](https://blue-build.org/).

It is based on [`ublue-os/silverblue-main`](https://github.com/ublue-os/main) and only changes what I actually care about: a few swapped apps, a different terminal, and Flatpaks for the rest. Everything else stays close to upstream.

## What's inside

- **Base:** `ghcr.io/ublue-os/silverblue-main:latest`
- **Terminal:** [Ghostty](https://ghostty.org/), installed from the [Terra](https://terra.fyrox.dev/) repository
- **Removed:** Firefox, Ptyxis and GNOME Software
- **Flatpaks:** Google Chrome and [Bazaar](https://github.com/kolunmi/bazaar) (as the app store replacement)

The full definition lives in [`recipes/recipe.yml`](recipes/recipe.yml); additional files and modules are in [`files/`](files/) and [`modules/`](modules/).

## Installation

> [!WARNING]
> Image-based Fedora is still [an experimental feature](https://www.fedoraproject.org/wiki/Changes/OstreeNativeContainerStable). Use at your own risk.

To rebase an existing Fedora Atomic installation to the latest build:

1. Rebase to the unsigned image first, so the proper signing keys and policies get installed:

   ```bash
   rpm-ostree rebase ostree-unverified-registry:ghcr.io/crian/thinkblue:latest
   ```

2. Reboot:

   ```bash
   systemctl reboot
   ```

3. Rebase to the signed image:

   ```bash
   rpm-ostree rebase ostree-image-signed:docker://ghcr.io/crian/thinkblue:latest
   ```

4. Reboot again to finish the installation:

   ```bash
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
cosign verify --key cosign.pub ghcr.io/crian/thinkblue
```

## Building your own

Want something similar? Fork this repo or start from the [BlueBuild template](https://github.com/blue-build/template) and follow the [setup guide](https://blue-build.org/how-to/setup/).

Offline ISOs can be generated on Fedora Atomic following the [BlueBuild ISO guide](https://blue-build.org/how-to/generate-iso/). They are too large to host on GitHub for free.

## License

[Apache-2.0](LICENSE)
