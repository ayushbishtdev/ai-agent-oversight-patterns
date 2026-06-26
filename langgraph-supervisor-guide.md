# LangGraph Supervisor Architecture Concepts

While LangGraph offers a prebuilt `create_supervisor` utility, production applications require custom routing to manage complex fallback logic and strict oversight pathways. 

## 1. Centralized State
LangGraph uses a centralized state object to share context. The supervisor evaluates this text and outputs the name of the next appropriate node (e.g., "coding_agent").

## 2. Conditional Edges
Routing is handled via conditional edges. 
*   The supervisor returns a string. 
*   LangGraph's edge logic uses that string to transfer state control directly to that specific worker. 
*   Once the worker completes, the edge routes execution back to the supervisor.

## 3. Guardian Evaluation Nodes
To supervise workers, create an evaluation node between the worker and the `END` node.
*   Workers route to `guardian_node`.
*   If semantic checks pass, the guardian routes to `END`.
*   If they fail, the guardian appends a critique to the state and routes back to the worker for revision.

## 4. Timeout Handling
To prevent hanging sub-agents from freezing the system, implement timeouts using Python's async libraries (e.g., `asyncio.wait_for`). If the timeout triggers, catch the exception, update the state graph with an error message, and return control to the supervisor.

## 5. State Persistence (Checkpoints)
To recover from crashes or implement Human-in-the-Loop (HITL), you must pass a checkpointer (like `MemorySaver` or Postgres) to the graph compiler. This saves a snapshot of the multi-agent state after every single node execution, allowing you to use `interrupt_before` to pause execution for human review.
