
• Completed TF-0027 under the Runtime → KB Development Loop.

  Runtime commit:
  ac7cc32 Add TF-0027 FastAPI runtime boundary

  KB commit:
  5af0b84 Capture TF-0027 FastAPI runtime reasoning

  Implemented:

  - Added FastAPI runtime dependency and updated uv.lock.
  - Added ADR 0020 for the HTTP boundary.
  - Added create_app() and module-level app in src/app/api/application.py.
  - Added /health runtime route.
  - Added tests proving startup and that lifecycle/replay/workspace endpoints remain out of scope.
  - Updated README, issue register, and roadmap.
  - Captured planning and implementation notes into KB raw/processed knowledge.

  Verification passed:

  - uv run pytest - 163 passed
  - uv run ruff check .
  - uv run mypy src tests
  - docker compose config
  - Local server check: http://127.0.0.1:8000/health

  Current state:

  - Runtime branch: feature/tf-0027-fastapi-runtime, clean
  - KB branch: M6, clean and ahead of origin/M6 by 1
  - FastAPI server is running locally at http://127.0.0.1:8000
