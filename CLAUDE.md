# District 18 清潔檢查紀錄系統
## 部署現況（重要 — 開發前先讀）
- **Production**：https://district18.chad-ai-lab.com
- **部署方式**：Chad（仲文，repo collaborator）的 Mac mini 跑 cron pull-based，每 2 分鐘從 `main` 自動拉最新版
- **HTTPS**：Cloudflare 自動接手
- **Deploy key**：repo Settings → Deploy keys 已有一把名為 `mac-mini-deploy` 的 **read-only** key，由 Chad 持有
## Chris 的工作流程
直接改 `index.html` → `git push` → 最多 2 分鐘自動上線。
不需要：build 步驟、commit `dist/`、設 GitHub Pages、設 Actions、本地起伺服器看 production。
## 給 AI 助手的注意事項
請**不要**做以下事情（已由 Chad 處理或不適用，做了反而會破壞既有部署）：
- ❌ 啟用 GitHub Pages — 已用 Mac mini 部署，會衝突
- ❌ 建議或設定 GitHub Actions self-hosted runner — 純靜態不需要，且會擴大攻擊面
- ❌ 新增別的 Deploy key 或把現有 `mac-mini-deploy` 改成 Allow write — 會破壞 Chad 的 read-only 隔離設計
- ❌ 改 repo 名稱、預設分支名（`main`）— 會打斷 cron pull
- ❌ 在 repo 加 workflow 自動部署到別處 — 會雙寫衝突
如果出現任何**部署、CI/CD、runner、Pages、DNS、SSL** 相關需求，請先告訴 Chris「這需要 Chad 處理」，不要自行設定。
純前端改動（HTML/CSS/JS、新功能、UI 調整、bug 修復）正常做即可。
## 快速驗證
push 後想確認是否上線：
```bash
curl -sI https://district18.chad-ai-lab.com | head -1
# 應該回 HTTP/1.1 200 OK
```
