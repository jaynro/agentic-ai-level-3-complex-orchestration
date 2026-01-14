# Project 2 – The Customer Resolution Squad (Retail Support AI) – README

## Introduction

The **Customer Resolution Squad** is an AI-driven customer support system built for a retail/e-commerce context. It utilizes a team of specialized AI agents to automatically handle customer inquiries in three main areas:

* **Order & Shipping Inquiries** – e.g. “Where’s my order?”, “When will my package arrive?”
* **Billing & Refund Requests** – e.g. “I was charged twice,” “I want to return my item for a refund.”
* **Technical Support** – e.g. “Your website crashes when I try to checkout,” “The mobile app isn’t working.”

By divvying up tasks among specialist agents (Order Agent, Billing Agent, Tech Agent) and coordinating them with a central agent, the system can provide quick, context-appropriate responses.

### Key Highlights

* Multi-agent orchestration using Google’s ADK (Agent Development Kit) patterns.
* Integration of external data (order database, FAQ knowledge base) through tools or API logic.
* Responsible AI principles: accuracy, non-harm, politeness.
* Domain-specific, but extensible design.

---

## System Architecture

### Components:

* **SupportCoordinator**: Classifies queries and delegates to specialists.
* **OrderAgent**: Fetches order status, tracking info from order DB.
* **BillingAgent**: Answers refund/charge issues using policy info or transaction data.
* **TechAgent**: Troubleshoots using a knowledge base of known issues.
* **Fallback/Escalation**: Handles unknown categories or failed resolutions.

```text
Customer Query --> [SupportCoordinator] --(classify)--> Specialist Agent (Order/Billing/Tech)
                   [SupportCoordinator] <--(response)-- Specialist Agent
Customer Query --> [SupportCoordinator] --(if unknown)--> Escalation (human)
```

All agents communicate via text and are simulated in a single Python script for clarity.

---

## Data Sources and Configuration

* **Order Database** (`data/orders.json`):

  ```json
  {
    "1001": { "status": "In Transit", "ETA": "2026-02-15", "carrier": "UPS", "tracking": "UPS12345" }
  }
  ```

* **Billing Policies** (`data/policies.json`): Refund timelines, return policies.

* **Tech Knowledge Base** (`data/knowledge_base.json`): Known issues and solutions.

No API keys are needed for this offline prototype.

---

## Running the Support AI

Run the main script:

```bash
python support_bot.py
```

### Sample Interactions

**Query:** "Where is my order #1001?"

* **Response:** “Hi there! I’m sorry your order #1001 hasn’t arrived yet. It is **In Transit** with UPS, expected by Feb 15, 2026. Tracking: UPS12345.”

**Query:** “I want a refund.”

* **Response:** “I’m sorry the item didn’t work out. A refund will be processed within 5–7 business days. A return label has been emailed to you.”

**Query:** “App crashes during checkout.”

* **Response:** “Please update to version 2.3.1. This fixes the checkout crash. If the issue persists, clear cache or reinstall.”

**Query:** “Do you have any job openings?”

* **Response:** “I’m sorry, I’m not able to assist with that. I’ve forwarded your message to our team.”

---

## Implementation Details

* **Google ADK Usage**: Follows patterns like `SequentialAgent` in spirit, though routing logic is implemented via `if/else` for simplicity.
* **LLM Integration**: GPT-4 or Gemini used mainly for composing responses. Templating is used for repetitive replies.
* **ADK Tools**: Simulated via local data loading; full ADK tool invocation omitted for simplicity.
* **Logging**: Logs include classification result, agent decisions, and LLM output.

---

## Customization & Extensions

* **Add New Agent**: Define a new agent class, update classifier, and coordinator logic.
* **Improve Classification**: Replace with a trained classifier or enhance prompt.
* **Real Data Integration**: Replace JSON with API queries or database connections.
* **UI Layer**: Wrap the logic into a Flask API or chatbot interface for deployment.

---

## Testing and Observations

* Handled edge cases like unknown queries with graceful fallback.
* Verified consistent tone using prompt templates and LLM tuning.
* Known limitation: single-category classification per query.

---

## Conclusion

Project 2 showcases how specialized agents coordinated by a central logic can autonomously handle retail customer support at scale. Each agent remains simple and focused, enabling a robust and extensible system aligned with Responsible AI principles.

**Next:** Project 3 – A self-healing software system (DevOps AI).
