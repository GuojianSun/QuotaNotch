# QuotaNotch

<p align="center">
  <img src="assets/brand/app-icon.png" width="128" alt="QuotaNotch 應用程式圖示">
</p>

<p align="center">
  <strong>把 Codex 用量放在 MacBook 瀏海旁。</strong>
</p>

<p align="center">
  一款輕巧的原生 macOS 輔助工具，不必離開目前工作，便能查看額度與可選的本機協作資料。
</p>

<p align="center">
  <strong>公開預覽 · macOS 14+ · Apple silicon</strong>
</p>

<p align="center">
  <a href="./README.md">English</a> ·
  <a href="./README.zh-CN.md">简体中文</a> ·
  <strong>繁體中文</strong>
</p>

> [!IMPORTANT]
> **目前是公開預覽階段，尚未提供可下載的二進位安裝檔。** QuotaNotch 現階段也沒有 Apple Developer ID，因此未來最初的預覽版將不會以 Developer ID 簽署，也不會經過 Apple 公證。請先閱讀下方的[安全安裝說明](#安全安裝公開預覽版)；若你不接受這項風險，請等候日後完成 Developer ID 簽署與公證的版本。

QuotaNotch 會在指標觸碰 MacBook 瀏海時展開，移動一下指標即可查看每週 Codex 用量。它也會常駐選單列，沒有瀏海的 Mac 或使用外接顯示器時，仍可從同一入口開啟。

## 介面預覽

### 展開面板

![QuotaNotch 繁體中文深色展開面板](assets/screenshots/zh-Hant/panel-expanded-dark.png)

### 精簡面板

![QuotaNotch 繁體中文淺色精簡面板](assets/screenshots/zh-Hant/panel-compact-light.png)

### 互動設定

![QuotaNotch 繁體中文互動設定視窗](assets/screenshots/zh-Hant/settings.png)

> 截圖直接來自真實的 SwiftUI/AppKit 介面。其中的彙總數值是隔離的本機截圖環境在某一時點的狀態；它們不由本儲存庫提供，也會與每位使用者的目前帳戶資料不同。橫線代表截圖時上游尚未提供該欄位。

## 核心體驗

- **快速開啟**：觸碰瀏海即可展開，也能隨時透過選單列開啟。
- **額度一目了然**：查看每週用量、剩餘比例、確切重設時間與最近更新狀態。
- **按需展開**：在精簡的每週用量之外，自行啟用 Token 活動、用量里程碑及本機協作時間。
- **自由編排**：選擇面板寬度，拖曳整列調整模組順序，並保留上次的展開狀態。
- **符合 macOS 習慣**：支援淺色、深色與系統主題，以及「減少動態效果」和「降低透明度」。
- **三語介面**：首次啟動可選擇 English、简体中文或繁體中文，之後可在設定中立即切換。

## 模組與資料來源

QuotaNotch 不自行推算帳戶額度，也不模擬 Codex 的網路請求。它會啟動由 OpenAI 簽署的 Codex／ChatGPT 應用程式內附 Codex，或你明確選取並核准的 Codex 命令列執行檔，再透過本機 `codex app-server` 取得資料。

| 模組 | 顯示內容 | 資料來源與邊界 |
| --- | --- | --- |
| **每週用量**（必選） | 已用／剩餘比例、進度、確切重設時間、方案標籤、更新狀態，以及符合條件時的重設卡提示 | Codex app-server 的帳戶 rate-limit 資料；伺服器回傳值是唯一依據。QuotaNotch 不會使用重設卡，按鈕僅會開啟官方用量頁面 |
| **Token 活動** | 近七日熱度、累計 Token、依本機日曆計算的本週 Token | Codex app-server 的帳戶用量摘要與每日資料桶；缺少的日期維持未知，不當作 0 |
| **用量里程碑** | 連續使用、最長連續、單日峰值、最長任務 | 與 Token 活動相同的帳戶用量摘要；預設關閉 |
| **今日／累計協作** | 這台 Mac 上可從終態記錄還原的協作時間，並可選擇顯示段數 | 使用者主動啟用後，定期掃描 Codex 已寫入本機的完成、失敗與中斷終態及計時中繼資料；不是即時計時器，也不是帳戶跨裝置總計 |

每週額度與 Token 活動是彼此獨立的讀取。舊版 Codex、不支援的方法、逾時或可選欄位缺漏，可能使 Token 或里程碑資料顯示為不可用，但不應抹除已取得的每週額度。

## 本機協作如何計算

本機協作功能預設關閉。啟用後，QuotaNotch 會在需要時於啟動階段掃描，執行期間約每 15 分鐘掃描一次，並在 Mac 喚醒與手動重新整理時更新。進行中的工作不會被猜測；Codex 寫入完成、失敗或中斷的終態後，資料才會出現。

- 完成與失敗的根任務會計入時間及一段協作。
- 中斷工作仍會計入已有時間；只有嚴格大於 20 分鐘的中斷才另計一段。
- 跨越午夜的工作仍是一段，但耗時會按本機曆日切分；段數歸到完成當日。
- 子代理記錄會排除，避免與使用者可見的根任務重複計算。
- 相同任務即使出現在封存、恢復或分支記錄中，也會以雜湊後的任務識別碼去重。

因此，「協作時間」代表**這台 Mac 上仍可由 Codex 本機記錄還原的經過時間**，不是打字時間、CPU 時間、官方帳戶統計或跨裝置完整歷史。若原始記錄已刪除、截斷、從未同步到這台 Mac，QuotaNotch 無法補回缺失資料。

## 系統需求

- macOS 14 Sonoma 或以上版本
- 配備 Apple 晶片的 Mac（M1 或更新）；目前不提供 Intel 或 Universal 2 安裝檔
- OpenAI 簽署的 Codex／ChatGPT macOS 應用程式所提供的 Codex，或由你明確選取並核准的 Codex 命令列程式
- 已登入 Codex

## 目前的發行狀態

此公開儲存庫目前提供產品介紹、預覽圖、政策與日後的官方發行入口；實作原始碼仍為私有。目前：

- 尚未上傳 DMG 或其他二進位安裝檔；
- 尚未建立可供一般使用者接收更新的正式 appcast；
- 沒有 Apple Developer ID，因而無法提供 Developer ID 簽署與 Apple 公證；
- 本機開發用、臨時簽署或更新測試用的建置都不是公開版本。

未來有預覽安裝檔時，只會從本儲存庫的 [GitHub Releases](https://github.com/GuojianSun/QuotaNotch/releases) 發布。README 中的狀態會與實際可下載內容同步更新。

## 安全安裝公開預覽版

> 以下步驟供日後出現首個公開預覽 DMG 時使用；目前 Releases 頁面尚無可安裝版本。

1. 只從本儲存庫的 [Releases](https://github.com/GuojianSun/QuotaNotch/releases) 下載 DMG，並取得同一筆 Release 公布的 SHA-256 校驗值。
2. 核對版本、檔名、發行說明與 SHA-256；不要使用轉載站、鏡像或他人重新封裝的版本。
3. 在終端機計算下載檔的雜湊值：

   ```bash
   shasum -a 256 "/path/to/QuotaNotch.dmg"
   ```

4. 確認結果與 Release 公布的值逐字相同後，再開啟 DMG 並把 **QuotaNotch** 拖入「應用程式」。
5. 在沒有 Developer ID 與公證的階段，macOS 可能阻擋首次開啟，或顯示無法驗證開發者的提示。不要關閉整台 Mac 的 Gatekeeper，也不要執行來源不明的 `xattr` 或安裝腳本。只有在你已核對來源與雜湊、並願意承擔風險時，才按 [Apple 針對單一 App 的官方步驟](https://support.apple.com/zh-tw/guide/mac-help/mh40617/mac)，到「系統設定 → 隱私權與安全性」對該應用程式選擇「強制打開」。

SHA-256 能確認你手上的 DMG 與 Release 所列位元組一致，也能發現下載損壞；**它不能證明開發者身分，不能取代程式碼簽署或 Apple 公證**。若 GitHub 帳戶與 Release 同時遭到控制，攻擊者也可能一併替換檔案與雜湊。這正是日後加入 Developer ID 與公證仍有價值的原因。

## 更新安全

QuotaNotch 已整合 Sparkle 更新框架，設計上使用 HTTPS appcast，並要求更新封包通過 Sparkle 的 EdDSA（Ed25519）簽章驗證。穩定版與測試版使用同一套格式但分開選取頻道；測試完成後，穩定版應沿用完全相同的已驗證檔案，而不是重新建置一份不同內容。

這項機制保護的是**已安裝 QuotaNotch 之後的更新鏈**。目前尚無公開二進位檔與正式 appcast，因此它不會替現在不存在的首次安裝建立信任，也不能取代 Developer ID 或 Apple 公證。首次安裝仍須依照 Release 來源、SHA-256 與上方風險說明自行判斷。

## 隱私權

- 不包含分析或廣告 SDK，也不會自動上傳崩潰報告、廣告識別碼或對話內容。
- 使用現有 Codex 登入，讓本機 Codex 程序與 OpenAI 服務通訊；QuotaNotch 不解析、複製或上傳驗證權杖。
- 額度快取只保存在本機，採私人檔案權限，且不含權杖或完整協定回應。
- Token 活動只供畫面顯示，不會把完整原始回應寫入永久記錄。
- 本機協作功能必須主動啟用；保存的是雜湊識別碼、終態與計時資料，不保存提示詞、回覆、任務標題、工作目錄、原始 JSONL 或原始路徑。
- Sparkle 系統描述資料回傳已停用；更新請求不附帶 Codex 權杖、用量內容或分析識別碼。

完整內容請閱讀[隱私權政策](PRIVACY.md)。安全問題請勿公開張貼憑證、權杖、私人路徑或漏洞細節，請依照[安全政策](SECURITY.md)使用 GitHub 私人漏洞回報。

## 已知限制

- 現階段沒有可下載的公開安裝檔，也沒有 Developer ID 簽署或 Apple 公證。
- 第一階段只規劃 Apple 晶片版本；Intel 與 Universal 2 尚不在支援範圍。
- QuotaNotch 不使用 App Sandbox，因為它需要啟動本機 Codex；只有明確啟用協作功能後，才會讀取本機 Codex 終態中繼資料。
- 額度、方案名稱、Token 摘要、每日資料桶與重設卡資訊均取決於 Codex app-server 實際提供的欄位；資料可能延遲、缺漏或因帳戶／Codex 版本而不同。
- 本機協作不是即時計時，也不是官方帳戶資料；已刪除或其他裝置上的記錄無法從這台 Mac 還原。
- 面板可用底部控制點拖曳，但停駐螢幕邊緣與拖回瀏海的完整行為仍屬實驗性功能，不視為目前版本承諾。
- 選單列是沒有瀏海、使用外接顯示器或瀏海觸發不適用時的固定替代入口。

## 常見問題

### 現在可以下載嗎？

還不可以。本儲存庫現階段是公開產品頁；首個預覽安裝檔與 SHA-256 準備好後，才會出現在 Releases。

### 為什麼首個預覽版會被 macOS 阻擋？

因為目前尚未加入 Apple Developer Program，沒有 Developer ID，也無法完成 Apple 公證。這與是否上架 Mac App Store 是兩回事。日後完成簽署與公證後，README 與 Release 說明會清楚標示。

### QuotaNotch 是開放原始碼嗎？

不是。這個公開儲存庫只包含產品文件、圖片、政策與發行資產；實作原始碼為私有。官方二進位檔依[專有二進位授權](BINARY-LICENSE.md)提供。

### QuotaNotch 會解析或保存我的對話嗎？

不會解析或保存對話內容。額度資料經本機 Codex app-server 取得；可選的協作掃描只擷取合格終態所需的任務識別、狀態、完成時間與耗時，對話內容不會進入本機台帳。

### 為什麼 Token 或本週資料顯示為未知？

每日 Token 資料可能不是即時更新，舊版 Codex 或部分帳戶也可能不提供相關方法或可選欄位。QuotaNotch 保留「未知」狀態，不把缺失資料誤報為 0。

### 協作時間會重複計算嗎？

相同終態會以雜湊後的任務識別碼去重；若同一識別碼出現互相衝突的狀態或時間，該筆資料會被排除，而不是任意挑選一份。不過，它仍只能反映這台 Mac 上尚存的記錄。

### QuotaNotch 會自動使用重設卡嗎？

不會。提示按鈕只會開啟官方 Codex 用量頁面，不會呼叫使用重設卡的動作。

### 是否支援沒有瀏海的 Mac？

可以透過選單列開啟，但目前公開版本規劃只提供 Apple 晶片建置。

## 路線圖

以下是方向而非日期承諾，實際順序會依測試結果與發行條件調整：

- 完成首個公開預覽 DMG、版本說明與 SHA-256；
- 在支援的 macOS 與不同顯示器配置上驗證首次啟動、資料刷新、拖曳及更新流程；
- 啟用分離的測試版／穩定版更新頻道，以相同已驗證檔案完成升級；
- 視使用規模與維護成本，加入 Apple Developer Program，補上 Developer ID 簽署與 Apple 公證；
- 繼續改善多語內容、輔助使用、減少動態效果與視窗互動；
- 評估短週期額度模組及螢幕邊緣停駐，但只在 Codex 實際提供可靠資料、互動通過驗證後加入。

## 支援與回報

歡迎透過 [GitHub Issues](https://github.com/GuojianSun/QuotaNotch/issues) 提交問題、介面建議與預覽版回饋。回報時可附上 macOS 版本、Mac 型號、QuotaNotch 版本與安裝方式，但請勿公開 `auth.json`、存取權杖、對話內容或私人檔案路徑。

安全漏洞請依照[安全政策](SECURITY.md)私下回報，不要建立包含利用細節的公開 Issue。

## 授權與歸屬

QuotaNotch 是以編譯後二進位形式發布的專有軟體；實作原始碼不包含在本儲存庫中。使用官方版本須遵守 [QuotaNotch 二進位授權](BINARY-LICENSE.md)。公開文件、截圖、美術素材與品牌識別仍受[著作權聲明](COPYRIGHT.md)保護；第三方元件保留各自授權，詳見[第三方聲明](THIRD_PARTY_NOTICES.md)。

Copyright © 2026 Guojian Sun. All rights reserved.

---

QuotaNotch 是獨立專案，**並非 OpenAI 官方產品，與 OpenAI 不存在隸屬、贊助、背書或其他關聯**。OpenAI、Codex 與 ChatGPT 是 OpenAI 的商標。
