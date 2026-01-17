# /etc Inventory (Safe Baseline)

Sumber bukti:
- core/evidence/02-etc/etc.ls-la.txt
- core/evidence/02-etc/etc.focus.txt

## Scope
Dokumen ini memetakan konfigurasi sistem di /etc untuk tujuan:
- restore/migrasi
- audit perubahan
- memahami “kenapa sistem begini”

## Exclusions (Secrets / Sensitive)
Tidak pernah dicapture di evidence karena rawan bocor:
- /etc/NetworkManager/system-connections
- /etc/wireguard
- /etc/ssh
- /etc/ssl/private
- /etc/pacman.d/gnupg

Aturan:
- konfigurasi yang mengandung credential/key hanya boleh disimpan sebagai:
  - template .example/.sample tanpa secret
  - atau disimpan di secret manager (Bitwarden)

## Struktur pembacaan /etc (rule of thumb)
- Network: /etc/NetworkManager, /etc/resolv.conf (kalau pinned)
- Boot/initramfs: /etc/mkinitcpio.conf, /etc/default/grub
- Package manager: /etc/pacman.conf, /etc/pacman.d/mirrorlist
- Locale/input: /etc/locale.conf, /etc/locale.gen, /etc/vconsole.conf
- Time sync: /etc/systemd/timesyncd.conf (+ drop-ins bila ada)
- Host identity: /etc/hostname, /etc/hosts

## Restore notes
- Jangan restore blind untuk file yang machine-specific (UUID disk, hostname, hosts custom).
- Restore dengan permission benar untuk file sensitif:
  - /etc/sudoers (+ visudo -c)
  - file di /etc/* yang mode-nya ketat

## Verifikasi (post-restore)
- Inventory sanity:
  - sudo findmnt -a
  - sudo systemctl --failed
- Sudo:
  - sudo visudo -c
  - sudo -l
- Network:
  - nmcli general status
  - cat /etc/resolv.conf

## DoD
- Evidence /etc tersimpan tanpa secret path.
- Ada daftar area penting + cara verifikasi restore.
- Map tidak lagi menandai /etc sebagai TBD.

## Risiko/Mitigasi
- Risiko: secret ikut kesimpan -> kebocoran.
  Mitigasi: exclusions + .gitignore + grep pattern audit.
- Risiko: restore bikin boot/network rusak.
  Mitigasi: verifikasi sebelum reboot + incremental restore.
