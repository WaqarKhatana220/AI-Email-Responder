## 📬 AI Email Processing & Auto-Reply Workflow (n8n + OpenAI + Pinecone)

This repository contains an automated email classification and smart auto-reply system built using n8n, OpenAI GPT models, and Pinecone vector database.
It is designed to intelligently classify incoming emails, decide the correct action, and generate accurate AI-written replies based on your company’s knowledge base.

## 🚀 Features
✓ Automated Email Intake

### Polls Gmail inbox every 1 minute.

### AI Email Classification (Strict JSON Output)

Emails are classified into exactly one of these categories:

- General inquiry

- Pricing request

- Urgent human attention

- Spam or irrelevant

The AI returns JSON in this format:

{
  "classification": "Pricing request",
  "email_body": "Cost?",
  "email_subject": "Price",
  "email_from": "example@gmail.com",
  "email_id": "19b13e72...",
  "email_threadId": "19b13e72..."
}

✓ Smart Routing Based on Classification

Spam / Irrelevant: ignored

Urgent: forwarded to a human instantly

General / Pricing: passed to AI for auto-draft reply

💡 AI-Generated Reply Using Vector Database

For reply-eligible emails:

The workflow retrieves relevant company info from Pinecone:

Services

Pricing

Policies

Company introduction

GPT uses only this data to write a safe, accurate reply.

n8n creates a Gmail draft—never auto-sending.

Final JSON format used for drafting:
{
  "content": "email draft you produce",
  "email_subject": "{{ $json.output[0].content[0].text.email_subject }}",
  "email_id": "{{ $json.output[0].content[0].text.email_id }}",
  "email_threadId": "{{ $json.output[0].content[0].text.email_threadId }}",
  "email_from": "{{ $json.output[0].content[0].text.email_from }}"
}


All values are strings.

🧠 Workflow Architecture
Gmail Trigger
      ↓
OpenAI Email Classifier (strict JSON)
      ↓
Decision Router
      ├── Spam → Ignore
      ├── Urgent → Forward to human
      └── General / Pricing → AI response generator
                ↓
        Pinecone Vector Search
                ↓
        OpenAI Draft Generator
                ↓
        Gmail Draft Creator

🧱 Tech Stack
Component	Description
n8n	Main automation engine
OpenAI GPT-4o / GPT-4o-mini	Classification + reply generation
Pinecone	Vector database with company knowledge
OpenAI Embeddings	Converts documents to vectors
Gmail API	Inbox polling + draft creation
