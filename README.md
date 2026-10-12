# mdci.setup.feature-myx.common

Installs `myx.common` on the host, once, and then the packages listed in `OS_PACKAGES`.

Maintainer notes: `MAGIC.md`.

## Provides

`standard/setup.feature-myx.common`, and nothing else. The bare name `os-myx.common` is the maintainer source project in the `myx.common` repo, so this package does not answer to it. Other packages require it as `standard/setup.feature-myx.common`: both OS floors, `setup.feature-mdci`, and the `myx` user projects of mel and mdv.

## Requires

- Nothing in `Requires:`.
- Network access to `raw.githubusercontent.com` and `github.com`, and one of `curl`, `fetch` or `wget`.
- Root, on FreeBSD, macOS or an apt-based Linux.

## What it installs and configures

One install step, `host/install/common-myx.common.txt`. It downloads `install-myx.common.sh` from `github.com/myx/os-myx.common` and runs it. A failed download stops the deploy. That script:

- On FreeBSD: `pkg bootstrap`, and `ca_root_nss` when it is missing.
- Downloads `https://github.com/myx/os-myx.common/archive/master.tar.gz` and installs `/usr/local/bin/myx.common` and `/usr/local/share/myx.common/`, with `rsync` onto an existing install and with `tar` otherwise.
- When `OS_PACKAGES` is set: `myx.common lib/installEnsurePackage $OS_PACKAGES`.

## Inputs

| variable | effect |
|---|---|
| `MYX_COMMON_SKIP_INSTALL` | Set to anything, the step does nothing. For a host that gets `myx.common` another way. |
| `OS_PACKAGES` | packages to ensure after the install; other packages add to it with `image-install:context-variable:OS_PACKAGES:insert:<pkg>` |

The script can also be run by hand as root; its header comment shows how.
