# gus releases

Signed release binaries for **gus**, a small terminal coding agent. This
repository only holds releases; the source code lives elsewhere.

## Install

```sh
curl -fsSL https://github.com/fossmo/gus-release/releases/latest/download/install.sh | sh
```

The script picks the right archive for your machine, checks its SHA-256
checksum (and the minisign signature, if `minisign` is installed), and installs
`gus` to `~/.local/bin`. Set `GUS_VERSION=v1.2.3` for a specific version or
`GUS_INSTALL_DIR=<dir>` for another location.

**macOS:** use the script (or `curl`) rather than downloading with a browser.
gus is not notarized by Apple, so Gatekeeper blocks browser downloads ("Apple
could not verify …"). If you already did, run
`xattr -d com.apple.quarantine /path/to/gus`.

Manual install: download `gus_<os>_<arch>.tar.gz` from the
[latest release](https://github.com/fossmo/gus-release/releases/latest)
(`darwin_arm64` for Apple Silicon, `darwin_amd64` for Intel Macs,
`linux_amd64` / `linux_arm64` for Linux) and unpack it with `tar -xzf`.

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
