# Runaway Agent Kill-Switch Checklist

Runaway agents will burn your API budget while returning perfectly healthy `HTTP 200 OK` statuses. Ensure your orchestration layer passes these checks before deploying.

- [ ] **Graph Depth Tracking:** Are you monitoring the number of sequential node-to-node hops? (e.g., trip the switch if transitions exceed 25 steps).
- [ ] **Token-Cost Accumulation:** Does your supervisor record cumulative token spend in the shared state after *every* node execution?
- [ ] **Hard Budget Caps:** Is there a conditional edge evaluating the session cost against a strict threshold (e.g., $5.00 per query) that routes directly to an `END` state if breached?
- [ ] **Repetitive Tool Detection:** Does your state monitor check if a worker agent invokes the exact same tool with identical parameters multiple times sequentially?
- [ ] **Swarm Circuit Breakers:** If a worker agent fails repeatedly or floods a downstream agent with queries, does the system open a circuit breaker to sever inter-agent communication and isolate the malfunctioning worker?
- [ ] **Rollback Capability:** If an agent fails, can you utilize your database checkpointer to revert the state graph to the last known successful node?
