# Changelog

Changes in this fork relative to [gongloo/TigoTell](https://github.com/gongloo/TigoTell) 0.2.10. Fork versions add a fourth number to the upstream version they are based on.

## 0.2.10.11

- Node-table module IDs carry bit 0x8000 while power reports do not; the bit is now stripped so barcodes attach to the right modules instead of creating duplicate entries.
- The module-ID-to-barcode map is stored in NVS and restored at boot, because the CCA sends its node table only occasionally.
- Barcodes are shown in Tigo's label format (`4-EF82C4J`) on power cards, in the registry, layout editor, MQTT and Home Assistant; `/json` also lists the raw `long_address`.
- Releases and update checks use the repository the firmware was built from (set by the release workflow); releases include `firmware.elf` for crash analysis.

## 0.2.10.10

- Device health: reset reason, previous run's uptime and lowest memory (kept in RTC memory), WiFi drops and reasons, crash reports from the core dump (kept in NVS). Shown on the dashboard and in `/json` under `system`, with a notice after unexpected restarts.
- Self-healing: loop watchdog; WiFi reconnect after 1 minute and restart after 10 minutes without WiFi (not while the setup portal is in use); restart when the largest free memory block stays below 10 KB.
- Fix: logging from several tasks into WebSerial could overrun its buffers and corrupt memory. Only the main loop writes to WebSerial now; other tasks queue their lines.
- Fix: the MQTT task no longer holds its queue lock during network I/O.
- `tools/decode-crash.py` maps crash backtraces to function names.

## 0.2.10.9

- TAP status from passive bus observation: polls answered (recent and total), receive/transmit buffers, slot counter, modules heard and their signal, TAP barcode, firmware version, radio channel and PAN ID. Dashboard section, `/json` `taps`, MQTT `<base>/tap/<id>` and Home Assistant devices.
- Power reports name the TAP they came through (`gateway`), shown as "Via TAP" in the layout.

## 0.2.10.8

- Layout storage checks free NVS space before writing and keeps a reserve for WiFi and settings; saves are read back to confirm them.
- The layout page never falls back to an empty layout when loading fails, so it cannot overwrite the stored one.
- Dashboard and layout page poll one request at a time with a timeout, so a slow device no longer exhausts the browser's connections.
- The MQTT queue is bounded in bytes as well as messages.
- `/json` reports free heap, largest block and free NVS entries.

## 0.2.10.7

- Layout: strings card starts collapsed and the layout is centred on desktop; compact top area on phones.

## 0.2.10.6

- Layout: tapping any panel shows its link status (live, stale, not seen, not linked), string and readings; touch taps are more tolerant of finger movement.

## 0.2.10.5

- Layout: strings with colors, assignment by tapping or swiping across panels, per-string power/voltage/reporting count, "color by string", string placeholders.

## 0.2.10.4

- Layout: rotatable text, simulation mode, fixed export/import icons.

## 0.2.10.3

- System layout designer and live view (`/layout.html`), stored in NVS with export/import.
- `/update` explains how to enable the upload page when no OTA credentials are set; the settings page shows whether it is active.

## 0.2.10.2

- Home Assistant MQTT discovery for TigoTell and every module.

## 0.2.10.1

- MQTT publishing of module data and system stats, configured in the web UI, running in its own task so a slow broker cannot stall RS485 reception.
