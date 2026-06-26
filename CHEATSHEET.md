# AI Agent Oversight Cheatsheet

Use this reference to map your defense-in-depth strategy for multi-agent systems.

## Guardrails vs. Guardian Agents
Never force a guardian agent to do a guardrail's job.

| Feature | Guardrails (Constrain) | Guardian Agents (Supervise) |
| :--- | :--- | :--- |
| **Logic Type** | Deterministic (Regex, hard limits) | Probabilistic (LLM inference) |
| **Execution** | Instant, extremely cheap | Slower, consumes LLM tokens |
| **Catches** | PII leaks, profanity, token size limits | Semantic hallucinations, logic loops, poor tool choices |
| **Placement** | Input/Output borders | Between worker outputs and final state routing |

## Autonomy Risk Tiers
Implement these tiers to prevent human operators from becoming a bottleneck.
1. **Tier 1 (Auto-Execute):** Internal web scraping, querying vector databases. 
2. **Tier 2 (Execute & Notify):** Creating Jira tickets, summarizing notes. Triggers an async webhook (Slack/Teams).
3. **Tier 3 (Require Approval):** Mutating production SQL, transferring funds, external emails. Halts graph execution for manual HITL sign-off.

## Tooling Matrix (2026)
*   **LangSmith:** Best for LangGraph. Visualizes explicit conditional edges and state checkpoints natively.
*   **Langfuse:** Best open-source option. Framework-agnostic, excellent granular cost tracking, self-hostable via Docker.
*   **AgentOps:** Best for CrewAI/AutoGen. Native SDK decorators track unique agent roles and tool failure diagnostics.
