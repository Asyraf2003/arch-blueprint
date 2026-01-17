# /etc Locale & Console Keymap Audit

Sources:
- core/etc-snapshots/00-baseline/locale.conf
- core/etc-snapshots/00-baseline/locale.gen
- core/etc-snapshots/00-baseline/vconsole.conf

## Current baseline
### locale.conf
- LANG=en_US.UTF-8

### locale.gen (enabled)
- en_US.UTF-8 UTF-8

### vconsole.conf
- KEYMAP=us
- XKBLAYOUT=us
- XKBMODEL=pc105+inet
- XKBOPTIONS=terminate:ctrl_alt_bksp

## Notes
- Minimal locale set (single UTF-8 locale) is simple and portable.
- US keymap is portable across machines.
- Ctrl+Alt+Backspace terminates X (useful for recovery).

## Migration rule (new PC)
- Keep these defaults unless you intentionally choose a different language/layout.
- After restore/change, regenerate locales:
  - locale-gen

## Verification
- locale
- localectl status

## Criticality
- Medium (wrong locale/layout is annoying; rarely blocks boot).
