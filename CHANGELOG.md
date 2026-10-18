# 變更紀錄

English: [CHANGELOG.en.md](CHANGELOG.en.md)

這個庫值得注意的變更都記在這裡，最新的在最上面。格式照 [Keep a Changelog 1.1.0](https://keepachangelog.com/en/1.1.0/)；版號照 [Semantic Versioning 2.0.0](https://semver.org/spec/v2.0.0.html)，1.0.0 之前照 [Cargo 的 0.y.z 慣例](https://doc.rust-lang.org/1.90.0/cargo/reference/semver.html)：不相容的改動遞增中間那一位，其他改動遞增最後一位。這個庫的版號和 HAL 庫的版號各自獨立；每個範例搭配的 HAL 庫版本寫在範例的 README。

## [Unreleased]

## [0.1.0] - 2026-10-19

### 新增（Added）

- 第一個範例 `examples/bmi160-bmp280-qmc5883l/`：GY-BMI160 接 SPI 匯流排 1，GY-BMP280 與 GY-271（QMC5883L）接 I²C 匯流排 1，搭配 HAL 庫 `v0.1.1` 與板型 `hub8735ultra`。內容有 hwdef 疊加檔、四份參數檔（只用 Wi-Fi 控制時的失效保護、室內沒有 GPS 與接收機、四軸 X 型配一般 PWM 的電調、羅盤），以及接線、建置與上板驗證的說明。
- 第二個範例 `examples/expansion-board-handbuilt/`：作者自己手焊的擴充板，感測器和第一個範例同一組，另有電池電壓、四路馬達、電調電流與回傳、GPS、遙控接收機與外接 I²C（第二條匯流排）的排針，搭配 HAL 庫 `v0.1.1` 與板型 `hub8735ultra`。內容有兩份疊加檔（一般版與電調版）、七份參數檔、洞洞板的接線表與作者的建置圖（零件面一張、焊接面四層，加上 v1 PCB 版的方塊圖與外接裝置接法），以及每個檔案要的 HAL 版本與上板驗證的狀態。電池電壓與電調版要之後的 HAL 版本。
- HUB 8735 ultra 的 32 腳排針圖（`docs/pinout.svg`，中英兩版）：每支腳的 D 編號與用途，標出不可接的五支；第一個範例的接線圖也標出這五支。
- README 與 CONTRIBUTING（中英兩版）、三種授權的全文。

[Unreleased]: https://github.com/rainwu0/ArduPilot-Hub8735Ultra/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/rainwu0/ArduPilot-Hub8735Ultra/tree/v0.1.0
