# Cleanup Policy (Orphans, Redundancy, Trash)

## Principles
- Tidak ada \"hapus\" tanpa bukti.
- Orphan = tidak dibutuhkan paket lain. Bukan berarti tidak dibutuhkan user.
- Hapus aman = (1) tidak dipakai runtime + (2) tidak dibutuhkan workflow + (3) tidak menjadi dependency penting.

## Allowed actions (safe tiers)
1) Remove detached debug symbol packages (\"*-debug\") bila tidak sedang debugging native crash.
2) Clean pacman cache dengan aturan (bukan hapus membabi buta).
3) Remove orphans hanya setelah klasifikasi:
   - KEEP: dibutuhkan workflow (dev tools, runtimes yang dipakai)
   - REMOVE: jelas tidak dipakai + tidak ada konfigurasi/service terkait
   - INVESTIGATE: runtime besar / platform libs (Qt/WebKit/Electron)

## Evidence types
- pacman -Qi, pacman -Qdtq (inventory)
- pactree -r (dependency reality check)
- systemctl list-unit-files / status (service reality check)
- presence of configs: /etc/<service> (configuration reality check)

