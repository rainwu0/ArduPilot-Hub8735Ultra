# Changelog

繁體中文：[CHANGELOG.md](CHANGELOG.md)

All notable changes to this repository are recorded here, newest first. The format follows [Keep a Changelog 1.1.0](https://keepachangelog.com/en/1.1.0/); versions follow [Semantic Versioning 2.0.0](https://semver.org/spec/v2.0.0.html), and before 1.0.0 the [Cargo 0.y.z convention](https://doc.rust-lang.org/1.90.0/cargo/reference/semver.html): incompatible changes increase the middle number, all other changes the last one. This repository's version numbers are independent of the HAL repository's; the HAL repository version each example needs is in that example's README.

## [Unreleased]

### Added

- The first example, `examples/bmi160-bmp280-qmc5883l/`: a GY-BMI160 on SPI bus 1, and a GY-BMP280 and GY-271 (QMC5883L) on I²C bus 1, for HAL repository `v0.1.1` and board type `hub8735ultra`. It has the hwdef overlay, four parameter files (failsafes for control over Wi-Fi only, indoors without GPS or receiver, a quad X frame with ESCs on normal PWM, the compass), and notes on wiring, building and the board test.
- README and CONTRIBUTING (in Chinese and English) and the full texts of the three licenses.
