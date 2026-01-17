# Boot Baseline (UEFI + GRUB on ESP mounted at /boot)

## Current design (evidence)
- Firmware mode: UEFI (Secure Boot disabled)
- ESP mountpoint: /boot (vfat)
- Bootloader: GRUB 2.13
- EFI loader path: \\EFI\\GRUB\\grubx64.efi
- GRUB config:
  - /etc/default/grub (source for grub-mkconfig)
  - /boot/grub/grub.cfg (generated)

Evidence:
- bootctl status (EFI vars show GRUB)
- efibootmgr -v (Boot0000 GRUB points to \\EFI\\GRUB\\grubx64.efi)
- /boot listing shows /boot/grub/grub.cfg
- /etc/default/grub captured

## Files to capture (blueprint)
### Baseline (commit-safe)
- /etc/default/grub

### Evidence (do not blindly reuse on other disks)
- /boot/grub/grub.cfg
Rationale:
- grub.cfg is generated and may embed UUIDs/device paths for the current disk.

## Migration rule (new PC / new disk)
DO NOT copy /boot/grub/grub.cfg as-is.
Preferred flow on target:
1) Ensure ESP is mounted at /boot (or document if different)
2) Install GRUB for UEFI and register boot entry:
   - grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
3) Generate config:
   - grub-mkconfig -o /boot/grub/grub.cfg
4) Verify boot entries:
   - efibootmgr -v
5) Reboot test.

## Current GRUB behavior
- Hidden menu + zero timeout:
  - GRUB_TIMEOUT=0
  - GRUB_TIMEOUT_STYLE=hidden
Impact:
- Fast boot, but recovery/menu access requires ESC/SHIFT timing.

Kernel cmdline:
- GRUB_CMDLINE_LINUX_DEFAULT=\"quiet loglevel=3\"

## Noted anomaly (needs investigation)
Observed files in /boot:
- 0-initramfs-linux.img (0 bytes)
- 0-initramfs-linux-fallback.img (0 bytes)
- 0-vmlinuz-linux (non-zero)
These look like artifacts/symlink replacements and should not be treated as canonical kernel/initramfs.
Action:
- Investigate origin before removing.

## Verification
- Confirm ESP mount:
  - findmnt /boot
- Confirm GRUB entry exists and is first in BootOrder:
  - efibootmgr -v
- Confirm grub.cfg regeneration works:
  - grub-mkconfig -o /boot/grub/grub.cfg
