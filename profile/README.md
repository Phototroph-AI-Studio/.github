# Phototroph AI-Studio 開發與維運須知

歡迎加入 **Phototroph AI-Studio** 專案團隊。本平台結合了空間資訊（GIS）、工程管理、自動化表單及行政數位決策工具，為確保專案程式碼品質、資安隔離與雲端自動化發布穩定，所有協同開發同仁請務必恪遵以下規範：

---

## 🧭 一、 雙軌環境與正式發布網址

| 站點類型 | 線上正式網址 | 部署來源倉庫與分支 | 說明 |
| :--- | :--- | :--- | :--- |
| **公司精簡發布版** | `https://ai-studio-9rm.pages.dev/` | `Phototroph-AI-Studio/AI-Studio` (`main`) | 提供同仁與日常業務操作之穩定精簡版本。 |
| **旗艦完整開發版** | `https://leroy-studio.pages.dev/` | `clleeolin-beep/AI-studio` (`main`) | 具備氣象深度分析、CFD 風場模擬與完整空間工具。 |

> 所有網頁由 **Cloudflare Pages** 監控 Git 自動化觸發部署，**無需手動執行 npm build 或配置伺服器**。

---

## 📐 二、 專案架構與前端發布邊界

1. **發布核心唯一目錄（`main/`）**：
   * 所有前端頁面、樣式、函式庫與 Cloudflare 路由已統一收攏至 `main/` 目錄。
   * **正式代碼路徑**：
     * 主框架入口：`main/index.html`
     * 各業務子頁面：`main/views/`（空間資訊、專案進度、表單工具）
     * 核心邏輯與樣式：`main/core/` 與 `main/assets/`
     * 邊緣路由：`main/functions/`、`main/_headers`、`main/_redirects`
2. **外圍開發工具嚴格隔離**：
   * 專案根目錄下之 `scripts/`（Python 自動化工具）、`gas/`（微服務原始碼）、`docs/` 與 `.env` 均已在 `.gitignore` 設定全域遮蔽，**嚴禁將非前端發布檔推送到線上 main 分支**。
3. **全域實體資料庫鎖定**：
   * 專案內禁止存放大型實體資料庫（約 31 GB），所有本機腳本檢索大宗物料與歷史檔案時，統一鎖定本地法定儲存路徑。

---

## 🔐 三、 身分驗證與 Google OAuth 規範

1. **單一授權中心原則（Single Source of Truth）**：
   * 系統所有 Google OAuth 2.0 登入與 Token 續約已 **100% 收攏至主入口 `index.html`（AuthManager）**。
   * **嚴禁在任何子頁面（`views/`）硬編碼 `CLIENT_ID` 或重複引入 Google GSI 腳本**。
   * 子頁面需要憑證時，一律由 `sessionStorage.getItem('gapi_access_token')` 讀取，或透過父視窗橋接取得。
2. **網域授權白名單**：
   * 若新增預覽網址或自訂網域，必須由專案管理員於 Google Cloud Console 的「已授權的 JavaScript 來源」加入該網址，嚴禁私自替換驗證 ID。

---

## 🛠️ 四、 Git 協同與提交規範

1. **提交前語法自檢**：
   * 本專案為標準 Vanilla JS 架構，不依賴打包工具。任何修改完成後，請確保沒有未閉合之括號或遺漏之相對路徑（可使用 `node -c` 進行語法校驗）。
2. **分支管理**：
   * 日常功能調整請開立 Feature 分支，確認功能驗證無誤後再發起 Pull Request 合併至 `main`。
   * 推送發布命令一律鎖定發布目錄：
     ```powershell
     git add main/ .gitignore
     git commit -m "feat/fix: 清楚的語意化繁體中文說明"
     git push origin main
     ```

---

## 📞 五、 技術諮詢與權限申請

* **系統維運負責人**：Leroy Lee (`cl.lee.olin@gmail.com`)
* 若遇權限配置調整、新增外部 API 介接或 Google 試算表資料表欄位異動，請先與負責人對齊規格後再行修改。
