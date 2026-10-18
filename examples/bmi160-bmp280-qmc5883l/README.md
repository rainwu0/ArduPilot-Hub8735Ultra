# 範例：BMI160 + BMP280 + QMC5883L

English: [README.en.md](README.en.md)

三片常見的感測器模組，接在 HUB 8735 ultra 上當飛控的 IMU、氣壓計和羅盤。範例開發板把模組焊在洞洞板上，開發板插在上面。

**這個範例的檔案還沒在公開的 HAL 庫標籤上做完上板驗證，也還沒飛過。** 狀態見〈上板驗證〉；動手之前先讀本庫首頁的[〈飛行安全須知〉](../../README.md#飛行安全須知)。

- HAL 庫標籤：`v0.1.1`
- 板型：`hub8735ultra`

## 感測器組合

| 感測器 | 模組 | 介面 | 位址或 CS | 疊加檔裡的方向 |
|---|---|---|---|---|
| IMU | GY-BMI160 | SPI 匯流排 1 | CS 接 D21 | `ROTATION_ROLL_180_YAW_270` |
| 氣壓計 | GY-BMP280 | I²C 匯流排 1 | 0x76 | — |
| 羅盤 | GY-271（QMC5883L） | I²C 匯流排 1，外接 | 0x0D | `ROTATION_NONE` |

方向對應範例開發板的擺法：開發板元件面朝上，前方是 microSD 那一端（USB-C 的另一端）。擺法不同時要重測，方法見〈上板驗證〉的重力測試與轉向測試。

## 接線

排針的位置依板商公開的 Gerber 與元件位置圖（ideashatch/HUB-8735 `870a7e08de7a` 的 `HUB 8735 ultra toolkit/Hardware/HUB 8735 ultra PCB Gerber File.7z`、`HUB 8735 ultra toolkit/Hardware/HUB 8735 ultra_PCB Placement_Top.pdf`）：從元件面看、USB-C 朝下，左右各一排，每排分上下兩組、每組 8 腳。左排上組是 JA1、下組是 JA7，右排上組是 JA2、下組是 JA8；每組的第 1 腳是方形焊墊，在靠 microSD 的那一端，腳號往 USB-C 端增加。完整的排針對照（每支腳是哪個 D 編號、電源與地）見板商公開的[原理圖](https://github.com/ideashatch/HUB-8735/blob/870a7e08de7a/HUB%208735%20ultra%20toolkit/Hardware/HUB%208735%20ultra%20Schematic.pdf)與[模組規格書](https://github.com/ideashatch/HUB-8735/blob/870a7e08de7a/HUB%208735%20ultra%20toolkit/Hardware/HUB%208735%20ultra%20Module%20Specification_V01_20240508.pdf)；這兩份標有 CONFIDENTIAL，這裡不摘錄內容。

下表是範例開發板的接法。照這樣接，HAL 庫 `v0.1.0` 的建置偵測得到三個感測器（HAL 庫 [BOARD_TESTS.md 的 T2](https://github.com/rainwu0/ArduPilot-RTL8735B/blob/v0.1.0/libraries/AP_HAL_RTL8735B/BOARD_TESTS.md)）。

| 模組 | 模組的腳 | 接到 | 排針位置 |
|---|---|---|---|
| GY-BMI160 | 3V3 | 3.3 V | JA2.1 |
| GY-BMI160 | GND | GND | JA2.4 |
| GY-BMI160 | SCL | D19（SPI 時脈） | JA8.2 |
| GY-BMI160 | SDA | D20（SPI MOSI） | JA8.3 |
| GY-BMI160 | SAO | D18（SPI MISO） | JA8.1 |
| GY-BMI160 | CS | D21（SPI CS） | JA8.4 |
| GY-BMI160 | VIN、OCS、INT1、INT2、SCX、SDX | 不接 | — |
| GY-BMP280 | VCC | 3.3 V | JA2.1 |
| GY-BMP280 | GND | GND | JA2.4 |
| GY-BMP280 | SCL | D1（I²C SCL） | JA1.4 |
| GY-BMP280 | SDA | D0（I²C SDA） | JA1.3 |
| GY-BMP280 | CSB | 3.3 V（選 I²C） | JA2.1 |
| GY-BMP280 | SDO | GND（位址 0x76） | JA2.4 |
| GY-271 | VCC | 3.3 V | JA2.1 |
| GY-271 | GND | GND | JA2.4 |
| GY-271 | SCL | D1（I²C SCL） | JA1.4 |
| GY-271 | SDA | D0（I²C SDA） | JA1.3 |
| GY-271 | DRDY | 不接 | — |

- 模組一律接 3.3 V，不要接 5 V：RTL8735B 的 I/O 是 3.3 V。GY-BMI160 從 3V3 腳供電，不經模組上的穩壓器。
- 3.3 V 與 GND 也可以照開發板排針旁的絲印找。
- I²C 上拉：範例開發板在 D0、D1 各加了一顆 10 kΩ 電阻到 3.3 V。GY-271 模組多半自帶上拉（這是依模組賣場規格的推論，沒有量過），所以這兩顆可以不加；之後再接其他自帶上拉的 I²C 模組時，就不要再加，否則總上拉會太強。
- 不要接的腳：D14、D12、D13、D16、D17。D14 在重置時的電位決定是否進燒錄模式，和 3.3 V 短路後重置就會進燒錄模式（板商中文使用說明 p.17–18：同一提交的 `HUB 8735 ultra toolkit/Software/HUB 8735 ultra 開發板【Arduino 使用說明】_20240508.pdf`）；D12 是 upgrade 鍵，D13 實測上控制板上的白色補光燈，D16、D17 是 LOG UART（見 HAL 庫 README 的[〈板型〉](https://github.com/rainwu0/ArduPilot-RTL8735B/blob/v0.1.1/libraries/AP_HAL_RTL8735B/README.md#板型)）。
- 桌上測試用 USB 供電就好。要改用電池時，經 5 V BEC 接到開發板的 5 V 輸入；腳位和容許的電壓見板商的模組規格書（同一提交的 `HUB 8735 ultra toolkit/Hardware/HUB 8735 ultra Module Specification_V01_20240508.pdf`）。**電源排針接著電時，不要再插 USB**；要用 USB，先拔掉排針的電源。
- 羅盤要遠離電源線、電池與磁性零件。

## 建置

環境、Wi-Fi 設定檔與燒錄照 HAL 庫的[〈從零開始〉](https://github.com/rainwu0/ArduPilot-RTL8735B/blob/v0.1.1/libraries/AP_HAL_RTL8735B/GETTING_STARTED.md)，建置時改用這個範例的疊加檔：

```bash
cd ~/ArduPilot-RTL8735B
git checkout v0.1.1
./waf configure --board hub8735ultra --extra-hwdef $HOME/ArduPilot-Hub8735Ultra/examples/bmi160-bmp280-qmc5883l/overlay.dat
./waf copter
```

- 疊加檔的感測器設定和 HAL 庫 `v0.1.1` 的 `hwdef/hub8735ultra/sensors-example.dat` 相同，另外明寫羅盤預設啟用。
- 不提供韌體映像：映像裡有你的 Wi-Fi 帳密，請自己建置。
- 需要 HAL 庫 `v0.1.1` 以上：`v0.1.1` 開機時會先清 I²C 匯流排，`v0.1.0` 開機偶爾出現的氣壓計初始化失敗已修正（HAL 庫的 [CHANGELOG](https://github.com/rainwu0/ArduPilot-RTL8735B/blob/v0.1.1/libraries/AP_HAL_RTL8735B/CHANGELOG.md)）。

## 參數

用地面站的載入參數檔功能，照編號順序載入 `params/` 的檔案，再重開機：

1. `10-board.parm`：只用 Wi-Fi 控制時的失效保護。遙控 override 停了就降落（`FS_THR_ENABLE 3`），地面站失聯就降落（`FS_GCS_ENABLE 5`），降落中碰到失效保護就繼續降落（`FS_OPTIONS 8`）。
2. `20-use-indoor-no-gps-no-rc.parm`：室內、沒有 GPS 與遙控接收機。`GPS1_TYPE 0`；`RC_OPTIONS 33`：不用接收機，而且油門在最低才能解鎖。加了 GPS 或接收機，就不要載入這份。
3. `30-frame-quad-x-esc.parm`：機架範例，四軸 X 型，無刷電調用一般 PWM、490 Hz。換成自己的機架要改 `FRAME_CLASS`、`FRAME_TYPE`。電調在 HAL 庫還沒上板試過；裝上電調後先做電調校正，全程拆槳。
4. `40-sensors.parm`：`COMPASS_USE 1`，用這顆羅盤算航向。

載入之後，在地面站做加速度計與羅盤校正。羅盤校正要拿起開發板照六個姿勢立體轉，只在桌面上平轉不夠。`AHRS_ORIENTATION` 依開發板裝在機架上的方向設定，再用下面的重力測試確認。

## 上板驗證

範例公開前，要用 HAL 庫 `v0.1.1` 加上這個目錄的檔案建出的映像，在開發板上做完下表。

| # | 檢查 | 狀態 |
|---|---|---|
| 1 | 開機與連線：Wi-Fi 連上，地面站收到心跳、讀得到參數 | 待驗 |
| 2 | 感測器：IMU、氣壓計、羅盤都偵測得到；載入 `params/`、校正之後健康檢查都通過；靜置時加速度大小約 1 g | 待驗 |
| 3 | IMU 方向：前端朝下、右側朝下、左側朝下，三種擺法的正負號都對（重力測試） | 待驗 |
| 4 | 羅盤方向：校正後水平轉 90°，航向跟著變約 90°，轉回原位後回到原值（轉向測試） | 待驗 |
| 5 | 失效保護：用 ArduPilot 的 SITL 載入 `params/` 四份 | SITL 通過，見下方 |
| 6 | 沒有私人資料：這個目錄沒有金鑰、Wi-Fi 帳密、本機路徑、IP、熱點名稱 | 已查 |

相關的公開紀錄：HAL 庫 `v0.1.0` 的 [BOARD_TESTS.md 的 T2](https://github.com/rainwu0/ArduPilot-RTL8735B/blob/v0.1.0/libraries/AP_HAL_RTL8735B/BOARD_TESTS.md) 用同一組感測器與相同的疊加設定，驗過開機、Wi-Fi 地面站連線、三個感測器偵測得到、前端抬起時 pitch 方向正確。當時沒有載入這個範例的參數，roll 與羅盤方向沒有直接量。

第 5 項的 SITL：ArduCopter 從 HAL 庫的公開提交 `9ba8930a83` 建置，四軸 X 模型，載入 `params/` 四份，再加一層只給模擬用的設定（一個羅盤、不量電池、飛行模式都設 Stabilize）。四種情況都通過：

- 操縱輸入停、地面站心跳照送：約 1 秒切到 LAND，落地後上鎖。
- 整條連線斷掉：約 1 秒切到 LAND，之後觸發地面站失效保護，一直維持 LAND 到落地上鎖。
- 在地上解鎖時斷線：約 1 秒上鎖。
- 斷線後恢復：維持 LAND，直到地面站選模式。

SITL 驗的是 ArduPilot 的失效保護邏輯與這組參數，不包含 HAL、Wi-Fi 與硬體。

### 重力測試（IMU 方向）

ArduPilot 的機體座標是 X 向前、Y 向右、Z 向下。開發板靜止，在地面站看加速度計的原始值（mG）：

| 擺法 | 應該看到 |
|---|---|
| 水平，元件面朝上 | Z 約 −1000，X、Y 約 0 |
| 前端（microSD 端）朝下 | X 約 −1000 |
| 右側朝下 | Y 約 −1000 |
| 左側朝下 | Y 約 +1000 |

某一軸的正負號錯了，或動的是別的軸，就改疊加檔裡 IMU 的 `ROTATION_*`（整片開發板轉了方向時，改參數 `AHRS_ORIENTATION`），再測一次。

### 轉向測試（羅盤方向）

1. 先在地面站做羅盤校正（拿起開發板照六個姿勢立體轉）。沒校正時磁航向不準。
2. 開發板水平放好，記下磁航向與 EKF 的偏航角。
3. 水平順時針轉 90°：兩個值都要增加約 90°，彼此只差幾度；再轉回原位，兩個值都要回到原來的值。
4. 磁航向往反方向走，或轉的角度差很多，就改疊加檔裡羅盤的 `ROTATION_*`，重新校正再測。
