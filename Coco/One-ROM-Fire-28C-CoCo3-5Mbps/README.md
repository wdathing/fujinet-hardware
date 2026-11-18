# One ROM Fire 28C FujiNet, CoCo 3 at 5 Mbps (unofficial build)

These are community builds of Bill Athing's (wdathing) One ROM FujiNet setup, made from his
public source on 2026-11-18. They were tested on a CoCo 3: it boots HDB-DOS to the FujiNet
Config screen, joins Wi-Fi, and mounts TNFS disk images. The September binaries in
`../One-ROM-Fire-28C` hang on a CoCo 3 because they come before DriveWire plugin 1.2.0.

## What's here (`binaries/`)

| File | What it is |
|---|---|
| `onerom-fire-28-c-dw130-full.bin` | The complete One ROM image (firmware + USB plugin 0.3.1 + DriveWire 1.3.0 + HDB-DOS). This is all you need for the One ROM. |
| `onerom-rp235x.bin` | One ROM base firmware v0.7.3 with Bill's PIO address-gap fix (wdathing/one-rom `177f453`) |
| `drivewire_plugin.bin` | DriveWire plugin 1.3.0 at 5 Mbps (wdathing/one-rom `177f453`) |
| `hdbonerom.rom` | HDB-DOS 1.5 ONEROM (wdathing/toolshed_n6il `onerom-hdb` @ `454855e`), `-DDW -DLevel=1 -DONEROM` |
| `fujinet-COCO-HS-UART-v1.6.2-dev-5mbps.zip` | ESP32 firmware at 5 Mbps: wdathing/dragon `centipede-onerom-5mbps-baud-v2` (`58ef802`), plus cherry-picks `e3cf6c2` (SET_SSID aux1 fix, without which Wi-Fi setup sends an empty SSID) and `0a8955e` (ROM auto-push). Built from env `fujinet-coco-devkitc-centipede` |

The two sides must match. The One ROM plugin 1.3.0 talks at 5 Mbps, so the ESP32 needs this
5 Mbps firmware, not the 921600 one.

## Flashing

One ROM (out of the CoCo, on USB):

```bash
onerom program --firmware onerom-fire-28-c-dw130-full.bin --verify
```

ESP32 DevKitC (unzip first):

```bash
esptool.py --chip esp32 --port <port> --baud 460800 write_flash -z \
  0x1000 bootloader.bin 0x8000 partitions.bin 0x10000 firmware.bin \
  0x510000 update.bin 0x570000 littlefs.bin
```

Wiring is unchanged from Bill's photo: GPIO40 (UART1 TX) goes to J1 TX, which is ESP32 GPIO33 (RX);
GPIO41 (UART1 RX) goes to J1 RX, which is ESP32 GPIO21 (TX); and GND goes to GND.

All credit goes to Bill Athing. This is just a convenience build until his official binaries are updated.
