# Lead Sniper – GitHub High-Value Leads Tracker

## Overview
**Lead Sniper** is an automated workflow built in **n8n** that tracks high-value leads on GitHub. It detects when influential users star a specified repository, analyzes their profile, and sends a formatted alert to **Slack** (or Discord).  

High-value leads are defined as GitHub users with:  
- **>100 followers** OR  
- **>50 public repositories**  

An AI/LLM node generates a **1-sentence sales pitch** from the user’s bio and company information for outreach.

---

## Features
- Monitors GitHub stargazers (Webhook or Polling mode).  
- Fetches full GitHub profile for each new stargazer.  
- Filters users based on followers and public repos.  
- Generates AI-powered sales pitch using OpenAI GPT-3.5.  
- Sends formatted messages to Slack or Discord.  
- Handles GitHub API rate limits gracefully.  

---

## Workflow Nodes (n8n)
1. **Trigger Node**:  
   - Webhook (preferred) or Cron/Polling for repository stars.  

2. **Fetch User Profile**:  
   - Uses GitHub API `/users/{username}` with authentication.  

3. **High-Value Lead Filter (IF Node)**:  
   - Checks if `followers > 100` OR `public_repos > 50`.  

4. **AI Node**:  
   - Generates a 1-sentence sales pitch based on `bio` and `company`.  

5. **Slack/Discord Node**:  
   - Sends formatted message with:  
     - Name, GitHub link, company, bio, followers, public repos  
     - AI-generated sales pitch  

6. **Set Last Star ID** *(Polling version only)*:  
   - Stores the last processed star ID to prevent duplicates.  

---

## Setup Instructions

### 1. Prerequisites
- n8n Desktop or Cloud instance  
- GitHub Personal Access Token (read:user scope)  
- Slack Incoming Webhook URL (or Discord Webhook)  
- OpenAI API Key (for AI/LLM node)  

### 2. Import Workflow
1. Download the `workflow.json` file.  
2. Open n8n → **Workflows → Import → Paste JSON**.  

### 3. Configure Credentials
- Replace `YOUR_GITHUB_PERSONAL_ACCESS_TOKEN` with your token.  
- Replace `YOUR_SLACK_WEBHOOK_URL` with your Slack webhook.  
- Configure OpenAI credentials in n8n.  

### 4. Run Workflow
- **Webhook version:** Triggered automatically by new stars.  
- **Polling version:** Runs every X minutes (configured in Cron node).  

---

## Logic Handling

### GitHub API Rate Limits
- Uses **Personal Access Token** (5000 requests/hour).  
- Checks headers `X-RateLimit-Remaining` and `X-RateLimit-Reset`.  
- Optional: Use **Wait node** if remaining requests are low.  

### Duplicate Handling (Polling)
- Stores **last processed star ID** in workflow variable.  
- Only processes stars with IDs greater than the last stored value.  

---

## Sample Slack Message
