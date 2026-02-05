# OpenClaw 完整實戰指南：從零到一打造全能 AI 助理 (繁體中文)

這份手冊彙整了 YY 在實戰中架設 OpenClaw 的所有細節，包含如何解決環境權限、打通遠端連線、串接 LINE 機器人，以及賦予助理搜尋與 GitHub 操作能力。

---

## 目錄
1. [環境準備 (WSL2)](#一環境準備-wsl2)
2. [安裝與基礎初始化](#二安裝與基礎初始化)
3. [核心技能：Web Search (Brave API)](#三核心技能web-search-brave-api)
4. [核心技能：GitHub 操作 (GHP Token)](#四核心技能github-操作-ghp-token)
5. [遠端連線與自動化 (Tailscale Funnel)](#五遠端連線與自動化-tailscale-funnel)
6. [串接 LINE 機器人](#六串接-line-機器人)
7. [進階安全與優化 (消除警告、提升體驗)](#七進階安全與優化)

---

## 一、環境準備 (WSL2)
OpenClaw 在 Linux 環境下表現最穩定。建議 Windows 使用者安裝 WSL2。
1.  **安裝 WSL2**：打開 PowerShell 並執行 `wsl --install`。
2.  **更新系統**：在 Ubuntu 中執行 `sudo apt update && sudo apt upgrade -y`。
3.  **安裝 Node.js** (建議 v20 以上)：
    ```bash
    curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
    sudo apt-get install -y nodejs
    ```

---

## 二、安裝與基礎初始化
1.  **全局安裝**：
    ```bash
    npm install -g openclaw
    ```
2.  **執行診斷工具**：
    ```bash
    openclaw doctor
    ```
    *這會幫你建立基礎目錄 `~/.openclaw` 並檢查環境。*

3.  **設定主模型**：
    編輯 `~/.openclaw/openclaw.json`，在 `models.providers` 填入 API Key。
    *   **OpenAI**: 填入 `sk-...`。
    *   **Gemini**: 填入你的 Google AI Studio Key。

---

## 三、核心技能：Web Search (Brave API)
讓你的 AI 具備上網查資料的能力。
1.  **取得 API Key**：前往 [Brave Search API](https://brave.com/search/api/) 申請。
2.  **配置設定檔**：在 `openclaw.json` 的 `tools.web.search` 區塊填入：
    ```json
    "tools": {
      "web": {
        "search": {
          "enabled": true,
          "provider": "brave",
          "apiKey": "你的BRAVE_API_KEY"
        }
      }
    }
    ```

---

## 四、核心技能：GitHub 操作 (GHP Token)
讓 AI 能幫你管理儲存庫（如：備份手冊）。
1.  **產生 Token**：在 GitHub 設定中產生一個 **Fine-grained Personal Access Token** (或 Classic PAT)，勾選 Repo 讀寫權限。
2.  **環境變數設定**：將 Token 加入 `openclaw.json` 的 `env.vars` 中：
    ```json
    "env": {
      "vars": {
        "GITHUB_TOKEN": "ghp_你的Token"
      }
    }
    ```
    *之後你就可以對 AI 下指令說：「幫我建立一個 GitHub Repo 並把這份文件傳上去」。*

---

## 五、遠端連線與自動化 (Tailscale Funnel)
解決「人在外面連不回家」的問題。
1.  **安裝 Tailscale**：在 WSL 中安裝並執行 `sudo tailscale up`。
2.  **授權自動控制 (關鍵)**：
    預設情況下，OpenClaw 需要 sudo 權限才能開關隧道。為了讓它自動化，請執行：
    ```bash
    sudo tailscale set --operator=$USER
    ```
3.  **啟用 Funnel**：在 `openclaw.json` 的 `gateway` 區塊設定：
    ```json
    "gateway": {
      "tailscale": { "mode": "funnel" }
    }
    ```

---

## 六、串接 LINE 機器人
1.  **建立 Channel**：在 [LINE Developers](https://developers.line.biz/) 建立 Messaging API Channel。
2.  **填入金鑰**：在 `openclaw.json` 的 `channels.line` 區塊填入 `channelSecret` 和 `channelAccessToken`。
3.  **Webhook 設定**：
    網址為：`https://你的Tailscale網址/api/inbound/line/default`
    *記得在 LINE 後台開啟 "Use webhook"。*

---

## 七、進階安全與優化
這是 YY 實測後最推薦的「大補帖」：

1.  **隱藏密碼**：
    不要將密碼寫在 `openclaw.json` 裡（會跳 WARN）。請改用 `OPENCLAW_GATEWAY_PASSWORD` 環境變數。
2.  **設定信任代理 (Trusted Proxies)**：
    解決遠端登入時顯示來源不明的問題：
    ```json
    "gateway": {
      "trustedProxies": ["127.0.0.1", "::1"]
    }
    ```
3.  **跳過繁瑣的裝置配對**：
    如果你覺得遠端連線每次都要「配對裝置 (Pairing)」很麻煩，可以開啟這個選項：
    ```json
    "gateway": {
      "controlUi": { "dangerouslyDisableDeviceAuth": true }
    }
    ```
4.  **修正檔案權限**：
    ```bash
    chmod 700 ~/.openclaw/credentials
    ```

---

## 結語
完成以上步驟後，你就擁有了：
*   ✅ 開機自動連線的雲端助理
*   ✅ 隨時隨地用 LINE 呼喚的精靈
*   ✅ 能上網查新聞、能操作 GitHub 的大腦

快去分享給你的朋友吧！✨🧚‍♀️
