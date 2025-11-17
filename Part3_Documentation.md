# 📘 Part 3 — Full Technical Documentation

## Overview  
This document details the architecture, logic, and integrations of the n8n multi‑agent content automation workflow.

---

## 🏗 Workflow Architecture  
The workflow uses **two AI agents**, **two data APIs**, and **two delivery outputs**:

- Google Trends API  
- YouTube Data API  
- AI Prompt Agent  
- AI Content Creator Agent  
- Google Sheets Output  
- Gmail Notification

---

## 🔄 Data Flow Summary  
1. User triggers workflow manually  
2. Topic is fetched  
3. Google Trends data collected  
4. YouTube videos collected  
5. Filter + formatting via code nodes  
6. AI generates prompt  
7. AI generates full content  
8. Data stored in Sheets  
9. Email notification sent  

---

## 🧠 Agent Logic  
### Prompt Agent  
Creates content outline + candidate ideas.

### Content Creator Agent  
Expands outline into full content package.

---

## 🧪 Error Handling  
- HTTP status checks  
- Empty response filters  
- Optional retry loops  

---

## 🔐 Credentials  
All API keys stored using n8n Credentials Manager.

---

