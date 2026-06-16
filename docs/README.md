# Fun Connect CMS — 內容管理系統

## 架構總覽

```
funconnect.techforliving.net
├── /                    → 雙語網站（靜態 HTML）
├── /admin/              → CMS 後台（密碼保護）
├── /content/zh-TW.json  → 中文內容（Cloudflare KV 即時讀取）
└── /content/en.json     → 英文內容（Cloudflare KV 即時讀取）

decap-cms-oauth.techforliving.net → API Worker（KV 讀寫）
```

## 技術棧

| 組件 | 技術 |
|------|------|
| 網站 | 靜態 HTML + CSS + JS（雙語 SPA） |
| CMS | 自製 HTML/JS 後台（即時預覽 + 雙語編輯） |
| API | Cloudflare Worker（KV 讀寫） |
| 儲存 | **Cloudflare KV**（即時生效，無需部署） |
| 部署 | Cloudflare Pages (`od-fun-connect-242b6b55-18f`) |
| 域名 | `funconnect.techforliving.net` |

## 資料流

```
編輯者 → CMS後台(/admin) → Worker API → Cloudflare KV
                                              ↓
訪客 → 網站(/) → Worker(/content/*) → Cloudflare KV → 即時顯示
```

**儲存後立即生效，無需重新部署！**

## 目錄結構

```
fun-connect/
├── index.html           → 網站主頁（含 CSS + JS + 內容載入器）
├── admin/
│   └── index.html       → CMS 後台
├── content/             → （KV 中，非靜態檔案）
└── docs/
    ├── README.md
    ├── USER-GUIDE.md
    └── screenshots/
```

## Worker 端點

| 方法 | 路徑 | 用途 |
|------|------|------|
| GET | `/content/{lang}.json` | 網站/CMS 讀取內容 |
| PUT | `/content/{lang}.json` | CMS 寫入內容 `{ content: "..." }` |

## 開發命令

```bash
# 部署網站
CLOUDFLARE_API_TOKEN=xxx npx wrangler pages deploy . --project-name=od-fun-connect-242b6b55-18f

# 部署 Worker
cd decap-oauth-worker && npx wrangler deploy
```
