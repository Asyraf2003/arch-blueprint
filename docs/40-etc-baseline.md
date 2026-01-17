# /etc Baseline Snapshot

Sumber snapshot:
- core/etc-snapshots/00-baseline/

Tujuan:
- Membekukan konfigurasi inti sistem supaya migrate ke PC lain bisa cepat dan deterministic.
- Semua restore berbasis bukti (diff), bukan ingatan.

## Contents (What / Why / Criticality / Restore)

### fstab
Fungsi:
- Mapping filesystem mount (root, home, swap, data, options).
Criticality: **Critical** (salah = gagal boot atau data tidak ke-mount).
Restore:
- Copy file lalu verify dengan: \`findmnt -a\`, \`mount -a\` (caution), reboot test.

### mkinitcpio.conf
Fungsi:
- Parameter pembuatan initramfs (MODULES, HOOKS, FILES).
Criticality: **High** (salah hook = boot fail, disk tidak kebaca).
Restore:
- Copy file lalu rebuild: \`mkinitcpio -P\` dan reboot test.

### pacman.conf
Fungsi:
- Repo config, SigLevel, parallel downloads, hooks, options.
Criticality: **High** (supply-chain + kemampuan install/update).
Restore:
- Copy file lalu verify: \`pacman -Syy\` (and check no errors).

### locale.gen / locale.conf
Fungsi:
- Locale generation + default LANG/LC_*.
Criticality: **Medium** (UX; bisa bikin tool error jika encoding aneh).
Restore:
- Copy files lalu: \`locale-gen\` dan verify: \`locale\`.

### vconsole.conf
Fungsi:
- Keymap/font untuk TTY.
Criticality: **Low/Medium** (akses darurat).
Restore:
- Copy file, relogin TTY.

### hostname / hosts
Fungsi:
- Identity + name resolution dasar.
Criticality: **Medium** (network tooling dan sebagian service peduli).
Restore:
- Copy files, verify: \`hostnamectl\`, \`getent hosts \$(hostname)\`.

### systemd/timesyncd.conf
Fungsi:
- Time sync baseline (penting untuk TLS/log/build).
Criticality: **Medium**.
Restore:
- Copy file, verify: \`timedatectl status\`, \`systemctl status systemd-timesyncd\`.

### sudoers / sudoers.d/
Fungsi:
- Privilege escalation policy.
Criticality: **Critical** (salah = lockout atau security hole).
Restore:
- Copy file(s) dengan permission yang benar.
- Verify dengan: \`visudo -c\` dan \`sudo -l\`.

## Safety rules before committing to Git
- Jangan commit secret/token/password (umumnya tidak ada di file ini, tapi tetap audit manual).
- Gunakan Bitwarden untuk secrets.
