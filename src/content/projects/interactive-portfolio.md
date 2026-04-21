---
title: "Interactive Portfolio — AI Recruiter Copilot"
description: "A dynamic portfolio that acts as my AI proxy — processes 10+ years of product leadership data to evaluate job fit and answer recruiter queries."
tech: ["Next.js", "Gemini API", "Netlify Functions", "RAG"]
url: "https://cv-widget.netlify.app/"
status: "live"
order: 3
hasMermaid: false
---

A dynamic, interactive portfolio that acts as my personal AI recruiter proxy. It processes my 10+ years of product leadership data to instantly evaluate job fit and answer recruiter queries.

**[cv-widget.netlify.app](https://cv-widget.netlify.app/)**

---

## What it does

**Recruiter Copilot (RAG)** — a chat interface that acts as my AI proxy. It strictly uses a dataset of my CV metrics and Notion notes to answer questions about my experience, scale, and impact. It doesn't hallucinate: if the answer isn't in the dataset, it says so.

**AI Job Fit Analyzer** — recruiters paste a job description or upload a PDF/TXT file directly. Gemini's multimodal capabilities read the document and generate a structured report: match score, overlapping strengths, potential gaps, and specific interview questions designed to probe my experience limits honestly.

**Smart model routing** — instead of breaking on rate limits (429 errors), the system dynamically fetches available Gemini models at runtime. If one fails, it falls back to the next available option. The user experience stays uninterrupted.

---

## Engineering decisions

**Secure serverless architecture** — runs entirely on Netlify Functions (Node.js) as a secure backend. API keys never hit the client. Built-in error handling catches unhandled exceptions and always returns a valid response rather than generic 502 crashes.

**Complete outputs** — a common failure mode with LLM-generated reports is mid-sentence truncation. The system uses strict prompt structures, increased token limits, and custom logic to stitch together multi-part API responses. The recruiter always gets the full picture.

**RAG over fine-tuning** — the CV data lives in a structured dataset rather than being baked into the model. This means the portfolio stays accurate as my experience grows: update the dataset, not the model.

---

**Pipeline:** Gemini Canvas → Cursor → GitHub → Netlify | Powered by Gemini 2.5 Flash
