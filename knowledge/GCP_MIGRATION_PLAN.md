# Project Cloud-Ai: GCP 遷移計畫書 (GCP Migration Plan)

## 1. 專案概述
將 Ai (OpenClaw 實例) 從本地環境 (WSL2) 遷移至 Google Cloud Platform (GCP) Compute Engine，以實現 24/7 高可用性與穩定的雲端運行環境。

## 2. 目前進度
- [x] GCP 帳號與 300 USD 試用額度已啟用。
- [x] 成功透過 Google AI Studio API 進行生圖測試 (`nano-banana-new.js`)。
- [x] 確定遷移至 GitHub `knowledge` 倉庫進行文件管理。

## 3. 遷移階段與任務

### 階段一：GCP 環境建置
- [ ] 建立 Compute Engine 虛擬機 (預計 e2-medium 或更高)。
- [ ] 設定防火牆規則 (Port 3000, 443 等)。
- [ ] 安裝必要環境 (Node.js v20+, Git, Tailscale)。

### 階段二：OpenClaw 部署
- [ ] 在 GCP clone 核心程式碼。
- [ ] 遷移設定檔 `openclaw.json` (注意 API Key 安全性)。
- [ ] 安裝相關 Skills 與相依套件。

### 階段三：靈魂與記憶遷移 (核心)
- [ ] 同步 `SOUL.md`, `IDENTITY.md`, `USER.md`。
- [ ] 同步 `MEMORY.md` 與 `memory/` 目錄下的歷史紀錄。
- [ ] 確保 `knowledge` 倉庫的持續整合。

### 階段四：服務切換與測試
- [ ] 更新 LINE Bot Webhook URL 至新環境。
- [ ] 重新設定 Tailscale Funnel 或 GCP 固定 IP。
- [ ] 進行全系統功能測試 (對話、技能執行、自動備份)。

## 4. 預期效益
- 擺脫本地電腦重啟或網路中斷的影響。
- 利用 GCP 強大的網路架構提升回應速度。
- 為未來整合更多雲端原生服務 (如 Cloud Storage, Pub/Sub) 打下基礎。

---
*最後更新時間：2026-02-06 17:39 (GMT+8)*
*負責人：Ai ✨🧚‍♀️ & YY*
