# 🎥 Part 4 — Loom Video Script

## 🎬 Loom Video Script: AI Multi-Agent Content Pipeline

### 🎤 INTRO
Hi, welcome to this walkthrough of my AI-powered multi-agent content automation workflow built in n8n.  
This system automatically pulls trending topics, generates AI-based content, logs everything into Google Sheets, and notifies the editor — all without manual work.

### 🟦 Step 1 — Workflow Overview
Here is the full workflow. It starts with a manual trigger, fetches Google Trends and YouTube trends, processes the data, runs two AI agents, and finally logs everything and sends notifications.

### 🟦 Step 2 — Google Trends Node
This node fetches trending search keywords from Google Trends.  
We target India, but you can change the region as needed.

### 🟦 Step 3 — YouTube Trending Node
This retrieves trending YouTube videos using the YouTube Data API.

### 🟦 Step 4 — Filtering Logic
The next node filters YouTube results to keep only AI-related content like:
- AI
- Automation
- Machine Learning
- Robotics
- ChatGPT

### 🟦 Step 5 — Formatting Google Trends
This node cleans and formats the Google Trends data, preparing it for the AI agent.

### 🟦 Step 6 — Delay
A simple delay to avoid rate-limit issues with our free LLM API provider.

### 🟦 Step 7 — Prompt Agent (OpenRouter)
This AI agent takes the filtered trend + video data and generates a precise writing prompt for a blog post.

### 🟦 Step 8 — Content Creator Agent
This second AI agent uses the prompt to write the full blog content.

### 🟦 Step 9 — Google Sheets Logging
The generated content, along with the related YouTube video link, gets saved automatically into Google Sheets.

### 🟦 Step 10 — Gmail Notification
Finally, the system sends an email to the editor including:
- Topic
- Full blog content
- Link to the chosen YouTube video
- Timestamp
- Status: Pending Review

### 🎤 OUTRO
And that’s the complete automated content pipeline — from trend collection all the way to content generation and notification.  
This setup saves hours of manual work and ensures a constant flow of fresh content ideas.
