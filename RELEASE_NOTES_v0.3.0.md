## MotifAI v0.3.0 — "No Nash Required"

> Formerly distributed as `choreo-mini`. Install with `pip install motif-ai`.

---

### Renamed: choreo-mini → MotifAI

The package is now published as `motif-ai` (`import motif_ai`) to avoid collision with the Choreo robotics framework. All APIs are unchanged — update your install command and imports, nothing else.

---

### CUSMA multi-party negotiation demo

The headline addition: three AI agents — USA, Canada, and Mexico — negotiate a trade agreement in real time. Each country runs its own `Workflow`, calls a live LLM, and independently decides when it is done.

The key architectural idea: **asymmetric termination**. Each party signals `READY_TO_STOP: YES` for its own reason — the USA when it has secured the lion's share, Canada when core interests are protected, Mexico when trade diversification makes further concessions irrelevant. No Nash equilibrium required. The episode ends when all three signal YES, or declares a deadlock under CUSMA Article 34.7 if `max_rounds` is exhausted first.

Each round produces a three-section proposal:
- **Negotiating Position** — demands and offers at the table
- **Domestic Policy Measures** — unilateral actions with specific Gini reduction mechanisms (points), inflation controls (CPI percentage points), and investment estimates
- **In Our National Interest** — GAGI trajectory with constituency-level approval changes by named state or province

Urgency escalates automatically each round. Deadlocks surface withdrawal-notice and MFN-reversion implications rather than silently terminating.

Run it in demo mode (no API key) or live with any OpenAI-compatible endpoint:

```bash
pip install motif-ai openai
export OPENAI_API_KEY=sk-...
python examples/cusma_negotiation.py
```

---

### LLM pool scheduling

`LLMPool` is a drop-in replacement for `llm=` on any `AgentNode`. It routes across multiple backends with four policies — `cost_first`, `priority`, `round_robin`, `reliability` — falls back transparently on failure, and emits observability events for every routing decision.

---

### Observability and OTEL tracing

A unified hook system fans out to any combination of `StdoutHook`, `JsonFileHook`, and `OTLPHook` via `CompositeHook`. Every agent call, LLM request, retry, pool routing decision, and episode step carries an OTEL-compatible `trace_id` / `span_id`. Write a custom hook in one method.

---

### Reliability

Automatic retry with exponential backoff built into the core LLM class. Transient HTTP errors surface as `LLMError` with status code and attempt count; permanent failures propagate immediately.

---

### Diagnostics

Typed exceptions throughout (`AgentNotFoundError`, `AgentRegistrationError`, `EpisodeError`, `LLMError`, `ConversionError`). `wf.dump()` returns a full JSON snapshot of agent call counts, latency, history length, belief state, and profiling data at any point.

---

**272 tests passing across Python 3.10–3.13.**
