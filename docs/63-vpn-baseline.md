# VPN Baseline (WireGuard + Secrets Policy)

## Scope
Blueprint ini mendokumentasikan baseline VPN di sistem ini:
- WireGuard (kernel + tools)
- Helper lokal `vpn on/off` (wrapper)
- Kebijakan secrets: TIDAK ADA key/config real di Git

---

## Evidence (current system)
- WireGuard kernel module tersedia dan bisa dimuat:
  - `sudo modprobe wireguard` => OK
  - module path dimiliki paket kernel `linux` (bukan DKMS)
- Tools tersedia:
  - `wg`, `wg-quick` dari `wireguard-tools`
- Config directory:
  - `/etc/wireguard/*.conf` (mengandung secrets)

---

## Important: configs contain secrets
`/etc/wireguard/*.conf` biasanya mengandung:
- `PrivateKey` (SECRET)
- `PresharedKey` (SECRET, jika ada)
- endpoint/AllowedIPs (sensitive metadata)

Rule:
- Jangan pernah commit file `.conf` real ke Git.
- Simpan secrets di Bitwarden.
- Di repo, hanya simpan template: `*.conf.example` TANPA `PrivateKey`/`PresharedKey`.

---

## Incident note (real evidence)
Pernah ditemukan:
- `/etc/wireguard/protonvpn.conf` world-readable + mengandung `PrivateKey` (high risk).
Remediation:
- Permission dikunci `0600 root:root`.
- File dikosongkan (0 bytes) dan config harus diregenerate dari ProtonVPN.
- Backup lokal dibuat dengan suffix `REVOKE_ME.<timestamp>` untuk forensik sementara.

---

## Usage
### Direct WireGuard
- Up:
  - `sudo wg-quick up <name>`
- Down:
  - `sudo wg-quick down <name>`
- Status:
  - `sudo wg`

Catatan:
- Unit template ada: `wg-quick@.service` (default disabled).
- Jika mau persist, dokumentasikan interface yang dipakai dan enable unit yang sesuai.

### Helper (wrapper)
Sistem ini memakai helper `vpn on/off` (custom script) untuk:
- bring up interface
- (optional) static route endpoint via gateway WiFi
- nft rules (policy routing)

Helper dianggap bagian dari workflow dan harus didokumentasikan di `/home` / dotfiles repo.

---

## Migration rules (new PC)
- Jangan copy config real dari Git.
- Restore file template dulu, lalu isi secrets via Bitwarden / provider portal.
- Pastikan permission:
  - `/etc/wireguard/*.conf` => `0600 root:root`
- Verify:
  - `sudo modprobe wireguard`
  - `command -v wg wg-quick`
  - `sudo wg-quick up <iface>` lalu `sudo wg`
  - `curl ifconfig.me` (atau bukti rute berubah) sesuai kebutuhan

---

## Cleanup / packages
- `wireguard-tools`: KEEP (required)
- `wireguard-dkms`: REMOVE candidate bila module sudah built-in kernel (umumnya iya di Arch kernel modern).
  Evidence check:
  - `modinfo -n wireguard` path harus dari `/usr/lib/modules/...` dan owned by `linux` package.

