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
      + 燒錄成功之後，**Heltec V3**會開機，並且LED螢幕會有文字。不過此時會卡在需要進行設定的提示，不會跑Lora功能，預設是會可以用USB通訊，也開啟藍芽可以通訊。此時耗電約0.6W。放著不動過了大概10分鐘LED螢幕就關閉，耗電約剩0.5W。短按可以再喚醒LED螢幕。長按關機後耗電剩0.04W以下，藍芽也都關閉。
      + 燒錄紀錄中可以查看到ESP32裝置的MAC。其中後面四個英文字XXXX將被Meshtastic取出來當作設備的唯一碼，顯示在LED中，LED中顯示的不能被更改。
  + 如果燒錄好了，建議在外殼上貼上 **Meshtastic_XXXX**的貼紙，以利在沒開電的狀況下判斷是哪台設備，並且已經燒錄Meshtastic韌體。
  + 此時，每次通電開機都會有個歡迎畫面，右上方有韌體版本與裝置唯一ID。左上方是Lora區域設定，預設是UNSET，表示尚未設定。

### Heltec V3按鈕
+ 可以參考: https://meshtastic.org/docs/hardware/devices/heltec-automation/lora32/buttons/
+ 包含兩個按鈕，上面那個標住為P，下面的標住為R。
+ R按鈕為**Reset**，單獨按一下就會重新啟動。
+ P按為**用戶/程式按鈕**：
  + 長按：將在 5 秒後觸發設備關閉信號。
  + 單次按壓：切換顯示在設備螢幕上的資訊頁面。
  + 雙次按壓：向網絡發送設備位置的臨時 Ping 訊號。
  + 三次按壓：根據需求啟用/禁用 GPS 模組。如果添加了 NPN 晶體管，它將切斷 GPS 板的電源。為了讓這個開關功能生效，必須在應用程式中的 Position 模組 裡配置 PIN_GPS_EN 來設定 NPN 腳位。

### 初次設定
+ 有很多用戶端，我選擇最簡單的網頁用戶端，只需要有**Edge (Microsoft Edge)**，不必安裝軟體。
  + 初次設定不能用WIFI，因為WIFI與藍芽只能同時存在一個，預設是藍芽。
  + 如果你的電腦有藍芽，也可以用藍芽，目前先不考慮使用。因為我在用的時候，都讀不到一些設備資訊，名稱、電壓之類的。
  + 使用USB連線。只要你有裝好Driver。
+ 設定區域：
  + 選`New Connection`，再找到`Serial`，會找到黑框的按鈕，就是可連線的裝置。
  + 到`Navigation`>`Config`>`Lora`>`Mesh Settings`>`Region`設定為**TW**，之後按右上角的儲存圖示。按下之後會跳出儲存成功的提示，且硬體將會重新啟動。
  + 重新啟動後，每次通電開機都會有個歡迎畫面，右上方有韌體版本與裝置唯一ID。左上方是Lora區域設定，顯示TW，表示使用台灣法律規定的適用頻段。官方已經查好台灣的法規並指定好頻段。
  + 因為已經成功設定好，啟動後就會依照預設參數開始成為一個Meshtastic節點。
  + LED螢幕顯示:
    + 下方有兩個圓點，表示有2個頁面，可單次短按P鍵切換頁面瀏覽內容。
    + 第一頁:
      + 左上有個電池符號，用來繪製無電池、有電池的電量、充電中等。
      + 最上方還會出現`Ⓞ在線節點數/總結點數`
      + 右上方會顯示`No GPS`
      + 接著第二行左邊有`#LongFast`的頻道ID，右邊有`[i]`再加上藍芽連線用的裝置名稱。
    + 第二頁:
      + 左上有個`B`，接著是`4.58V`，接著是`100%`，接著是`+U`。`4.58V`是電池電壓，`100%`是電量百分比，`+U`是指用USB幫電池充電及運作硬體。


### 情境測試:
+ 有三台：71a01、73e4、e58c
  + 71a01: 用USB web client
  + 73e4: 用WIFI web client
    + 手機開熱點加相容性(準備英文的SSID與密碼)
    + 先用電腦操作:
      + 燒錄有`Bundle Web UI`的韌體。
      + 連接USB。
      + 用web client以USB連接。
      + 選`New Connection`，再找到`Serial`，會找到黑框的按鈕，就是可連線的裝置。
      + 到`Navigation`>`Config`>`Radio Config`>`Lora`>`Mesh Settings`>`Region`設定為**TW**，之後按右上角的儲存圖示。按下之後會跳出儲存成功的提示，且硬體將會重新啟動。
      + 到`Navigation`>`Config`>`Radio Config`>`Network`>`WiFi Config`
        + Enabled: ON
        + SSID: 填入
        + 密碼: 填入
        + 之後按右上角的儲存圖示。按下之後會跳出儲存成功的提示，且硬體將會重新啟動。
  + e58c: 用藍芽 手機App
    + 都用APP操作
    + 按+來找裝置配對。 
    + 配對成功後，顯示連線成功後，設區域為TW。
    + 重開機。









### Meshtastic Taiwan
+ 他是一個社群，方便台灣普通使用者來快速使用，但有點...。
+ 他有一個QRcode可以設定些東西。
  + 基本上是個圖檔。
    + <img src="QRCODE.png" alt="描述文字" width="300">
  + 內容是以下網址：https://meshtastic.org/e/#CgMSAQEKLxIgisDhHrNpJPlGX3GBJBX6kjuK7KQNp4Z0M7OTDpnX5N4aBk1lc2hUVyUBAAAACjMSIMtR3IlYKZeR84dOSiVHv7pllB_F4Rt1I0da72R31BxVGgpTaWduYWxUZXN0JQIAAAAKMxIgy2jnf86fTpf_4AFAf-mCwbzRoxpCV0P90dqJo0-w_SYaCkVtZXJnZW5jeSElAwAAABIOCAE4CEADSAFQEVgQaAE
  + 沒有看到文件說明這個QRcode幹了什麼事情...
  + 來抄一下可能App設定的東西：
    + 名稱:沒改，區域=TW(會改)。
    + 裝備設定:
      + **User**: 沒改
      + **Channels**: 出現四個(原本1個)
        + 0 LongFast
          + PSK: AQ==
          + Uplink enabled: OFF
          + Downlink enabled: OFF
          + Position enabled: OFF
        + 1 MeshTW
          + PSK: isDhH... 
          + Uplink enabled: OFF
          + Downlink enabled: OFF
          + Position enabled: OFF
        + 2 SignalTest
          + PSK: y1Hci... 
          + Uplink enabled: OFF
          + Downlink enabled: OFF
          + Position enabled: OFF
        + 3 Emergency!
          + PSK: y2jnf... 
          + Uplink enabled: OFF
          + Downlink enabled: OFF
          + Position enabled: OFF
      + **Device**:
        + Role: CLIENT
          + Redefine PIN_BUTTON: 0
          + Redefine PIN_BUZZER: 0 
        + Rebroadcast mode: ALL
          + NodeInfo broadcast interval (seconds): 10800
        + Double tap as button press: OFF
        + Disable triple-click: OFF
        + Disable LED heartbeat
        + 
        + Double tap as button press
        + Dis 
      + **Position**:
        + Position broadcast interval (seconds): 900
        + Smart position enabled: ON
        + Smart broadcast minimum distance (meters): 100
        + Smart broadcast minimum interval (seconds): 30
        + Use fixed position: OFF
        + GPS mode: NOT_PRESENT
        + GPS update interval(seconds): 120
        + Position flags: 811
        + Redefine GPS_RX_PIN: 0
        + Redefine GPS_TX_PIN: 0
        + Redefine GPS_GPS_EN: 0
     + **Power**:
       + Enable power saving mode: OFF
       + Shutdown on battery delay (seconds): 0
       + ADC multiplier override ratio: 0.0
       + Wait for Bluetooth duration (seconds): 60
       + Super deep sleep duration (seconds): 4294967295
       + Light sleep duration (seconds): 300
       + Minimum wake time (seconds): 10
       + Battery INA_2XX I2C address: 0
    + **Network**:
       + WiFi enabled: OFF
         + SSID
         + PSK
       + Ethernet enabled: OFF
         + NTP Server: meshtastic.pool.ntp.org
         + rsyslog server: null
       + IPv4 mode: DHCP
         + IP 
         + Gateway
         + Subnet
         + DNS
    + **Display**:
       + Screen timeout (seconds): 600
       + GPS coordinates format: DEC
       + Auto screen carousel (seconds): 0
       + Compass north top : OFF
       + Flip screen: OFF
       + Display units: METRIC
       + Override OLED auto-detect: OLED_AUTO
       + Display mode: DEFAULT
       + Heading bold: OFF
       + Wake screen on tap or motion: OFF
       + Compass orientation: DEGREES_0
    + **LoRa**: 有改
       + Use modem preset: ON
       + Modem preset: LONG_FAST
       + Frequency offset (MHz): 0.0
       + Region (frequency plan): Taiwan
       + Hop limit: 3
       + Tx enabled: ON
         + Tx power (dBm): 17
         + Frequency slot: 16
       + Override Duty Cycle: OFF
       + SX126X RX boosted gain: ON
       + Override frequency (MHz): 923.875
       + Ignore MQTT: OFF
       + OK to MQTT: OFF
    + **Bluetooth**:
       + Bluetooth enabled: ON
       + Pairing mode: RANDOM_PIN
         + fixed PIN: 123456
    + **Security**:
       + Public Key: Edbzc..
       + Private Key: OD7UI...
       + Managed Mode: OFF
       + Serial console: ON
       + Debug log API enabled: OFF
       + Legacy Admin channel: OFF
    + 模組設定:
    + **MQTT**: 有改
      + MQTT enabled: OFF
        + Address: mqtt.meshtastic.org
        + Username: meshdev
        + Password: large4cats
      + Encryption enabled: ON
      + JSON output enabled: OFF
      + TLS enabled: OFF
      + Root topic: msh/TW (有改)
      + Proxy to client enabled: OFF
      + Map reporting: OFF
        +  Map reporting interval(seconds): 0
    + **Serial**:
      + Serial enabled: OFF
      + Echo enabled: OFF
        + RX: 0
        + TX: 0
      + Serial baud rate: BAUD_DEFAULT
        + timeout: 0
      + Serial mode: DEFAULT
      + Override console Serial port: OFF 



### 有趣的視覺化地圖
+ https://meshsight.nfs.tw/
+ https://meshmap.net/ 壞了?
+ https://meshtastic.liamcottle.net/
