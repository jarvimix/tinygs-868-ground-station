# Low-Cost 868 MHz Ground Station for Direct-to-Satellite IoT

A low-cost ground station that receives LoRa packets from satellites in low Earth orbit, built on the open-source [TinyGS](https://tinygs.com) network. Research project (REP, 10 ECTS) at INSA Lyon, Telecommunications department, autumn 2026.

The project builds the station, designs and builds its antenna, and runs a measurement campaign: what reception data (RSSI, SNR, frequency error, pass geometry) shows about LoRa on a satellite link.

> **Status (10/2026):** work in progress. The station is online in the TinyGS network with the stock antenna. The self-built ground plane antenna is in design. This README is preliminary and is updated as the project advances.

## Why 868 MHz

TinyGS recommends 433 MHz for new stations because most LoRa satellites transmit there. This project uses **868 MHz on purpose**: it is the European LoRaWAN band, and the topic is Direct-to-Satellite IoT, where devices connect to a satellite without a terrestrial gateway.

What that means in practice:

- **Few satellites.** On 10/2026, TinyGS lists 17 satellites in the 868–915 MHz range: ConnectaIoT-1…16 and KOSAR 1.5 868.
- **Few packets.** Stations in the TinyGS community report about 5–10 packets per day at 868 MHz with a preamplifier.
- **Encrypted payloads.** ConnectaIoT satellites send encrypted LoRaWAN frames. The content cannot be decoded, but RSSI, SNR, frequency error and timestamp are still recorded. Those are the quantities the measurement campaign needs.

The 868 MHz board cannot hear 433 MHz satellites with any setting: its RF front end is tuned to the band.

## Hardware

Signal chain:

```
[¼λ ground plane antenna, 869.5 MHz]
        │  SMA
[SAW filter + LNA]            optional upgrade
        │  SMA → u.FL pigtail, 15 cm
[Heltec WiFi LoRa 32 V3]      ESP32-S3 + SX1262
        │  Wi-Fi → MQTT
[TinyGS network]
```

Design principle: **keep the board next to the antenna.** The RF path stays under 0.5 m, so cable loss stays under about 0.5 dB and no expensive low-loss coax is needed. Power travels the long distance, not the radio signal.

Full parts list: [`hardware/BOM.md`](hardware/BOM.md).

## Setup (preliminary)

Tested with Heltec WiFi LoRa 32 V3 (868 MHz) on macOS and Chrome.

1. **Connect the antenna before powering the board.** Receiving without an antenna is harmless, but transmitting without one can damage the radio.
2. **Flash TinyGS** from [installer.tinygs.com](https://installer.tinygs.com) in Chrome: *Automatic Setup* with *Erase device*.
   - If you see *"Failed to open serial port"*, close other TinyGS tabs and reconnect the USB cable.
3. **Connect the board to Wi-Fi** through its configuration portal.
4. **Register the station:** get a one-time password from the board's OLED, log in through the TinyGS Telegram bot (`/weblogin`) and create a *New Station* on the dashboard.
5. **Set the board type** on the station's own page. If the console shows *"Radio Status: Error: null"* or *"Band: null"*, the board type is missing.
6. **Select the band:** *OPERATE → Auto Tune → 868-915*. The default is 433 MHz, which an 868 MHz board cannot receive.
   - *Antenna range* under *EDIT STATION* does **not** set the tuned band. Only Auto Tune does.
7. **Keep transmission disabled.** The optional LNA is receive-only and can be damaged by transmitting through it.

**Doppler compensation** (*OPERATE → Enable Doppler Compensation by TLE*): the firmware tunes the radio to nominal frequency + offset + Doppler computed from the TLE, and updates the correction when the error exceeds 1200 Hz (`Radio.cpp`). With compensation on, the reported *frequency error* is the **residual** error, not the full Doppler shift. Take this into account in any Doppler analysis.

## Antenna: ¼λ ground plane

| | |
|---|---|
| Design frequency | 869.5 MHz (ConnectaIoT downlink ≈ 869.525 MHz) |
| Wavelength λ | 34.5 cm |
| Radiator | λ/4 = 8.6 cm; cut to 9 cm and trimmed to resonance (≈ 8.2 cm expected) |
| Radials | 4 × ≈ 9 cm, bent 45° downwards |
| Feed impedance | ≈ 50 Ω without a matching network (45° radials raise ≈ 36 Ω towards 50 Ω) |
| Pattern | Omnidirectional in azimuth, strongest at low elevation, null at zenith; linear vertical polarisation |

**Trim long, then shorten while measuring.** Each millimetre removed raises resonance by roughly 10 MHz. Shorten 0.5–1 mm at a time and measure the SWR between cuts with a VNA. Length cannot be added back.

A 3D-printed jig holds the radials at exactly 45° so the build is repeatable.

Why a ground plane rather than a circularly polarised antenna: at 5–10 packets per day, time on air matters more than pattern optimisation. A ground plane can be built in one session, needs no matching network, and its pattern is simple to model (NEC), so RSSI can be corrected for antenna gain as a function of elevation.

## Measurement plan

Planned experiments:

- **A. LNA on vs. bypassed**: same antenna, same site, alternating.
- **B. Stock antenna vs. ground plane**: same site, the stock helical antenna gives the baseline.

Data handling:

- Each received packet is logged locally from the station's MQTT feed, in addition to the TinyGS server.
- The TLE valid at reception time is archived daily. TLEs go stale within days, and elevation angles are reconstructed afterwards from timestamp, satellite ID, station position and TLE.
- Raw data is write-once. All cleaning happens in scripts that write to `data/derived/`.

## Repository structure

```
hardware/      parts list (BOM); antenna jig and enclosure models later
data/raw/      received packets as logged, never edited
data/derived/  cleaned tables joined with pass geometry
data/tle/      orbital elements used, by date
```

## Acknowledgements

Built on [TinyGS](https://github.com/G4lile0/tinyGS) and its community. Research project at INSA Lyon, supervised by Carlos Fernandez Hernandez.
