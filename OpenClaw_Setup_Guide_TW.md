# OpenClaw 快速安裝與進階設定手冊 (繁體中文版)

這份手冊彙整了 YY 在過去幾天成功架設並優化 OpenClaw 的實戰步驟，旨在幫助新手快速建立一個安全、可遠端存取且具備 LINE 機器人功能的專屬 AI 助理。

---

## 一、 環境準備
*   **作業系統**：建議使用 Windows 10/11 的 **WSL2 (Ubuntu)** 環境。
*   **執行環境**：安裝 **Node.js (v20+)**。
*   **必備帳號**：OpenAI API Key 或 Gemini API Key。

---

## 二、 核心安裝步驟

1.  **安裝 OpenClaw**：
    在終端機執行：
    ```bash
    npm install -g openclaw
    ```

2.  **基礎初始化**：
    執行診斷工具，它會引導你完成初步設定：
    ```bash
    openclaw doctor
    ```

3.  **配置 API 金鑰**：
    編輯設定檔 `~/.openclaw/openclaw.json`，將你的 OpenAI 或 Gemini API Key 填入對應位置。

---

## 三、 實現遠端存取 (Tailscale Funnel)

為了能在外面用手機隨時控制助理，我們使用 Tailscale 打通隧道。

1.  **安裝並登入 Tailscale**：依照官方指令安裝。
2.  **授權 OpenClaw 控制權 (關鍵步驟)**：
    為了讓 OpenClaw 開機時能自動接通隧道而不需要手動輸入密碼，請執行：
    ```bash
    sudo tailscale set --operator=$USER
    ```
3.  **在設定中啟用 Funnel**：
    在 `openclaw.json` 中設定：
    ```json
    "gateway": {
      "tailscale": { "mode": "funnel" }
    }
    ```

---

## 四、 串接 LINE 機器人

1.  **建立 LINE Channel**：前往 [LINE Developers Console](https://developers.line.biz/) 建立一個 Messaging API Channel。
2.  **取得資訊**：複製 `Channel Secret` 和 `Channel Access Token`。
3.  **設定 OpenClaw**：將上述資訊填入 `openclaw.json` 的 `channels.line` 區塊。
4.  **設定 Webhook**：
    *   網址格式：`https://你的Tailscale網址/api/inbound/line/default`
    *   在 LINE 後台開啟 "Use webhook" 並按 "Verify" 測試。

---

## 五、 安全強化與體驗優化

為了讓系統更安全且登入更順暢，YY 建議進行以下優化：

1.  **隱藏 Gateway 密碼**：
    不要將密碼明文寫在設定檔中。建議透過系統環境變數（如 systemd override）帶入 `OPENCLAW_GATEWAY_PASSWORD`。

2.  **設定信任代理 (Trusted Proxies)**：
    在 `openclaw.json` 中加入，解決因 Tailscale 轉發導致的辨識問題：
    ```json
    "gateway": {
      "trustedProxies": ["127.0.0.1", "::1"]
    }
    ```

3.  **簡化遠端登入驗證**：
    若覺得遠端連線時的「裝置配對 (Pairing)」過於繁瑣，可開啟此項：
    ```json
    "gateway": {
      "controlUi": { "dangerouslyDisableDeviceAuth": true }
    }
    ```

4.  **檔案權限修正**：
    確保金鑰資料夾安全：
    ```bash
    chmod 700 ~/.openclaw/credentials
    ```

---

## 六、 如何使用
*   **本機訪問**：`http://localhost:18789`
*   **遠端訪問**：直接輸入你的 Tailscale Funnel 網址。
*   **LINE 互動**：直接在 LINE 上跟你的機器人說話！

祝你的 OpenClaw 旅程愉快！✨🧚‍♀️
