# Panama APT Repository

Public signed Debian packages for the standalone Panama Rust service.
Panama source remains in a separate private repository. This repository contains
only built packages, signed APT metadata, the public signing key, and publishing
configuration. No application credentials or private signing keys are included.

Feed: https://tcox2.github.io/panama-apt

Signing key fingerprint:

`D8B8 34DD A6D6 939D CC2A 579A 4A54 12C6 8D3B FC6D`

Supported architecture: amd64. Packages are built on Ubuntu 24.04; APT enforces
the actual library dependencies determined during packaging.

## Installation

Download `panama.gpg` from the feed, verify the fingerprint above, and install
it as `/etc/apt/keyrings/panama.gpg`. Add `/etc/apt/sources.list.d/panama.sources`:

```text
Types: deb
URIs: https://tcox2.github.io/panama-apt
Suites: stable
Components: main
Architectures: amd64
Signed-By: /etc/apt/keyrings/panama.gpg
```

```sh
sudo apt-get update
sudo apt-get install panama
```

The package installs `/usr/bin/panama` and `panama.service`. Existing files in
`/etc/panama` and the database are preserved. Fresh machines require their own
configuration and credentials before the service starts. Logs are available
with `journalctl -u panama.service -f`.

Updates are built and signed by Panama's private GitHub Actions workflow, then
pushed here using a repository-specific deploy key. This repository's Pages
workflow publishes the feed. Each package records its source commit under
`/usr/share/doc/panama/build-info`. APT clients verify signed repository metadata
and package checksums. Do not use `trusted=yes` or disable signature verification.
