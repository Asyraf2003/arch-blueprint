# /etc/fstab Audit

Source:
- core/etc-snapshots/00-baseline/fstab

## Current mounts
- / (ext4) via UUID=19b3ac90-7734-43c0-8951-cdf47cc4e5c3
  - options: rw,relatime,lazytime
  - fsck pass: 1
- /boot (vfat) via UUID=14A6-36BD
  - options: rw,relatime,fmask=0022,dmask=0022,...,errors=remount-ro
  - fsck pass: 2

## Notes
- Setup is minimal (no separate /home, no swap entries, no remote mounts).
- This is safe and simple for a single-disk install.

## Migration rule (new PC / new disk)
DO NOT blindly reuse UUIDs.
- Preferred: regenerate fstab on target system:
  - Identify partitions: lsblk -f
  - Generate entries: genfstab -U /mnt >> /mnt/etc/fstab (during install)
- After editing fstab, verify before reboot:
  - findmnt -a
  - mount -a (caution)
  - reboot test

## Criticality
- Critical: wrong fstab can break boot or mounts.
