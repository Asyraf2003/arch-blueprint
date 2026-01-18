# Downloads Inbox Policy

Tujuan: ~/Downloads hanya inbox sementara.

## Rule
- Semua file di Downloads harus diproses (move/rename) maksimal 24-72 jam.
- Kategori tujuan:
  - Secrets (keys, creds, accessKeys, *.pem, *.key, *.csv berisi access key) -> ~/secrets (chmod 700 dir, file 600)
  - DB dumps (*.sql, *.dump, *.gz) -> ~/data/db-dumps
  - Reports / exports -> ~/data/exports/(reports|csv|pdf)
  - Media (jpg/png/mp4/zip aset) -> ~/data/exports/(media|assets)
  - Scratch / temporary scripts -> ~/scratch/(web|tmp|notes)

## Guardrail
- Jangan pernah commit isi ~/Downloads ke repo mana pun.
- Audit cepat:
  - find ~/Downloads -maxdepth 1 -type f -printf '%m %u:%g %s %TY-%Tm-%Td %TH:%TM %f\n' | sort
  - rg -n "(BEGIN .* PRIVATE KEY|PrivateKey|PresharedKey|AKIA|accessKey|secret)" -S ~/Downloads || true
