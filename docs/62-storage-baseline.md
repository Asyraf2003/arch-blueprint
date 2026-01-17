# Storage Baseline (mounts, filesystems, layout)

Evidence captured:
- core/storage/00-baseline/lsblk-f.txt
- core/storage/00-baseline/findmnt-root.txt
- core/storage/00-baseline/df-Th.txt

## Rules for migration (new PC / new disk)
- DO NOT reuse UUIDs from old machine.
- Use fresh discovery on target:
  - lsblk -f
  - genfstab -U /mnt >> /mnt/etc/fstab (during install)
- Verify mounts before reboot:
  - findmnt -a
  - mount -a (caution)
- For ESP (/boot):
  - Ensure it is vfat and mounted correctly.
  - Reinstall bootloader (GRUB) on target ESP, do not assume copy-paste works.

## What must remain consistent
- The intended mountpoints (/ and /boot).
- Filesystem type expectations (ext4 for /, vfat for ESP) unless you intentionally change architecture.

## What must change
- UUID/PARTUUID values.
- Device paths (/dev/nvme... may differ).
