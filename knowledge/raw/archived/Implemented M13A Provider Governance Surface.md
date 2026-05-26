• Implemented M13A end to end, one issue at a time, using the runtime-KB loop.

  What landed:

  - M13A roadmap/register entries completed through TF-F068.
  - Provider governance design docs added under DOCS/.
  - Read APIs:
      - GET /provider-governance
      - GET /provider-governance/ai-gateway
      - POST /admin/credentials/{provider_id}/validate
  - Credential status now supports invalid and last_validated_at.
  - LiteLLM is surfaced as an AI gateway with route aliases and safe metadata, not as a normal provider.
  - Frontend now has a dedicated Provider Governance surface at:
      - http://127.0.0.1:5173/workspaces/provider-governance
  - Right rail provider setup was replaced with a compact provider-status rail and configure link.
  - M13A readiness record added: DOCS/m13a-readiness-gate.md.
  - KB planning/implementation captures were written and processed for TF-F064 through TF-F068.

  Verification passed:

  - uv run pytest tests/test_provider_governance_api.py tests/test_admin_credentials.py tests/test_fundamentals_overlay.py tests/
    test_default_advisory_provider_bootstrap.py
  - uv run mypy src\app\api\admin_routes.py src\app\api\routes.py src\security\credential.py tests\test_admin_credentials.py
    tests\test_provider_governance_api.py
  - npm.cmd run typecheck
  - npm.cmd run build
  - git diff --check

  The frontend dev server is running on http://127.0.0.1:5173. Note: the runtime API still needs to be running separately for the page to load live data.
  Full ruff on routes.py still has pre-existing long-line/B008 findings outside the M13A changes; targeted ruff checks for changed test/admin surfaces
  passed.