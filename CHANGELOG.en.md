# Changelog

繁體中文：[CHANGELOG.md](CHANGELOG.md)

All notable changes to this repository are recorded here, newest first. The format follows [Keep a Changelog 1.1.0](https://keepachangelog.com/en/1.1.0/); versions follow [Semantic Versioning 2.0.0](https://semver.org/spec/v2.0.0.html), and before 1.0.0 the [Cargo 0.y.z convention](https://doc.rust-lang.org/1.90.0/cargo/reference/semver.html): incompatible changes increase the middle number, all other changes the last one. This repository's version numbers are independent of the HAL repository's; the HAL repository version each example needs is in that example's README.

## [Unreleased]

## [0.1.0] - 2026-10-19

### Added

- The first example, `examples/bmi160-bmp280-qmc5883l/`: a GY-BMI160 on SPI bus 1, and a GY-BMP280 and GY-271 (QMC5883L) on I²C bus 1, for HAL repository `v0.1.1` and board type `hub8735ultra`. It has the hwdef overlay, four parameter files (failsafes for control over Wi-Fi only, indoors without GPS or receiver, a quad X frame with ESCs on normal PWM, the compass), and notes on wiring, building and the board test.
- The second example, `examples/expansion-board-handbuilt/`: the author's own hand-built expansion board, with the same sensor set as the first example plus headers for battery voltage, four motors, ESC current and telemetry, a GPS, an RC receiver and external I²C (a second bus), for HAL repository `v0.1.1` and board type `hub8735ultra`. It has two overlays (normal and ESC), seven parameter files, the perfboard wiring table and the author's build figures (one of the parts side, four of the solder side, plus the block diagram and device hookup of the v1 PCB), and the HAL version each file needs and the board test status. Battery voltage and the ESC version need a later HAL release.
- A figure of the 32 header pins of the HUB 8735 ultra (`docs/pinout.svg`, in Chinese and English): the D number and use of each pin, with the five pins not to connect marked; the first example's wiring diagram marks the same five.
- README and CONTRIBUTING (in Chinese and English) and the full texts of the three licenses.

[Unreleased]: https://github.com/rainwu0/ArduPilot-Hub8735Ultra/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/rainwu0/ArduPilot-Hub8735Ultra/tree/v0.1.0
