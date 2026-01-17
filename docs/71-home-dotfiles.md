# /home Baseline (Dotfiles + Workflow)

Sumber bukti:
- core/home/00-manifest/home.ls-la.txt
- core/home/00-manifest/config.ls-la.txt
- core/home/00-manifest/config.dirs.txt
- core/home/00-manifest/local-bin.ls-la.txt
- core/packages/pacman.foreign.txt

## Scope
Dokumen ini memetakan isi penting di /home untuk tujuan:
- restore/migrasi workstation
- audit perubahan workflow

## Exclusions (Secrets / Sensitive)
JANGAN pernah dimasukkan repo:
- ~/.ssh/**
- ~/.gnupg/**
- ~/.aws/** (credentials)
- ~/.config/* yang berisi token (mis. browser profiles)
- *.pem, *.key, *PrivateKey*, *PresharedKey*
- .env, *.env.*, credentials.json

Aturan penyimpanan:
- hanya manifest (ls/find) + template contoh (.sample/.example) tanpa secret
- secrets disimpan di Bitwarden / secret manager

## Restore notes
- Restore bertahap:
  1) Struktur folder (tanpa file secret)
  2) Dotfiles non-secret
  3) Tools/scripts di ~/.local/bin
  4) Reinstall package sesuai pacman.*.txt
- Hindari restore blind untuk:
  - browser profile (sering korup/berantakan lintas versi)
  - cache (hapus saja)

## Verifikasi (post-restore)
- Login shell OK
- PATH berfungsi (tools di ~/.local/bin terdeteksi)
- Aplikasi utama jalan (terminal, editor, browser)
- Git/SSH OK (keys di-inject manual)

## DoD
- Ada baseline /home yang aman (tidak mengandung secret).
- Ada aturan restore + verifikasi.
- Map tidak lagi menandai /home sebagai TBD.

## Risiko/Mitigasi
- Risiko: secret bocor via dotfiles.
  Mitigasi: allowlist + grep pattern audit sebelum commit.
- Risiko: restore bikin env “berbeda” karena versi paket.
  Mitigasi: pin package list + catat foreign packages.
