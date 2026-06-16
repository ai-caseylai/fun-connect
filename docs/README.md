# Fun Connect CMS — 內容管理系統

## 架構總覽

```
funconnect.techforliving.net
├── /                    → 雙語網站（靜態 HTML）
├── /admin/              → CMS 後台（密碼保護）
├── /content/zh-TW.json  → 中文內容資料
├── /content/en.json     → 英文內容資料
└── decap-cms-oauth.techforliving.net → API Worker（GitHub 讀寫代理）
```

## 技術棧

| 組件 | 技術 |
|------|------|
| 網站 | 靜態 HTML + CSS + JS（雙語 SPA） |
| CMS | 自製 HTML/JS 後台（即時預覽 + 雙語編輯） |
| API | Cloudflare Worker（GitHub API 代理） |
| 儲存 | GitHub (`ai-caseylai/fun-connect`) |
| 部署 | Cloudflare Pages (`od-fun-connect-242b6b55-18f`) |
| 域名 | `funconnect.techforliving.net` |

## 資料流

```
編輯者 → CMS後台(/admin) → Worker API → GitHub API → content/*.json
                                                    ↓
網站 ← Cloudflare Pages ← GitHub Push ← 手動部署
```

## 目錄結構

```
fun-connect/
├── index.html           → 網站主頁（含 CSS + JS + 翻譯）
├── admin/
│   ├── index.html       → CMS 後台
│   └── config.yml       → Decap CMS 設定（未使用）
├── content/
│   ├── zh-TW.json       → 繁體中文內容
│   └── en.json          → 英文內容
└── images/（未來）
```

## 開發命令

```bash
# 部署網站
CLOUDFLARE_API_TOKEN=xxx npx wrangler pages deploy . --project-name=od-fun-connect-242b6b55-18f

# 部署 Worker
cd decap-oauth-worker && npx wrangler deploy
```

## 環境變數（Worker）

| Secret | 說明 |
|--------|------|
| GITHUB_TOKEN | GitHub Personal Access Token（repo 權限） |
