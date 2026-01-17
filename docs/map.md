# Map (PATH -> Doc -> Criticality -> Restore)

## Mapped

| PATH | Doc | Criticality | Notes |
|------|-----|-------------|-------|
| /etc | docs/41-etc-inventory.md | Critical | /etc inventory baseline (evidence-based, secrets excluded) |
| / | docs/01-root-fhs.md | High | FHS + merged-/usr |
| cleanup ADR | adr/0005-cleanup-policy.md | Medium | Decision log for cleanup approach |
| vpn ADR | adr/0006-vpn-baseline-and-secrets.md | High | Decision log for VPN + incident remediation |
| systemd enabled units | docs/10-services-baseline.md | High | Baseline service state |
| audio stack | docs/20-audio.md | High | PipeWire + WirePlumber baseline |
| cleanup policy | docs/30-cleanup-policy.md | Medium | Rules for removing orphans safely |
| cleanup orphans | docs/31-cleanup-orphans.md | Medium | Decisions for orphan removal candidates |
| orphan packages (KEEP set) | docs/31-cleanup-orphans.md | Medium | pacman -Qdtq snapshot tracked; do not remove without new evidence |
| /etc baseline snapshot | docs/40-etc-baseline.md | Critical | core/etc-snapshots/00-baseline contains restore-critical configs |
| boot baseline | docs/60-boot-baseline.md | High | UEFI + GRUB on ESP (/boot) |
| network baseline | docs/61-network-baseline.md | High | NetworkManager + dns=none + pinned /etc/resolv.conf |
| storage baseline | docs/62-storage-baseline.md | High | lsblk/findmnt/df evidence + migration rules |
| vpn baseline | docs/63-vpn-baseline.md | High | WireGuard usage + secrets policy + migration rules |
| home manifest | docs/70-home-manifest.md | Critical | /home structure + foreign packages inventory + safety rules |

## TBD

| PATH | Doc | Criticality | Notes |
|------|-----|-------------|-------|
