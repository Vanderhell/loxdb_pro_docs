# loxdb_pro – ESP32-S3 SD Stress Bench (Notes)

Date: 2026-05-06

## What was integrated

The SD stress bench sketch has been imported into this repository:

- `examples/esp32_sd_stress_bench/esp32_sd_stress_bench.ino`

It is based on the reference bench provided in:

- `C:\Users\vande\Desktop\loxdb\bench\loxdb_esp32_s3_sd_stress_bench`

## Hardware pinout (reference)

### SD_MMC (1-bit)

- `CLK=GPIO17`
- `CMD=GPIO18`
- `D0=GPIO16`
- `D3=GPIO47`

### ST7735 LCD

- `SCLK=GPIO10`
- `MOSI=GPIO11`
- `CS=GPIO12`
- `DC=GPIO13`
- `RST=GPIO14`
- `BL=-1` (set to GPIO if your board requires explicit backlight control)

## Build / upload

Example (ESP32-S3 Dev Module):

```powershell
arduino-cli compile --fqbn esp32:esp32:esp32s3 .\examples\esp32_sd_stress_bench
arduino-cli upload  -p COM19 --fqbn esp32:esp32:esp32s3 .\examples\esp32_sd_stress_bench
```

## Important: Serial port selection (ESP32-S3 native USB)

When ESP32-S3 is built with **USB CDC on boot** (Arduino menu option `CDCOnBoot=cdc`),
the runtime log console (`Serial`) can appear on a *different* USB interface than the one used by the uploader (`USB-Serial/JTAG`).

If you see upload working on `COM19` but Serial Monitor shows no sketch output:

1. Open Windows Device Manager → **Ports (COM & LPT)**
2. Identify the ESP32-S3 entries (usually `USB Serial Device (COMxx)`; often there are two)
3. Try Serial Monitor on the *other* ESP32-S3 COM port at `115200`

If you want the sketch to always log on the same port used for flashing, disable CDC-on-boot in the Arduino board menu
or force the sketch to use UART0 (`Serial0`) instead of `Serial`.

## LCD disable switch (recommended for first SD bring-up)

In this repo the LCD is disabled by default:

- `SDSTRESS_LCD_ENABLE=0`

This simplifies first validation (SD init and serial command loop) before enabling display rendering.

