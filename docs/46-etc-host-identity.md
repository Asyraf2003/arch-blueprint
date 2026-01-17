# /etc/hostname and /etc/hosts Audit

Sources:
- core/etc-snapshots/00-baseline/hostname
- core/etc-snapshots/00-baseline/hosts

## hostname
- arch

## hosts entries (observed)
Baseline:
- 127.0.0.1 localhost
- ::1 localhost
- 127.0.0.1 arch.localdomain arch

Dev overrides / convenience:
- 127.0.0.1 school.local
- 127.0.0.1 commerce.local
- 47.129.218.152 app1.asyrafff.my.id
- 47.129.218.152 app2.asyrafff.my.id

## Notes
- Baseline entries are required for sane local name resolution.
- Additional entries act as local DNS overrides:
  - Useful for development shortcuts.
  - Can hide real DNS issues and can become stale if IP changes.

## Migration rule (new PC)
- Always keep baseline entries.
- Only carry Dev overrides if the new machine also needs the same dev shortcuts.
- Prefer real DNS (Cloudflare/Route53) unless you intentionally want an override.

## Verification
- hostnamectl
- getent hosts localhost
- getent hosts arch
- getent hosts app1.asyrafff.my.id

## Criticality
- Medium (can cause confusing networking behavior if wrong).
