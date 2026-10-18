# 變更紀錄

English: [CHANGELOG.en.md](CHANGELOG.en.md)

這個庫值得注意的變更都記在這裡，最新的在最上面。格式照 [Keep a Changelog 1.1.0](https://keepachangelog.com/en/1.1.0/)；版號照 [Semantic Versioning 2.0.0](https://semver.org/spec/v2.0.0.html)，1.0.0 之前照 [Cargo 的 0.y.z 慣例](https://doc.rust-lang.org/1.90.0/cargo/reference/semver.html)：不相容的改動遞增中間那一位，其他改動遞增最後一位。這個庫的版號和 HAL 庫的版號各自獨立；每個範例搭配的 HAL 庫版本寫在範例的 README。

## [Unreleased]

### 新增（Added）

- 第一個範例 `examples/bmi160-bmp280-qmc5883l/`：GY-BMI160 接 SPI 匯流排 1，GY-BMP280 與 GY-271（QMC5883L）接 I²C 匯流排 1，搭配 HAL 庫 `v0.1.1` 與板型 `hub8735ultra`。內容有 hwdef 疊加檔、四份參數檔（只用 Wi-Fi 控制時的失效保護、室內沒有 GPS 與接收機、四軸 X 型配一般 PWM 的電調、羅盤），以及接線、建置與上板驗證的說明。
- README 與 CONTRIBUTING（中英兩版）、三種授權的全文。
