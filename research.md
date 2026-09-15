# Research Log — Agentic AI Customer 360

This file contains the main papers, blogs, and resources I studied
while designing the preliminary architecture for the Agentic AI
Customer 360 problem.

The focus was on understanding multi-agent coordination, memory,
conflict resolution, human oversight, and how to combine these ideas
for the requirements of the problem statement.

---

## 1. Problem Statement

### Natural Language Processing — Agentic AI Customer 360

**Resource:** Inter IIT NLP Problem Statement

**What I used:**
- Parallel specialist agents for different customer signals
- Sequential handoffs between dependent reasoning stages
- Swarm / shared-state coordination
- Agent debate for disagreement
- Critique-refiner before action
- Event-based, time-based and agent-dependent triggers
- Human-in-the-loop approval before consequential actions
- Customer state, memory, guardrails and auditability

**Architecture impact:**

The problem statement is the main source of the architecture
requirements. The final design combines parallel specialist agents,
shared customer state, sequential reasoning, conditional debate,
action refinement and HITL.

---

## 2. MemGPT: Towards LLMs as Operating Systems

**Authors:** Charles Packer et al., 2023

**Link:** https://arxiv.org/abs/2310.08560

**What I studied:**

MemGPT addresses the limited context available to LLMs by using
virtual context management and different memory levels. Information
outside the active context can be accessed when required.

**What I used:**

I used the idea of keeping the active context focused instead of
putting the complete customer history into every agent prompt.
Relevant historical information should be retrieved when needed.

**Architecture impact:**

This motivated the use of Working Memory, Episodic Memory
(Vector DB) and Semantic Memory in the architecture.

The exact three-part memory separation is my adaptation to the
Customer 360 problem, not a taxonomy directly proposed by MemGPT.

---

## 3. Improving Factuality and Reasoning in Language Models through
## Multiagent Debate

**Authors:** Yilun Du et al., 2024

**Link:** https://proceedings.mlr.press/v235/du24e.html

**What I studied:**

The paper studies multiple language-model instances that propose and
debate their answers over multiple rounds before reaching a final
answer.

**What I used:**

I used debate as a mechanism for examining competing conclusions when
different parts of the system produce conflicting evidence.

**Architecture impact:**

Agent Debate and Resolution is placed behind a
conflict/ambiguity check.

Therefore, debate is not run for every customer. The conditional
trigger is my design choice, while the debate mechanism is motivated
by this work.

---

## 4. Building Effective Agents

**Source:** Anthropic, 2024

**Link:** https://www.anthropic.com/engineering/building-effective-agents

**What I studied:**

The guide describes simple agentic workflow patterns including
parallelization, sequential workflows and evaluator-optimizer
workflows.

**What I used:**

I used parallelization where customer signals can be analysed
independently, sequential workflows where later stages depend on
earlier results, and evaluator/refinement for checking generated
actions.

**Architecture impact:**

This led to:

- Parallel Usage Agent
- Parallel Support Agent
- Parallel Transaction Agent
- Parallel KYC Agent
- Sequential downstream reasoning
- Critique/Compliance-Refiner before HITL

---

## 5. Exploring Advanced LLM Multi-Agent Systems Based on
## Blackboard Architecture

**Authors:** Bochen Han and Songmao Zhang, 2025

**Link:** https://arxiv.org/abs/2507.01701

**What I studied:**

The paper explores using a shared blackboard where agents with
different roles can share information and use the current shared
state during problem solving.

**What I used:**

I used the shared-state idea to avoid requiring direct communication
between every pair of specialist agents.

**Architecture impact:**

The Usage, Support, Transaction and KYC agents write structured
findings to a Per-Customer State Board.

Downstream stages can then read the relevant customer state from
this shared board.

---

## 6. Human-in-the-Loop Confirmation with Amazon Bedrock Agents

**Source:** AWS, 2025

**Link:**
https://aws.amazon.com/blogs/machine-learning/implement-human-in-the-loop-confirmation-with-amazon-bedrock-agents/

**What I studied:**

The guide describes pausing an agent workflow before a sensitive
action so that a human can validate it. It also discusses allowing
human input to modify parameters or provide additional context.

**What I used:**

I used the idea of inserting human validation before consequential
customer actions.

**Architecture impact:**

The proposed action goes through the
Critique/Compliance-Refiner before reaching HITL.

The HITL stage supports:

- Approve
- Modify
- Reject

A modification returns the action to the refinement flow before
execution.

---

## 7. Choosing the Right Multi-Agent Architecture

**Author:** Sydney Runkle, LangChain, 2026

**Link:**
https://www.langchain.com/blog/choosing-the-right-multi-agent-architecture

**What I studied:**

The article describes different multi-agent patterns such as
parallel execution, routers and handoffs. It also discusses context
management and choosing an architecture according to the task
requirements.

**What I used:**

I used this pattern-based approach to avoid forcing one coordination
method onto the complete Customer 360 workflow.

**Architecture impact:**

Different patterns are used for different stages:

- Parallel coordination for independent customer signals
- Sequential handoffs for dependent reasoning
- Conditional debate for conflicting conclusions
- Shared state for coordinating specialist findings

---

# Overall Architecture Direction

The research led to a combined architecture rather than a single
multi-agent topology.

The main design is:

**Event Sources**
→ **Event Stream / Live Processing**
→ **Specialist Agents**
→ **Per-Customer State Board**
→ **Synthesis / Correlation**
→ **Life-Event Inference**
→ **Conflict / Ambiguity Check**
→ **Debate + Resolution when required**
→ **Offer / Eligibility**
→ **Retention / Action**
→ **Critique / Compliance-Refiner**
→ **HITL**
→ **Action / Escalation Execution**

The architecture, I made is still a preliminary design 
