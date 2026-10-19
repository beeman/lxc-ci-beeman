# Debian Trixie images

Builds an ARM64 Debian Trixie cloud container image with distrobuilder in GitHub
Actions. The Debian cloud recipe includes `ssh-import-id` so cloud-init can import
GitHub SSH keys during first boot with its default module order.

## Build

The `Build Debian Trixie` workflow runs when its recipe or workflow changes and can
also be started from the Actions tab. It uses a native ARM64 Ubuntu runner and
calls `distrobuilder build-incus` directly.

Each successful run uploads the `debian-trixie-arm64-cloud` artifact containing:

- `debian-trixie-arm64-cloud.tar.gz`
- `SHA256SUMS`

## Download and import

Find the successful build's run ID, download its artifact, and verify the checksum:

```bash
gh run list --repo beeman/lxc-ci-beeman --workflow build-debian.yml
gh run download RUN_ID --repo beeman/lxc-ci-beeman --name debian-trixie-arm64-cloud --dir build
(cd build && shasum -a 256 -c SHA256SUMS)
```

Import the image using the Incus CLI with your configured destination remote:

```bash
incus image import build/debian-trixie-arm64-cloud.tar.gz bee-m4-incus: --alias debian-trixie-ssh
```

The output is an Incus container image. VM images and other architectures are not
built by this workflow.

## Source

The Debian recipe comes from [lxc/lxc-ci](https://github.com/lxc/lxc-ci). This
repository retains its authorship and Apache 2.0 license in `AUTHORS` and `COPYING`.
Build tool documentation: [distrobuilder](https://linuxcontainers.org/distrobuilder/docs/latest/).
