# Meshtastic_Study
+ Meshtastic研究
+ 作者: HsiupoYeh
+ 更新日期：2025-03-30

### Meshtastic 簡介
+ 整個字讀起來像是 **"麥許塔斯提克"** 。我決定翻譯為 **網狀塔斯提克通訊**
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
