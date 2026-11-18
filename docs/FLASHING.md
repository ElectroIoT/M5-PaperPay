# Flashing & installing

Target: **M5Stack M5Paper** (ESP32, 16 MB flash). Not for other M5 devices.

| File (Releases page) | Flash at | Use it for |
|---|---|---|
| `M5-PaperPay-v1.0.0-full.bin` | `0x0` | **first install** / M5Burner - everything in one 16 MB file. Erases saved settings. |
| `M5-PaperPay-v1.0.0-app.bin` | `0x10000` | **update the app only** - keeps Wi-Fi, UPI ID and payment log |
| `M5-PaperPay-v1.0.0-web.bin` | `0xc90000` | update the web dashboard only |
| `SHA256SUMS.txt` | - | checksums |

## Option 1 - in the browser

Open the **[web installer](https://electroiot.github.io/M5-PaperPay/flasher/)** in Chrome or Edge,
connect the M5Paper (switched on), press **Install**, pick the CH9102 port. Takes about 3 minutes.

## Option 2 - M5Burner

Search "M5PaperPay" in M5Burner (device type *PAPER*), download and burn.
(Maintainers: [M5BURNER.md](M5BURNER.md).)

## Option 3 - esptool

```bash
pip install esptool
esptool.py --chip esp32 --port COM17 --baud 460800 write_flash 0x0 M5-PaperPay-v1.0.0-full.bin
```

Update the app and keep settings:

```bash
esptool.py --chip esp32 --port COM17 --baud 460800 write_flash 0x10000 M5-PaperPay-v1.0.0-app.bin
```

## Option 4 - PlatformIO

```bash
git clone https://github.com/ElectroIoT/M5-PaperPay
cd M5-PaperPay
pio run -t upload        # firmware
pio run -t uploadfs      # web dashboard (data/)
```

The first build downloads the libraries and can take 15-20 minutes.

## First start

1. Join the **M5PaperPay-Setup** Wi-Fi from your phone, enter your Wi-Fi, UPI ID, payee and shop name.
2. Open `http://m5paperpay.local` (or the IP shown on the screen) for the dashboard and settings.
3. Optional: add a Telegram bot token and chat id in Settings.
