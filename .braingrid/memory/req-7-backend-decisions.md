---
name: "REQ-7 Backend Execution Decisions"
description: "Architectural decisions for REQ-7 Python Backend & Inference Execution"
type: project
---

# REQ-7 Backend Execution Decisions

# REQ-7 Python Backend & Inference Execution Decisions

The following architectural decisions were made by the user for REQ-7:

1. **Execution Method**: Python scripts must run as isolated **subprocesses** on the local server (via Node's `child_process.spawn` or Python's `asyncio.create_subprocess_exec`), rather than a separate microservice.
2. **Task Handling**: Inference tasks must be **asynchronous** with real-time status updates pushed via WebSockets (Supabase Realtime) or polled by the client.
3. **Concurrency Limits**: Administrators must be able to **set the maximum number of concurrent Python subprocesses** that can run at a time.
4. **Process Cleanup**: Subprocesses must be **explicitly closed and terminated** after execution finishes (or upon cancellation/timeout) to prevent zombie processes.

**Why:** To optimize local server resource utilization, prevent event loop blocking, and maintain system stability under heavy user load.
