# Agentic AI Customer 360

> **Inter IIT NLP - Mid-Term Research Submission**

An Agentic AI system for building a continuously updated **Customer 360 view** from multiple customer signals and using specialized agents to infer customer state and make bounded, explainable decisions.

---

## 🎯 Problem

Traditional customer systems often process signals such as:

- Transaction and billing activity
- Support interactions
- App/Web usage
- KYC and compliance updates

independently.

This project explores an **agentic multi-agent architecture** where these signals are processed by specialized agents, combined into a shared customer state, and used for downstream reasoning, decision-making, and human-approved actions.

---

## 🏗️ Preliminary Architecture

The current architecture follows a combination of **parallel processing, sequential handoffs, conditional debate, and human-in-the-loop approval**.

### High-Level Flow

```text
Customer Data Sources
        ↓
Event Stream / Live Processing
        ↓
Agent Trigger Router
        ↓
┌──────────────┬──────────────┬────────────────┬──────────────┐
│ Usage Agent  │ Support      │ Transaction    │ KYC Agent   │
│              │ Agent        │ Agent          │             │
└──────────────┴──────────────┴────────────────┴──────────────┘
        ↓
Per-Customer State Board
        ↓
Synthesis / Correlation
        ↓
Life-Event Inference
        ↓
Conflict / Ambiguity Check
        ↓
Conditional Multi-Agent Debate
        ↓
Resolved Customer State
        ↓
Offer / Eligibility
        ↓
Retention / Action Decision
        ↓
Draft Action
        ↓
Critique / Compliance Refinement
        ↓
Human-in-the-Loop
        ↓
Action / Escalation Execution
