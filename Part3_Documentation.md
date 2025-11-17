# 🌟 Part 3 — Full Technical Documentation

## 📘 Technical Documentation: AI-Powered Multi-Agent Content Pipeline (n8n)

A fully automated multi-agent workflow that generates AI-driven content using Google Trends + YouTube data, processes it through LLM agents, logs results in Google Sheets, and sends editor notifications.

---

## 🔧 Workflow Overview

The workflow performs:

- Fetching trends from Google
- Fetching trending video topics from YouTube
- Filtering and normalizing data
- Generating content prompts
- Generating blog content
- Logging results to Google Sheets
- Sending automated emails

---

## 🧩 Node-by-Node Explanation

(Part 1 already provided — linked to this section.)

### 1. Manual Trigger
Starts the workflow manually via n8n.

### 2. Google Trends API Node
Fetches trending search topics from India using the Google Trends public page endpoint.

**Method:** GET  
**URL:** https://trends.google.com/trending?geo=IN

### 3. YouTube Trending API
Pulls trending YouTube videos using the official YouTube Data API.

### 4. Filter YouTube Node
JavaScript-based filtering:
- Keeps only AI/automation related topics
- Extracts meaningful metadata
- Produces a clean JSON output

### 5. Format Google Trends Node
Normalizes the raw Google Trends data into structured fields:
- topic
- trendScore
- relatedQueries

Then filters for AI-related words.

### 6. Delay Node
Prevents rate-limiting when hitting OpenRouter’s free endpoints.

### 7. Prompt Agent (OpenRouter)
Uses deepseek-chat-v3.1 to convert topic data → creative writing prompt.

**System Instruction:**
“Generate a clean, high-quality content prompt for a 300-word blog post.”

**Output:** prompt_output

### 8. Content Creator Agent
Again uses deepseek-chat-v3.1 to write the blog post.

**Output:** content_output

### 9. Google Sheets Logging Node
Appends:
- topic
- prompt
- blog
- video link
- timestamp
- status

### 10. Gmail Notification Node
Sends email with:
- Topic
- Blog
- YouTube link
- Timestamp
- Status: Pending Review

---

## 📂 Data Flow Summary

Google Trends + YouTube  
⬇
Filtering + Formatting  
⬇
Prompt Agent  
⬇
Content Generator  
⬇
Sheets Logging  
⬇
Email Notification

---

## 🔐 Credentials Required

| Service        | Required | Notes               |
|----------------|---------|-------------------|
| YouTube API    | Yes     | API Key           |
| Google Sheets  | Yes     | OAuth             |
| Gmail          | Yes     | OAuth             |
| OpenRouter     | Yes     | Free models |

---

## 🎯 Output Description

Every run produces:
- One AI-generated prompt
- One complete blog post
- One related YouTube video
- One entry logged in Google Sheets
- One email notification
