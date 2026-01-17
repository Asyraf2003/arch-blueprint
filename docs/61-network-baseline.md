# Network Baseline (NetworkManager, manual DNS)

## Baseline (evidence)
- systemd service:
  - NetworkManager.service: enabled + active
  - systemd-resolved.service: disabled + inactive
- DNS model:
  - /etc/NetworkManager/conf.d/90-dns-none.conf => dns=none
  - /etc/resolv.conf is a regular file (not a symlink)
  - /etc/resolv.conf content:
    - nameserver 8.8.8.8
    - nameserver 1.1.1.1

Meaning:
- NetworkManager manages links (Wi-Fi/Ethernet).
- DNS is NOT managed by NM (dns=none).
- Resolver config is manually pinned in /etc/resolv.conf.

## Files captured (blueprint snapshot)
- core/etc-snapshots/00-baseline/resolv.conf
- core/etc-snapshots/00-baseline/NetworkManager/NetworkManager.conf
- core/etc-snapshots/00-baseline/NetworkManager/conf.d/

## Sensitive data rule (DO NOT COMMIT)
DO NOT commit:
- /etc/NetworkManager/system-connections/*.nmconnection
Reason:
- Contains Wi-Fi profiles and secrets (even if encrypted, still sensitive).
Strategy:
- Recreate Wi-Fi profiles manually on new machine OR export/import securely outside Git.

## Migration note (new PC)
If you want the same DNS behavior:
1) Ensure NetworkManager installed + enabled.
2) Place dns=none drop-in.
3) Write resolv.conf with desired nameservers.
4) Verify:
   - systemctl status NetworkManager
   - resolvectl status (if resolved is used) OR check /etc/resolv.conf content
   - nmcli dev status

If you prefer a more standard stack later:
- Switch to systemd-resolved or another resolver, then write an ADR (future change).
