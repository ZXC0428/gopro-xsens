# GoPro & Xsens 同步資料擷取系統

這是一套 Windows 桌面工具，用於同步控制 GoPro 攝影機與 Xsens 無線慣性感測器，適合運動表現、動作捕捉與後續資料分析。

## 功能概覽

- 透過 Bluetooth 搜尋並連線一台或多台 GoPro。
- 批次設定影片畫面比例、解析度與影格率。
- 透過 Xsens Wireless Master 連線多個 MTw 感測器。
- 同步開始/停止 GoPro 錄影與 Xsens 資料記錄。
- 將 Xsens 姿態、加速度、角速度與磁力計資料輸出為 CSV。
- 透過 GoPro Wi-Fi 或 USB/MTP 下載最新影片。
- 以 UDP 串流顯示 GoPro 即時預覽。

## 主要程式

| 檔案 | 說明 |
| --- | --- |
| `main.py` | 目前的完整 GUI 主程式，整合 GoPro、Xsens、預覽與下載流程。 |
| `xsens_manager.py` | Xsens 掃描、連線、方向歸零與 CSV 記錄。 |
| `gopro_preview.py` | GoPro Wi-Fi 預覽視窗與 UDP 影像串流。 |
| `gopro_control.py` | 單台 GoPro 的獨立控制工具。 |
| `find_sensor.py` | 只掃描 Xsens 接收器與感測器。 |
| `xsens_record_v2.py` | 命令列版 Xsens 記錄工具。 |
| `xsens_collect_test.py` | Xsens 單獨收集與診斷工具。 |
| `test_xsens_only.py` | Xsens 獨立測試程式。 |
| `test_gopro_wifi.py` | GoPro Wi-Fi 與 HTTP 連線測試。 |
| `usb_transfer_test.py` | GoPro USB/MTP 影片傳輸測試。 |
| `0523test.py` | 舊版整合測試程式，僅供參考。 |

## 執行流程

1. 啟動 `main.py`。
2. 掃描並連線 Xsens 感測器。
3. 搜尋並連線 GoPro。
4. 選擇影片比例、解析度與 FPS。
5. 按下開始，系統同步錄影與記錄感測器資料。
6. 按下停止，影片留在 GoPro，Xsens 資料寫入 `xsens_output/`。
7. 使用 Wi-Fi 或 USB 功能下載影片至 `video_output/`。

## 安裝環境

需求包含：

- Windows
- Python 3.8 以上
- GoPro Bluetooth/Wi-Fi 控制所需的 `bleak`、`requests`、`qasync`
- GUI 所需的 `PyQt5`
- 即時預覽所需的 `opencv-python`
- Xsens MT Software Suite 與對應的 `xsensdeviceapi`

專案依賴版本記錄在 `pyproject.toml` 與 `uv.lock`。Xsens Python SDK 需要依照電腦上的 MT Software Suite 安裝位置設定。

## 本機設定與隱私

Wi-Fi 名稱、密碼及其他裝置資訊屬於本機設定，不應放進 GitHub。

1. 複製 `config.example.json` 為 `config.json`。
2. 只在本機的 `config.json` 填入實際設定。
3. 不要把 `config.json`、影片、CSV、試算表或其他測量輸出提交到 Git。
4. `gopro_control.py` 與 `test_gopro_wifi.py` 的獨立工具可使用環境變數提供本機 Wi-Fi 資訊：

```powershell
$env:GOPRO_WIFI_SSID = "your-camera-ssid"
$env:GOPRO_WIFI_PASS = "your-camera-password"
$env:GOPRO_LOCAL_WIFI_SSID = "your-local-wifi-ssid"
```

本文件不包含任何實際 Wi-Fi 名稱、密碼或個人裝置資訊。

## 輸出資料

- `video_output/`：下載的 GoPro 影片。
- `xsens_output/`：Xsens 感測器 CSV。
- `build/`、`dist/`：PyInstaller 建置產物。

上述資料通常較大或可能包含個人/實驗資料，已列入 `.gitignore`。

## 打包

專案包含 `main.spec`，可使用 PyInstaller 建立 Windows 執行檔。建置產物會放在 `build/` 與 `dist/`，不應提交到 GitHub。

## 注意事項

- Xsens 接收器的序列埠與無線頻道可能因電腦環境不同而需要調整。
- GoPro 的 Bluetooth、Wi-Fi、USB/MTP 功能需要相機處於對應模式。
- 錄影與感測器資料的時間軸主要由程式啟動記錄的時間與 Xsens packet counter 建立，正式實驗前應先進行同步驗證。