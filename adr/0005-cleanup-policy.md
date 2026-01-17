# ADR 0005: Cleanup policy and debug packages removal

## Status
Accepted

## Context
Sistem harus optimal dan bisa dipahami. Orphan packages muncul dari eksperimen/ketergantungan yang sudah hilang.
Penghapusan tanpa bukti berisiko memutus runtime.

## Decision
- Terapkan cleanup policy berbasis bukti (lihat docs/30-cleanup-policy.md).
- Detached debug symbol packages yang tidak dibutuhkan dihapus:
  - c-client-debug
  - telegram-cli-git-debug
  - whatsapp-for-linux-git-debug
  - xwinwrap-git-debug
  - yay-bin-debug

## Consequences
- Disk lebih bersih tanpa risiko fungsi runtime.
- Semua langkah cleanup harus melalui klasifikasi + bukti.

