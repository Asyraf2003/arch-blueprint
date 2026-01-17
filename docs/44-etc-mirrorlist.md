# /etc/pacman.d/mirrorlist Audit

Current baseline:
- Server = https://geo.mirror.pkgbuild.com/\$repo/os/\$arch

## Why this works
- Geo endpoint selects a nearby mirror automatically (portable across locations).
- Simple and low-maintenance.

## Tradeoffs
- Speed can vary (geo selection is not always the fastest).
- Single endpoint dependency (if geo service has issues, updates stall).

## Recommended modes

### Mode A: Portable baseline (default)
Keep geo as baseline for portability.
- Good for fresh installs/migrations where simplicity matters.

### Mode B: Fast predictable updates (optional)
If reflector is available, generate a ranked mirrorlist (top N).
Typical approach (example only):
- choose latest/fastest HTTPS mirrors for your region (ID/SG).
- keep geo as fallback at the bottom.

## Verification
- pacman -Syy (no errors)
- Optional: time pacman downloads during big upgrade

## Criticality
- Medium (does not change system behavior, but affects recovery speed).
