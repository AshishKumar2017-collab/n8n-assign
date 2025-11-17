# 🌟 Part 2 — Screenshot Text for Every Node

(You can paste these directly into the assignment sheet next to each screenshot.)

## 📌 1. Manual Trigger — “Execute Workflow”  
**Screenshot Text:**
This node manually starts the workflow when the “Execute Workflow” button is clicked. No configuration required.

## 📌 2. Google Trends (HTTP Request) — “Fetch trending keywords”  
**Screenshot Text:**
This node fetches trending Google Trends keywords using a simple GET API call. Output is raw trending topics used in later filtering and formatting.

**Method:** GET  
**URL:** https://trends.google.com/trending?geo=IN

## 📌 3. YouTube Topic (HTTP Request) — “Fetch trending YouTube topics”  
**Screenshot Text:**
Fetches most popular YouTube videos using the YouTube Data API. Results are passed into the filtering code node.

**Method:** GET  
**URL:** YouTube Data API — Most Popular videos

## 📌 4. Code Node — “Filter YouTube and mapping”  
**Screenshot Text:**
Filters YouTube results to keep only AI-related topics and maps fields required for downstream AI agents.

**Logic:**
- Keep topics containing AI, Automation, ML, Robotics, ChatGPT
- Extract: title, channel, URL, published date

## 📌 5. Code Node — “Format Google Trends”  
**Screenshot Text:**
Normalizes Google Trends API data and filters only relevant AI-related trends.
Creates a clean, structured object for the Prompt Agent.

## 📌 6. Delay — “Throttle AI Requests”  
**Screenshot Text:**
Adds a 10-second delay to avoid rate-limits from the free OpenRouter API.
Prevents back-to-back AI calls from failing.

## 📌 7. Prompt Agent (OpenRouter) — “AI Prompt Generator”  
**Screenshot Text:**
Uses the OpenRouter LLM API (deepseek-chat-v3.1) to generate high-quality writing prompts.
Returns creative blog prompt text for the next agent.

## 📌 8. Content Creator Agent — “AI Content Generator”  
**Screenshot Text:**
Takes the prompt from the previous agent and generates the full blog content (title, blog body, structured text).
Uses OpenRouter model deepseek-chat-v3.1.

## 📌 9. Google Sheets — “Append Content Row”  
**Screenshot Text:**
Appends a new row to the sheet with the topic, prompt, blog, video link, timestamp, and status.

## 📌 10. Gmail — “Send Notification Email”  
**Screenshot Text:**
Sends an email to the editor including topic, generated blog, YouTube link, and timestamp.
Status automatically set to “Pending Review”.

