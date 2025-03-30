# Meshtastic_Study
+ Meshtastic研究
+ 作者: HsiupoYeh
+ 更新日期：2025-03-30

### Meshtastic 簡介
+ 整個字讀起來像是 **"麥許塔斯提克"** 。我決定翻譯為 **網狀塔斯提克通訊** 。
+ Meshtastic 是一個基於 **LoRa** 技術的通訊協定。專注於設備之間的 **長距離**、**低功耗**、**低速**通訊。
+ Meshtastic 是一個開源的項目。源代碼使用的是 **GPL-3.0許可證**，可以商業使用 Meshtastic，但如果基於 Meshtastic 進行修改並將其分發（即使是商業產品），需要遵守 GPL-3.0 許可證 的條款，這包括開源和共享修改過的源代碼。
+ Meshtastic 目前是由志願者社群維護的開源專案，並且通過一個 官方網站 和 GitHub 頁面 來統一管理所有的韌體、軟體更新以及硬體支援。
+ Meshtastic 的通訊方式類似於一個**共享的聊天室**，當多個設備加入時，所有人都共用同一個頻道。這意味著每個設備發送的消息都會佔用這個頻道的頻寬和處理能力。所有設備基本上都處於 **公開聊天室** 的狀態，消息會廣播到整個網絡。每個設備都能接收到來自其他設備的訊息，但這些訊息是群組共享的，而不是單獨的對話。

### Meshtastic官方網站
+ 官方網站： https://meshtastic.org/
+ GitHub 頁面： https://github.com/meshtastic

### 選擇硬體
+ 中國公司Heltec Automation設計的HELTEC LoRa V3。
  + 產品頁面：https://heltec.cashier.ecpay.com.tw/product/000000000612633
    + HTIT-WB32C (V3) -ESP32 LoRa (WiFi LoRa 32)完整模組(已集成外殼與天線）
  + 特色：MCU用ESP32，同時有WIFI+藍芽。好像需要WIFI都只有這顆MCU。使用SX1262。
  + 優點：便宜、台灣有官網可購買。天線都裝好了。
  + 缺點：中國產品。使用的TypeC要小心，請避免使用TypeCtoC的線，在電池充電的時候可能會出現問題。

### 燒錄
+ 目前只練習使用官方開發功能的韌體，所以依照官方的指示進行燒錄。
+ 需求：
  + 要有**網路**。
  + 要有**CP2102**的驅動。通常Win10以後有內建，沒有的話自己去找。
  + 使用 **Edge (Microsoft Edge)** 或 Chrome (Google Chrome) 。這兩個才支援一些序列埠API。
+ 步驟：
  + 先用USB連接**Heltec V3**與電腦。用裝置管理員確認有找到新增一個CP2102的COM裝置，並記得他的COM編號。
  + 使用 **Edge** 前往燒錄網站：https://flasher.meshtastic.org/
  + 依照網頁指示先`選擇裝置(Select Target Device)`為 **Heltec V3**
  + 再選擇`韌體版本(Select Firmware Version)`，目前(2025-03-30)為：**2.5.20.4c97351**
  + 再按下`燒錄(Flash)`。出現更新版本細節之提示，按下`Continue`表示同意。接著按照提示要設定:
    + (1) `確認裝置已經用USB連接Ensure device is plugged in via USB`。如果板子有需要，請手動按住燒錄鈕。不過Heltec V3不需要。
    + (2) `Choose baud rate選擇鮑率`。使用預設的115200就可以。
    + (3) `Flash firmware燒錄韌體`。
      + 請手動打開`Full Erase and Install`，如果沒有空間問題，也打開`Bundle Web UI`。
      + 接著就可以按下`燒錄(Erase Flash and Install)`。跳出提示框讓你選擇COM裝置，請找到之前確認的COM編號，然後按下連線。若成功則會看到燒錄進度，等他跑完就燒錄完成，燒錄過程中LED螢幕是黑的，有個橘色的LED燈慢慢地閃。
      > 對於某些確定自己不需要ESP32架設一個區域網路的Web用戶端，可以不要打開`Bundle Web UI`來節省空間。
      + 燒錄成功之後，**Heltec V3**會開機，並且LED螢幕會有文字。不過此時會卡在需要進行設定的提示，不會跑Lora功能。
  + 如果燒錄好了，建議在外殼上貼上 **Meshtastic_XXXX**的貼紙，以利在沒開電的狀況下判斷是哪台設備，並且已經燒錄Meshtastic韌體。

### Heltec V3按鈕
+ 可以參考: https://meshtastic.org/docs/hardware/devices/heltec-automation/lora32/buttons/
+ 包含兩個按鈕，上面那個標住為P，下面的標住為R。
+ R按鈕為**Reset**，單獨按一下就會重新啟動。
+ P按為**用戶/程式按鈕**：
  + 長按：將在 5 秒後觸發設備關閉信號。
  + 單次按壓：切換顯示在設備螢幕上的資訊頁面。
  + 雙次按壓：向網絡發送設備位置的臨時 Ping 訊號。
  + 三次按壓：根據需求啟用/禁用 GPS 模組。如果添加了 NPN 晶體管，它將切斷 GPS 板的電源。為了讓這個開關功能生效，必須在應用程式中的 Position 模組 裡配置 PIN_GPS_EN 來設定 NPN 腳位。
+ 2. 設定區域為台灣。不設定區域，則LED螢幕會提示我進行設定，否則無法讓其他Lora通訊功能開始運作。設定了會自動決定台灣的頻段，官方已經查好台灣的法規。
