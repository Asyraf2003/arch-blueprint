# Home Baseline (manifest only, no secrets)

Evidence captured:
- core/home/00-manifest/home.ls-la.txt
- core/home/00-manifest/config.ls-la.txt
- core/home/00-manifest/config.dirs.txt
- core/home/00-manifest/local-bin.ls-la.txt
- core/packages/pacman.foreign.txt (foreign/AUR/3rd-party packages)

## Why manifest-first
- /home is where the \"2-hour restore\" lives (dotfiles, scripts, editor config).
- We start with a directory map to decide what becomes dotfiles repo vs what stays local-only.

## Safety: NEVER commit these
- ~/.ssh (private keys), ~/.gnupg (private keys)
- Browser profiles (cookies/sessions): ~/.config/*chrome* / *chromium* / *brave*
- Password managers vault exports (Bitwarden stays as source of truth)
- NetworkManager secrets: /etc/NetworkManager/system-connections (WiFi PSK)
- API tokens, cloud creds: ~/.aws, ~/.kube, ~/.docker, ~/.npmrc, etc (unless sanitized)

## Next step after manifest
- Choose dotfiles tool (stow/chezmoi) and split:
  - Trackable: shell, terminal, editor, WM, scripts
  - Local-only: caches, secrets, machine-specific IDs

## Foreign package decisions

Aturan:
- Foreign (AUR/non-official) = perlu bukti pemakaian + risiko supply-chain.
- Hapus hanya jika: tidak dipakai + tidak jadi dependency workflow + tidak ada config/service terkait.

### xwinwrap-git
Evidence:
- command: xwinwrap gone (removed)
- Use-case: wallpaper/video overlay (non-essential)

Decision:
- REMOVED (unneeded desktop gimmick; reinstall if needed).

### nitrogen
Evidence:
- command: nitrogen gone (removed)
- Reverse deps (historical): nitrogen <- gtkmm <- gtk2
- Post-removal: pacman -Qdtq no longer lists nitrogen/gtk* as orphans => gtk2/gtkmm still required by other packages (keep).

Decision:
- REMOVED (not used for wallpaper management).

### cheese
Evidence:
- command: cheese gone (removed)
- Reverse deps (historical): cheese pulled clutter/clutter-gtk/clutter-gst/cogl/libcheese chain
- Use-case: simple webcam tester/recorder (non-essential; OBS can replace for recording)

Decision:
- REMOVED (unused; reduces GTK/Clutter surface area).

