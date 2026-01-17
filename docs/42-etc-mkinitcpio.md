# /etc/mkinitcpio.conf Audit

Source:
- core/etc-snapshots/00-baseline/mkinitcpio.conf

## Current config summary
- MODULES=() (no hardcoded modules; portable default)
- Initramfs style: systemd (HOOKS includes \`systemd\`)
- Root/filesystem: ext4 simple setup (no encrypt/lvm/raid hooks)

## HOOKS meaning (current)
HOOKS=(base systemd autodetect microcode modconf kms keyboard keymap sd-vconsole block filesystems fsck)

- base: required base files
- systemd: systemd-based initramfs
- autodetect: include only modules needed for current machine (smaller/faster)
- microcode: CPU microcode early load
- modconf: include modprobe config from /etc/modprobe.d
- kms: early kernel mode-setting (smoother graphics)
- keyboard/keymap/sd-vconsole: early input + keymap for initramfs/TTY
- block: storage stack
- filesystems: filesystem drivers
- fsck: filesystem check support

## Migration rule (new PC / new hardware)
Default: keep current HOOKS and rebuild initramfs.
- Rebuild: mkinitcpio -P
- Test boot

If boot fails due to missing modules (rare but possible with very different hardware):
- Temporary safe mode: remove \`autodetect\` from HOOKS to build a more universal image.
  Example “full image” style:
  HOOKS=(base systemd microcode modconf kms keyboard keymap sd-vconsole block filesystems fsck)
- Rebuild: mkinitcpio -P
- Boot test again

## Criticality
- High: wrong hooks can break boot.
