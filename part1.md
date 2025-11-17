# 🌟 Part 1 — Node‑by‑Node Export (Beautifully Formatted)

This document describes each node of the n8n workflow using clean formatting, icons, and structured sections.

---

## 🚀 1. **Manual Trigger — “Execute Workflow”**
**Type:** Manual Trigger  
**Purpose:** Starts the workflow when you click *Execute Workflow* in n8n.  
**Notes:** No configuration needed.

---


## 📈 3. **Google Trends — “Fetch trending Google Trends keywords”**
**Type:** HTTP Request  
**Purpose:** Fetch trending Google Trends keywords
**Recommended Config:**
- **Method:** GET  
- **URL:** `https://trends.google.com/trending?geo=IN`  

---

## ▶️ 4. **YouTube Topic — “Fetch trending YouTube video topics”**
**Type:** HTTP Request
**Purpose:** Fetch trending YouTube video topics.  
**Recommended Config:**
- **Method:** GET  
- **URL:** `https://www.googleapis.com/youtube/v3/videos?chart=mostPopular&regionCode=US&part=snippet&maxResults=10&key=AIzaSyADMljdV0h0xDLzgQkYkUR_AGg-x8eei6w`  


---

## 🧹 5. **Code Node — “Filter YouTube and mapping”**
**Purpose:** Filter YouTube API output based on keywords.
**Logic:**
- Keep only videos whose titles contain AI, Automation, ML, Robotics, or ChatGPT.
- Extract title, channel, URL, and published date for further use.

```js
return $json["items"]
  .filter(item => /AI|Automation|ML|Robotics|ChatGPT/i.test(item.snippet.title))
  .map(item => ({
    title: item.snippet.title,
    channel: item.snippet.channelTitle,
    url: `https://www.youtube.com/watch?v=${item.id}`,
    publishedAt: item.snippet.publishedAt
  }));

```

---

## 🧩 6. **Code Node — “Format Google Trends”**
**Purpose:** Normalize Google Trends output for the AI agent and filter relevant keywords (AI, Automation, ML, Robotics, ChatGPT). 

```js
// Get all input items from previous node
const trends = $input.all();

// Process each trend item
return trends
  .map(item => {
    const t = item.json;
    return {
      json: {
        // Take keyword or title, default to 'unknown'
        topic: t.keyword || t.title || "unknown",
        // Trend score or default 0
        trendScore: t.score || 0,
        // Top 5 related queries
        relatedQueries: (t.relatedQueries || []).slice(0, 5)
      }
    };
  })
  // Filter only AI/Automation/ML/Robotics/ChatGPT related keywords
  .filter(item => /AI|Automation|ML|Robotics|ChatGPT/i.test(item.json.topic));


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
