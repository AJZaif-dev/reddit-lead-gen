# Reddit AI Lead Generation & Social Listening Engine

An automated intent-based lead generation workflow for n8n. This system monitors targeted subreddits for active pain points and buying signals, filters and cleans the data, uses an AI Agent to qualify the intent, and routes the high-value leads directly to your inbox or communication channel.

---

## How It Works

The workflow executes a linear pipeline from data ingestion to lead qualification and notification:

```
[Schedule] ➔ [RSS Read] ➔ [Limit] ➔ [Data Clean Up] ➔ [AI Agent + Gemini] ➔ [Email/Notification]

```

1. **Trigger (`Every Day`):** Runs automatically on a daily schedule (or can be adjusted to hourly for real-time monitoring).
2. **Ingestion (`RSS Read`):** Polls the public RSS feeds of specified subreddits to fetch the newest posts without requiring official Reddit API keys.
3. **Throttling (`Limit`):** *Optional.* Limits the number of posts processed per run to optimize API token usage. This can be deactivated if you want to parse 100% of the feed volume.
4. **Data Normalization (`Data Clean Up`):** A custom JavaScript node that strips raw HTML, normalizes text formatting, and structures the payload (Title, Author, Content) for the LLM.
5. **Intent Analysis (`AI Agent`):** Leverages the **Google Gemini** model to evaluate the cleaned post details. The agent judges if the poster is actively looking for a solution, experiencing a relevant pain point, or fits your Ideal Customer Profile (ICP).
6. **Delivery (`Email`):** Automatically dispatches the qualified lead details (including the direct link to the Reddit thread) via Gmail.

---

## Prerequisites & Configuration

To deploy this workflow, you need to configure the following nodes:

### 1. RSS Read Node

* **URL:** Set this to the target subreddit's new posts RSS feed.
* *Example:* `https://www.reddit.com/r/sales/new/.rss`
* *Multi-subreddit tip:* You can combine feeds using `+` (e.g., `https://www.reddit.com/r/sales+marketing/new/.rss`).



### 2. AI Agent & Gemini Model

* **Credentials:** Requires a Google Gemini API key.
* **System Prompt:** Configure the agent with a precise persona.
* *Example prompt:* *"You are a social listening assistant. Analyze the incoming Reddit post title and content. Determine if the user is looking for [Your Service/Product]. If they are experiencing [Specific Pain Point], extract their core problem and mark them as a Qualified Lead. If they are just sharing a meme or blog post, reject them."*



### 3. Email Node (Or Alternatives)

* **Credentials:** Connect your Gmail account via OAuth2.
* **Customization:** Map the output fields from the AI Agent (e.g., Lead Summary, Urgency Level, Reddit Link) directly into the email body text.

---

## Customization & Extensions

This workflow is modular and can be adapted depending on your tech stack:

* **Alternative Output Channels:** Swap out the Gmail node for a **Discord**, **Telegram**, or **Slack** node to route leads straight into your team's chat channels.
* **CRM / Storage Logging:** Add a **Google Sheets**, **Airtable**, or **Notion** node right after the AI Agent to log qualified leads into a dashboard for tracking.
* **Strict Keyword Filtering:** If you want to save AI tokens, add an n8n **Filter** node right after the RSS Read node to only pass posts containing specific keywords (e.g., *"recommendation"*, *"looking for"*, *"how do I"*).
