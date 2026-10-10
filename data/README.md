# data/

| Folder | Content | Rule |
|---|---|---|
| `raw/` | Packets as received and logged | Write once, never edit |
| `derived/` | Cleaned tables joined with pass geometry | Produced only by scripts, so every step from raw to result is reproducible |
| `tle/` | Orbital elements used, by date | Store the TLE valid at reception time. Do not download a newer one later: elevation angles would no longer match what the antenna saw |

Every dataset records its station configuration (antenna, LNA on/off, site, firmware version) and period. Without that, a measurement is not a measurement.
