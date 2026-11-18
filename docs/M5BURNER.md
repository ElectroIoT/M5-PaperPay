# Publishing to M5Burner

How to share this firmware in [M5Burner](https://docs.m5stack.com/en/uiflow/m5burner/publish)
(*USER CUSTOM > Publish*, logged in with an M5Stack community account).

> **Do not use M5Burner's "Export" on your own M5Paper.** Export reads the whole flash, including
> your Wi-Fi password, UPI ID, Telegram token and payment log. Upload the clean `-full.bin` from the
> Releases page instead: it is built from source and its settings area (NVS, `0x9000`) is blank.

## Fields

| Field | Value |
|---|---|
| Name | `M5PaperPay` |
| Version | `v1.0.0` |
| Device Type | `PAPER` (M5Stack M5Paper) |
| Github | `https://github.com/ElectroIoT/M5-PaperPay` |
| FirmWare | `M5-PaperPay-v1.0.0-full.bin` (single 16 MB image, flashed at `0x0`, includes the web dashboard) |
| Cover | [`docs/images/m5burner_cover.png`](images/m5burner_cover.png) (800 x 600) |

## Description (paste as is)

```text
A touchscreen UPI payment counter for the M5Stack M5Paper.

- On-device calculator (+ - x / =): tap the amount, press CHARGE
- Big UPI QR code on the e-ink screen for the customer to scan with any UPI app
- MARK PAID / CASH PAID / CANCEL, payment log with today's sales
- Date, 12-hour time (RTC) and battery level in the header
- Phone/PC dashboard over Wi-Fi (calculator, payments, settings)
- Optional Telegram bot: bill and paid alerts, forward the bank "credited" SMS to auto-mark paid
- No cloud, no app store

First setup: join the "M5PaperPay-Setup" Wi-Fi on your phone, enter your home Wi-Fi and your UPI ID
(VPA), shop name and payee name. Then open http://m5paperpay.local.

Works with the M5Stack M5Paper only. Flashing erases saved settings.
Guide, source and updates that keep settings: see the GitHub link.
```

## Version description (markdown, paste as is)

```markdown
## v1.0.0 - first release

- On-device calculator and big UPI QR on the M5Paper touchscreen
- MARK PAID / CASH PAID / CANCEL, payment log, daily totals
- RTC date + 12-hour time, battery % with low-battery indicator
- Wi-Fi web dashboard (http://m5paperpay.local) and optional Telegram bot with SMS auto-confirm
- Captive-portal first-time setup (Wi-Fi + UPI ID). For **M5Stack M5Paper** only

Source and guide: https://github.com/ElectroIoT/M5-PaperPay
```

## Making the image (maintainers)

```bash
pio run && pio run -t buildfs
python ~/.platformio/packages/tool-esptoolpy/esptool.py --chip esp32 merge_bin \
  -o release/M5-PaperPay-vX.Y.Z-full.bin --flash_mode keep --flash_freq keep --flash_size 16MB \
  0x1000 .pio/build/m5paper/bootloader.bin \
  0x8000 .pio/build/m5paper/partitions.bin \
  0xe000 ~/.platformio/packages/framework-arduinoespressif32/tools/partitions/boot_app0.bin \
  0x10000 .pio/build/m5paper/firmware.bin \
  0xc90000 .pio/build/m5paper/littlefs.bin
```

`littlefs.bin` is the web dashboard (`data/`); without it the dashboard page would be missing.
Before uploading, check the image holds no personal data: `grep -c -a "<your Wi-Fi name>" release/*.bin`
must print `0`, and the bytes at `0x9000-0xDFFF` must all be `0xFF`.
