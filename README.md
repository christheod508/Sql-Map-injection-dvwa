# Sql-Map-injection-dvwa
Automated SQL injection detection and exploitation using sqlmap — a hands-on case study on DVWA covering detection, enumeration, data extraction, and WAF/sanitization bypass attempts.

# SQLmap — Topic 12 (CEID, University of Patras)

Αυτοματοποιημένη ανίχνευση και εκμετάλλευση ευπαθειών SQL Injection με το εργαλείο **sqlmap**, πάνω στο Damn Vulnerable>

## Εγκατάσταση

```bash
sudo apt install sqlmap
```

ή απευθείας από το official repo:

```bash
git clone --depth 1 https://github.com/sqlmapproject/sqlmap.git sqlmap-dev
```

## Βασικό αποτέλεσμα (DVWA, security=low)

Dump του πίνακα `users` (βλ. αναλυτικά §8.4 στο Case-Study.txt):

| user    | password hash                     | cracked  |
|---------|------------------------------------|----------|
| admin   | 5f4dcc3b5aa765d61d8327deb882cf99   | password |
| gordonb | e99a18c428cb38d5f260853678922e03   | abc123   |
| 1337    | 8d3533d75ae2c3966d7e0d4fcc69216b   | charley  |
| pablo   | 0d107d09f5bbe40cade3de5c71e9e9b7   | letmein  |
| smithy  | 5f4dcc3b5aa765d61d8327deb882cf99   | password |

> Σημείωση: τα παραπάνω είναι τα **default DVWA test credentials** — δεν αφορούν πραγματικό, παραγωγικό σύστημα.

## Σημαντικά ευρήματα

- 4 ταυτόχρονες τεχνικές injection (boolean, error-based, time-based, UNION) στο Low level — 182 requests
- Το `mysqli_real_escape_string()` (Medium level) αντιστάθηκε στα default heuristics και σε δοκιμασμένα tamper scripts
- OS/file-system access απέτυχε λόγω απουσίας global `FILE` privilege στον DB user — παράδειγμα defense in depth

