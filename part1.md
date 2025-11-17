# 🌟 Part 1 — Node‑by‑Node Export (Beautifully Formatted)

This document describes each node of the n8n workflow using clean formatting, icons, and structured sections.

---

## 🚀 1. **Manual Trigger — “Execute Workflow”**
**Type:** Manual Trigger  
**Purpose:** Starts the workflow when you click *Execute Workflow* in n8n.  
**Notes:** No configuration needed.

---

## 🌐 2. **HTTP Request — “Fetch Input Parameters”**
**Type:** HTTP Request  
**Purpose:** Fetches initial parameters before Google Trends & YouTube API calls.  
**Recommended Config:**
- **Method:** GET  
- **URL:** `https://api.example.com/inputs`  
- **Headers:** Authorization Bearer Token  
- **Output:** JSON with `topic`

---

## 📈 3. **Google Trends — “Google Trends Fetcher”**
**Type:** HTTP Request (or custom Trends node)  
**Purpose:** Retrieves trend score + related searches for the input topic.

---

## ▶️ 4. **YouTube Topic — “YouTube Topic Search”**
**Type:** HTTP Request (YouTube Data API v3)  
**Purpose:** Fetch relevant recent videos about the topic.  
**Params:**
- `q = {{ $json.topic }}`
- `part = snippet`
- `maxResults = 10`
- `type = video`

---

## 🧹 5. **Code Node — “Filter YouTube Results”**
**Purpose:** Filters YouTube API output.  
**Logic:**
- Remove videos with low views  
- Keep trending or “how to” content  

```js
const items = $input.all();
const minViews = 10000;

return items
  .filter(i => {
    const stats = i.json.statistics || {};
    const views = Number(stats.viewCount || 0);
    const title = (i.json.snippet.title || "").toLowerCase();
    return views >= minViews || title.includes("trend") || title.includes("how to");
  })
  .map(i => ({ json: i.json }));
```

---

## 🧩 6. **Code Node — “Format Google Trends”**
**Purpose:** Normalize Trends data for the AI agent.  

```js
const t = $input.item.json;

return [{
  json: {
    topic: t.keyword || "unknown",
    trendScore: t.score || 0,
    relatedQueries: (t.relatedQueries || []).slice(0, 5)
  }
}];
```

---

## ⏳ 7. **Delay Node — “Throttle AI Requests”**
**Type:** Delay  
**Purpose:** Prevents rate‑limits when using free AI endpoints.  
**Duration:** 10 seconds  

---

## 🤖 8. **Prompt Agent — “AI Prompt Generator (Google Router)”**
**Purpose:** Generates outlines + prompt seeds for the topic.  
**Payload Example:**
```json
{
  "type": "prompt_generation",
  "topic": "{{ $json.topic }}",
  "trendScore": {{ $json.trendScore }},
  "relatedQueries": {{ $json.relatedQueries }}
}
```

---

## ✍️ 9. **Content Creator Agent — “AI Content Generator”**
**Purpose:** Produces full content (title, script, tags, description).  

---

## 📊 10. **Google Sheets — “Append Content Row”**
**Operation:** Append Row  
**Columns:**  
- Title  
- Description  
- Tags  
- GeneratedAt  
- TrendScore  

---

## 📧 11. **Gmail — “Send Notification Email”**
**Purpose:** Delivers completed content to editor/team.

---

## 🔗 12. **Merge Node (if used)**
**Mode:** *Wait for All Inputs*  
**Purpose:** Ensures Trends + YouTube data combine properly.

---

## ⚠️ 13. **Error Handler Node (optional)**
Routes failures to retry or admin alert.

---
