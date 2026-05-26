Implemented TF-F070 and ran the feedback loop.

  Changed:

  - Added Vite proxy coverage for /advisory, /provider-governance, and /admin.
  - Added explicit Generate Advisory Review action in Plan Review.
  - Added Provider Governance Test Advisory Route smoke test.
  - Added backend POST /provider-governance/ai-gateway/smoke-test.
  - Improved advisory fetch handling so proxy/non-JSON failures show operational diagnostics instead of raw JSON.parse errors.
  - Refreshed the in-process LiteLLM advisory provider after credential reload.
  - Updated DOCS/ISSUE_REGISTER.md and wrote the processed synthesis note in the knowledge base.

  Verified:

  - uv run pytest tests/test_provider_governance_api.py tests/test_default_advisory_provider_bootstrap.py
  - uv run mypy src\app\api\routes.py tests\test_provider_governance_api.py
  - uv run ruff check tests\test_provider_governance_api.py
  - npm.cmd run typecheck
  - npm.cmd run build
  - git diff --check

  One note: full ruff on src/app/api/routes.py still fails on pre-existing line-length and FastAPI Query(...) findings outside this
  issue. I fixed the TF-F070-added ruff finding and left the older backlog alone. Your existing litellm_config.yaml change to ollama/
  granite4:350m is still present; I did not revert it.