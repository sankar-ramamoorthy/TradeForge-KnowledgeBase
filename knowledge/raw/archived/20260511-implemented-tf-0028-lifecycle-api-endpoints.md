 Implemented TF-0028 under the runtime↔KB loop and kept the boundary aligned with ADR 0002 and ADR 0020: the new POST /lifecycle/transitions endpoint is
  only an HTTP adapter over LifecycleOrchestrationService, so lifecycle validation and event appends still happen in the service layer, not in FastAPI. The
  main changes are in src/app/api/application.py:12 and src/app/api/routes.py:31, with coverage added in tests/test_fastapi_runtime.py:28.

  I also updated the issue register to mark TF-0028 done and record verification in DOCS/ISSUE_REGISTER.md:1027.

  Verification passed:

  - uv run pytest tests\test_fastapi_runtime.py tests\test_lifecycle_orchestration_service.py
  - uv run pytest
  - uv run ruff check .
  - uv run mypy src tests

  166 tests passed.