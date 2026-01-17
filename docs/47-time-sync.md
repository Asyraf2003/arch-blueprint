# Time Sync (systemd-timesyncd)

Source:
- core/etc-snapshots/00-baseline/timesyncd.conf
- Services baseline: systemd-timesyncd.service enabled

## Current config
[Time]
NTP=0.id.pool.ntp.org 1.id.pool.ntp.org 2.id.pool.ntp.org 3.id.pool.ntp.org
FallbackNTP=0.arch.pool.ntp.org 1.arch.pool.ntp.org

## Notes
- Using ID pool NTP reduces latency in Indonesia.
- Fallback uses Arch pool.

## Migration rule (new PC)
- Keep as default if operating mainly in Indonesia.
- If used in other regions, consider switching to region-appropriate pool servers or leaving defaults.

## Verification
- timedatectl status
- timedatectl timesync-status
- systemctl status systemd-timesyncd --no-pager

Expected:
- NTP service active
- System clock synchronized: yes

## Criticality
- High (time affects TLS, logs, builds).
