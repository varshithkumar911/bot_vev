# Best Friend Telegram Bot (n8n + Groq)

An n8n workflow that turns a Telegram bot into "Buddy", a warm, funny best-friend chatbot powered by Groq. Each Telegram chat has its own memory (last 10 interactions).

## Flow

```
Telegram Trigger -> Has text? --true--> Buddy Agent -> Send Reply
                              \-false-> Send Text-Only Notice
Buddy Agent sub-nodes: Groq Chat Model, Simple Memory
```

- Text messages go to an AI Agent (Groq `openai/gpt-oss-120b`, temperature 0.8) and the reply is sent back as plain text.
- Non-text messages (stickers, photos, etc.) get a friendly "text only" reply.
- Agent node retries up to 3 times, 2000 ms apart.

## Setup

1. In n8n: **Workflows -> Import from File** and choose `workflows/best-friend-telegram-bot.json`.
2. Create or select credentials (none are stored in this repo):
   - **Telegram API** on: Telegram Trigger, Send Reply, Send Text-Only Notice
   - **Groq API** on: Groq Chat Model
3. Toggle the workflow to **Active**.
4. Telegram requires a public HTTPS URL for webhooks. n8n Cloud has one. If self-hosting, set `WEBHOOK_URL` to your public HTTPS address (e.g. via ngrok or Cloudflare Tunnel).

## Model note

`llama-3.3-70b-versatile` was retired by Groq for free and developer tiers on 2026-08-16, so this workflow uses `openai/gpt-oss-120b`. Change it in the Groq Chat Model node if your account has a different model available. Check https://console.groq.com/docs/models.

## Test

Send `/start`, then `hi`, then a sticker to your bot. Expect a friendly reply, then the "text only" reply.

## Security

Never commit API keys or exported credentials. Credentials are intentionally omitted from the workflow JSON.
