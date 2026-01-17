# Pacman Keyring & Trust (GnuPG)

## Why this matters
Pacman relies on GPG trust to verify package signatures. Broken keyring/trustdb = installs/updates fail.

## Evidence (this machine)
Key packages:
- archlinux-keyring present
- gnupg present
- pacman present

Trustdb check (correct method):
- gpg --homedir /etc/pacman.d/gnupg --check-trustdb
Result:
- No \"unsafe permissions\" warning after permission fix
- Note about SHA1 third-party signatures being rejected is informational (expected)

## Permission rule
GnuPG homedir must be private:
- /etc/pacman.d/gnupg => 700 root:root

Fix:
- chmod 700 /etc/pacman.d/gnupg

## Migration rule (new install)
Do NOT copy /etc/pacman.d/gnupg blindly across machines.
Instead:
- pacman-key --init
- pacman-key --populate archlinux
- keep archlinux-keyring up to date (pacman -Syu)

## Verification
- sudo gpg --homedir /etc/pacman.d/gnupg --check-trustdb
- sudo pacman-key --list-keys | head
- sudo pacman -Syu (signature verification should pass)

## Criticality
- High (package management trust chain).
