# CSAPP × DDIA 2e 學習進度頁風格規範

> 給負責更新 `index.html` 的 bot：**每次更新進度頁之前，先讀完這份文件。**
> 跟其他 repo（daily-news／x-trends／…）不同，這個頁面**不是逐日產生的快照**，而是單一
> `index.html` 隨著 `README.md` 的 30 週計畫、`specs/`、`notes/`、`csapp-labs/`、`ddia-labs/`
> 的實際進度持續更新。以下規則是直接從目前 `index.html` 裡已經在用的 CSS／結構抽出來的，
> 不是重新發明一套規範——改動時延續這些 class 和 token，不要另外長出一套新寫法。

## 0. 硬性規則

1. **跟其他站不同，這裡用單一 `<style>` 區塊，不是行內 CSS。** 不要把這個頁面改成逐行 `style="..."`，會跟現有結構衝突。
2. 進度資訊（哪週做完、哪個 Lab 卡關）要跟 `README.md` 的 30 週表、`specs/W*.md`、`notes/` 底下的實際內容一致，不可以憑空標成「已完成」。
3. 影片／筆記連結（`.t-video`／`.t-rec`／`.t-reading` 區塊）要核對過真的存在、對應得上，不可編造不存在的連結。
4. 深色模式用 `@media (prefers-color-scheme:dark)` 覆寫 `:root`，兩套 token 要同步更新，不要只改亮色版。

## 1. 設計 token（取自 `index.html` 實際 `:root`）

亮色：

| 用途 | 值 |
|---|---|
| 背景 | `--bg:#f6f7f9` |
| 卡片 | `--card:#fff` |
| 主文字 | `--fg:#1a1c20` |
| 次文字 | `--fg2:#4a4f5a` |
| 弱化文字 | `--muted:#6b7280` |
| 分隔線 | `--line:#e5e7eb` |
| 強調（連結／進度條） | `--accent:#2563eb` |
| 完成勾選 | `--ok:#16a34a` |
| 本週高光 | `--hl:#f59e0b` |

內容類型色（`.t-xxx` 用 `--c` 這個 CSS 變數套色，見第 2 節）：
`--c-video:#dc2626`　`--c-rec:#4f46e5`　`--c-read:#2563eb`　`--c-lab:#7c3aed`　`--c-build:#c2410c`　`--c-note:#0f766e`　`--c-reading:#be185d`　`--c-algo:#15803d`　`--c-sys:#a16207`　`--c-beh:#0e7490`

深色模式每個 token 都有對應覆寫（例如 `--bg:#0e1014`、`--accent:#60a5fa`），新增 token 時兩邊都要補。

字體：無襯線 `-apple-system, BlinkMacSystemFont, "PingFang TC", "Noto Sans TC", "Microsoft JhengHei", "Heiti TC", "Segoe UI", sans-serif`；程式碼用 `ui-monospace, SFMono-Regular, Menlo, Consolas, monospace`。

## 2. 內容區塊類型（`.blk.t-xxx`）

每段內容用 `.blk` 卡片＋ `.t-xxx` 決定左側色條與標籤色：

| class | 對應內容 |
|---|---|
| `.t-read` | 讀書（CSAPP／DDIA 章節） |
| `.t-lab` | Lab 段落 |
| `.t-build` | 週末小專案 |
| `.t-note` | 筆記題目 |
| `.t-video` | 課程影片 |
| `.t-rec` | 推薦延伸資源 |
| `.t-reading` | Reading（論文／延伸閱讀） |
| `.t-algo` | 面試準備・演算法 |
| `.t-sys` | 面試準備・系統設計 |
| `.t-beh` | 面試準備・behavioral |

新增內容一律套用對應的 `.t-xxx`，不要混用或自己發明新的顏色。

## 3. 結構元件

- **進度條**（`.prog`／`.pr`／`.bar`）：整體進度用 `.pr.all`（粗條），分支（CSAPP 階段 vs 面試準備軌道）用 `.pr.p2`。
- **本週重點**（`.hero`）：用 `--hl` 橙框標出「現在進行中」的那一週，離開這一週要記得把 `.hero` 移到新的一週。
- **週清單**（`.week` + `<details>`）：每週一個可展開列，完成的週加 `.done`（標題加刪除線），目前這週加 `.current`（橙框高光）。
- **段落 navigation**（`.secnav`）：sticky 錨點列，手機寬度（≤640px）隱藏（`@media (max-width:640px){.secnav{display:none}}`），新增段落時要同步在這裡加一個錨點。
- **badge**：`.b-now`（進行中）、`.b-local`（只在本機、虛線框）、`.b-two`（跨兩週）——意義要跟實際狀態一致，不要照抄舊的。

## 4. 響應式

`@media (max-width:640px)` 已經處理了手機版的 padding 收窄、`.secnav` 隱藏、`.spec`／`.kv` 兩欄變單欄。新增區塊要檢查在手機寬度下有沒有橫向捲軸或擠在一起。
