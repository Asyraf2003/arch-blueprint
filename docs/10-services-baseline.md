# Services Baseline (Enabled)

Sumber: core/services/enabled-unit-files.txt

## Network
- NetworkManager.service
- NetworkManager-dispatcher.service
- NetworkManager-wait-online.service
Catatan: ini berarti stack network kamu distandarkan ke NetworkManager.

## Time sync
- systemd-timesyncd.service
Fungsi: sinkron waktu (penting untuk TLS, log, build, dan hidup manusia modern).

## Storage maintenance
- fstrim.timer
Fungsi: TRIM berkala untuk SSD.

## Power / ACPI events
- acpid.service
Fungsi: menangani event ACPI (tombol power/lid dsb, tergantung setup).

## Boot/login
- getty@.service (template)
Fungsi: TTY login.

## User/identity db
- systemd-userdbd.socket
Fungsi: socket untuk userdb.

## Filesystem mounts
- remote-fs.target
Fungsi: target untuk mount filesystem remote (aktif kalau ada konfigurasi mount remote).

