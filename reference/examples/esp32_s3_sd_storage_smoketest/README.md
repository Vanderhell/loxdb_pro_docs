# ESP32-S3 SD Storage Smoketest

Arduino firmware harness for real-hardware validation of SD storage behavior.

This example is intentionally independent from host-side CMake tests and is
meant for an ESP32-S3 board with an SD card connected over SD_MMC (1-bit).

Current default in the sketch is `SD_MMC` in 1-bit mode.
The sketch also supports live progress on ST7735 LCD (if display libraries are installed).
LCD is enabled by default (`SDTEST_LCD_ENABLE=1`) and compile will fail if LCD libraries are missing.

## What It Tests

- SD mount + card/filesystem info
- Sequential write/read verification
- Random read spot checks
- `flush()` persistence marker for reboot/power-cycle checks
- Human-readable run report in `/loxdb_sd_report.txt`

## Files On SD

- `/loxdb_sd_test.bin` - generated test payload
- `/loxdb_sd_persist.bin` - monotonic marker (`uint32_t`)
- `/loxdb_sd_report.txt` - last run report

## Pin Configuration

Default `SD_MMC` 1-bit pin macros in the `.ino`:

- `SDMMC_PIN_CLK` (default `17`)
- `SDMMC_PIN_CMD` (default `18`)
- `SDMMC_PIN_D0` (default `16`)
- `SDMMC_PIN_D3` (default `47`)
- `SDMMC_FORMAT_IF_MOUNT_FAILED` (default `false`)

This matches common ESPHome configuration:

- `clk_pin: gpio17`
- `cmd_pin: gpio18`
- `data0_pin: gpio16`
- `data3_pin: gpio47`

If your dongle uses different pins, edit these defines before upload.

If you need SPI mode instead, set `#define SD_USE_MMC_1BIT 0` and use the SPI
pin defines in the sketch.

## Build/Flash

1. Open `esp32_s3_sd_storage_smoketest.ino` in Arduino IDE.
2. Select an ESP32-S3 board profile.
3. Ensure board package includes `SD` + `SD_MMC` + `SPI`.
4. For LCD output, install libraries:
   - `Adafruit GFX Library`
   - `Adafruit ST7735 and ST7789 Library`
5. Upload and open serial monitor at `115200`.

## LCD Live Progress

Default LCD pins in sketch:

- `LCD_PIN_SCLK=10`
- `LCD_PIN_MOSI=11`
- `LCD_PIN_CS=12`
- `LCD_PIN_DC=13`
- `LCD_PIN_RST=14`
- `LCD_PIN_BL=-1` (set to your backlight GPIO if your board requires it)

During tests, LCD shows phase and progress bar:

- mount status
- write progress
- sequential verify progress
- random verify progress
- final `PASS/FAIL`

## Runtime Commands

- `run` - full test (`256 KiB`)
- `quick` - quick test (`64 KiB`)
- `powermark` - only persist-marker write+flush
- `status` - print marker/file status
- `wipe` - delete test artifacts from SD
- `reboot` - software restart

## Power-Cycle Validation Flow

1. Run `powermark`.
2. Physically power-cycle or hard-reset board.
3. Run `status`.
4. Confirm marker increased and persisted.

If marker is not persisted, storage `sync/flush` path is not reliable on your
board/card stack and should be investigated before production use.

## Verified Run Snapshot

Measured on `2026-04-27` with ESP32-S3 dongle over `COM19`:

- disclaimer: results come from the hardware unit available during testing
  (reference sample). Your board revision/listing may use different routing.
- always verify pin mapping against your board schematic/vendor documentation.
- SD mode: `SD_MMC` 1-bit (`CLK=17`, `CMD=18`, `D0=16`, `D3=47`)
- `quick` (`64 KiB`):
  - write: `~209-244 ms`
  - sequential verify: `~123-124 ms`
  - random verify (`256 checks`): `~1025-1037 ms`
  - result: `PASS`
- `run` (`256 KiB`):
  - write: `~530-837 ms`
  - sequential verify: `~445-446 ms`
  - random verify (`256 checks`): `~1137-1139 ms`
  - result: `PASS`
- reboot-stress and marker persistence:
  - repeated `powermark -> reboot -> status` cycles
  - marker advanced monotonically (`25 -> 33`)
