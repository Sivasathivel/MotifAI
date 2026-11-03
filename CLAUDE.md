# MotifAI — Claude Code project guide

## What this project is
MotifAI (`pip install motif-ai`, `import motif_ai`) is a lightweight Python framework
for multi-agent LLM workflows — MARL episode loop, cost-aware LLM pool, OTEL
observability, and compile-to-any-runtime.

## Package structure
- `motif_ai/` — core package (was `choreo_mini/` before v0.3.0 rename)
- `motif_ai/core/` — Workflow, AgentNode, Episode, LLM, LLMPool, hooks, belief state
- `examples/` — runnable demos; `cusma_negotiation.py` is the headline demo
- `tests/` — 272 tests, all must pass before any commit

## Run commands
```bash
python3 -m pytest --tb=short -q          # full test suite
python3 examples/cusma_negotiation.py    # demo (no API key = demo mode)
python3 -m build                         # build dist/
python3 -m twine upload dist/*           # publish to PyPI
```

## Key conventions
- Never hardcode API keys — always `os.getenv("OPENAI_API_KEY")`
- Import root: `from motif_ai import ...` or `from motif_ai.core.X import ...`
- All public API surfaces are in `motif_ai/__init__.py`
- Tests live in `tests/`; match the file they test (`test_workflow.py` → `workflow.py`)
- Commit messages: conventional commits (`feat:`, `fix:`, `chore:`)

## Branch strategy
- `main` — stable, PyPI-published
- `dev` — active development; PR into main when ready to release

## PyPI / release
- Package: `motif-ai` version `0.3.0`
- Rebuild dist after any rename: `rm dist/* && python3 -m build`
- GitHub release tag: `v0.3.0 — "No Nash Required"`
