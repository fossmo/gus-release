# gus releases

Signed release binaries for **gus**, a small terminal coding agent. This
repository only holds releases; the source code lives elsewhere.

## Install

Download the archive for your platform from the
[latest release](https://github.com/fossmo/gus-release/releases/latest):

| Platform | File |
|----------|------|
| Linux x86-64 | `gus_linux_amd64.tar.gz` |
| Linux ARM64 | `gus_linux_arm64.tar.gz` |
| macOS Intel | `gus_darwin_amd64.tar.gz` |
| macOS Apple Silicon | `gus_darwin_arm64.tar.gz` |

```sh
tar -xzf gus_linux_amd64.tar.gz
install gus ~/.local/bin/   # or anywhere on your PATH
```

## Verify

Checksums are signed with [minisign](https://jedisct1.github.io/minisign/):

```sh
base=https://github.com/fossmo/gus-release/releases/latest/download
curl -LO $base/SHA256SUMS -LO $base/SHA256SUMS.minisig -LO $base/minisign.pub
minisign -Vm SHA256SUMS -p minisign.pub
sha256sum -c --ignore-missing SHA256SUMS   # macOS: shasum -a 256 -c --ignore-missing SHA256SUMS
```

The trusted comment in the signature is `gus <tag>`, e.g. `gus v1.2.3`.

## Updates

gus checks this repository at startup and asks before installing a newer
version. Updates are only accepted when they are signed with the key built into
gus. Set `GUS_NO_UPDATE_CHECK=1` to turn the check off, or run `/update`
manually.
