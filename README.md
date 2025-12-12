## 📬 AI Email Processing & Auto-Reply Workflow (n8n + OpenAI + Pinecone)

This repository contains an automated email classification and smart auto-reply system built using n8n, OpenAI GPT models, and Pinecone vector database.
It is designed to intelligently classify incoming emails, decide the correct action, and generate accurate AI-written replies based on your company’s knowledge base.

## 🚀 Features
### ✓ Automated Email Intake

Polls Gmail inbox every 1 minute.

### ✓ AI Email Classification (Strict JSON Output)

Emails are classified into exactly one of these categories:

- General inquiry

- Pricing request

- Urgent human attention

- Spam or irrelevant

The AI returns JSON in this format:

### ✓ Smart Routing Based on Classification

- **Spam / Irrelevant:** ignored

- **Urgent:** forwarded to a human instantly

- **General / Pricing:** passed to AI for auto-draft reply

## 💡 AI-Generated Reply Using Vector Database

For reply-eligible emails:

The workflow retrieves relevant company info from Pinecone:

- Services

- Pricing

- Policies

- Company introduction

GPT uses only this data to write a safe, accurate reply.

n8n creates a Gmail draft—never auto-sending.

## 🧠 Workflow Architecture
```
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
```

## 🧱 Tech Stack
| Tool / Service            | Purpose                                      |
|---------------------------|----------------------------------------------|
| **n8n**                   | Main automation engine                       |
| **OpenAI Chat Models** | Classification + reply generation     |
| **Pinecone**              | Vector database with company knowledge       |
| **OpenAI Embeddings**     | Converts documents to vectors               |
| **Gmail API**             | Inbox polling + draft creation               |

