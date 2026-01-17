# Network Baseline (NetworkManager + Static DNS)

## Stack decision (evidence-based)
- Link/DHCP/connection management: NetworkManager (system service enabled + running)
- systemd-resolved: disabled + inactive
- DNS management: **manual static** (/etc/resolv.conf) because NetworkManager is configured with `dns=none`

Evidence:
- systemctl status NetworkManager
- systemctl status systemd-resolved (inactive)
- /etc/NetworkManager/conf.d/90-dns-none.conf => [main] dns=none
- /etc/resolv.conf contains static nameservers

## Files and boundaries
### Baseline (portable, commit-safe)
- /etc/NetworkManager/conf.d/90-dns-none.conf
- /etc/resolv.conf (static DNS)
- /etc/NetworkManager/NetworkManager.conf (currently default, keep for completeness)

### Machine-specific (DO NOT commit to Git)
- /etc/NetworkManager/system-connections/*.nmconnection
Reason:
- Can contain WiFi secrets (PSK) and is not portable across devices.
- Better recreated per machine (or restored from encrypted secret manager, not git).

## Current DNS (this machine)
- 8.8.8.8
- 1.1.1.1

## Migration rule (new PC)
1) Install NetworkManager and enable it:
   - systemctl enable --now NetworkManager
2) Apply baseline:
   - Place `90-dns-none.conf` and `/etc/resolv.conf` as documented
3) Recreate WiFi connections on target machine:
   - via nmtui / nmcli
   - store WiFi secrets in Bitwarden, not git

## Optional: revert to DHCP-provided DNS (if needed)
If captive portals / specific networks require DHCP DNS:
- Temporarily disable static DNS behavior by removing or renaming:
  - /etc/NetworkManager/conf.d/90-dns-none.conf
Then restart:
- systemctl restart NetworkManager
And allow NM to manage resolv.conf (implementation depends on your setup).

## Verification
- ip route, DNS reachability:
  - ping -c 1 1.1.1.1
  - resolvectl status (if systemd-resolved is used) OR:
  - dig archlinux.org @1.1.1.1 (if `bind` tools exist) / `getent hosts archlinux.org`
- Confirm resolv.conf stays static after reconnect:
  - cat /etc/resolv.conf

## Criticality
- Medium/High: wrong DNS can break internet even if WiFi is connected.
