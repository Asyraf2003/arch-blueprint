# Cleanup Orphans Decisions

Sumber inventory:
- core/packages/orphans.txt
- pacman -Qi (details)
- systemctl list-unit-files/status
- filesystem presence under /etc

## apache
Evidence:
- /etc/httpd exists, but appears default (httpd.conf head is stock).
- /etc/systemd/system/httpd.service.d/hardening.conf exists but is template (all options commented).
- httpd.service: disabled + inactive.

Decision:
- REMOVE candidate (not part of baseline desktop/workflow) unless a \"local web server\" role is explicitly adopted later.

Next proof if keeping:
- show active use-case (vhost/docroot customization, enabled service, or documented workflow).

## net-snmp
Evidence:
- snmpd.service: disabled + inactive.
- No usage evidence collected yet.

Decision:
- REMOVE candidate pending config/usage check.

Next proof:
- presence of /etc/snmp or use of snmp tools in workflow.


## breezy
Evidence:
- bzr exists but no Bazaar repos found: find /home -maxdepth 4 -name .bzr => empty
- pactree -r breezy => breezy only (no reverse deps)

Decision:
- REMOVED (unused toolchain).

## c-client
Evidence:
- pactree -r c-client => c-client only (no reverse deps)
- legacy IMAP client library; previously observed as non-official build metadata (supply-chain risk if kept without need)

Decision:
- REMOVED (unused legacy library).


## tidy
Evidence:
- tidy command not required for baseline workflow; removed during orphan cleanup.
- Orphan list updated after removal.

Decision:
- REMOVED (niche HTML tidy tool; reinstall if needed).


## libconfig
Evidence:
- Orphan package, no reverse dependencies.
- Small C/C++ config library; removed as leftover dependency.

Decision:
- REMOVED (cleanup orphan).


## libdispatch
Evidence:
- Orphan package, no reverse dependencies.
- Concurrency runtime library (swift-corelibs-libdispatch); removed as leftover dependency.

Decision:
- REMOVED (cleanup orphan).


## wayland-protocols
Evidence:
- Orphan package, typically used for development/build (protocol specifications), not required for baseline runtime.
- Removed during orphan cleanup.

Decision:
- REMOVED (cleanup orphan; reinstall if building Wayland software).


## ada
Evidence:
- Orphan C++ URL parser library.
- Removed during orphan cleanup as leftover dependency.

Decision:
- REMOVED (cleanup orphan).


## python-* tooling (build/installer/docutils/colorama/rsa/s3transfer/tzlocal)
Evidence:
- aws present: /usr/local/bin/aws
- Python 3.14.2
- Imports OK: s3transfer, rsa, tzlocal, colorama, docutils, installer, build

Decision:
- KEEP (workflow tooling; do not remove even if orphan).

## rust
Evidence:
- cargo present: /usr/bin/cargo
- cargo -V: cargo 1.92.0 (Arch rust 1:1.92.0-1)

Decision:
- KEEP (dev toolchain).

## protobuf
Evidence:
- protoc present: /usr/bin/protoc
- protoc --version: libprotoc 33.1

Decision:
- KEEP (dev toolchain).

## patchelf
Evidence:
- patchelf present: /usr/bin/patchelf
- patchelf --version: 0.18.0

Decision:
- KEEP (build/packaging utility).


## electron37
Evidence:
- Orphan package.
- VS Code package (code) depends on electron39, not electron37 (pacman -Qi code).
- No explicit app identified requiring electron37.

Decision:
- REMOVED (unused Electron runtime version).


## webkit2gtk
Evidence:
- pactree -r webkit2gtk => webkit2gtk only (no reverse deps).
- Removed during orphan cleanup.

Decision:
- REMOVED (unused WebKitGTK runtime).


## gst-libav
Evidence:
- pactree -r gst-libav => gst-libav only (no reverse deps declared).
- Runtime evidence: gst-inspect-1.0 libav => detected (plugin available).

Decision:
- KEEP (codec plugin; removing may reduce media support).


## kcoreaddons
Evidence:
- pactree -r kcoreaddons => kcoreaddons only (no reverse deps).
- Removed during orphan cleanup.

Decision:
- REMOVED (unused KDE Frameworks component).


## qt6-wayland (and qt6-declarative)
Evidence:
- pactree -r qt6-wayland => qt6-wayland only (no reverse deps).
- Removed qt6-wayland; qt6-declarative removed as dependency chain.

Decision:
- REMOVED (unused Qt6 Wayland/QML stack).


## qt6-imageformats (and jasper/libmng)
Evidence:
- pactree -r qt6-imageformats => qt6-imageformats only (no reverse deps).
- Removed qt6-imageformats; libmng and jasper removed as dependency chain.
- Note: graphicsmagick optionally requires jasper for JP2 module (feature loss acceptable unless JP2 workflow needed).

Decision:
- REMOVED (unused Qt image format plugins and related deps).


# Cleanup Phase Summary

Current orphan snapshot (core/packages/orphans.txt) is now a KEEP set:
- These packages are marked orphan by pacman, but are required by workflow/runtime based on evidence (tools present, imports OK, plugin detected).
- Policy: do not remove remaining orphans unless new evidence shows they are unused.

