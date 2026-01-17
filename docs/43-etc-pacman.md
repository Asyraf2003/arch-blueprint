# /etc/pacman.conf Audit

Source:
- core/etc-snapshots/00-baseline/pacman.conf

## Security & integrity
- SigLevel = Required DatabaseOptional
  - Packages must be signed by trusted keys (good baseline).
- LocalFileSigLevel = Optional
  - Local package files may be unsigned (OK for local builds; do not install random unsigned packages).

## Stability & UX
- HoldPkg = pacman glibc (protect core tooling)
- CheckSpace enabled (prevents mid-upgrade failures)
- ParallelDownloads = 5 (performance)
- DownloadUser = alpm (default/secure behavior)

## Repositories
Enabled:
- [core]
- [extra]
Disabled:
- testing repos
- multilib (32-bit)
- custom repos

Mirror source:
- Include = /etc/pacman.d/mirrorlist

## Migration notes (new PC)
- Keep this pacman.conf as baseline.
- Regenerate/refresh mirrorlist on the target machine based on location/connectivity.
- Always ensure keyring is initialized:
  - pacman-key --init
  - pacman-key --populate archlinux

## Criticality
- High (controls package integrity and update ability).
