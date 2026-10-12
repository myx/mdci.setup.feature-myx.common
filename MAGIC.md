# MAGIC.md — setup.feature-myx.common

Maintainer notes. Users read `README.md`. How the packages compose: `../MAGIC.md`.

## One step bootstraps `myx.common` for every host

- Both OS floors and the mel and mdv `myx` user projects require `standard/setup.feature-myx.common`, so the step runs once, before any step that calls `myx.common`.
- It holds the one-liner and nothing else. The downloaded `install-myx.common.sh` is the one in `github.com/myx/os-myx.common`. No copy of it lives in this repo, because a local copy goes stale: the mel and mdv copies had.
- `MYX_COMMON_SKIP_INSTALL` is the only switch. It was the FreeBSD floor's variable and moved here, because this step is the only reader.
- The script is held in a variable before it runs. A pipe straight into `sh -e` runs an empty script when the download fails, and that counts as success.

## This step is where most `OS_PACKAGES` get installed

- Declaring `image-install:context-variable:OS_PACKAGES:insert:<pkg>` only puts the name in the installer environment. Neither OS floor installs `OS_PACKAGES`.
- The downloaded script ends with `myx.common lib/installEnsurePackage $OS_PACKAGES`. `setup.feature-mdci`'s `0.prepare` makes the same call. Nothing else in this repo does.
- So the packages of the FreeBSD floor (`ca_root_nss`, `bash`, `rsync` …), of `setup.feature-monit` (`monit`) and of the l6route profiles reach the host only where one of those two steps runs.

## A bare FreeBSD host

- The download runs before the script's own `pkg bootstrap` and `ca_root_nss` step, so it relies on the base system's CA bundle. FreeBSD 15 has one: `bhv2h1` holds 152 trusted certificates in base, and its `/etc/ssl/cert.pem` is a generated file, not a link into the port. Not tested on a bare host.
- Every `myx.common` call in the floors and features needs `myx.common` on the host. This step is the only one that puts it there.
