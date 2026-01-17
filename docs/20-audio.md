# Audio Stack

## Baseline (Evidence)
- pipewire (running, socket-activated)
- pipewire-pulse (running, socket-activated) => PulseAudio compatibility layer
- wireplumber (enabled + running) => session manager

Evidence:
- systemctl --user status wireplumber pipewire pipewire-pulse

## Packages present
- pipewire, wireplumber
- pipewire-alsa, pipewire-pulse, pipewire-jack (compat layers)
- alsa-* (ALSA base)
- sof-firmware (SoF firmware)

## About \"pipewire-session-manager\"
Walaupun runtime session manager yang aktif adalah WirePlumber, paket pipewire-session-manager TIDAK boleh dianggap sampah di sistem ini karena menjadi dependency (reverse deps) untuk beberapa paket penting:
- pipewire-alsa, pipewire-jack, pipewire-pulse
- gst-plugin-pipewire
dan turunannya (mpv, obs-studio, ffmpeg, dll)

Evidence:
- pactree -r pipewire-session-manager

Decision:
- KEEP (do not remove) sampai ada bukti resmi/upgrade path yang menghilangkan dependency ini tanpa memutus stack multimedia.
