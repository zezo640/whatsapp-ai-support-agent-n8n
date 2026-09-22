# WhatsApp AI Support Agent With Human Escalation

A 24/7 WhatsApp support agent that answers customer questions from a company knowledge base — and hands the conversation to a human when it cannot answer confidently.

Built with **n8n** for orchestration and **OpenAI** for the reasoning layer.

> Self-directed project, built July 2026. This repository documents the system design; see *Repository contents* below.

---

## The problem

Most customer questions are the same handful of questions. Answering them manually costs a team its whole day, and questions that arrive at night wait until morning. But a support bot that answers *everything* confidently is worse than no bot — it will invent an answer on the cases that actually matter.

## How it works

```
Incoming WhatsApp message
        |
        v
  [ Context assembly ] -- conversation history + company knowledge base
        |
        v
  [ OpenAI response generation ] -- grounded in the knowledge base,
        |                            not open-ended
        v
  [ Confidence check ] -- can this be answered reliably?
        |
        +--> Yes --> reply on WhatsApp
        |
        +--> No  --> escalate to the human team on Telegram
                     with the full conversation context
```

### Design decisions

**Escalation is the feature, not the fallback.** The interesting design question is not "can the model answer?" but "does it know when it cannot?" Unresolved cases route to a human with the conversation attached, so the handoff does not restart the customer from zero.

**Answers are grounded in the knowledge base.** Replies are generated against company content rather than the model's open-ended knowledge. A support bot that improvises policy is a liability.

**Escalation goes to Telegram, not another WhatsApp thread.** The operator channel is kept separate from the customer channel, so the team has one queue to watch.

## Stack

| Component | Role |
|---|---|
| n8n | Workflow orchestration and routing logic |
| OpenAI | Response generation against company knowledge |
| WhatsApp | Customer-facing channel |
| Telegram | Human operator escalation channel |

## Design goals

These are the outcomes the system was built to achieve:

- Cover the majority of routine, repeated questions automatically
- Provide round-the-clock coverage without staffing gaps
- Keep a human in the loop for every case the model cannot handle reliably

## Repository contents

This README documents the architecture and the reasoning behind it. The exported n8n workflow JSON is being added.

---

**Ziad Hady** — AI Automation & Integration Specialist
Portfolio: https://ziad-portfolio-omega.vercel.app/
