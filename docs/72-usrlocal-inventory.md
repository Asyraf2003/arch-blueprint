# /usr/local Inventory (Baseline)

Sumber bukti:
- core/evidence/03-usrlocal/usrlocal.maxdepth2.txt
- core/evidence/03-usrlocal/usrlocal.getcap.txt

## Scope
Dokumen ini memetakan isi /usr/local untuk tujuan:
- restore/migrasi (new PC / reinstall)
- audit perubahan (apa yang “dipasang manual” di luar pacman)
- keamanan (capabilities / binary aneh)

## Rule of thumb
- /usr/local = area “lokal/manual/custom”, bukan paket resmi pacman.
- Fokus utama biasanya:
  - /usr/local/bin (custom CLI/tools)
  - /usr/local/share (data/tooling, termasuk blueprint ini)
  - /usr/local/lib (jarang, tapi bisa ada)

## Evidence summary
- Listing + ownership (max depth 2): lihat `usrlocal.maxdepth2.txt`
- Linux capabilities:
  - `usrlocal.getcap.txt` kosong (baseline saat ini: tidak ada capabilities terdeteksi)

## Restore / migration notes
- Jangan restore blind:
  - Review dulu isi /usr/local/bin: pastikan memang tool yang kamu butuh.
  - Jika ada file owned by root karena dibuat via sudo, itu normal untuk sistem-area, tapi untuk repo dokumen sebaiknya ownership konsisten (ideal: user).
- Kalau pindah mesin:
  - Install ulang tool official via pacman/AUR bila tersedia.
  - Untuk tool custom, restore dari repo/backup yang jelas (bukan copy acak dari /usr/local).

## Verifikasi (post-restore)
- Inventory sanity:
  - ls -la /usr/local
  - find /usr/local -maxdepth 2 -mindepth 1 -printf "%M %u:%g %s %TY-%Tm-%Td %TH:%TM %p\n" | sort
- Capabilities:
  - getcap -r /usr/local 2>/dev/null
- PATH/tooling check:
  - command -v <tool>
  - <tool> --version

## DoD
- Evidence /usr/local tersimpan.
- Ada doc inventory yang refer ke evidence dan aturan restore.
- Map tidak lagi menandai /usr/local sebagai TBD.

## Risiko/Mitigasi
- Risiko: ada binary custom yang ternyata backdoor (ya, dunia memang hobi gitu).
  Mitigasi: audit isi /usr/local/bin, cek capabilites, dan pastikan source of truth dari repo milikmu.
- Risiko: restore copy-paste bikin environment drift.
  Mitigasi: reinstall via pacman/AUR bila tersedia; untuk custom, versioning di repo.
