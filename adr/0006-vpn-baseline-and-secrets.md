# ADR 0006: VPN baseline and secrets handling (WireGuard)

## Status
Accepted

## Context
Sistem butuh VPN yang:
- reproducible saat migrasi (target < 2 jam)
- aman (secrets tidak bocor ke Git)
Ditemukan insiden nyata:
- file WireGuard config (`/etc/wireguard/protonvpn.conf`) sempat world-readable dan mengandung `PrivateKey`.

## Decision
1) Standarkan VPN ke WireGuard:
- kernel module tersedia dari paket kernel (`linux`)
- gunakan `wireguard-tools` (`wg`, `wg-quick`)
- helper lokal `vpn on/off` dipertahankan sebagai workflow wrapper

2) Secrets policy:
- Tidak ada file config VPN real di Git.
- Repo hanya menyimpan template `*.conf.example` tanpa `PrivateKey`/`PresharedKey`.
- Secrets disimpan di Bitwarden / provider portal.

3) Incident remediation rule:
- Jika ditemukan `PrivateKey` di file yang tidak semestinya:
  - segera kunci permission (0600 root:root)
  - revoke/regen config dari provider
  - kosongkan file lama bila tidak dipakai
  - buat backup forensik lokal berlabel `REVOKE_ME.<timestamp>` (tetap tidak boleh masuk Git)

## Consequences
- Migrasi lebih cepat: cukup restore template + inject secrets dari Bitwarden.
- Risiko kebocoran turun drastis karena ada guardrail (.gitignore + policy).
- Ketergantungan VPN lebih kecil: hindari toolchain ProtonVPN CLI yang rawan break karena Python packaging.

## References
- docs/63-vpn-baseline.md
- .gitignore secrets rules
