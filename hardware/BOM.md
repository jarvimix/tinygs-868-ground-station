# Bill of Materials (preliminary)

Status 10/2026. **Core** is what the station needs to run with the self-built antenna. **Upgrade** items are optional, and each one has a stated benefit.

## Core

| Part | Qty | Purpose | Link | Status |
|---|---|---|---|---|
| Heltec WiFi LoRa 32 V3, 868 MHz, with stock antenna | 1 | Receiver: ESP32-S3 + SX1262, runs TinyGS. Stock antenna is the baseline for experiment B | [Heltec docs](https://wiki.heltec.org/docs/devices/open-source-hardware/esp32-series/lora-32/wifi-lora-32-v3/) | ✅ received |
| USB-C cable + 5 V power supply | 1 | Power for the board | [Raspberry Pi PSU](https://www.ldlc.com/fiche/PB00273949.html) | ⬜ |
| SMA female panel connector, 4-hole flange | 1 | Antenna feed: radiator soldered to the centre pin, radials screwed to the flange | [Molex 0733910040](https://www.digikey.fr/fr/products/detail/molex/0733910040/2755906) | ⬜ |
| Copper-phosphorus brazing rod, Ø 2 mm | 50 cm | Radiator and radials: stiff, no insulation | [Castorama](https://www.castorama.fr/baguettes-de-brasage-castolin-l-500mm-2-mm-10-pieces/3509350002704_CAFR.prd) | ⬜ |
| *Alternative:* solid copper wire 2.5 or 4 mm² (conductor Ø ≈ 1.8 or 2.3 mm) | 50 cm | Same, if brazing rod is not available | [Amazon](https://www.amazon.fr/dp/B00SGA82CY) | ⬜ |
| SMA male → u.FL pigtail, 15 cm | 1–2 | Antenna (or LNA) to board. u.FL tolerates few mating cycles, so connect once and switch at the SMA end | [WiMo 40350-SM-15](https://www.wimo.com/fr/40350-sm-15) | ⬜ |
| M3 × 10 mm screws + nuts | 4 + 4 | Radials to flange | [screws](https://www.amazon.fr/dp/B0DHPKJQR7) · [nuts](https://www.amazon.fr/dp/B0DCXVB3ZT) | ⬜ |
| Cable lugs | 4 | Radial ends to screws | [Amazon](https://www.amazon.fr/dp/B07Y1WXNR2) | ⬜ |
| M3 × 16 mm screws | 4 | 3D-printed jig and mast adapter | [Amazon](https://www.amazon.fr/dp/B0DHPJJ3KG) | ⬜ |
| 3D-printed jig + mast adapter | 1 | Holds radials at exactly 45° for a repeatable build | printed at FabLab | ⬜ |
| Vector network analyser (VNA) | 1 | Tunes the radiator to resonance and gives the SWR curve; without it the tuning is an estimate | lab equipment | ❓ lab |

## Upgrade

| Part | Qty | Purpose | Link | Status |
|---|---|---|---|---|
| SAW filter + LNA, 868 MHz | 1 | ≈ 4–5 dB better sensitivity; experiment A. **The strongest single upgrade:** the 5–10 packets/day reported at 868 MHz were measured with a preamplifier | [Kubii](https://www.kubii.com/fr/modules-de-reseaux/3349-1311-modules-amplificateurs-lna-avec-filtre-saw-3272496306646.html) (47.64 €, 08/10/2026) | ❓ |
| SMA male – SMA male cable | 1 | Antenna to LNA inside the enclosure | [WiMo 40310-SMSM15](https://www.wimo.com/fr/40310-smsm15) | ❓ |
| Power bank, 10 000 mAh, 2 × USB-C, always-on / low-current mode | 1 | Outdoor power for board + LNA (≈ 2 days) | — | ❓ |
| PVC tube Ø 40 mm + mount | 1 m | Lifts the antenna above surrounding obstacles on a roof site | [Leroy Merlin](https://www.leroymerlin.fr/produits/tube-pvc-evacuation-diam-40-62256285.html) | ❓ |
| 3D-printed enclosure (board only, or board + LNA) | 1 | Weather protection for an outdoor site | printed at FabLab | ❓ |

Status: ✅ in hand · ⬜ to source · ❓ availability being checked
