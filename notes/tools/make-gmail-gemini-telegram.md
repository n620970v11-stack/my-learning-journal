# Gmail × Gemini AI × Telegram 自動摘要流程

## 用途
監聽 Gmail 未讀信件 → Gemini AI 整理成結構化摘要 → 推送到 Telegram

## 流程架構
Gmail (Watch emails) → Google Gemini AI → Tools (Sleep 2s) → Telegram Bot

## 各節點設定

### 1. Gmail — Watch emails
- Folder：All mail
- Filter type：Simple filter
- Criteria：Only unread messages
- Mark as read when fetched：Yes（抓取後自動標為已讀，避免重複觸發）
- Include spam/trash：No
- Limit：20 封／次

### 2. Google Gemini AI — Generate a response
- Model：Gemini 3.1 Flash Lite
- User Prompt：
  請幫我將以下信件內容整理成重點摘要：
  發信人：{{From (name)}}
  信件主旨：{{Headers: Subject}}
  信件內文：{{Full text body}}
- System Instruction：科技產品專家角色，輸出 HTML 格式結構化摘要
  - 🚀 本日重點事件 (Market Shakers)
  - 🛠️ 新技術與產品動態
  - 🎯 市場訊號與解讀 (Signals & Insights)
  - 排版規則：完全使用 HTML 標籤（<b>粗體</b>、換行），禁用 Markdown 星號

### 3. Tools — Sleep
- 延遲：2 秒

### 4. Telegram Bot — Send a Text Message
- Text：綁定 Gemini AI 輸出（Candidates[].Content.Parts[].Text）
- Parse Mode：HTML（配合 Gemini 輸出格式，確保粗體在手機正確顯示）
- Message Thread ID：空白

## 設計重點
- System Instruction 強制 HTML 排版 + Telegram 設 Parse Mode HTML，兩端格式對齊
- Sleep 2s 避免 API 速率問題
- Mark as read = Yes 防止重複處理同一封信
