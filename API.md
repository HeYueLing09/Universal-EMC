**1. 修改設定檔**

先關閉遊戲／伺服器，打開遊戲實例裡的：
```
config/universalemc/settings.json
```

找到 `"api"` 區塊，改成下面這樣，其他設定保留：
```
"api": {
  "enabled": true,
  "endpoint": "https://你的API網址/v1/chat/completions",
  "model": "供應商提供的模型名稱",
  "keyEnvironment": "UNIVERSAL_EMC_API_KEY",
  "batchSize": 32,
  "maxBatchesPerScan": 4,
  "timeoutSeconds": 60
}
```

`endpoint` 要填**完整請求網址**，不是只有網域；實際路徑以供應商提供的為準。

**2. 設定 API Key**

這版從環境變數讀取 Key。在 Windows 開啟 PowerShell，執行：
```
[Environment]::SetEnvironmentVariable(
  "UNIVERSAL_EMC_API_KEY",
  "在這裡填入你的API金鑰",
  "User"
)
```

`keyEnvironment` 保持原樣，**不要把 Key 填進那個欄位**。設定後完全退出並重開啟動器／開服終端機，讓遊戲讀到新變數。

**3. 進遊戲執行**
```
/uemc scan
```

API 只會替**既有 EMC 與配方都無法定價的物品**估值，不會把所有物品交給 AI 重算。上述設定每次最多請求 4 批、每批 32 個物品；失敗或超出批次上限時，使用離線估值。

結果可以查看：
```
config/universalemc/report.json
```

`apiNotes` 是 API 處理訊息，`sources` 裡的 `api-estimate` 表示採用了 API 估值。
