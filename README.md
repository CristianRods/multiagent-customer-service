# Multi-Agent Customer Service System

Build a multi-agent customer service system using Amazon Bedrock and AgentCore. The system uses specialized AI agents that collaborate to handle customer inquiries — looking up orders, checking policies, and managing escalations.

## Getting Started

Use the [student starter and setup guide](starter/README.md) alongside the Udacity classroom instructions. Runtime deployment uses **AgentCore CLI 0.30.0**; policy retrieval uses **S3 Vectors**.

Complete the TODOs in `starter/src/agent_orchestrator.py`. Implement Tasks 2–6 and create/sync the three Knowledge Bases before the full deployment. The optional Lambda-backed gateway is not required. The starter includes scaffolding and tests, not completed exercise solutions.

---------------------

# NovaMart Multi-Agent Customer Support

Udacity Capstone — Enterprise multi-agent customer support built with the **Strands Agents SDK** and **Amazon Bedrock AgentCore**.

## Overview

NovaMart routes customer requests through an Orchestrator → Workers hierarchy:

- **OrchestratorAgent** (Claude Haiku 4.5) — intent routing and WorkflowState management  
- **InventoryAgent** — order/customer facts from DynamoDB  
- **PolicyAgent** — parallel multi-agent RAG across Returns / Shipping / Warranty Knowledge Bases  
- **RefundAgent** — return eligibility (Standard 30 days / Premium 60 days)  
- **CommunicationAgent** — final customer-facing response  

The system includes Bedrock Guardrails, AgentCore Runtime + Memory, CloudWatch logging, and X-Ray distributed tracing.

Implementation lives in [`starter/src/agent_orchestrator.py`](starter/src/agent_orchestrator.py).  
Setup details: [`starter/README.md`](starter/README.md).

## Submission evidence for reviewers

Screenshots are in [`screenshots/`](screenshots/). Please review them in this order:

### 1. Automated tests (120/120)

| File | What it shows |
|------|----------------|
| [`screenshots/tests-120-all-multiagent.png`](screenshots/tests-120-all-multiagent.png) | `python tests/test_agent.py all` — **120/120 pts**, Tasks 2–6 all pass |

### 2. Live end-to-end run

| File | What it shows |
|------|----------------|
| [`screenshots/test-agent_orchestrator.png`](screenshots/test-agent_orchestrator.png) | Local run of `python src/agent_orchestrator.py test` — three scenarios (refund, policy, math) |

### 3. X-Ray / observability (Task 6)

| File | Scenario | Call chain shown |
|------|----------|------------------|
| [`screenshots/trace-map-test-agent-novamart.png`](screenshots/trace-map-test-agent-novamart.png) | Aggregated / policy path | **NovaMart-Orchestrator** → PolicyAgent, CommunicationAgent, KnowledgeBase:returns / shipping / warranty |
| [`screenshots/console-agent_orchestrator_refund_orderid.png`](screenshots/console-agent_orchestrator_refund_orderid.png) | Return `ORD-27176` | Orchestrator → InventoryAgent → RefundAgent → CommunicationAgent |
| [`screenshots/console-agent_orchestrator-return_policy.png`](screenshots/console-agent_orchestrator-return_policy.png) | Return policy question | Orchestrator → PolicyAgent + 3 KB retrievers → CommunicationAgent |
| [`screenshots/console-agent_orchestrator-calculation_math.png`](screenshots/console-agent_orchestrator-calculation_math.png) | Math / discount question | Orchestrator → CommunicationAgent only (no Inventory / Policy / Refund) |

## Rubric checklist

- [x] Completed agent builders and routing (Task 2)  
- [x] Guardrail + AgentCore Runtime (Task 3)  
- [x] AgentCore Memory — SESSION_SUMMARY, 7-day retention (Task 4)  
- [x] Three Bedrock Knowledge Bases synced (Task 5)  
- [x] CloudWatch + X-Ray observability (Task 6)  
- [x] Populated [`starter/.env`](starter/.env) with KB IDs, runtime ARN, and guardrail config  

## Project layout

```text
multiagent-customer-service/
├── screenshots/          # Reviewer evidence (this README)
├── starter/              # Student workspace
│   ├── .env              # KB IDs, AgentCore Runtime ARN, Guardrail
│   ├── src/agent_orchestrator.py
│   ├── tests/test_agent.py
│   └── README.md         # Full setup / architecture guide
└── requirements.txt
```

## License

[License](../LICENSE.md)
