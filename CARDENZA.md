# Cardenza target

Build with `pio run -e cardenza`. This explicit target requires the current
Cardenza PCB and checks ES8156 identity before initializing it. Keep the original
upstream device target for its hardware. The shared HAL files retain their MIT
license; this does not replace the application's license.

Hardware: ES8156 address0x08 on SDA2/SCL1; Philips16-bit stereo32 clocks/frame;
I2S BCLK41/LRCK43/DATA42. Original Cardputer PDM microphone DATA46/CLK43 is
fitted. Keyboard LED_EN21 is held high (off). Display backlight is GPIO38.
Gyro, battery ADC, charging detection and onboard WS2812 are absent.
The future LED_EN47 PCB revision needs a separate pin-policy update.

The target uses QIO80MHz, 8MB flash and no PSRAM. Install application-only
`firmware.bin` through Cardenza Launcher with its existing Cardenza bootloader;
do not replace the launcher partitions with a stock merged image. Do not erase
shared NVS. The target does not use M5Unified; no M5 power initialization runs.

A native GPIO matrix scan replaces ADV TCA8418 keyboard. ES8156 replaces ES8311 playback; separate PDM microphone replaces ES8311 ADC. ES8311 PGA/ALC/DRC hardware effects are unavailable and report N/A. Volume is capped at codec0dB. Sample buffers use internal RAM. No M5Unified dependency or wrapper is used.


## Validation

The Cardenza target is built separately from the upstream targets. Compilation
and source review do not prove the physical display, keyboard, audio, microphone,
or SD-card behavior; verify these on the target hardware before a release.

The Cardenza build rejects full DATA/NVS partition erases, including Arduino
startup recovery that would clear the Launcher's shared settings. Normal NVS
writes and erases of other explicitly selected partitions are unaffected. An
NVS recovery error requires deliberate repair rather than automatic deletion.

## Runtime M5Unified migration exception (2026-10-04)

Cardputer-Studio uses independent native hardware/audio drivers rather than M5Unified. Its existing Cardenza target remains unchanged; runtime migration is deferred to a separate native-HAL phase.
